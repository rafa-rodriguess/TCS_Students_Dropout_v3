rafa# Analise da questao: mechanism-aware ad hoc e fragilidade de sinal

Data: 2026-03-09

## Questao avaliada
"The mechanism-aware counterfactual is ad hoc (click boosting and state propagation) and not grounded in a validated generative model of behavior; results for this branch are weak/negative, underscoring fragility."

## Veredito rapido
- Validade: **sim, a critica e valida**.
- Classificacao: **melhoria metodologica relevante** (nao bug de execucao).
- Gravidade para narrativa: **media-alta** (afeta forca interpretativa do ramo mechanism-aware, nao invalida todo o framework).

## Evidencias objetivas no pacote atual
1. O operador mechanism-aware e explicitamente proxy e predominantemente click-based:
- `outputs_v2/metadata.json:21` -> `covariate_impact_scope = click_based_engagement_proxy_in_exported_run`.
- `outputs_v2/tables/table_policy_mech_operator_spec.csv:2` -> `channel_mode = clicks_plus_stateful_recency_streak`, `W_weeks=2`, `alpha_week0=0.35`, `alpha_week1=0.10`.

2. O efeito de overwrite ficou concentrado em clicks, sem alteracoes observadas de recency/streak nos artefatos compactos:
- `outputs_v2/tables/table_policy_covariate_overwrites.csv:2`
  - `rows_changed_total_clicks=4816` (2.07% das linhas)
  - `rows_changed_recency=0`
  - `rows_changed_streak=0`
- `outputs_v2/tables/table_policy_covariate_propagation_checks.csv:2`
  - `frac_changed_recency=0.0`
  - `frac_changed_streak=0.0`

3. O sinal agregado do ramo mechanism-aware no relatorio principal e fraco/negativo:
- `outputs_v2/tables/table_policy_deltaS_at_horizons_by_scenario.csv`
  - `deltaS_mech` em `T_policy=18`: `-0.0006035`
  - `deltaS_mech` em `T_eval_policy=38`: `-0.0062371`
- `results_body.tex:220` ja reconhece que o mechanism-aware nao gera ganho positivo na rodada reportada.

4. O proprio texto metodologico assume que o canal e um proxy, nao uma replicacao gerativa validada:
- `experimental_design_body.tex:308`
- `methodology_body.tex:253`

## Julgamento tecnico (erro vs melhoria)
1. Nao e erro de implementacao:
- O pipeline executa e exporta o contrato do operador de forma auditavel.

2. E fragilidade de validade externa/mecanistica:
- O mecanismo nao deriva de modelo gerativo comportamental validado.
- O canal efetivo no pacote ativo ficou estreito (clicks), reduzindo plausibilidade de mecanismo comportamental rico.
- O sinal fraco/negativo do ramo mechanism-aware e coerente com essa restricao.

## Proposta (somente o necessario, em ordem de impacto)
1. Manuscrito (curto e imediato)
- Rebaixar a linguagem de "mechanism-aware" para "operator-based proxy mechanism" na interpretacao de resultados.
- Explicitar que os resultados desse ramo sao dependentes da escolha de operador/canal e que, na rodada atual, o efeito foi fraco/negativo.

2. Notebook (minimo viavel para reduzir fragilidade)
- Adicionar um diagnostico de consistencia do mecanismo:
  - falhar/alertar quando `rows_changed_total_clicks > 0` e `rows_changed_recency == rows_changed_streak == 0`.
- Exportar tabela de atribuicao por canal e por horizonte:
  - `table_policy_mech_channel_attribution.csv`.

3. Notebook (melhoria forte, se houver tempo)
- Rodar ablation por canal no ramo mechanism-aware (clicks-only vs clicks+recency/streak efetivos).
- Recalcular `DeltaS_mech` em `T_policy` e `T_eval_policy` por variante de canal.
- Exportar comparativo:
  - `table_policy_mech_channel_ablation_horizons.csv`.

## Decisao pratica
- A critica **permanece valida** mesmo apos as correcoes recentes de censoring.
- No estado atual, o correto e tratar o ramo mechanism-aware como **analise estrutural de proxy operacional**, nao como mecanismo comportamental validado.
