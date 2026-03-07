# Plano de Criacao do Paper (v3)

## 1) Objetivo fixo do paper (nao muda)
Com base em `Introduction` e `Background` (especialmente `Intervention Practice Gaps: Documentation, Evaluation, and Experimental Frictions`), o objetivo central e:

- Sair de predicao estatica de risco (quem) para um framework temporal orientado a decisao (quando agir).
- Traduzir risco temporal em regras de decisao auditaveis (gatilho + janela + cenario explicito).
- Permitir comparacao estrutural de politicas simuladas quando os dados observacionais nao sustentam identificacao causal de efeito real de intervencao.
- Incluir validacao por subgrupos para verificar se a mesma regra gera ganhos diferentes entre grupos (motivando desenho de suporte mais direcionado).

Resumo operacional do objetivo:
- Contribuicao principal = framework matematico e pipeline executavel de `temporal hazard -> survival trajectories -> policy simulation -> subgroup gap diagnostics`.
- Nao e objetivo do paper estimar efeito causal identificado de intervencao em OULAD.

## 2) Principio editorial para organizar secoes
Regra de ouro para evitar mistura:
- `Methodology`: definicoes, estimadores, protocolo fixo, track design, formulas e parametros de cenario.
- `Experimental Design`: como o experimento foi configurado e executado (dados, split, horizontes, metricas, robustez planejada, fluxo de avaliacao).
- `Results and Discussion`: achados numericos/figuras/tabelas + interpretacao + limitacoes.

## 3) O que do notebook entra em cada secao

## 3.1 Methodology (o que entra)
Objetivo da secao Methodology no nosso paper:
- Definir formalmente o mecanismo do framework (sem resultados finais), para sustentar a ponte `temporal hazard -> policy simulation -> subgroup diagnostics`.

Blocos do notebook que entram:
- `P3` e `P4`: definicao de evento/censura e variaveis de tempo (`event_withdrawn`, `t_event_week`, `t_last_obs_week`, `t_final_week`), incluindo regra para `Withdrawn` sem `date_unregistration` valido.
- `P5.*`: engenharia temporal semanal (recency, streak, total_clicks, submitted_this_week) e construcao person-period.
- `P6`: split temporal estratificado por `event_withdrawn x time_bucket` com `q=4`, `test_size=0.30` e `time_for_split`.
- `P7-v3`: modelo principal de hazard discreto, preprocessamento e calibracao agrupada (com controle de leakage).
- `P7.2-v3`, `P7.3-v7`, `P7.4-v4`: track de censoring/IPCW (`G_hat_cens`, `ipcw_w`, calibracao e estabilidade por `g_min`).
- `P9` e `P9.1`: formalizacao de benchmark track como parte do desenho metodologico.
- `P12`, `P13-v7`, `P13.1`, `P14*`: definicao da policy (`r_star`, `W`, shock e mechanism-aware stateful), e definicao de `Delta S(t)`.
- `P22` a `P28.2`: definicao da fairness track (gap por grupo, `Delta Gap`, bootstrap).

Graphviz distribuidos nesta secao:
- `G2 Policy mechanism e propagacao stateful` (na subsecao de policy).
- `G1 Pipeline end-to-end` no fechamento da Methodology (ou transicao para Experimental Design).

O que preciso provar/mostrar aqui (sem numeros finais):
- O framework esta formalmente bem especificado e auditavel.
- A policy mechanism-aware e stateful (nao one-shot).
- A separacao de horizontes e parte do protocolo, nao ajuste ad hoc de resultado.

No texto da Methodology, manter:
- Formulas, protocolos, parametros e contratos de execucao.
- Linguagem de cenario estrutural (nao causal identificado).

No texto da Methodology, evitar:
- Tabelas finais de desempenho e deltas finais de policy/fairness.
- Discussao interpretativa longa (fica em Results).

### 3.1.1 Subsecoes sugeridas para a Methodology (com definicoes matematicas)
Objetivo deste bloco:
- Servir como roteiro de escrita da secao `Methodology` no `.tex`, ja com definicoes formais e equacoes nucleares.

1. `3.1.1 Problem Setup, Unidade de Analise e Notacao`
- O que escrever:
  - Definir a unidade como enrollment `i` e tempo discreto em semanas `t`.
  - Declarar que as variaveis sao construidas em formato person-period.
- Definicoes matematicas:
  - Indices: $i \in \{1,\dots,N\}$, $t \in \{0,\dots,T\}$.
  - Covariaveis semanais: $X_{it}$ (vetor de atributos no tempo $t$).

2. `3.1.2 Endpoint Primario e Censura`
- O que escrever:
  - Definir evento primario de dropout observado.
  - Explicar regra de censura para `Withdrawn` sem data valida de unregistration.
- Definicoes matematicas:
  - Indicador de evento por enrollment:
    $$
    E_i = \mathbb{1}\{\texttt{final\_result}_i=\texttt{Withdrawn} \land \texttt{date\_unregistration}_i\ \text{valida}\}
    $$
  - Semana de evento (quando $E_i=1$):
    $$
    t_i^{event}=\left\lfloor \frac{\texttt{date\_unregistration}_i}{7} \right\rfloor
    $$
  - Semana final observada:
    $$
    t_i^{final}=\begin{cases}
    t_i^{event}, & E_i=1 \\
    t_i^{last\_obs}, & E_i=0
    \end{cases}
    $$

3. `3.1.3 Construcao Person-Period e Label Terminal`
- O que escrever:
  - Definir expansao enrollment-semana ate $t_i^{final}$.
  - Definir label terminal unico por enrollment.
- Definicoes matematicas:
  - Label semanal:
    $$
    event_{it}=\mathbb{1}\{E_i=1 \land t=t_i^{final}\}
    $$
  - Propriedade de evento unico:
    $$
    \sum_{t=0}^{t_i^{final}} event_{it} \le 1
    $$

4. `3.1.4 Engenharia Temporal de Covariaveis`
- O que escrever:
  - Definir indexacao semanal e features de engajamento temporal.
  - Explicitar recency/streak/submitted_this_week/total_clicks.
- Definicoes matematicas:
  - Mapeamento dia-semana:
    $$
    w(d)=\max\left(\left\lfloor d/7 \right\rfloor,0\right)
    $$
  - Atividade semanal:
    $$
    A_{it}=\mathbb{1}\{\texttt{total\_clicks}_{it}>0\}
    $$
  - Atualizacao de recency:
    $$
    Recency_{it}=\begin{cases}
    0, & A_{it}=1 \\
    Recency_{i,t-1}+1, & A_{it}=0
    \end{cases}
    $$
  - Atualizacao de streak:
    $$
    Streak_{it}=\begin{cases}
    Streak_{i,t-1}+1, & A_{it}=1 \\
    0, & A_{it}=0
    \end{cases}
    $$

5. `3.1.5 Split Temporal Estratificado e Prevencao de Leakage`
- O que escrever:
  - Definir `time_for_split` e estratos por evento x bucket temporal.
  - Descrever exclusao de variaveis de leak e grouped calibration por enrollment.
- Definicoes matematicas:
  - Tempo para split:
    $$
    time\_for\_split_i=\begin{cases}
    t_i^{event}, & E_i=1 \\
    t_i^{final}, & E_i=0
    \end{cases}
    $$
  - Estrato:
    $$
    stratum_i=(E_i,\ bucket(time\_for\_split_i))
    $$

6. `3.1.6 Modelo Principal de Hazard Discreto (RQ1)`
- O que escrever:
  - Definir hazard semanal condicional e reconstrucao de sobrevivencia.
  - Declarar especificacao principal (logit + calibracao agrupada).
- Definicoes matematicas:
  - Hazard discreto:
    $$
    h_{it}=P(event_{it}=1 \mid T_i\ge t, X_{it})
    $$
  - Modelo logit:
    $$
    	ext{logit}(\hat h_{it})=\beta_0+\beta^\top X_{it}
    $$
  - Sobrevivencia prevista:
    $$
    \hat S_i(t)=\prod_{k\le t}(1-\hat h_{ik})
    $$

7. `3.1.7 Track de Censura/IPCW`
- O que escrever:
  - Definir probabilidade de permanecer nao-censurado e pesos IPCW.
  - Explicar regra de estabilidade por horizonte.
- Definicoes matematicas:
  - Sobrevivencia de censura condicional:
    $$
    \hat G_{it}=P(C_i\ge t \mid X_{it})
    $$
  - Peso IPCW truncado:
    $$
    w_{it}=\frac{1}{\max(\hat G_{it}, g_{min})}
    $$
  - Regra de horizonte estavel para metricas:
    $$
    T_{eval\_metrics}=\max\{t: \hat G(t)\ge g_{min}\}
    $$

8. `3.1.8 Policy Simulation (RQ2): Shock vs Mechanism-Aware`
- O que escrever:
  - Definir gatilho por recency e janela de ativacao.
  - Separar regime shock (escala direta do hazard) de regime mechanism-aware stateful.
- Definicoes matematicas:
  - Tempo de gatilho:
    $$
    t_i^*=\min\{t: Recency_{it}\ge r^*\}
    $$
  - Indicador de ativacao:
    $$
    active_{it}=\mathbb{1}\{t\in[t_i^*, t_i^*+W)\}
    $$
  - Shock:
    $$
    \hat h^{(1,shock)}_{it}=\hat h^{(0)}_{it}(1-\delta_{shock})\ \text{se }active_{it}=1
    $$
  - Mechanism-aware (plug-in):
    $$
    \hat h^{(1,mech)}_{it}=f(X^{cf}_{it})
    $$
    onde $X^{cf}_{it}$ e atualizado com propagacao stateful de covariaveis de engajamento.

9. `3.1.9 Contraste Estrutural de Sobrevivencia (RQ2)`
- O que escrever:
  - Definir claramente o estimando estrutural reportado no paper.
- Definicoes matematicas:
  - Media de sobrevivencia por regime:
    $$
    \bar S^{(a)}(t)=\frac{1}{N}\sum_i \hat S_i^{(a)}(t)
    $$
  - Contraste estrutural principal:
    $$
    \Delta S(t)=\bar S^{(1)}(t)-\bar S^{(0)}(t)
    $$

10. `3.1.10 Fairness/Subgrupo (RQ3)`
- O que escrever:
  - Definir metrica de gap por regime e mudanca de gap sob policy.
  - Descrever bootstrap para incerteza.
- Definicoes matematicas:
  - Media por grupo $g$ e regime $a$:
    $$
    \bar\mu_g^{(a)}(t)=\frac{1}{N_g}\sum_{i:g(i)=g}\hat S_i^{(a)}(t)
    $$
  - Gap por regime:
    $$
    Gap^{(a)}(t)=\bar\mu_1^{(a)}(t)-\bar\mu_0^{(a)}(t)
    $$
  - Mudanca de gap:
    $$
    \Delta Gap(t)=Gap^{(1)}(t)-Gap^{(0)}(t)
    $$

11. `3.1.11 Suposicoes e escopo de interpretacao`
- O que escrever:
  - Declarar explicitamente: resultados de policy/fairness sao contrasts estruturais simulados.
  - Delimitar ausencia de identificacao causal observacional no OULAD.
- Definicao textual obrigatoria:
  - "Os resultados reportados para $\Delta S$ e $\Delta Gap$ sao model-implied simulated regime contrasts, nao estimativas causais identificadas de efeito de intervencao."

## 3.2 Experimental Design (o que entra)
Objetivo da secao Experimental Design no nosso paper:
- Mostrar como o framework foi testado de forma verificavel para responder RQ1, RQ2 e RQ3.

Blocos do notebook que entram:
- Escopo e perguntas: mapeamento operacional de RQ1-RQ3 no pipeline.
- Protocolo de dados/particao: cohort final, contagens de split de `P6`, unidade enrollment e person-period.
- Horizonte e avaliacao: `T_policy`, `T_eval_policy`, `T_eval_metrics`, criterio de estabilidade IPCW, e pacote de metricas (AUC row-level, Brier/IBS IPCW, C-index discreto).
- Comparacao de modelos: modelo principal (`P7-v3`), Cox (`P9`), baseline nao-temporal (`P9.1`), benchmark unificado (`P17.6`).
- Robustez planejada: ablation (`P19*`), OOD (`P20`), endpoint sensitivity (`P21*`).
- Avaliacao de policy/fairness: policy trajectories (`P13-P14`) e fairness com bootstrap (`P28*`).

Graphviz distribuidos nesta secao:
- `G1 Pipeline end-to-end` (se nao entrar no fim da Methodology, entra na abertura do Experimental Design).
- `G3 Censoring e dual horizons` no trecho de protocolo de avaliacao sob censura.

O que preciso provar/mostrar aqui:
- O protocolo e coerente com o objetivo de decisao temporal (quando agir).
- A avaliacao e reproduzivel e tem regras claras de horizonte sob censura.
- O plano de benchmark/robustez foi definido antes da discussao de achados.

No texto de Experimental Design, manter:
- "Como foi testado" (procedimento, fluxos, criterios, dependencias entre P{n}).
- Ligacao explicita entre tracks e RQs.

No texto de Experimental Design, evitar:
- Repetir derivacoes matematicas da Methodology.
- Antecipar interpretacao de resultados.

### 3.2.1 Subsecoes sugeridas para Experimental Design (com definicoes formais)
Objetivo deste bloco:
- Servir como roteiro de escrita da secao `Experimental Design` no `.tex`, com protocolo, criterios e metricas explicitamente definidos antes dos resultados.

1. `3.2.1 Design Goals e RQs Operacionais`
- O que escrever:
  - Declarar o que o desenho experimental precisa validar para RQ1, RQ2 e RQ3.
  - Explicitar que policy/fairness sao avaliados como contrasts estruturais simulados.
- Definicoes formais:
  - RQ1: qualidade do hazard semanal.
  - RQ2: contraste estrutural $\Delta S(t)$ sob politica definida.
  - RQ3: contraste de gap $\Delta Gap(t)$ com incerteza bootstrap.

2. `3.2.2 Cohort, Unidade e Tabela de Analise`
- O que escrever:
  - Definir universo de analise e unidade (enrollment).
  - Definir tabela person-period como base de treino/teste.
- Definicoes formais:
  - Numero de enrollments: $N$.
  - Numero de linhas person-period: $N_{pp}=\sum_i (t_i^{final}+1)$.

3. `3.2.3 Protocolo de Split Temporal Estratificado`
- O que escrever:
  - Descrever `time_for_split`, buckets temporais e estratificacao por evento.
  - Registrar `q=4` e `test_size=0.30`.
- Definicoes formais:
  -
    $$
    time\_for\_split_i=\begin{cases}
    t_i^{event}, & E_i=1 \\
    t_i^{final}, & E_i=0
    \end{cases}
    $$
  -
    $$
    stratum_i=(E_i,\ bucket_q(time\_for\_split_i))
    $$
  - Meta de amostragem por estrato:
    $$
    n_{test,s}\approx 0.30\cdot n_s
    $$

4. `3.2.4 Variantes de Modelo no Experimento`
- O que escrever:
  - Definir modelo principal e variantes de comparacao (Cox, nao-temporal, benchmark unificado).
  - Separar claramente o papel de cada variante (principal vs robustez).
- Definicoes formais:
  - Modelo principal: hazard discreto calibrado (P7-v3).
  - Baselines: Cox time-varying (P9), baseline nao-temporal (P9.1), comparacao unificada (P17.6).

5. `3.2.5 Horizonte de Avaliacao e Regra de Estabilidade`
- O que escrever:
  - Distinguir horizonte de reporte de policy e horizonte para metricas IPCW estaveis.
  - Explicar por que dois horizontes sao necessarios.
- Definicoes formais:
  - Horizonte de policy: $T_{policy}$.
  - Horizonte tecnico de policy: $T_{eval\_policy}$.
  - Horizonte de metricas estaveis:
    $$
    T_{eval\_metrics}=\max\{t: \hat G(t)\ge g_{min}\}
    $$

6. `3.2.6 Metricas Primarias e Secundarias`
- O que escrever:
  - Definir conjunto minimo de metricas para cada bloco de avaliacao.
  - Mapear metrica -> pergunta que responde.
- Definicoes matematicas:
  - AUC row-level:
    $$
    AUC_{row}=AUC(y_{it},\hat h_{it})
    $$
  - Probabilidade por horizonte:
    $$
    \hat p_i^{(T)}=1-\hat S_i(T)
    $$
  - Brier IPCW em $T$:
    $$
    BS_{IPCW}(T)=\frac{1}{n}\sum_i w_i(T)\left(Y_i^{(T)}-\hat p_i^{(T)}\right)^2
    $$
  - IBS discreto:
    $$
    IBS(0{:}T)=\frac{1}{T+1}\sum_{t=0}^{T}BS_{IPCW}(t)
    $$

7. `3.2.7 Protocolo de Avaliacao da Policy (RQ2)`
- O que escrever:
  - Definir cenarios policy comparados (shock vs mechanism-aware).
  - Definir saidas obrigatorias por semana e por horizonte.
- Definicoes matematicas:
  -
    $$
    \Delta S(t)=\bar S^{(1)}(t)-\bar S^{(0)}(t)
    $$
  - Resumo em horizontes:
    $$
    \Delta S(T_{policy}),\ \Delta S(T_{eval})
    $$

8. `3.2.8 Protocolo de Avaliacao por Subgrupo (RQ3)`
- O que escrever:
  - Definir comparacao de gaps por regime e incerteza por bootstrap.
  - Especificar variavel de grupo usada na demonstracao (ex.: `gender`).
- Definicoes matematicas:
  -
    $$
    \Delta Gap(t)=Gap^{(1)}(t)-Gap^{(0)}(t)
    $$
  - Bootstrap com $B$ reamostragens:
    $$
    \{\Delta Gap^{*(b)}(t)\}_{b=1}^{B}
    $$
  - Intervalo de 95%:
    $$
    CI_{95\%}(t)=\left[Q_{0.025},Q_{0.975}\right]
    $$

9. `3.2.9 Robustez Planejada`
- O que escrever:
  - Definir a bateria de robustez antes dos achados.
  - Explicitar objetivo de cada eixo: ablation, OOD, endpoint sensitivity.
- Definicoes formais:
  - Ablation (P19*): remover blocos de features e recomputar metricas.
  - OOD (P20): treino em runs vistos e teste em run hold-out.
  - Endpoint sensitivity (P21*): repetir avaliacao sob endpoint alternativo.

10. `3.2.10 Regras de Comparacao e Criterios de Leitura`
- O que escrever:
  - Definir quais diferencas sao substantivas vs apenas estatisticas.
  - Definir criterio de consistencia entre metricas e horizontes.
- Definicoes formais:
  - Consistencia de sinal em policy:
    $$
    sign\left(\Delta S(T_{policy})\right)=sign\left(\Delta S(T_{eval})\right)
    $$
  - Consistencia de fairness:
    $$
    0 \notin CI_{95\%}\left(\Delta Gap(T)\right)
    $$

11. `3.2.11 Artefatos, Rastreabilidade e Reprodutibilidade`
- O que escrever:
  - Definir politica de evidencia: toda afirmacao em Results precisa apontar para tabela/figura exportada.
  - Definir hierarquia main text vs apendice (qualidade e foco narrativo).
- Definicoes formais:
  - Mapeamento minimo por claim:
    $$
    claim_j \rightarrow \{figure_k, table_m, P\{n\}\}
    $$

12. `3.2.12 Limites do Design Experimental`
- O que escrever:
  - Delimitar escopo observacional: desenho nao identifica efeito causal de intervencao.
  - Delimitar que policy/fairness sao avaliados como simulacao estrutural.
- Definicao textual obrigatoria:
  - "O desenho experimental valida desempenho temporal e contrasts estruturais sob regras explicitas; nao constitui desenho de identificacao causal de efeito de intervencao."

## 3.3 Results and Discussion (o que entra)
Objetivo da secao `Results and Discussion` no nosso paper:
- Demonstrar que o framework realmente suporta decisao temporal auditavel e comparacao estrutural de regimes, sem extrapolar para causalidade identificada.

### 3.3.1 Blocos de prova (o que mostrar em cada item)
| Bloco em Results | O que preciso provar/mostrar | Evidencia minima (qualidade) | P{n} fonte |
|---|---|---|---|
| RQ1: qualidade do hazard temporal | O modelo discrimina/calibra bem no nivel semanal | `fig_calibration_reliability_test.png` + `table_calibration_metrics.csv` + `table_calibration_bin_support_tail3_test.csv` + AUC train/test | `P7-v3`, `P11.*`, `P17*` |
| RQ1: janela temporal baseline | Existe leitura temporal de risco (quando agir) | curva baseline em `fig_policy_survival_by_week_baseline_shock_mech.png` | `P14*` |
| Validade sob censura | Justificar dois horizontes e estabilidade IPCW | `fig_censoring_survival_G_test.png`, `table_policy_horizons_dual.csv`, `table_censoring_survival_G_test_by_week.csv` | `P7.2`, `P14.2`, `P17-v5` |
| RQ2: contraste de policy | Mostrar magnitude e perfil temporal de `Delta S` | `fig_policy_deltaS_by_week_shock_vs_mech.png` + `table_policy_deltaS_at_horizons.csv` | `P13`, `P14`, `P14.3` |
| RQ2: leitura mecanistica | Explicar por que mechanism-aware foi pequeno no setup default | `fig_policy_covariate_deltas_by_week.png`, `table_policy_covariate_deltas_by_week.csv`, `table_policy_activation_summary.csv` | `P13`, `P13.1` |
| Benchmark e robustez de familia | Conclusao nao depende de um unico baseline | `fig_benchmark_dual_panel_auc.png` + `table_benchmark_unified.csv` (+ Cox) | `P9`, `P9.1`, `P17.6` |
| Robustez adicional | Nao e artefato de feature/endpoint/run | `table_ablation_m4.csv`, endpoint sensitivity, OOD | `P19.*`, `P20`, `P21*` |
| RQ3 fairness/subgrupo | Mesma politica altera gap com incerteza reportada | `fig_rq3_gap_Tpolicy.png`, `fig_rq3_gap_Teval.png`, bootstrap + `rq3_policy_bootstrap_wide.csv`, `rq3_fairness_extended.csv` | `P25-P28.2` |
| Discussao e limites | Delimitar escopo estrutural e proximos passos | sintese dos blocos + limitacoes explicitas | secao V |

### 3.3.2 Distribuicao de `7.1` (Main text) nas subsecoes de Results
| Subsecao em `V. Results and Discussion` | Evidencia principal | Paragrafo alvo |
|---|---|---|
| Primary Discrete-Time Hazard Model | `outputs_v2/figures/fig_calibration_reliability_test.png`, `outputs_v2/tables/table_calibration_metrics.csv`, `outputs_v2/tables/table_calibration_bin_support_tail3_test.csv` | Paragrafo imediatamente apos reportar AUC/Brier/ECE (com nota tecnica curta de suporte amostral da cauda) |
| RQ2: Policy Simulation and Aggregate Structural Contrast | `outputs_v2/figures/fig_policy_deltaS_by_week_shock_vs_mech.png`, `outputs_v2/tables/table_policy_deltaS_at_horizons.csv` | Paragrafo apos definir `Delta S(t)` e paragrafo de fechamento com valores nos horizontes |
| Survival Metrics Under Censoring | `outputs_v2/figures/fig_censoring_survival_G_test.png`, `outputs_v2/tables/table_policy_horizons_dual.csv`, `outputs_v2/tables/table_censoring_survival_G_test_by_week.csv` | Paragrafo de abertura (justificativa de horizonte) e paragrafo tecnico de suporte |

### 3.3.3 Distribuicao dos Graphviz (`8`) nas subsecoes de Results
- Abertura de Results (ou transicao Design -> Results): `G3 Censoring e dual horizons`.
- Inicio do bloco benchmark/robustez: `G4 Benchmark e robustez`.
- Inicio do bloco fairness (RQ3): `G5 Fairness pipeline`.

### 3.3.4 Itens de apendice (qualidade boa, menor prioridade no corpo)
- Calibration tail support (same bins as main reliability figure): `table_calibration_bin_support_test.csv` + `table_calibration_bin_support_tail3_test.csv` (n, events e non-events por bin).
- Tail snapshot (P11.3, test): Bin 8 `[0.533, 0.600)` -> n=9, events=3, non-events=6; Bin 9 `[0.600, 0.667)` -> n=7, events=0, non-events=7; Bin 10 `[0.667, 0.733)` -> n=3, events=3, non-events=0.
- Calibration robustness (IPCW): `fig_calibration_ipcw_reliability_test.png` + `table_calibration_ipcw_metrics.csv`.
- Policy mechanism diagnostics: `fig_policy_covariate_deltas_by_week.png` + tabelas de deltas/ativacao.
- Fairness appendix: pacote `fig_rq3_*`, bootstrap tables e `fig_calibration_by_group.png` + `rq3_fairness_extended.csv`.

### 3.3.5 Estrutura recomendada de paragrafos da secao V
- Abertura: reafirmar escopo estrutural/model-implied.
- Bloco RQ1: qualidade preditiva e leitura temporal baseline.
- Bloco validade de avaliacao: justificativa tecnica de horizontes sob censura.
- Bloco RQ2: contraste shock vs mechanism-aware e magnitude.
- Bloco benchmark/robustez: convergencia de conclusao entre familias e cenarios.
- Bloco RQ3: gap por subgrupo com CI e interpretacao prudente.
- Fechamento: o que foi demonstrado, o que nao foi (causalidade), e proximo passo experimental.

Checklist da secao V (minimo obrigatorio):
- Mostrar calibracao/discriminacao do hazard principal.
- Mostrar `Delta S` em curva e em horizonte.
- Mostrar justificativa de horizonte por `G(t)`.
- Mostrar ao menos um bloco de robustez forte.
- Mostrar fairness com intervalo de incerteza.
- Declarar limite de causalidade observacional.

### 3.3.6 Subsecoes sugeridas para Results and Discussion (com criterios formais de reporte)
Objetivo deste bloco:
- Servir como roteiro de escrita da secao `Results and Discussion` no `.tex`, com regras objetivas de como reportar achados, incerteza e limites sem overclaim.

1. `3.3.6.1 Opening Statement e Escopo de Interpretacao`
- O que escrever:
  - Abrir a secao reafirmando que os resultados de policy/fairness sao contrasts estruturais simulados.
  - Declarar que o foco de evidencia e: qualidade temporal, contraste de regime e diagnostico por subgrupo.
- Definicao textual obrigatoria:
  - "As estimativas de policy e fairness sao model-implied simulated regime contrasts sob regras explicitas, nao efeitos causais identificados de intervencao."

2. `3.3.6.2 RQ1: Desempenho do Hazard Principal`
- O que escrever:
  - Reportar discriminacao e calibracao no teste para o modelo principal.
  - Conectar a qualidade do hazard com a validade dos blocos RQ2/RQ3.
- Definicoes formais de reporte:
  - Gap de generalizacao em AUC:
    $$
    \Delta AUC=AUC_{test}-AUC_{train}
    $$
  - Erro de calibracao agregado (ja no pacote de metricas):
    $$
    ECE=\sum_{b=1}^{B} \frac{n_b}{n}\left|acc(b)-conf(b)\right|
    $$

3. `3.3.6.3 Janela Temporal de Risco (Baseline)`
- O que escrever:
  - Mostrar que a trajetoria baseline de sobrevivencia permite leitura de "quando agir".
  - Explicitar intervalo temporal em que a queda e mais acentuada.
- Definicoes formais de reporte:
  - Queda acumulada ate horizonte $T$:
    $$
    Drop_{base}(T)=1-\bar S^{(0)}(T)
    $$
  - Declinio semanal discreto:
    $$
    d_t=\bar S^{(0)}(t)-\bar S^{(0)}(t+1)
    $$

4. `3.3.6.4 Validade sob Censura e Regra de Horizonte`
- O que escrever:
  - Justificar explicitamente por que metricas IPCW sao lidas em horizonte estavel.
  - Reportar qual semana define `T_eval_metrics` e qual semana define `T_eval_policy`.
- Definicoes formais de reporte:
  - Conjunto de semanas admissiveis para IPCW:
    $$
    \mathcal{T}_{stable}=\{t:\hat G(t)\ge g_{min}\}
    $$
  - Horizonte tecnico de metricas:
    $$
    T_{eval\_metrics}=\max \mathcal{T}_{stable}
    $$

5. `3.3.6.5 RQ2: Contraste Estrutural de Policy`
- O que escrever:
  - Reportar curva completa de `Delta S(t)` e resumo em horizontes fixos.
  - Comparar shock vs mechanism-aware no mesmo quadro narrativo.
- Definicoes formais de reporte:
  - Contraste semanal:
    $$
    \Delta S(t)=\bar S^{(1)}(t)-\bar S^{(0)}(t)
    $$
  - Ganho acumulado no horizonte:
    $$
    A_{\Delta S}(T)=\sum_{t=0}^{T}\Delta S(t)
    $$

6. `3.3.6.6 Diagnostico Mecanistico da Policy`
- O que escrever:
  - Explicar magnitude observada com base em ativacao e deltas de covariaveis.
  - Evitar interpretar magnitude pequena como "falha" sem mostrar mecanismo.
- Definicoes formais de reporte:
  - Taxa de ativacao:
    $$
    \pi_{act}=\frac{1}{N}\sum_i \mathbb{1}\{\exists t:active_{it}=1\}
    $$
  - Delta medio de covariavel-chave $k$:
    $$
    \Delta X_k(t)=\frac{1}{N}\sum_i\left(X^{(1)}_{itk}-X^{(0)}_{itk}\right)
    $$

7. `3.3.6.7 Benchmark e Convergencia entre Familias`
- O que escrever:
  - Comparar modelo principal, Cox e baseline nao-temporal de forma simetrica.
  - Enfatizar convergencia qualitativa de conclusao, nao apenas ranking numerico.
- Definicoes formais de reporte:
  - Delta de metrica entre modelos $m_1,m_2$:
    $$
    \Delta M(m_1,m_2)=M(m_1)-M(m_2)
    $$
  - Criterio de convergencia de sinal entre familias:
    $$
    sign\left(\Delta S_{main}(T)\right)=sign\left(\Delta S_{benchmark}(T)\right)
    $$

8. `3.3.6.8 Robustez Adicional (Ablation, OOD, Endpoint)`
- O que escrever:
  - Reportar se a narrativa principal se mantem sob perturbacoes planejadas.
  - Organizar por eixo de robustez e nao por tabela isolada.
- Definicoes formais de reporte:
  - Spread de sensibilidade por eixo $r$:
    $$
    Spread_r=\max_j M_{r,j}-\min_j M_{r,j}
    $$
  - Criterio de estabilidade direcional:
    $$
    sign\left(\Delta S_r(T)\right)=sign\left(\Delta S_{main}(T)\right)
    $$

9. `3.3.6.9 RQ3: Fairness e Incerteza`
- O que escrever:
  - Reportar `Gap^(0)`, `Gap^(1)` e `Delta Gap` nos dois horizontes.
  - Interpretar mudanca de gap com CI bootstrap, evitando linguagem normativa forte.
- Definicoes formais de reporte:
  - Mudanca de gap:
    $$
    \Delta Gap(T)=Gap^{(1)}(T)-Gap^{(0)}(T)
    $$
  - Regra de leitura com incerteza:
    $$
    0\notin CI_{95\%}(\Delta Gap(T))\Rightarrow \text{sinal robusto no horizonte }T
    $$

10. `3.3.6.10 Discussao Integrada e Limitacoes`
- O que escrever:
  - Integrar RQ1-RQ3 em uma narrativa unica: previsao temporal -> decisao -> heterogeneidade.
  - Fechar com limites: observacional, dependencia do modelo e ausencia de identificacao causal.
- Definicao textual obrigatoria:
  - "Nossas conclusoes sao sobre desempenho temporal e contrasts estruturais simulados sob o protocolo definido; nao sobre efeito causal identificado de intervencao real."

11. `3.3.6.11 Contrato de Evidencia por Paragrafo`
- O que escrever:
  - Exigir que cada paragrafo de resultado tenha um claim verificavel e referencia explicita a figura/tabela.
  - Separar no texto principal apenas o essencial para argumento central; diagnosticos extensos vao para apendice.
- Definicoes formais:
  - Mapeamento minimo obrigatorio:
    $$
    claim_j \rightarrow \{metric_j, figure_k, table_m, horizon_h\}
    $$
  - Regra de completude da secao:
    $$
    \forall j\in \mathcal{C}_{Results},\ \exists\ \, evidence(j)
    $$

## 4) Matriz rapida: "vai para onde"

- Definicao de evento/censura: `Methodology`.
- Formula de hazard/survival/Delta S/Delta Gap: `Methodology`.
- Parametros fixos de policy (`r_star`, `W`, `delta_shock`, schedule): `Methodology` (+ citacao curta em Design).
- Contagens do split e protocolo de avaliacao: `Experimental Design`.
- Quais metricas e por quais horizontes: `Experimental Design`.
- Numeros finais de desempenho e contrasts: `Results and Discussion`.
- Interpretacao substantiva e implicacoes: `Results and Discussion`.

## 5) Pontos de atencao para as proximas iteracoes
- Garantir consistencia terminologica em todo texto:
  - usar sempre "simulated regime contrast" para RQ2/RQ3.
  - evitar linguagem causal forte (efeito, impacto causal estimado) quando nao suportado.
- Manter consistencia entre mechanism-aware no texto e implementacao stateful do notebook.
- Evitar duplicacao entre Methodology e Experimental Design.
- Garantir que cada tabela/figura citada em Results tenha origem clara no notebook/export.

## 6) Proxima iteracao sugerida
Na v2 deste plano, podemos transformar isso em um checklist de escrita por paragrafo, com:
- "frase-guia" por subsecao,
- quais tabelas/figuras entram em cada subsecao,
- e quais frases devem ser evitadas para nao introduzir ambiguidade causal.

## 7) Vinculo explicito: arquivo exportado -> paragrafo alvo (somente itens com qualidade)
Nota: conteudo desta secao ja foi distribuido dentro do item `3` (principalmente em `3.3`) e permanece aqui apenas como referencia rastreavel.

Base de selecao usada:
- Classificacao registrada no notebook em `Appendix: Analise Critica por Imagem (sem reexecucao)`.
- Apenas itens marcados como `Main text`, `Apendice forte` ou `Apendice`.
- Itens marcados como `Segurar por enquanto` ficam fora deste vinculo.

### 7.1 Itens para Main text
| Tipo | Arquivo exportado | Classificacao no notebook | Secao/Subsecao no paper | Paragrafo alvo (onde citar) | Funcao no argumento |
|---|---|---|---|---|---|
| Figura | `outputs_v2/figures/fig_calibration_reliability_test.png` | Main text | `V. Results and Discussion -> Primary Discrete-Time Hazard Model` | Paragrafo imediatamente apos reportar AUC/Brier/ECE do modelo principal | Sustentar qualidade preditiva do hazard usado nos tracks de policy/fairness |
| Tabela | `outputs_v2/tables/table_calibration_metrics.csv` | Herdada da mesma classificacao da figura principal de calibracao | `V. Results and Discussion -> Primary Discrete-Time Hazard Model` | Mesmo paragrafo da figura, com numeros compactos em formato de tabela | Dar suporte numerico objetivo para a curva de calibracao |
| Tabela | `outputs_v2/tables/table_calibration_bin_support_tail3_test.csv` | Main text (nota tecnica) | `V. Results and Discussion -> Primary Discrete-Time Hazard Model` | Frase imediatamente apos a figura principal, quando discutir bins de alto risco | Tornar auditavel a leitura da cauda com suporte direto por bin (`n`, events, non-events) |
| Figura | `outputs_v2/figures/fig_policy_deltaS_by_week_shock_vs_mech.png` | Main text (resultado central) | `V. Results and Discussion -> RQ2: Policy Simulation and Aggregate Structural Contrast` | Paragrafo logo apos definir/lembrar `Delta S(t)` | Mostrar contraste temporal entre shock e mechanism-aware |
| Tabela | `outputs_v2/tables/table_policy_deltaS_at_horizons.csv` | Herdada da classificacao da figura central de policy | `V. Results and Discussion -> RQ2: Policy Simulation and Aggregate Structural Contrast` | Paragrafo de fechamento da subsecao RQ2 (com valores em `T_policy` e `T_eval`) | Fixar magnitudes reportadas no texto |
| Figura | `outputs_v2/figures/fig_censoring_survival_G_test.png` | Main text (justificativa de horizonte) | `V. Results and Discussion -> Survival Metrics Under Censoring` | Primeiro paragrafo da subsecao, quando justificar `T_eval_policy` vs `T_eval_metrics` | Defender validade dos horizontes para metricas IPCW |
| Tabela | `outputs_v2/tables/table_policy_horizons_dual.csv` | Herdada da classificacao da figura de censura/horizontes | `V. Results and Discussion -> Survival Metrics Under Censoring` | Paragrafo imediatamente apos a figura de `G(t)` | Tornar auditavel a decisao dos dois horizontes |
| Tabela | `outputs_v2/tables/table_censoring_survival_G_test_by_week.csv` | Herdada da classificacao da figura de censura/horizontes | `V. Results and Discussion -> Survival Metrics Under Censoring` | Paragrafo tecnico de suporte (ou nota curta apos tabela principal) | Evidencia granular da queda de `G(t)` |

### 7.2 Itens para Apendice (qualidade tecnica boa, menor prioridade no corpo principal)
| Tipo | Arquivo exportado | Classificacao no notebook | Secao alvo | Paragrafo alvo (onde citar) | Funcao no argumento |
|---|---|---|---|---|---|
| Figura | `outputs_v2/figures/fig_calibration_ipcw_reliability_test.png` | Apendice forte | `Appendix: Calibration Diagnostics` | Primeiro paragrafo da subsecao de robustez de calibracao IPCW | Mostrar ganho incremental vs baseline |
| Tabela | `outputs_v2/tables/table_calibration_ipcw_metrics.csv` | Herdada da classificacao da figura IPCW | `Appendix: Calibration Diagnostics` | Paragrafo logo apos a figura IPCW | Quantificar diferenca de Brier/ECE |
| Figura | `outputs_v2/figures/fig_policy_covariate_deltas_by_week.png` | Apendice forte | `Appendix: Policy Mechanism Diagnostics` | Paragrafo de abertura da subsecao de mecanismo | Explicar por que o regime mechanism-aware teve efeito pequeno |
| Tabela | `outputs_v2/tables/table_policy_covariate_deltas_by_week.csv` | Herdada da classificacao da figura de mecanismo | `Appendix: Policy Mechanism Diagnostics` | Mesmo paragrafo da figura, como suporte numerico | Mostrar deltas semanais de covariaveis |
| Tabela | `outputs_v2/tables/table_policy_activation_summary.csv` | Herdada da classificacao da figura de mecanismo | `Appendix: Policy Mechanism Diagnostics` | Paragrafo curto apos deltas por semana | Contextualizar taxa de ativacao da politica |
| Figura | `outputs_v2/figures/fig_rq3_gap_Tpolicy.png` | Apendice | `Appendix: Fairness (RQ3)` | Paragrafo onde reporta `Delta Gap(T_policy)` com CI95 | Evidencia visual do efeito por grupo em `T_policy` |
| Figura | `outputs_v2/figures/fig_rq3_gap_Teval.png` | Apendice | `Appendix: Fairness (RQ3)` | Paragrafo seguinte para `Delta Gap(T_eval)` | Evidencia visual em horizonte longo |
| Figura | `outputs_v2/figures/fig_rq3_bootstrap_hist_deltagap_Tpolicy.png` | Apendice | `Appendix: Fairness (RQ3)` | Paragrafo de incerteza bootstrap (apos gaps pontuais) | Auditoria da distribuicao bootstrap |
| Figura | `outputs_v2/figures/fig_rq3_bootstrap_hist_deltagap_Teval.png` | Apendice | `Appendix: Fairness (RQ3)` | Mesmo bloco de incerteza | Confirmar estabilidade do sinal em `T_eval` |
| Tabela | `outputs_v2/tables/rq3_policy_bootstrap_wide.csv` | Herdada da classificacao das figuras bootstrap RQ3 | `Appendix: Fairness (RQ3)` | Paragrafo imediatamente apos histogramas | Resumir CI e estimativas por horizonte |
| Figura | `outputs_v2/figures/fig_calibration_by_group.png` | Apendice forte | `Appendix: Fairness (RQ3)` | Paragrafo final da subsecao fairness | Mostrar calibracao por grupo |
| Tabela | `outputs_v2/tables/rq3_fairness_extended.csv` | Herdada da classificacao da calibracao por grupo | `Appendix: Fairness (RQ3)` | Mesmo paragrafo da figura de calibracao por grupo | Consolidar metricas de fairness por grupo |

### 7.3 Fora do vinculo desta versao
- Itens de endpoint sensitivity (P20/P21*) ficam fora nesta rodada, conforme nota `Segurar por enquanto` do notebook.

## 8) Design visual com Graphviz (Graphwiz) para facilitar leitura
Nota: os Graphviz desta secao ja foram distribuidos em `3.1`, `3.2` e `3.3`; esta secao permanece como checklist de design.

Objetivo desta secao:
- Tornar o paper visualmente rastreavel entre narrativa, formulas e execucao do notebook.
- Reduzir carga cognitiva do leitor com diagramas curtos, cada um respondendo uma pergunta especifica.

Regra de design (para todos os graficos Graphviz):
- Usar `cluster` por track (temporal, IPCW, policy, benchmark, fairness).
- Em cada no, mostrar `P{n}` + nome curto da etapa.
- Diferenciar por cor: dados/processo (cinza), modelo (azul), policy (laranja), avaliacao (verde), fairness (vermelho claro).
- Limitar cada diagrama a 7-12 nos para manter legibilidade.

### 8.1 Graphviz G1: "Pipeline end-to-end"
- Pergunta que responde: "Como os dados viram evidencias para RQ1-RQ3?"
- Melhor lugar no paper: fim da `Methodology` (ou inicio de `Experimental Design`).
- P{n} incluidos:
  - Ingestao e definicao temporal: `P0 -> P1 -> P2 -> P3 -> P4 -> P5.*`
  - Split e modelo principal: `P6 -> P7-v3 -> P11.*`
  - Policy e trajetorias: `P12 -> P13 -> P14 -> P14.1 -> P14.2`
  - Avaliacao consolidada: `P17.* -> P21b -> P21c`
  - Ramificacao fairness: `P22 -> P23 -> P24 -> P25 -> P26 -> P27 -> P28 -> P28.1 -> P28.2`
- Valor para o leitor: mostra claramente o "prediction -> policy -> subgroup diagnostics" que e o objetivo central do paper.

### 8.2 Graphviz G2: "Policy mechanism e propagacao stateful"
- Pergunta que responde: "A politica altera apenas hazard ou altera estado/covariaveis ao longo do tempo?"
- Melhor lugar no paper: `Methodology` (subsecao de Policy Track).
- P{n} incluidos:
  - `P12` (trigger `t_star`)
  - `P13-v7` (shock + mechanism-aware com propagacao stateful)
  - `P13.1` (QA da propagacao)
  - `P14`/`P14.3` (trajetorias e `Delta S`)
- Valor para o leitor: evita interpretacao errada de que mechanism-aware e "one-shot".

### 8.3 Graphviz G3: "Censoring e dual horizons"
- Pergunta que responde: "Por que dois horizontes (`T_eval_policy` vs `T_eval_metrics`) e quando IPCW fica instavel?"
- Melhor lugar no paper: inicio de `Results` (antes de metricas IPCW) ou fim de `Experimental Design`.
- P{n} incluidos:
  - `P7.2` (estimacao `G_hat_cens` e `ipcw_w`)
  - `P14.2` (reconciliacao de horizontes)
  - `P17-v5` (metricas IPCW com horizonte estavel)
- Valor para o leitor: fortalece validade metodologica dos resultados de sobrevivencia sob censura.

### 8.4 Graphviz G4: "Benchmark e robustez"
- Pergunta que responde: "As conclusoes dependem de um unico modelo/endpoint?"
- Melhor lugar no paper: inicio da parte de benchmark/robustez em `Results`.
- P{n} incluidos:
  - `P9` (Cox baseline)
  - `P9.1` (baseline nao-temporal)
  - `P17.6` (benchmark unificado)
  - `P19.*` (ablation)
  - `P20` (OOD held-out run)
  - `P21*` (endpoint sensitivity e pacotes finais)
- Valor para o leitor: evidencia que o framework e robusto e nao artefato de um setup unico.

### 8.5 Graphviz G5: "Fairness pipeline"
- Pergunta que responde: "Como RQ3 e calculada e auditada?"
- Melhor lugar no paper: transicao para subsecao de fairness (RQ3) em `Results`.
- P{n} incluidos:
  - `P22-P24` (preparacao de grupos)
  - `P25-P27` (estimacao de gap e bootstrap)
  - `P28` (gaps em `T_policy`/`T_eval`)
  - `P28.1` (calibracao por grupo)
  - `P28.2` (metricas fairness extendidas)
- Valor para o leitor: deixa explicito que fairness aqui e camada de validacao/subgrupo no mesmo motor de policy.

### 8.6 Prioridade recomendada de inclusao dos Graphviz
- Main text:
  - `G1 Pipeline end-to-end`
  - `G2 Policy mechanism stateful`
  - `G3 Censoring e dual horizons`
- Apendice (ou material suplementar):
  - `G4 Benchmark e robustez`
  - `G5 Fairness pipeline`

## 9) Results expandido: objetivo de cada item e o que provar
Nota: os objetivos/provas/evidencias desta secao ja foram incorporados em `3.3`; esta secao permanece como checklist detalhado.

Pergunta-mestra da secao `Results`:
- "O framework temporal realmente habilita decisao auditavel de quando agir e como comparar regimes, sem extrapolar para causalidade identificada?"

### 9.1 Item a item (objetivo -> prova -> evidencia)
| Item em Results | Ligacao com objetivo central | O que preciso provar/mostrar | Evidencias minimas (qualidade) | P{n} fonte |
|---|---|---|---|---|
| 1) Qualidade do hazard temporal (RQ1) | Sem hazard confiavel, nao existe policy simulation confiavel | O modelo discrimina e calibra bem no nivel semanal | `fig_calibration_reliability_test.png` + `table_calibration_metrics.csv` + AUC train/test | `P7-v3`, `P11.*`, `P17*` |
| 2) Trajetoria de sobrevivencia baseline | Objetivo e sair do "quem" para "quando" | Existe janela temporal legivel de risco ao longo das semanas | curva baseline dentro de `fig_policy_survival_by_week_baseline_shock_mech.png` + resumo textual em horizonte | `P14*` |
| 3) Validade sob censura (IPCW/horizonte) | Objetivo exige comparacao auditavel; sem estabilidade de `G(t)` isso quebra | Justificar tecnicamente por que usar dois horizontes e por que metrica IPCW usa horizonte estavel | `fig_censoring_survival_G_test.png`, `table_policy_horizons_dual.csv`, `table_censoring_survival_G_test_by_week.csv` | `P7.2`, `P14.2`, `P17-v5` |
| 4) Contraste de policy (RQ2) | Nucleo do paper: comparar regimes explicitos | Shock vs mechanism-aware e magnitude de `Delta S` em semana-a-semana e em horizontes | `fig_policy_deltaS_by_week_shock_vs_mech.png` + `table_policy_deltaS_at_horizons.csv` | `P13`, `P14`, `P14.3` |
| 5) Interpretacao mecanistica (policy internals) | Evita leitura superficial do efeito | Mostrar por que mechanism-aware foi pequeno no setup default (ativacao, deltas de covariaveis) | `fig_policy_covariate_deltas_by_week.png`, `table_policy_covariate_deltas_by_week.csv`, `table_policy_activation_summary.csv` | `P13`, `P13.1` |
| 6) Benchmark e robustez de familia de modelo | Objetivo inclui framework reutilizavel, nao um modelo fragil | Temporal e competitivo e conclusao nao depende de um baseline unico | `fig_benchmark_dual_panel_auc.png` + `table_benchmark_unified.csv` (+ Cox table) | `P9`, `P9.1`, `P17.6` |
| 7) Robustez adicional (ablation, endpoint, OOD) | Mostrar que narrativa principal sobrevive a perturbacoes | Efeito principal nao e artefato de feature set, endpoint ou run especifico | `table_ablation_m4.csv`, tabelas de endpoint sensitivity, tabela OOD | `P19.*`, `P20`, `P21*` |
| 8) Fairness/subgrupo (RQ3) | Objetivo cita avaliacao consistente por subgrupos | Mesma politica pode alterar gap entre grupos; reportar magnitude e incerteza sem overclaim | `fig_rq3_gap_Tpolicy.png`, `fig_rq3_gap_Teval.png`, bootstrap hists + `rq3_policy_bootstrap_wide.csv`, `rq3_fairness_extended.csv` | `P25-P28.2` |
| 9) Discussao e limites | Alinhamento com gap pratico da Introduction/Background | Reforcar "simulated regime contrast" e nao efeito causal identificado; implicacoes para experimentos institucionais futuros | Sintese dos itens 1-8 + secao explicita de limitacoes | sintese da secao V |

### 9.2 Estrutura recomendada de paragrafos em `V. Results and Discussion`
- Paragrafo de abertura:
  - Reafirmar escopo: resultados sao estruturais/model-implied sob cenario explicito.
- Bloco RQ1:
  - Provar qualidade preditiva/calibracao e interpretacao temporal baseline.
- Bloco de validade de avaliacao:
  - Provar que escolha de horizontes sob censura e tecnicamente defensavel.
- Bloco RQ2:
  - Provar contraste de policy e magnitude; separar efeito shock vs mechanism-aware.
- Bloco benchmark/robustez:
  - Provar que conclusao central nao depende de um unico modelo/setup.
- Bloco RQ3:
  - Provar possibilidade de leitura por subgrupo com incerteza.
- Fechamento (discussion):
  - O que foi demonstrado, o que nao foi demonstrado (causalidade), e proximo passo experimental.

### 9.3 Checklist rapido: "se nao mostrar isso, enfraquece objetivo"
- Mostrar qualidade do hazard principal (discriminacao + calibracao).
- Mostrar `Delta S` em curva e em horizonte fixo.
- Mostrar justificativa de horizonte via `G(t)`.
- Mostrar ao menos um bloco de robustez (benchmark/ablation/ood).
- Mostrar fairness com CI (mesmo que no apendice).
- Declarar explicitamente limite de causalidade observacional.
