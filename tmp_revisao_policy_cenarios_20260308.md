# Revisao em 3 etapas: policy anchor, cenarios alternativos e apresentacao no paper

Data: 2026-03-08

## Escopo desta revisao

Esta nota consolida o que hoje esta coerente, o que esta apenas parcialmente claro, e o que precisa ser alterado no notebook, no `ref.bib` e nos arquivos `.tex` para:

1. deixar explicito o que realmente esta ancorado em Kay & Bostock (2023);
2. introduzir tres cenarios de intensidade para `delta_shock_ablation` (`0.08`, `0.20`, `0.60`);
3. apresentar esses cenarios no paper de forma metodologicamente limpa.

Observacao critica: o DOI `10.5204/ssj.2848` confirma o artigo **The Power of the Nudge: Technology Driving Persistence**, de Ellie Kay e Paul Bostock, `Student Success`, `14(2)`, `8-18`. Portanto, a referencia textual "Using nudges to improve student engagement in learning management systems: A learning analytics approach. Student Success, 14(2), 24-39" nao bate com o DOI nem com a pagina oficial do periodico. Nao recomendo sobrescrever o `ref.bib` com essa forma sem nova verificacao externa.

---

## Etapa 1 - Conferencia da ancoragem em Kay & Bostock (2023)

### 1.1 Trigger: "sem engajamento no LMS por 7 dias"

Status: **parcialmente ok**.

O que esta ok:

- O notebook e os artefatos exportados registram explicitamente o trigger como `flag if no LMS engagement in last 7 days (approx recency>=1 week)`.
- Isso aparece no contrato exportado em `outputs_v2/tables/table_policy_spec.csv`.
- A metodologia ja cita isso de forma textual.

O que ainda falta:

- Em `experimental_design_body.tex`, a tabela/descricao da policy ainda fala em `r* = 1` e `W = 2`, mas nao explicita, no proprio texto do protocolo, que `r*=1` esta sendo usado como proxy semanal para "7 dias sem engajamento no LMS" ancorado em Kay & Bostock.
- Em `results_body.tex`, o leitor ve os numeros da policy, mas nao ve claramente a origem operacional do gatilho.

Acao recomendada:

- Em `experimental_design_body.tex`, substituir a formulacao abstrata do trigger por uma formulacao operacional: `weekly trigger activated after 7 days without LMS engagement (implemented as recency >= 1 week)`.
- Em `results_body.tex`, incluir uma frase curta lembrando que o RQ2 usa o mesmo gatilho operacional descrito na metodologia.

### 1.2 Frequencia: checagem semanal

Status: **ok no notebook / pouco visivel no paper**.

O que esta ok:

- `table_policy_spec.csv` registra `trigger_frequency = weekly`.
- `methodology_body.tex` ja diz que a ancora e registrada com `weekly checking frequency`.

O que ainda falta:

- Isso ainda nao aparece com destaque suficiente no bloco de desenho experimental.

Acao recomendada:

- Em `experimental_design_body.tex`, acrescentar `weekly checking frequency` na tabela ou no paragrafo da scenario specification.

### 1.3 Janela de 2 semanas (`W = 2`)

Status: **ok, mas precisa ser melhor amarrado ao estudo-fonte**.

O que esta ok:

- O notebook/export trava `W_weeks = 2`.
- A metodologia ja diz `W_weeks = 2`.
- O desenho experimental tambem mostra `W = 2`.

O que ainda falta:

- O paper ainda nao distingue com clareza o que vem de Kay & Bostock como ancora operacional do tipo de intervencao e da janela curta, versus o que e apenas especificacao congelada do seu cenario.

Acao recomendada:

- Em `methodology_body.tex` e `experimental_design_body.tex`, escrever explicitamente que a **classe da intervencao** e uma `short-term digital nudge` com janela curta de duas semanas, inspirada no estudo de Kay & Bostock, mas que a intensidade numerica do efeito continua sendo uma escolha de cenario.

### 1.4 Propagacao em covariantes de engajamento

Status: **conceitualmente declarada, empiricamente incompleta no pacote atual**.

O que o paper diz hoje:

- O paper fala em regime `mechanism-aware`, com overwrite de covariantes e propagacao stateful.
- A metodologia descreve `total_clicks`, `recency`, `streak`, `submitted_this_week` e covariantes contextuais como features de engajamento de curto prazo.

O que o pacote exportado mostra hoje:

- `outputs_v2/tables/table_policy_covariate_overwrites.csv` mostra mudanca apenas em `total_clicks`.
- `outputs_v2/tables/table_policy_covariate_propagation_checks.csv` mostra `frac_changed_clicks > 0`, mas `frac_changed_recency = 0` e `frac_changed_streak = 0`.
- `rows_changed_nonactive = 0`, ou seja, o pacote atual nao materializa evidencias de propagacao alem das linhas ativas.

Implicacao:

- Se o texto disser que a policy propaga de forma visivel em multiplas covariantes de engajamento, isso hoje esta forte demais para o evidence package exportado.
- Se a intencao e sustentar uma narrativa de propagacao mais ampla, o notebook precisa realmente exportar evidencias de mudanca em `recency` e `streak` ou em outras covariantes derivadas.

Sobre o exemplo de `39%`:

- Nao encontrei esse valor no pacote atual.
- O notebook registra internamente marcadores de ancora como `time_on_task_uplift_day7_35pct` e `uplift_day14_lt_10pct`, isto e, hoje o proprio notebook parece congelar `35%` na semana 0 e `<10%` em 14 dias, nao `39%`.
- Portanto, no estado atual, **nao ha base interna para escrever `39%` no paper**.

Acao recomendada:

- Escolher uma das duas linhas abaixo e mantela consistente:

Linha A: corrigir o notebook para produzir evidencias reais de propagacao stateful alem de `total_clicks`.

Linha B: restringir o texto do paper para algo mais fiel ao que esta exportado hoje, por exemplo: `the mechanism-aware regime primarily perturbs click-based engagement intensity, with limited downstream state propagation in the exported run`.

### 1.5 As covariantes sao "inspiradas" em Kay & Bostock?

Status: **isso ainda nao esta claro o suficiente no paper**.

Leitura tecnica:

- As covariantes `total_clicks`, `recency`, `streak`, `submitted_this_week` sao plausiveis como proxies de engajamento e reengajamento em LMS.
- Elas sao coerentes com a logica do estudo de Kay & Bostock, que trata de nudges apos risco de desengajamento no LMS.
- Mas o paper ainda nao diz claramente que essas covariantes sao **proxies inspiradas no mecanismo de engajamento/reengajamento** do estudo, em vez de variaveis literalmente copiadas do artigo.

Acao recomendada:

- Em `methodology_body.tex`, apos a frase sobre o feature set principal, inserir uma frase como:

`These engagement-state covariates are not literal replicas of Kay and Bostock's variables, but LMS-based proxies inspired by the same disengagement/re-engagement logic used to operationalize the nudge trigger.`

### 1.6 Tipo de intervencao: digital nudge de curto prazo

Status: **insuficiente no paper atual**.

O que esta faltando:

- O manuscrito fala em `operational nudge rule`, mas nao deixa suficientemente claro que o estudo-fonte e um **digital short-term nudge**, isto e, um contato automatizado (texto/email) para incentivar reengajamento no LMS.
- Tambem nao esta suficientemente claro que a sua simulacao pretende representar **a mesma classe de intervencao**, nao qualquer policy abstrata.

Acao recomendada:

- Em `methodology_body.tex` e `experimental_design_body.tex`, inserir uma frase do tipo:

`The simulated intervention is intended to represent the same class of short-term digital nudge studied by Kay and Bostock (2023), namely an automated re-engagement prompt triggered after LMS inactivity; the notebook simulates its structural impact on model-implied hazard rather than the observed causal effect of real messages.`

---

## Etapa 2 - Tres cenarios finais para `delta_shock_ablation`

### 2.1 Recomendacao de tres cenarios finais

Decisao consolidada:

- `0.08` = **cenario conservador ancorado**.
- `0.20` = **cenario hipotetico A**.
- `0.60` = **cenario hipotetico B**.

Justificativa:

- `0.08` continua funcionando como leitura prudente e conservadora.
- `0.20` preserva o baseline legado do notebook, mas passa a ser explicitamente hipotetico.
- `0.60` cria um stress test forte, util para mostrar sensibilidade da politica a uma intensidade muito mais agressiva.

Observacao metodologica importante:

- `0.60` so faz sentido se for apresentado como **upper-bound hipotetico** ou **stress test estrutural**.
- Ele nao deve ser descrito como intensidade plausivel ou empiricamente ancorada.
- No texto principal, esse cenario deve aparecer como teste de sensibilidade extrema, nao como expectativa substantiva central.

### 2.2 Como fundamentar o `0.08`

Importante: `0.08` nao deve ser vendido como conversao literal do efeito de Sneyers & De Witte para reducao de hazard semanal.

Forma correta de escrever:

- `0.08` e um **cenario conservador ancorado qualitativamente** na ideia de efeitos moderados de retencao reportados por Sneyers & De Witte (2018).
- Ou seja, ele e uma calibracao prudente e nao uma traducao algebraica `1:1` do `d = 0.15` da meta-analise para `delta_shock_ablation`.

Formula recomendada para o paper:

`We treat delta as a scenario-intensity parameter rather than a directly identified causal effect. The conservative setting (delta = 0.08) is anchored qualitatively to the moderate retention gains synthesized by Sneyers and De Witte (2018), whereas higher settings are used as hypothetical stress-test intensities.`

### 2.3 Como enquadrar `0.20` e `0.60`

Leitura recomendada:

- `0.20` = cenario hipotetico A, correspondente ao baseline historico do notebook e util como comparador com a versao anterior do paper.
- `0.60` = cenario hipotetico B, interpretado como teste extremo para avaliar monotonicidade, saturacao e sensibilidade estrutural do contraste `Delta S(t)`.

Advertencia:

- Se `0.60` produzir curvas muito distantes ou pouco criveis, o paper nao deve vender isso como resultado substantivo; deve apresentar como limite superior ilustrativo.
- Se ele dominar demais a narrativa visual, a figura principal pode mostrar `0.08` e `0.20`, deixando `0.60` em apendice ou em painel secundario.

### 2.4 O que precisa mudar no notebook

Hoje o notebook ainda esta montado para um unico baseline `delta_shock_ablation = 0.20` e a tabela `table_rq2_sensitivity_grid.csv` varia `W` e o schedule mecanismo-aware, mas **nao** varia `delta_shock_ablation` como eixo principal de cenario do paper.

Alteracoes necessarias:

1. Criar um grid explicito de cenarios para o shock regime:

```python
shock_scenarios = [
    {
        "scenario_id": "anchored_conservative",
        "scenario_label": "Conservative anchored",
        "delta_shock_ablation": 0.08,
        "scenario_source": "SneyersDeWitte2018_anchored",
        "scenario_status": "anchored"
    },
    {
    "scenario_id": "hypothetical_A",
    "scenario_label": "Hypothetical A",
    "delta_shock_ablation": 0.20,
    "scenario_source": "hypothetical_legacy_baseline",
        "scenario_status": "hypothetical"
    },
    {
    "scenario_id": "hypothetical_B",
    "scenario_label": "Hypothetical B",
    "delta_shock_ablation": 0.60,
    "scenario_source": "hypothetical_extreme_stress_test",
        "scenario_status": "hypothetical"
    },
]
```

2. Fazer `P13`/`P14` recalcular `hazard_1_shock`, `pp_traj`, `weekly` e `deltaS` para cada `scenario_id`.

3. Incluir colunas de rastreabilidade em todas as saidas de policy:

- `scenario_id`
- `scenario_label`
- `scenario_status`
- `scenario_source`
- `delta_shock_ablation`

4. Exportar pelo menos estes artefatos novos ou ampliados:

- `table_policy_scenarios_main.csv`
- `table_policy_deltaS_at_horizons_by_scenario.csv`
- `table_policy_deltaS_by_week_by_scenario.csv`
- `fig_policy_deltaS_by_week_shock_scenarios.png`
- `fig_policy_deltaS_at_horizons_scenarios.png`

5. Atualizar `metadata.json` para registrar claramente:

- qual parte da policy e ancorada em Kay & Bostock (`trigger`, `weekly frequency`, `W=2`, intervention type);
- qual parte e ancorada em Sneyers & De Witte (`delta=0.08` como cenario conservador);
- quais partes sao hipoteticas (`0.20`, `0.60`).

6. Corrigir `table_policy_scenario_params.csv`, que hoje perde informacao importante de ancora:

- `anchor_study` esta vazio;
- `anchor_trigger_note` esta vazio;
- os campos de metrica ancorada (`anchor_metric_week0`, `anchor_metric_week1`) tambem saem vazios.

### 2.5 Evidencias minimas para o paper

Se voce quiser apenas RQ2 com tres cenarios, o minimo necessario e:

- uma tabela de horizontes (`T_policy`, `T_eval_policy`) por cenario;
- uma figura com `Delta S(t)` para os tres valores de `delta`, ou uma figura principal com `0.08` e `0.20` e o `0.60` em painel secundario/apendice, se a escala ficar distorcida;
- uma tabela curta com `Delta S(T_policy)` e `Delta S(T_eval_policy)` por cenario.

Se voce quiser que o paper diga que a leitura e robusta tambem no eixo de equidade/subgrupo, ai sera necessario rerodar tambem o RQ3 para cada cenario e exportar bootstrap por cenario.

---

## Etapa 3 - Como apresentar os tres cenarios no paper

### 3.1 Regras de interpretacao recomendadas

O paper deve separar explicitamente tres camadas:

1. **Anchor da regra operacional e do tipo de intervencao**: Kay & Bostock (2023).
2. **Anchor conservadora de intensidade**: Sneyers & De Witte (2018), apenas para `delta = 0.08`.
3. **Stress tests hipoteticos**: `delta = 0.20` e `delta = 0.60`.

Essa separacao e importante porque hoje o texto ainda tende a misturar `trigger anchor` com `effect-size anchor`.

### 3.2 Estrutura sugerida no paper

#### Metodologia / Experimental Design

Substituir a tabela unica de policy por duas camadas:

Tabela A - `Operational intervention anchor`

- intervention class: short-term digital nudge;
- trigger: no LMS engagement for 7 days;
- checking frequency: weekly;
- active window: 2 weeks (`W=2`);
- source: Kay & Bostock (2023).

Tabela B - `Scenario intensities`

- `0.08` - anchored conservative scenario (`Sneyers & De Witte, 2018`);
- `0.20` - hypothetical A scenario;
- `0.60` - hypothetical B stress-test scenario.

#### Results (RQ2)

Usar:

- uma figura principal com tres curvas de `Delta S(t)` para os tres cenarios de intensidade;
- uma tabela curta com `Delta S(T_policy)` e `Delta S(T_eval_policy)` por cenario;
- um paragrafo de leitura monotona: se o contraste cresce com `delta`, isso mostra sensibilidade controlada a intensidade assumida.

#### Appendix / Reproducibility

Registrar:

- o grid de cenarios;
- o status de ancora (`anchored` vs `hypothetical`);
- os CSVs exportados por cenario.

### 3.3 Formula textual pronta para o paper

Texto sugerido para Metodologia:

`We anchor the intervention rule to Kay and Bostock (2023): a weekly digital re-engagement nudge triggered after 7 days without LMS activity and evaluated over a two-week action window. Because the OULAD data do not observe the real intervention exposure or identify its causal magnitude, effect intensity is treated as a scenario parameter. We therefore report three settings: a conservative anchored scenario (delta = 0.08), qualitatively calibrated to the moderate retention effects synthesized by Sneyers and De Witte (2018), a hypothetical A scenario (delta = 0.20), and a hypothetical B stress-test scenario (delta = 0.60).`

Texto sugerido para Results:

`RQ2 should therefore be read as a scenario-intensity analysis under a fixed operational nudge rule. The conservative scenario (delta = 0.08) is externally anchored, whereas the two higher-intensity settings are hypothetical. This distinction matters because the evidence supports the trigger and intervention class more directly than any single causal effect magnitude in the present observational data.`

---

## Mudancas necessarias por arquivo

### `TCM-Student-Dropout_updated.ipynb`

- Introduzir `shock_scenarios = [0.08, 0.20, 0.60]` com metadados de ancora.
- Generalizar P13/P14/P14b/P15 para gerar saidas por cenario.
- Corrigir `table_policy_scenario_params.csv` para nao deixar `anchor_study` e `anchor_trigger_note` vazios.
- Se a narrativa de propagacao stateful permanecer, garantir evidencia exportada para `recency` e `streak`; caso contrario, restringir a descricao para click-based engagement uplift.
- Preparar o notebook para manter um `reference_scenario_id` apenas por compatibilidade, sem voltar a tratar `delta_shock_ablation` como escalar unico.

### `methodology_body.tex`

- Manter a ancora atual em Kay & Bostock, mas explicitar o tipo de intervencao (`short-term digital nudge`).
- Explicitar que as covariantes sao proxies inspiradas na logica de desengajamento/reengajamento em LMS, nao copias literais do estudo.
- Explicitar que so o trigger/timing e ancorado em Kay; as intensidades sao tratadas como cenarios.

### `experimental_design_body.tex`

- Reescrever a tabela de policy para separar `operational anchor` de `scenario intensities`.
- Trocar a apresentacao atual de `delta_shock = 0.20` como parametro unico por um grid de tres cenarios.
- Acrescentar a descricao da frequencia semanal e do trigger de 7 dias no proprio protocolo.
- Explicitar que `0.60` e um stress test extremo e nao uma intensidade empiricamente ancorada.

### `results_body.tex`

- Trocar a tabela unica de RQ2 por tabela/figura multicenarios.
- Deixar claro que `0.08` e ancorado e `0.20/0.60` sao hipoteticos.
- Sincronizar os numeros do RQ2 com o evidence package atual antes de introduzir os novos cenarios, porque a tabela atual ainda esta desatualizada em relacao aos CSVs exportados.
- Se o `0.60` distorcer demais a escala da figura principal, desloca-lo para painel secundario ou apendice.

### `appendix_b_body.tex`

- Atualizar a secao de reproducibilidade para incluir o grid de cenarios e seu status de ancora.

### `ref.bib`

- **Adicionar** Sneyers & De Witte (2018).
- **Manter e citar** a entrada ja existente de Kay & Bostock (2023), que esta coerente com o DOI `10.5204/ssj.2848`.
- **Nao substituir** a entrada de Kay pelo titulo `Using nudges to improve student engagement in learning management systems: A learning analytics approach` sem nova verificacao bibliografica, porque isso conflita com o DOI e com a pagina oficial do artigo.

---

## Inclusoes e correcoes no `ref.bib`

### Entrada a adicionar

```bibtex
@article{SneyersDeWitte2018InterventionsMeta,
  author  = {Sneyers, Eline and De Witte, Kristof},
  title   = {Interventions in higher education and their effect on student success: A meta-analysis},
  journal = {Educational Review},
  year    = {2018},
  volume  = {70},
  number  = {2},
  pages   = {208--228},
  doi     = {10.1080/00131911.2017.1300874},
  url     = {https://doi.org/10.1080/00131911.2017.1300874}
}
```

### Entrada de Kay & Bostock a manter

O `ref.bib` atual ja contem uma entrada coerente com o DOI confirmado:

```bibtex
@article{KayBostock2023PowerNudge,
  title   = {The Power of the Nudge: Technology Driving Persistence},
  author  = {Kay, Ellie and Bostock, Paul},
  journal = {Student Success},
  year    = {2023},
  volume  = {14},
  number  = {2},
  pages   = {8--18},
  doi     = {10.5204/ssj.2848},
  url     = {https://doi.org/10.5204/ssj.2848}
}
```

---

## Fechamento tecnico

Resumo curto:

- `7 dias`, `frequencia semanal` e `W=2` estao essencialmente ancorados no notebook/export e ja aparecem na metodologia, mas ainda precisam ficar mais visiveis no desenho experimental e mais semanticamente ligados ao tipo de intervencao.
- O tipo de intervencao como `short-term digital nudge` ainda nao esta claro o suficiente no paper.
- A inspiracao das covariantes de engajamento esta apenas implicita; precisa ser explicitada.
- A alegacao de propagacao stateful ampla nao esta bem sustentada pelo pacote exportado atual.
- O eixo de intensidade (`0.08`, `0.20`, `0.60`) precisa ser promovido a grid formal de cenarios do notebook e do paper.
- `0.08` deve ser apresentado como ancorado em Sneyers & De Witte (2018); `0.20` e `0.60` devem ser apresentados como hipoteticos.

---

## Preparacao imediata para alterar o notebook

Blocos que devem ser alterados primeiro no `TCM-Student-Dropout_updated.ipynb`:

- `P12` para introduzir o catalogo formal `shock_scenarios = [0.08, 0.20, 0.60]`.
- `P13-v7` para substituir o `delta_shock_ablation` escalar por iteracao por `scenario_id`.
- `P14-v4.0` e `P14-v4.3` para materializar `weekly` e `horizons` por cenario e exportar artefatos multicenario.
- `P14b-v4` para decidir se o `0.60` entra no painel principal ou fica como stress test de apendice.
- `P15`, `P15.2` e `P15.0` para congelar o novo contrato (`effect_shared` + `shock_scenarios` + `reference_scenario_id`).
- `P16-v3` para auditar o novo contrato multicenario.
- hardcode legado `delta_shock_ablation = 0.20` para remocao.

Critico para a proxima edicao:

- manter `0.20` apenas como `hypothetical_A`, nao como ancora implicita;
- manter `0.60` como `hypothetical_B` ou `extreme_stress_test`, nunca como cenario empiricamente plausivel;
- preservar um `reference_scenario_id` somente para compatibilidade com blocos legados e exportacoes ainda nao refatoradas.