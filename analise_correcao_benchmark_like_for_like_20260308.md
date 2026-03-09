# Analise da inconsistencia de benchmark e proposta de correcao (like-for-like)

## Resumo executivo

O questionamento e valido: os numeros atuais misturam tarefas diferentes e tratamento de censura diferente. Isso permite observar AUC alto no nivel de linha (hazard semanal) ao mesmo tempo que AUC proximo de chance no nivel de matricula por horizonte (evento ate T).

Conclusao pratica: nao e uma contradicao matematica do modelo, e uma contradicao de protocolo de comparacao.

## Evidencias objetivas no projeto

1. Tabela do manuscrito mostra AUC de horizonte temporal perto de chance e AUC de linha alto em bloco separado:
	- `results_body.tex` (secao de benchmark):
	- Temporal hazard: `AUC_event_by_T_policy ~ 0.4803`, `AUC_event_by_T_eval_metrics ~ 0.4979`.
	- Temporal hazard + IPCW: `~0.5220` e `~0.5291`.
	- Non-temporal RSF: `~0.6457` e `~0.6388`.
	- O texto ja indica que `AUC_row_test` usa unidade diferente (linhas pessoa-semana) em relacao ao benchmark de horizonte (matriculas).

2. O benchmark de horizonte no notebook usa rotulo de evento-ate-horizonte em nivel de matricula:
	- `TCM-Student-Dropout_updated.ipynb:4146` define `event_by_horizon(df, T)`.
	- `TCM-Student-Dropout_updated.ipynb:4149` retorna `((e == 1) & (t_event <= T)).astype(int)`.

3. No mesmo notebook, o baseline nao-temporal pode nao ser RSF real nesta execucao:
	- `TCM-Student-Dropout_updated.ipynb:3934` e `TCM-Student-Dropout_updated.ipynb:3976` indicam `rf_classifier_fallback` e `sklearn_fallback_no_sksurv (ModuleNotFoundError)`.
	- `TCM-Student-Dropout_updated.ipynb:4243` e `TCM-Student-Dropout_updated.ipynb:4244` mostram `p_test_policy = p_any` e `p_test_eval = p_any` no fallback.

4. Existem metricas de censura-aware no notebook (C-index discreto, IPCW Brier), mas o AUC de horizonte do benchmark comparativo principal permanece como ROC AUC padrao (nao IPCW), gerando assimetria de tratamento de censura entre metricas e entre familias.

## Por que o padrao observado acontece

1. Unidade de analise diferente
	- `AUC_row_test`: discrimina risco semanal por linha pessoa-periodo.
	- `AUC_event_by_T`: discrimina probabilidade acumulada de evento ate T por matricula.
	- Alto desempenho em uma tarefa nao implica alto desempenho na outra.

2. Rotulo de horizonte sob censura
	- Em `Y(T)=1{evento e t_event<=T}`, individuos censurados antes de T nao tem desfecho plenamente observavel para esse horizonte.
	- AUC sem ajuste IPCW tende a atenuar sinal e aproximar de 0.5 quando ha censura relevante.

3. Comparabilidade entre familias pode ficar quebrada
	- Temporal usa dinamica semanal e agregacao para `1-S(T)`.
	- Nao-temporal nesta execucao foi fallback RF (nao RSF), com mesma probabilidade para dois horizontes, o que nao e estritamente equivalente a um modelo de sobrevivencia por horizonte.

4. Mistura de metricas primarias e secundarias
	- Parte do pipeline ja usa metricas IPCW (Brier/IBS), mas a leitura central do benchmark fica ancorada em AUC de horizonte sem o mesmo tratamento de censura.

## Proposta de correcao metodologica (like-for-like)

### Regra central

Toda comparacao principal entre familias deve fixar simultaneamente:
- mesma unidade (`enrollment`),
- mesmo horizonte (`T_policy=18` e `T_eval_metrics=37`),
- mesmo tratamento de censura na avaliacao (IPCW),
- mesma politica de calibracao (ou sem calibracao para todas, ou calibracao para todas no mesmo esquema).

### Novo pacote de tabelas recomendado

1. `table_benchmark_row_level_auc.csv` (secundaria)
	- Manter apenas para analise interna temporal.
	- Nao usar para comparacao principal com modelos estaticos.

2. `table_benchmark_horizon_auc_unweighted.csv` (diagnostico)
	- Manter como leitura auxiliar, explicitamente marcada como nao-censura-aware.

3. `table_benchmark_horizon_ipcw_primary.csv` (principal)
	- Comparacao oficial entre familias em nivel de matricula e por horizonte.
	- Colunas minimas:
	  - `model`
	  - `T`
	  - `AUC_IPCW`
	  - `IPCW_Brier`
	  - `n_enroll_test`
	  - `n_effective_weight` (soma de pesos)
	  - `model_impl` (ex.: `rsf`, `rf_fallback`, `temporal_hazard`)

### Ajustes concretos no notebook

1. No bloco de benchmark unificado (P17.6), adicionar AUC IPCW por horizonte para todos os modelos.
2. Se `sksurv` nao estiver disponivel:
	- renomear claramente o baseline para `rf_classifier_fallback` no texto e tabelas (sem chamar de RSF no corpo principal), ou
	- instalar/garantir `sksurv` e rerodar para obter RSF real.
3. Evitar comparacao direta entre `AUC_row_test` e `AUC_event_by_T_*` na mesma linha narrativa de ranking.
4. Incluir guardrail automatico que falha quando houver mistura de unidade na mesma tabela principal.

### Formula operacional sugerida para AUC IPCW de horizonte

Para cada horizonte `T`:
- `y_i(T) = 1{E_i=1 e t_event_i<=T}`
- peso IPCW:
  - se evento ate `T`: `w_i = 1 / G_hat(t_event_i-)`
  - se sem evento observado ate `T` e ainda em risco em `T`: `w_i = 1 / G_hat(T)`
  - se censurado antes de `T`: `w_i = 0` (ou excluir)
- calcular `roc_auc_score(y, p_T, sample_weight=w)` no subconjunto observavel.

## Proposta de correcoes no manuscrito

1. Em `results_body.tex`, separar explicitamente:
	- comparacoes primarias (horizonte IPCW, enrollment-level),
	- comparacoes secundarias (row-level temporal).

2. Atualizar rotulo da familia nao-temporal quando fallback ocorrer:
	- de `Non-temporal RSF` para `Non-temporal RF fallback (no sksurv)` nessa execucao.

3. Inserir frase de conformidade metodologica:
	- "As comparacoes principais entre familias foram conduzidas em nivel de matricula, no mesmo horizonte e com o mesmo tratamento de censura (IPCW)."

4. Manter tabela de linha (`AUC_row_test`) apenas como resultado complementar de discriminacao local semanal.

## Criterios de aceitacao

1. Nao ha tabela principal misturando unidade de linha e unidade de matricula.
2. Toda comparacao cross-family em horizonte reporta metrica censura-aware (AUC IPCW e/ou Brier IPCW).
3. Implementacao do baseline nao-temporal esta corretamente nomeada (`RSF` apenas quando RSF real foi usado).
4. Texto do manuscrito impede interpretacao de ranking cruzado entre tarefas diferentes.

## Decisao recomendada

Classificar a inconsistencia como "metodologica de comparacao" (nao como "erro matematico do modelo"). Corrigir protocolo de benchmark para like-for-like e atualizar texto/tabelas para refletir esse contrato.

## Adendo de validacao (nova rodada executada em 2026-03-08)

### Resultado da verificacao

As adequacoes like-for-like ainda nao foram efetivamente aplicadas na execucao validada.

### Evidencia em artefatos CSV

1. Arquivos gerados nesta rodada:
	- `outputs_v2/tables/table_benchmark_horizon_auc.csv`
	- `outputs_v2/tables/table_benchmark_row_level_auc.csv`
	- `outputs_v2/tables/table_benchmark_unified.csv`

2. Arquivo principal esperado para comparacao primaria IPCW nao existe:
	- ausente: `outputs_v2/tables/table_benchmark_horizon_ipcw_primary.csv`

3. Schema observado permanece no formato antigo (v1), sem colunas IPCW de AUC por horizonte:
	- `AUC_event_by_T_policy`, `AUC_event_by_T_eval_metrics`, `n_test_enrollments`, `source`
	- sem `AUC_IPCW_event_by_T_policy` e sem `AUC_IPCW_event_by_T_eval_metrics`

4. Rotulo nao-temporal continua como `non_temporal_rsf` nos CSVs exportados.

### Evidencia no notebook

1. Marcadores de execucao e codigo continuam em `P17.6-v1`:
	- banner de output: `=== P17.6-v1 — Unified Benchmark Pack ... ===`
	- termino: `=== P17.6-v1 DONE ===`

2. Nao ha marcador `P17.6-v2` nem referencias ao artefato `table_benchmark_horizon_ipcw_primary.csv` no bloco de benchmark executado.

### Conclusao objetiva desta rodada

Status do ipynb frente ao plano de correcao: **NAO corrigido ainda** para o contrato like-for-like.

### Proximo passo tecnico minimo

1. Atualizar o bloco P17.6 para versao com AUC IPCW por horizonte e export do arquivo primario IPCW.
2. Reexecutar apenas a cadeia minima (P9.1 e P17.6) e revalidar:
	- existencia de `table_benchmark_horizon_ipcw_primary.csv`
	- colunas `AUC_IPCW_event_by_T_policy` e `AUC_IPCW_event_by_T_eval_metrics`
	- rotulo coerente do baseline nao-temporal (`non_temporal_rsf` vs `non_temporal_rf_fallback`).

## Adendo de revalidacao (2026-03-09)

### Correcao da leitura anterior

A conclusao "NAO corrigido ainda" da rodada anterior ficou parcialmente invalida por estado local de arquivos.

### Evidencia objetiva (estado atual)

1. O arquivo IPCW principal esta presente no workspace e rastreado no `HEAD`:
	- `outputs_v2/tables/table_benchmark_horizon_ipcw_primary.csv`

2. Header do arquivo IPCW principal (9 colunas) inclui as metricas alvo:
	- `AUC_IPCW_event_by_T_policy`
	- `AUC_IPCW_event_by_T_eval_metrics`

3. `table_benchmark_horizon_auc.csv` e `table_benchmark_unified.csv` tambem carregam colunas AUC unweighted + AUC IPCW no schema atual.

### Conclusao atualizada

No nivel de artefato CSV, o pacote de benchmark like-for-like esta **presente** para a parte de AUC IPCW por horizonte. A ausencia reportada antes deve ser tratada como falso negativo de sincronizacao/local state, nao como falha estrutural do notebook.
