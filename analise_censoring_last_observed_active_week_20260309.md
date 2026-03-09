# Analise da questao: censoring em nao-eventos ancorado em "last observed active week"

Data: 2026-03-09

## Questao avaliada
"Censoring construction for non-events uses last observed active week, which risks informative censoring tied to engagement; this is acknowledged but not deeply probed."

## Veredito rapido
- Validade da critica: **sim, valida**.
- Classificacao: **melhoria metodologica importante** (nao e bug de execucao), com potencial de virar erro de interpretacao se o texto sugerir inferencia causal forte.
- Vale a pena corrigir: **sim, vale muito a pena** para robustez cientifica e blindagem em revisao por pares.

## Evidencias no repositorio
1. O notebook define o tempo final de nao-eventos a partir da ultima semana observada no VLE:
- `TCM-Student-Dropout_updated.ipynb:684` (comentario: `Compute last observed VLE week (t_last_obs_week)`).
- `TCM-Student-Dropout_updated.ipynb:704` (`groupby(...).max()` para obter `t_last_obs_week`).
- `TCM-Student-Dropout_updated.ipynb:755` (`t_final_week = ... else t_last_obs_week`).

2. O evento de censura no modelo de censoring e construido exatamente no terminal dos nao-eventos:
- `TCM-Student-Dropout_updated.ipynb:2264` (`def build_cens_frame(...)`).
- `TCM-Student-Dropout_updated.ipynb:2270` (`cens_event = (event_withdrawn==0) & (week==t_final_week)`).

3. O manuscrito explicita a mesma logica:
- `experimental_design_body.tex:458` (`Compute censoring anchor ... last observed week ...`).
- `methodology_body.tex:122` (`t_i^{final}=t_i^{last_obs}` quando `E_i=0`).

4. O manuscrito tambem ja admite limite de identificacao:
- `experimental_design_body.tex:484` (nao persegue identificacao causal para censoring potencialmente informativo).

## Analise tecnica (erro vs melhoria)
1. Nao e erro de implementacao:
- A implementacao e consistente entre notebook e TEX.
- Nao ha incoerencia interna no pipeline.

2. E risco metodologico real:
- Se o fim de observacao dos nao-eventos depende de engajamento (atividade VLE), a censura pode ser informativa.
- Isso pode enviesar metricas e comparacoes se a modelagem de censoring nao capturar suficientemente esse mecanismo.

3. O projeto ja mitiga parcialmente:
- Usa IPCW condicional `G(t|X_t)` com variaveis de engajamento (`recency`, `streak`, `total_clicks` etc.).
- Isso reduz risco, mas nao "prova" ignorabilidade condicional.

Conclusao: hoje esta em zona de **assuncao plausivel + mitigacao parcial**, sem auditoria de sensibilidade suficientemente profunda.

## Proposta de correcao no notebook
Objetivo: transformar o ponto em analise de robustez auditavel (e nao apenas limitacao textual).

1. Padronizar nomenclatura para evitar sobreinterpretacao
- Trocar mensagens de "active week" por "last observed VLE week (observation proxy)".
- Deixar explicito que e proxy de observacao, nao censoring administrativo puro.

2. Adicionar bloco de sensibilidade de construcao de censura (novo bloco, ex.: `P7.2b`)
- Comparar pelo menos 3 regras para `t_final_week` de nao-eventos:
  - Regra A (atual): `t_last_obs_week`.
  - Regra B: censura administrativa no fim do curso/run (quando disponivel).
  - Regra C (hibrida): `min(t_last_obs_week + grace_weeks, t_admin_end)` com `grace_weeks` em `{1,2,3}`.
- Reestimar `G_hat_cens`, `ipcw_w`, `BS_IPCW(T_eval_metrics)`, `IBS`, `C-index`, e deltas de politica (`Delta S`) sob cada regra.
- Exportar:
  - `outputs_v2/tables/table_censoring_anchor_sensitivity.csv`
  - `outputs_v2/tables/table_censoring_anchor_sensitivity_by_group.csv`

3. Adicionar diagnosticos de censoring informativo
- AUC/calibracao do modelo de `cens_event` por horizonte.
- Estratificacao por decis de risco base e taxa de censura terminal por decil.
- Exportar:
  - `outputs_v2/tables/table_censoring_informativeness_diagnostics.csv`

4. Definir criterio de estabilidade para claim principal
- Exemplo: se variacao relativa de `BS_IPCW` > 10% entre regras de censura, marcar resultados como sensiveis no report pack.

## Proposta de correcao no TEX
1. `methodology_body.tex`
- Incluir frase formal: censura de nao-eventos e proxy observacional baseada em ultima observacao VLE.
- Declarar assuncao: independenca condicional da censura dado `X_t` (nao verificavel diretamente).

2. `experimental_design_body.tex`
- Expandir a secao de limites com referencia aos novos testes de sensibilidade de censura.
- Ajustar texto do algoritmo para "last observed VLE week (observation proxy)" em vez de "active".

3. `results_body.tex` ou apendice
- Inserir tabela curta com resultados da sensibilidade de censura e impacto em metricas/chaves de decisao.

## Vale a pena? (decisao)
- **Sim**.
- Prioridade sugerida: **alta para manuscrito** (revisao/metodologia), **media para engenharia** (nao quebra pipeline).
- Ganho: reduz risco de critica de viés por censoring informativo e fortalece defensabilidade dos resultados.

## Recomendacao pratica (minimo viavel)
1. Fazer primeiro a sensibilidade A vs B (atual vs administrativo) + 1 grafico de impacto em `BS_IPCW` e `Delta S(T_policy)`.
2. Atualizar TEX com 1 paragrafo metodologico + 1 paragrafo de limites + 1 tabela compacta.
3. Se diferencas forem pequenas, manter regra atual como primaria e reportar robustez.
4. Se diferencas forem relevantes, rebaixar forca de claim e promover resultado robusto como principal.
