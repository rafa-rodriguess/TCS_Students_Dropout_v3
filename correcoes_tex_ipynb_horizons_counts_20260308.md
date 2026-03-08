# Correcoes e reconciliacao (TEX + IPYNB)

Data: 2026-03-08

## 1) Fonte de verdade reconciliada

### 1.1 Horizontes canonicos

| Parametro | Valor | Uso correto | Evidencia principal |
|---|---:|---|---|
| `T_policy` | 18 | Horizonte substantivo de reporte (contrastes/politica e fairness principal) | `experimental_design_body.tex:170`, `experimental_design_body.tex:336`, `outputs_v2/tables/table_policy_horizons_dual.csv:2`, `TCM-Student-Dropout_updated.ipynb:3936` |
| `T_eval_metrics` | 37 | Horizonte tecnico estavel para metricas IPCW | `experimental_design_body.tex:182`, `methodology_body.tex:76`, `results_body.tex:143`, `outputs_v2/tables/table_policy_horizons_dual.csv:4`, `TCM-Student-Dropout_updated.ipynb:3937` |
| `T_eval_policy` | 38 | Horizonte de suporte bruto para trajetorias de politica (nao usar para metricas IPCW) | `experimental_design_body.tex:185`, `methodology_body.tex:76`, `results_body.tex:144`, `outputs_v2/tables/table_policy_horizons_dual.csv:3`, `TCM-Student-Dropout_updated.ipynb:7749` |

### 1.2 Sobre semanas 57/63

- Semanas tardias (ex.: 57 e 63) aparecem como **cauda de suporte bruto** (coorte completa e/ou treino), nao como horizonte de reporte.
- Isso esta consistente com:
- `experimental_design_body.tex:185` (treino ate semana 63; teste ate 38)
- `results_body.tex:175` (57/63 como cauda, nao horizonte)
- `TCM-Student-Dropout_updated.ipynb:617` (`max_t_final_week: 63`)
- `TCM-Student-Dropout_updated.ipynb:1233` (registro de semana 57)
- `TCM-Student-Dropout_updated.ipynb:1239` (registro de semana 63)

## 2) Tabela definitiva por bloco de analise

| Bloco de analise | Unidade | Tamanho de amostra correto | Horizonte(s) correto(s) | Evidencia |
|---|---|---|---|---|
| Construcao da coorte | Enrollments / person-period rows | 32,593 enrollments; 775,295 pp-rows | Suporte bruto ate semana 63 | `results_body.tex:164` |
| Split temporal | Enrollments / person-period rows | Train: 22,815 e 542,878 rows; Test: 9,778 e 232,417 rows | Train ate 63; test ate 38 | `results_body.tex:165`, `graphviz_pipeline_endtoend.txt:32`, `outputs_v2/tables/table_ipcw_weights_summary_train.csv:2` |
| RQ1 metricas ponderadas | Test enrollments | 9,778 test enrollments; 2,216 eventos | `T_policy=18` e `T_eval_metrics=37` | `results_body.tex:166`, `outputs_v2/tables/table_survival_metrics_ipcw.csv:15`, `outputs_v2/tables/table_survival_metrics_ipcw.csv:16` |
| RQ2 trajetorias de politica | Test enrollments | 9,778 test enrollments | `T_policy=18` e `T_eval_policy=38` | `results_body.tex:167`, `outputs_v2/tables/table_policy_horizons_dual.csv:2`, `outputs_v2/tables/table_policy_horizons_dual.csv:3` |
| Benchmark em horizonte | Test enrollments | 9,778 test enrollments | `T_policy=18` e `T_eval_metrics=37` | `results_body.tex:168`, `outputs_v2/tables/table_non_temporal_rsf_metrics.csv:4`, `outputs_v2/tables/table_non_temporal_rsf_metrics.csv:5`, `outputs_v2/tables/table_non_temporal_rsf_metrics.csv:7` |
| Diagnosticos row-level | Test person-period rows | 232,417 test rows | Sem resumo por horizonte (row-level semanal) | `results_body.tex:169` |
| RQ3 subgroup/fairness | Test pp-rows + resumo por horizonte | 232,417 test rows + mesma coorte de teste para contrastes | `Delta Gap(18)` e `Delta Gap(37)` | `results_body.tex:170`, `outputs_v2/tables/rq3_policy_gaps_unified.csv:1` |

## 3) Conflitos encontrados e reconciliacao

### 3.1 `22,853` vs `22,815`

- No estado atual do repositorio, **nao foi encontrada ocorrencia textual valida de `22,853`** em TEX, notebook ou tabelas de resumo.
- O valor canonicamente consistente e exportado e **22,815 (train enrollments)**.
- Observacao: buscas por `22853` retornaram apenas trechos numericos incidentais em valores float, nao contagens de amostra.

### 3.2 `37` vs `38` vs `57`

- `37` = `T_eval_metrics` (IPCW estavel) -> usar para metricas ponderadas.
- `38` = `T_eval_policy` (suporte bruto de teste) -> usar para trajetorias RQ2, nao para metricas IPCW.
- `57` = semana de cauda em saidas semanais, **nao horizonte oficial**.

## 4) Correcoes recomendadas no TEX

Status geral: o TEX principal esta majoritariamente coerente com a fonte de verdade.

Status de implementacao (2026-03-08):

- Concluido: nenhuma inconsistência textual critica restante encontrada em `*.tex` para `T_policy`, `T_eval_policy`, `T_eval_metrics`.
- Concluido: nenhuma ocorrência valida de `22,853` nos TEX.
- Nao aplicavel no momento: ajuste de figura externa com `22,853` (nao encontrada no estado atual do repo).

Ajustes recomendados:

1. Manter `T_eval_policy` e `T_eval_metrics` sempre explicitos (sem alias `T_eval`) em qualquer novo trecho.
2. Onde houver mencao a semanas tardias, repetir a ressalva "cauda de suporte, nao horizonte de reporte" (ja presente em `results_body.tex:175`).
3. Se houver figura renderizada externa ainda com `22,853`, atualizar para `22,815` e regenerar a partir das fontes (`graphviz_pipeline_endtoend.txt:32` ja esta correto).

## 5) Correcoes recomendadas no IPYNB (`TCM-Student-Dropout_updated.ipynb`)

Foram encontrados pontos de ambiguidade de nomenclatura entre `T_eval` e os dois horizontes finais.

Ajustes recomendados:

1. Padronizar variavel:
- `T_eval` (alias historico) -> `T_eval_policy` quando representar suporte bruto (38).
- Manter `T_eval_metrics` para o horizonte estavel (37).

2. Padronizar nomes de colunas/export:
- `AUC_event_by_T_eval` -> `AUC_event_by_T_eval_metrics` quando o calculo usa 37.
- `y_event_by_T_eval`/`p_event_by_T_eval` -> explicitar `..._T_eval_metrics` ou `..._T_eval_policy` conforme o caso.

3. Inserir check unico (antes dos blocos de relatorio):
- `assert T_policy <= T_eval_metrics <= T_eval_policy`
- E imprimir tabela curta com (`T_policy`, `T_eval_metrics`, `T_eval_policy`, `N_train_enrollments`, `N_test_enrollments`, `N_test_events`).

4. Pontos concretos para revisar no notebook (linhas aproximadas):
- Alias `T_eval` e mensagens: `TCM-Student-Dropout_updated.ipynb:6679`, `TCM-Student-Dropout_updated.ipynb:6906`, `TCM-Student-Dropout_updated.ipynb:8083`
- Uso correto dos 3 horizontes (ja presente, consolidar): `TCM-Student-Dropout_updated.ipynb:7738`, `TCM-Student-Dropout_updated.ipynb:7750`, `TCM-Student-Dropout_updated.ipynb:9724`

Status de implementacao (2026-03-08):

- Concluido: padronizacao de nomes de metrica para horizonte estavel.
- Detalhe: `AUC_event_by_T_eval` -> `AUC_event_by_T_eval_metrics`; `y_event_by_T_eval` -> `y_event_by_T_eval_metrics`; `p_event_by_T_eval` -> `p_event_by_T_eval_metrics` no notebook e nas saidas impressas associadas.
- Concluido: introducao explicita de `T_eval_policy` nos blocos centrais de politica (P14/P14b), mantendo `T_eval` como alias de compatibilidade para nao quebrar blocos legados.
- Concluido: ajuste de mensagens de log/horizonte para explicitar `T_eval_policy` nos trechos principais de politica.
- Concluido: inserida celula de validacao de contrato de horizontes e contagens-chave: `P14.2b` (cell id `#VSC-1f8b4a81`).
- Concluido: prioridade de leitura de `T_eval_policy` (com fallback legado para `T_eval`) no bloco de dual horizons.
- Concluido: bloco de proveniencia (P15.1) passa a carregar `T_eval_policy` explicitamente quando disponivel.
- Parcial: ainda existem menções legadas a `T_eval` em partes descritivas/artefatos historicos do notebook para compatibilidade e rastreabilidade; o fluxo principal de politica e benchmark ficou com nomenclatura explicita.

## 6) Conclusao operacional

- Fonte de verdade final:
- Horizontes: `T_policy=18`, `T_eval_metrics=37`, `T_eval_policy=38`.
- Contagens principais: train `22,815`, test `9,778`, test events `2,216`, test pp-rows `232,417`, full cohort `32,593`.
- Semanas 57/63: apenas cauda de suporte, nao horizonte de inferencia/claim.
