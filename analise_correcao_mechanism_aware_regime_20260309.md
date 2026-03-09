# Analise e Proposta de Correcao — Mechanism-aware Regime (2026-03-09)

## Pergunta auditada
Mechanism-aware regime: detalhar sobrescritas de covariaveis (unidades, magnitudes, timing), interacao com escalonamento e propagacao temporal (`recency`, `streak`), escopo de varredura de parametros (`alpha_week0`, `alpha_week1`, regras de janela) e diagnosticos para `Delta S` negativo.

## Status objetivo
A pergunta continua valida para o manuscrito, por dois motivos:
1. A implementacao existe no notebook, mas a documentacao no texto principal ainda nao fecha todas as condicoes operacionais (canal efetivamente impactado, unidade da intervencao e diagnostico causal-operacional do sinal de `Delta S`).
2. Ha evidencia de discrepancia entre o que parece varrido na grade de sensibilidade e o que foi efetivamente alterado no pacote exportado principal.

## Evidencias tecnicas (estado atual)

### 1) Sobrescritas e timing estao codificados no notebook
- Parametros default compartilhados: `W=2`, `alpha_week0=0.35`, `alpha_week1=0.10`, `decay_type=kb2023_step_2w`, `window_exclusive_upper=True`.
  - Fonte: `TCM-Student-Dropout_updated.ipynb` (bloco P13/P15; linhas aproximadas 5520+ no JSON).
- Janela ativa: `week >= t_star` e `week < t_star + W` (limite superior exclusivo).
- `boost_factor` por semana ativa:
  - semana 0: `1 + alpha_week0` (uplift de +35%)
  - semana 1: `1 + alpha_week1` (uplift de +10%)
- Trigger: primeiro `t_star` com `recency >= r_star` (regra semanal).

### 2) Canais e propagacao
- Implementacao principal de politica aplica overwrite em `total_clicks` com `boost_factor`.
- Existe bloco com recalculo stateful de `recency/streak` a partir de atividade contrafactual (`activity_cf`) no notebook.
- No pacote exportado atual, os diagnosticos indicam impacto efetivo somente em `total_clicks`:
  - `table_policy_covariate_overwrites.csv`: `rows_changed_total_clicks=4816`, `rows_changed_recency=0`, `rows_changed_streak=0`.
  - `table_policy_covariate_propagation_checks.csv`: `frac_changed_recency=0.0`, `frac_changed_streak=0.0`.
- Metadata ja registra essa limitacao:
  - `outputs_v2/metadata.json` -> `covariate_impact_scope = click_based_engagement_proxy_in_exported_run`.

### 3) Escalonamento e pipeline
- Features numericas incluem `total_clicks`, `recency`, `streak`, com transformacao dentro do pipeline (padronizacao antes da inferencia).
- As sobrescritas sao aplicadas no dominio bruto e depois passam no mesmo pipeline de preprocessamento/inferencia.
  - Fontes: `experimental_design_body.tex` (secao de modelagem e calibracao) e notebook.

### 4) Sweep de parametros
- Sim, houve varredura de grade:
  - `table_rq2_sensitivity_grid.csv` com `grid_size=216` (tambem em `table_feature_set_provenance.csv` e `metadata.json`).
  - Variacao de `r_star`, `W`, `decay_type`, `alpha_week0`, `alpha_week1` e cenarios de choque.
- Nao ha evidencia de sweep sistematico de canais alem de `total_clicks` no pacote principal reportado.

### 5) Diagnosticos para `Delta S` negativo observado
- No resultado principal por horizonte:
  - `table_policy_deltaS_at_horizons.csv` (cenario referencia `hypothetical_A`):
    - `deltaS_mech(T_policy=18) = -0.007766`
    - `deltaS_mech(T_eval_policy=38) = -0.013352`
- Diagnostico de hazard indica deslocamento medio adverso no ramo mechanism-aware:
  - `table_policy_covariate_propagation_checks.csv`:
    - `hazard_delta_mech_mean = +0.000207` (aumento de risco)
    - `hazard_delta_shock_mean = -0.001719` (reducao de risco)
- Cobertura de efeito e pequena em termos de linhas alteradas:
  - `rows_changed_any/rows_total = 0.0207` (aprox. 2.07%).

## Leitura tecnica consolidada
O ramo mechanism-aware esta operacionalizado, mas no pacote exportado ativo ele se comporta como perturbacao predominantemente em `total_clicks` com janela curta, sem alteracao efetiva de `recency/streak` nos artefatos de auditoria. Isso reduz a forca mecanistica esperada via propagacao temporal e pode explicar por que o sinal agregado de `Delta S` permaneceu negativo na execucao reportada.

---

## Proposta de correcao em duas etapas

## Etapa 1 — Notebook (fonte de verdade)
Objetivo: tornar o regime mechanism-aware auditavel e coerente com a narrativa causal-operacional.

1. Unificar a funcao de politica mechanism-aware em um unico operador reutilizado por RQ2 e RQ3.
   - Evitar implementacoes paralelas com comportamento diferente para `recency/streak`.

2. Tornar explicito no codigo o contrato de canais:
   - `channel_mode` com opcoes minimas:
     - `clicks_only`
     - `clicks_plus_stateful_recency_streak`
     - `clicks_plus_direct_recency_streak`

3. Exportar tabela de especificacao operacional por execucao:
   - `table_policy_mech_operator_spec.csv` com:
     - unidade (`weeks`), magnitudes (`alpha_*`), regra de janela, regra de trigger,
     - canal ativo, regra de atualizacao de `recency/streak`, e hash da funcao.

4. Expandir a sensibilidade para incluir canais (nao so agenda temporal):
   - cruzar grade atual (`r_star`, `W`, `alpha_*`, `decay_type`) com `channel_mode`.
   - exportar `table_rq2_sensitivity_grid_channels.csv`.

5. Adicionar diagnostico de atribuicao de sinal:
   - decompor `Delta hazard` e `Delta S` por canal e por semana.
   - exportar `table_policy_mech_attribution_by_week.csv` e resumo em horizonte.

6. Definir gate de consistencia antes de export final:
   - falhar execucao se metadata declarar `click_based_engagement_proxy...` quando manuscrito estiver em modo de narrativa multicanal.

## Etapa 2 — TEX (manuscrito)
Objetivo: remover ambiguidade metodologica para revisao.

1. Inserir um paragrafo formal com formula do overwrite:
   - `total_clicks_cf(t) = total_clicks(t) * boost_factor(t)`
   - `boost_factor(t)` com `alpha_week0`, `alpha_week1`, `W`, e regra de janela.

2. Declarar explicitamente unidade e timing:
   - trigger semanal por `recency >= r_star`;
   - janela ativa com limite superior exclusivo (`[t_star, t_star + W)`).

3. Declarar escopo de canal da execucao reportada:
   - se `clicks_only`, escrever isso literalmente e tratar como limitacao.

4. Relatar varredura de parametros com fronteiras:
   - o que foi varrido (agenda) e o que nao foi (canais, se aplicavel na rodada).

5. Incluir diagnostico do `Delta S` negativo com tabela curta:
   - `deltaS_mech` em `T_policy` e `T_eval_policy`;
   - `hazard_delta_mech_mean` vs `hazard_delta_shock_mean`;
   - `frac_changed_any`.

6. Ajustar conclusao RQ2:
   - separar "resultado observado nesta execucao" de "efeito estrutural esperado sob operadores alternativos".

---

## Criterio de aceite para fechar a pergunta
1. O notebook exporta especificacao operacional + sensibilidade por canais + atribuicao de sinal.
2. O manuscrito descreve exatamente a implementacao executada (sem inferencia implícita de canal).
3. A explicacao do `Delta S` negativo fica ancorada em diagnostico numerico reproduzivel, nao apenas em interpretacao narrativa.
