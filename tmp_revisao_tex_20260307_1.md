# Revisao Cruzada dos `.tex` vs plano_criacao_paper.md (temporario)

Data: 2026-03-07

Escopo desta revisao:
- Nao foi alterado nenhum arquivo `.tex`.
- Este arquivo apenas identifica o que precisa ser revisto depois.
- Fonte de verdade usada: `plano_criacao_paper.md` (snapshot sincronizado), `TCM-Student-Dropout_updated.ipynb`, `outputs_v2/metadata.json` e tabelas/figuras exportadas em `outputs_v2/`.

## Regras globais que os `.tex` devem seguir na proxima etapa

1. Desambiguar horizonte longo:
- `T_eval_policy = 38` para trajectory/policy/`Delta S`.
- `T_eval_metrics = 37` para metricas IPCW, fairness agregada e bootstrap de RQ3.
- Quando um arquivo/figura usar o nome legado `T_eval` ou `Teval`, o texto deve explicitar a semantica correta.

2. Congelar parametros da policy com os nomes canonicamente exportados:
- `r_star = 1`
- `W_weeks = 2`
- `alpha_week0 = 0.35`
- `alpha_week1 = 0.10`
- `decay_type = kb2023_step_2w`
- `window_exclusive_upper = True`
- `delta_shock_ablation = 0.20`

3. A ancora bibliografica da policy nao deve ser citada a partir de `table_policy_scenario_params.csv`.
- Usar `table_policy_spec.csv` e `metadata.policy.policy_spec` para ancora/definicao do gatilho.
- A ancora atual e `Kay & Bostock (2023) - The Power of the Nudge: Technology Driving Persistence`.

4. Em RQ3, o caso demonstrado atualmente e `gender`.
- Ordenacao exportada: `group0 = M`, `group1 = F`.
- `treated_enrollments = 8387`.
- O texto nao deve continuar exemplificando com `disability status` se a demonstracao reportada no paper for a execucao atual.

5. Benchmark:
- Nao escrever ou implicar que o framework temporal vence todos os benchmarks.
- O estado atual dos resultados mostra dominancia de `non_temporal_rsf` em `AUC_event_by_T_*`.
- O argumento do paper deve ficar ancorado em decisao temporal auditavel, trajetorias e contrasts estruturais de regime.

6. Ancoragem do impacto em covariaveis no regime mechanism-aware:
- O manuscrito deve deixar explicito que a propagacao em covariaveis de engajamento e inspirada qualitativamente na logica de desengajamento/reengajamento de `Kay & Bostock (2023)`, e nao apresentada como replicacao literal das variaveis ou magnitudes do estudo.
- Se o evidence package exportado mostrar evidencia material concentrada em `total_clicks` ou em proxy de participacao click-based, o texto deve refletir isso e evitar prometer propagacao ampla em multiplas covariaveis sem suporte exportado.

## Dimensoes da revisao editorial pedida nesta etapa

Nesta expansao, a revisao passa a cobrir quatro camadas por arquivo:

1. Frases:
- trechos que hoje estao fortes demais, vagos demais, ou semanticamente desalinhados com os outputs atuais.

2. Conceitos:
- ideias que precisam ser melhor delimitadas no texto para nao misturar protocolo, evidencia, interpretacao e escopo causal.

3. Conclusoes:
- afirmacoes finais ou de transicao que hoje extrapolam o que os resultados sustentam.

4. Justificativas faltantes:
- pontos onde o texto precisa explicar melhor por que uma escolha metodologica, de design ou de leitura e aceitavel.

Regra de leitura deste arquivo temporario:
- `frase` = problema local de redacao.
- `conceito` = problema de enquadramento teorico/metodologico.
- `conclusao` = problema no que o texto afirma como takeaway.
- `justificativa` = argumento que precisa aparecer no `.tex` para a secao ficar defensavel.

## Resumo por arquivo

### 1) `TCM-Student-Dropout.tex`
Status: sem alteracao obrigatoria identificada.

Observacao:
- O arquivo mestre apenas inclui os corpos e nao contem, neste momento, inconsistencias semanticas com o snapshot travado.

### 2) `abstract_body.tex`
Status: sem alteracao obrigatoria identificada.

Observacoes:
- O abstract ja esta alinhado com a leitura nao-causal e com a ideia de `structural policy evaluation`.
- Os valores de AUC (`0.8359` e `0.8396`) estao coerentes com o snapshot travado por arredondamento.

### 3) `introduction_body.tex`
Status: revisao obrigatoria.

Criticidade: media.

Trecho a revisar:
- `introduction_body.tex:14`

Problema:
- O texto usa `disability status` como exemplo de subgrupo, mas o snapshot travado do paper consolidou a demonstracao atual de RQ3 em `gender`.

Direcao da futura alteracao:
- Trocar o exemplo explicito para `gender`, ou generalizar o trecho para nao nomear um grupo especifico na introducao.
- Se o texto optar por nomear o grupo, precisa bater com os outputs atuais: `group0 = M`, `group1 = F`.

Revisao de frases:
- A frase atual em `introduction_body.tex:14` cria expectativa de um exemplo empirico que depois nao bate com a execucao atual.
- Melhor evitar uma frase do tipo “e.g., disability status” se o paper nao vai entregar esse caso nos resultados finais.

Revisao de conceitos:
- A introducao precisa apresentar RQ3 como capacidade do framework demonstrada em um subgrupo observado na execucao atual, nao como promessa aberta de qualquer subgrupo especifico.
- O conceito central aqui deve ser: “subgroup-sensitive structural evaluation”, nao “fairness claim” nem “targeted support design” como se isso ja estivesse empiricamente demonstrado.

Revisao de conclusoes:
- O final do paragrafo atual encosta em uma leitura prospectiva relativamente forte (“may motivate future extensions toward more targeted support design”). Isso pode permanecer, mas so se a frase deixar claro que se trata de motivacao metodologica futura, nao de evidencia aplicada suficiente para desenho de targeting real.

Justificativas a incluir:
- Se o texto nomear `gender`, convem explicar em uma frase curta que esse foi o grupo demonstrado na execucao atual por disponibilidade/clareza do output, sem sugerir que e o unico subgrupo possivel.
- Se preferir manter a introducao geral, a justificativa nao precisa aparecer aqui; pode ser empurrada para Design/Results.

### 4) `background_body.tex`
Status: sem alteracao obrigatoria identificada.

Observacao:
- O arquivo esta conceitual e nao carrega os pontos que ficaram sensiveis apos a sincronizacao de policy/horizons/results.

### 5) `methodology_body.tex`
Status: revisao obrigatoria.

Criticidade: media.

Trechos a revisar:
- `methodology_body.tex:229`
- Regiao da subsecao `Policy Simulation (RQ2): Shock vs. Mechanism-Aware`
- Regiao final de `Reproducibility`

Problemas identificados:
- A notacao da janela continua em `W` sem amarrar o nome canonico exportado `W_weeks`.
- O texto matematico usa `delta_shock` apenas como simbolo, mas a metodologia ainda nao deixa claro o nome congelado do parametro exportado: `delta_shock_ablation`.
- Falta amarrar explicitamente a ancora da policy ao artefato correto (`table_policy_spec.csv`) e ao estudo de `Kay & Bostock (2023)`.
- Falta explicitar que o bloco mechanism-aware usa covariaveis de engajamento como proxies inspiradas na logica de participacao/reengajamento de `Kay & Bostock (2023)`, e nao como reproducao literal do paper-fonte.
- Falta delimitar, no proprio texto metodologico, o que esta ancorado externamente e o que e apenas modelado como proxy interna; isso vale especialmente para `total_clicks` e demais covariaveis de estado de engajamento.
- Falta registrar, na metodologia/reprodutibilidade, a regra de citacao entre `table_policy_spec.csv` e `table_policy_scenario_params.csv`.

Direcao da futura alteracao:
- Manter `W` e `delta_shock` na notacao matematica, se desejado, mas declarar explicitamente no texto que os nomes canonicos exportados sao `W_weeks` e `delta_shock_ablation`.
- Inserir a ancora bibliografica e a definicao operacional do gatilho no bloco metodologico de policy.
- Inserir uma frase explicita dizendo que as covariaveis mechanism-aware sao proxies LMS-based inspiradas pela logica de `Kay & Bostock (2023)`, com destaque para o eixo de participacao click-based quando isso for o que o pacote exportado efetivamente sustenta.
- Se o run exportado atual nao materializa propagacao ampla alem de `total_clicks`, o texto deve assumir essa limitacao de forma aberta, e nao vender um mecanismo mais rico do que a evidencia exportada mostra.
- Acrescentar referencia explicita aos artefatos `table_policy_spec.csv` e `table_policy_horizons_dual.csv` como parte do contrato reprodutivel.

Revisao de frases:
- A secao esta tecnicamente limpa, mas ainda “seca” demais no bloco de policy: faltam 1-2 frases de transicao explicando que o gatilho nao foi escolhido ad hoc e que sua ancora operacional vem de literatura externa mais policy contract interno.
- A frase sobre interpretacao estrutural esta boa, mas pode ser reforcada no fechamento da secao com formula mais curta e memoravel.

Revisao de conceitos:
- O conceito de `policy contract` precisa ficar mais concreto. Hoje ele aparece, mas sem dizer claramente do que ele e composto: trigger rule, effect schedule, window rule e horizon semantics.
- O texto precisa separar melhor “notacao matematica” de “nome canonico de export”. Hoje isso fica implicito; para reproducibilidade, precisa ficar explicito.
- O conceito de ancora bibliografica da policy ainda nao esta suficientemente internalizado na metodologia. Isso importa porque o paper quer defender que a policy foi parametrizada com referencia, nao inventada no corpo do notebook sem lastro.
- O conceito de `mechanism-aware` tambem precisa ficar mais disciplinado: ele deve ser descrito como propagacao via proxies de engajamento inspiradas por uma logica externa de reengajamento, nao como simulacao empiricamente validada do efeito causal do paper de `Kay & Bostock (2023)`.

Revisao de conclusoes:
- A conclusao metodologica deve permanecer modesta: esta secao define e delimita o framework. Ela nao deve insinuar que o setup default ja foi “validado” ou “justificado empiricamente” aqui.
- Nao convem fechar a secao com linguagem que soe como defesa de resultado. O fechamento deve ser sobre contrato metodologico e limite inferencial.

Justificativas a incluir:
- Justificar por que a ancora de `Kay & Bostock (2023)` e usada como referencia operacional do gatilho, dado que o OULAD nao traz log observacional de intervencao real.
- Justificar por que covariaveis como `total_clicks` e outras features de estado entram como proxies inspiradas no mecanismo de participacao/reengajamento do paper-fonte, sem alegar equivalencia literal entre as variaveis do artigo e as covariaveis do notebook.
- Justificar por que, se o pacote exportado atual sustenta principalmente o eixo click-based, o texto principal deve adotar essa leitura mais restrita do mecanismo.
- Justificar por que a notacao compacta (`W`, `delta_shock`) e mantida nas equacoes mesmo quando os nomes exportados sao mais longos.
- Justificar por que a policy spec completa precisa ser citada via `table_policy_spec.csv` e nao via a tabela compacta de parametros.

### 6) `experimental_design_body.tex`
Status: revisao obrigatoria.

Criticidade: alta.

Trechos a revisar:
- `experimental_design_body.tex:213`
- `experimental_design_body.tex:216`
- `experimental_design_body.tex:219`
- `experimental_design_body.tex:254`
- `experimental_design_body.tex:265`
- `experimental_design_body.tex:294`
- `experimental_design_body.tex:347`
- `experimental_design_body.tex:403`
- `experimental_design_body.tex:427`
- `experimental_design_body.tex:430`
- `experimental_design_body.tex:432`

Problemas identificados:
- A tabela de metricas ainda usa `T_eval` de forma generica nas linhas de IPCW Brier, IBS e C-index. Neste contexto, o nome correto e `T_eval_metrics`.
- A especificacao da policy ainda esta escrita como `W`/`delta_shock`/`schedule`, sem congelar a nomenclatura exportada (`W_weeks`, `alpha_week0`, `alpha_week1`, `decay_type`, `window_exclusive_upper`, `delta_shock_ablation`).
- O bloco `Outputs reported` esta semanticamente incorreto para RQ2: em `experimental_design_body.tex:294`, o texto diz `Delta S(T_eval_metrics)`, mas o snapshot travado define que o horizonte longo de policy para `Delta S` e `T_eval_policy = 38`.
- O bloco de rastreabilidade (`experimental_design_body.tex:403`) ainda lista a policy com nomes antigos e sem amarrar os artefatos corretos.
- O algoritmo executivo ainda usa `W=2` e depois reporta RQ3 em `T_eval=37`, sem explicitar `T_eval_metrics`.

Direcao da futura alteracao:
- Substituir `T_eval` por `T_eval_metrics` em todas as metricas IPCW e em RQ3.
- Em RQ2/policy trajectory, usar `T_eval_policy` quando o texto estiver falando de `Delta S` ou de horizonte longo de policy.
- Reescrever a tabela/descricao da policy para refletir o congelamento atual dos parametros e a ancora do gatilho.
- Acrescentar na parte de traceability que a ancora bibliografica vem de `table_policy_spec.csv`, e nao de `table_policy_scenario_params.csv`.

Revisao de frases:
- A secao tem varias frases corretas isoladamente, mas algumas ficam semanticamente erradas porque usam `T_eval` como se existisse um unico horizonte longo. Isso precisa ser corrigido antes de qualquer refinamento estilístico.
- A frase em `experimental_design_body.tex:294` e a mais critica do arquivo, porque amarra a leitura de RQ2 ao horizonte errado.
- A tabela de policy em `experimental_design_body.tex:259-270` comunica os numeros, mas nao comunica o status deles como “frozen scenario parameters” nem o vinculo com a ancora externa.

Revisao de conceitos:
- Esta secao precisa deixar cristalino que dual horizons nao sao um remendo tardio: sao uma consequencia do protocolo sob censura e da separacao entre leitura de trajectory e leitura de metricas IPCW.
- O conceito de “design” aqui deve ser procedimental. Sempre que aparecer linguagem mais interpretativa, ela precisa ser rebaixada ou deslocada para Results.
- O protocolo de policy e fairness precisa parecer pre-especificado. Para isso, faltam justificativas textuais breves para:
  - por que `g_min = 0.05` e o criterio operacional de estabilidade,
  - por que `T_eval_policy = 38` pode existir como raw support horizon mesmo quando nao e horizonte de metricas IPCW,
  - por que `gender` foi usado como demonstration variable nesta execucao.

Revisao de conclusoes:
- Design nao deve concluir que o benchmark “confirma” a narrativa principal. Ele deve apenas dizer qual criterio de leitura sera usado depois.
- Design tambem nao deve insinuar que a policy sera lida como ganho positivo em todos os horizontes; isso e materia de Results.

Justificativas a incluir:
- Em torno de `experimental_design_body.tex:178-183`, incluir a justificativa operacional de por que dois horizontes sao necessarios e por que eles tem papeis diferentes.
- Em torno de `experimental_design_body.tex:254-270`, incluir justificativa curta da policy spec: trigger ancorado externamente, effect schedule congelado para reprodutibilidade.
- Em torno de `experimental_design_body.tex:303-312`, incluir justificativa de por que `gender` entra como demonstracao atual de RQ3.
- Em torno de `experimental_design_body.tex:403`, incluir justificativa de rastreabilidade: qual artefato guarda ancora, qual artefato guarda parametros compactos, qual artefato guarda dual horizons.

Frases que convem enfraquecer ou reestruturar no futuro:
- Qualquer frase que fale de `T_eval` sem adjetivo.
- Qualquer frase que fale da policy como se fosse um unico bloco paramétrico sem distinguir trigger, effect e horizon semantics.
- Qualquer frase que trate benchmark como validacao confirmatoria, e nao como comparacao/familia de referencia.

### 7) `results_body.tex`
Status: revisao obrigatoria.

Criticidade: alta.

Trechos a revisar:
- `results_body.tex:29`
- `results_body.tex:43`
- `results_body.tex:67`
- `results_body.tex:70`
- `results_body.tex:86`
- `results_body.tex:118`
- `results_body.tex:151`
- `results_body.tex:152`
- `results_body.tex:153`
- `results_body.tex:159`
- `results_body.tex:177`
- `results_body.tex:195`
- `results_body.tex:199`
- `results_body.tex:201`
- `results_body.tex:222`
- `results_body.tex:225`
- `results_body.tex:230`
- `results_body.tex:237`
- `results_body.tex:257`
- `results_body.tex:264`
- `results_body.tex:265`
- `results_body.tex:266`
- `results_body.tex:301`
- `results_body.tex:307`
- `results_body.tex:329`
- `results_body.tex:342`
- `results_body.tex:347`
- `results_body.tex:361`

Problemas identificados:
- O arquivo usa `T_eval` genericamente em varios contextos que agora precisam ser separados:
  - policy trajectory / `Delta S`: deve usar `T_eval_policy`.
  - metricas IPCW e fairness: deve usar `T_eval_metrics`.
- Os valores centrais de RQ2 em `results_body.tex:70`, `results_body.tex:199` e `results_body.tex:201` estao desatualizados em relacao ao snapshot travado.
  - O texto atual usa `Delta S_shock(18) = 0.0274` e `Delta S_mech(18) = 0.000065`.
  - O snapshot sincronizado trava, em `table_policy_deltaS_at_horizons.csv`, os valores:
    - `deltaS_shock = 0.025960139487519962`
    - `deltaS_mech = -0.0006035298138894474`
- O arredondamento de AUC em `results_body.tex:67` e `results_body.tex:86` precisa ser uniformizado com o snapshot: `AUC_train = 0.8359`, `AUC_test = 0.8396`.
- A tabela de diagnostico de horizontes usa apenas `T_eval = 37` e depois menciona `G(38)` como instavel, mas nao materializa a semantica dual no proprio texto/tabela. O snapshot travado exige explicitar `T_eval_metrics = 37` e `T_eval_policy = 38`.
- A tabela resumida de RQ2 ainda usa `W` e `delta_shock` em vez da nomenclatura congelada da policy.
- O benchmark continua descrito de forma mais forte do que o estado atual dos resultados permite:
  - `results_body.tex:225` fala em `directional convergence` como leitura principal.
  - `results_body.tex:230` pede verificar se a conclusao central e estavel entre modelos.
  - `results_body.tex:361` diz que a interpretacao principal permanece `directionally coherent under benchmark and robustness axes`.
  - Isso precisa ser rebaixado para uma formulacao compativel com a dominancia atual do `non_temporal_rsf` em `AUC_event_by_T_*`.
- Em RQ3, o texto ainda deixa implicita a codificacao do grupo (`results_body.tex:307`) e usa `T_eval` em captions/tabelas de fairness (`results_body.tex:329`, `results_body.tex:342`). O snapshot travado pede explicitar que o horizonte longo correto de fairness e `T_eval_metrics = 37`, com grupo demonstrado `gender`.

Direcao da futura alteracao:
- Reescrever todas as referencias de horizonte com a semantica dual correta.
- Atualizar os numeros centrais de RQ2 e manter arredondamento consistente com o snapshot travado.
- Inserir explicitamente o contexto de `gender`, `group0 = M`, `group1 = F` e `treated_enrollments = 8387` na parte de RQ3.
- Reescrever o benchmark para evitar qualquer leitura de superioridade global do temporal.

Revisao de frases:
- `results_body.tex:70` e o principal ponto frasalmente critico do arquivo: a frase esta assertiva, numericamente desatualizada e conceitualmente incompleta porque nao explicita que o sinal do mechanism-aware no snapshot atual e ligeiramente negativo em `T_policy`.
- `results_body.tex:159` precisa de reescrita porque “The distinction between T_policy and T_eval” ficou conceitualmente ultrapassada; agora o texto precisa nomear `T_eval_metrics` e `T_eval_policy`.
- `results_body.tex:225`, `results_body.tex:230` e `results_body.tex:361` pedem rebaixamento de tom. O problema aqui nao e apenas numero; e framing.
- `results_body.tex:307` esta vaga demais para um resultado de fairness: o leitor fica sem saber qual codificacao do grupo esta em jogo.

Revisao de conceitos:
- RQ2 hoje precisa ser lido como “regime dependence under explicit scenario assumptions”, nao como “policy benefit established”. O texto atual chega perto dessa leitura, mas ainda usa formula de resposta positiva forte demais.
- O bloco de benchmark precisa migrar de “convergencia de conclusao” para “comparacao honesta entre familias com contribuicoes diferentes”.
- O bloco de RQ3 precisa diferenciar melhor tres coisas:
  - sinal estatisticamente robusto,
  - magnitude substantivamente pequena,
  - interpretacao nao normativa de fairness.
- O bloco de horizons under censoring precisa deixar claro que existem dois horizontes longos com papeis distintos. Sem isso, Results parece contradizer Design/metadata.

Revisao de conclusoes:
- A conclusao de RQ2 precisa ser revista para algo como: o framework suporta contraste estrutural auditavel, mas a magnitude e o sinal dependem fortemente do regime parametrizado e do mecanismo de propagacao.
- A conclusao de benchmark nao deve ser “directionally coherent” de forma solta, porque isso soa mais forte do que os resultados sustentam hoje.
- A conclusao de RQ3 precisa incluir a advertencia de que o sinal e robusto, mas a magnitude absoluta e muito pequena. Sem isso, o paper pode parecer inflar relevancia pratica.
- O fechamento geral de Results deve dizer explicitamente o que ficou demonstrado apesar de o temporal nao liderar os benchmarks de `AUC_event_by_T_*`.

Justificativas a incluir:
- Em `results_body.tex:67-86`, explicar por que a combinacao AUC/Brier/ECE e suficiente para sustentar o uso do hazard como backbone de simulacao.
- Em `results_body.tex:145-159`, justificar por que `G(38)` inviabiliza metricas IPCW, mas nao impede o uso de `T_eval_policy = 38` como raw support horizon para trajectory reporting.
- Em `results_body.tex:188-201`, justificar por que o mechanism-aware pequeno ou negativo nao invalida o framework; ele informa sobre o regime e sua propagacao no setup atual.
- Em `results_body.tex:222-230`, justificar explicitamente a contribuicao do framework temporal mesmo diante da dominancia do `non_temporal_rsf` em `AUC_event_by_T_*`.
- Em `results_body.tex:301-347`, justificar que a leitura de RQ3 e sobre change-in-gap sob simulacao estrutural, nao sobre inequidade causal identificada ou recomendacao operacional pronta.

Frases que convem substituir por formulacoes mais defensaveis:
- Em vez de “Yes, with clear regime dependence”, usar algo que acomode tambem o sinal observado do mechanism-aware no snapshot atual.
- Em vez de “directionally coherent under benchmark and robustness axes”, usar algo como “the central temporal framework remains informative for auditable regime analysis, while benchmark results show that discrimination leadership is not universal across model families”.
- Em vez de “Negative values indicate reduced gap under policy relative to baseline for the group coding used in this run”, explicitar a codificacao de grupo e lembrar que a leitura e puramente estrutural/model-implied.

Pontos onde vale inserir mini-justificativas argumentativas no texto final:
- logo apos a tabela de horizontes,
- no primeiro paragrafo de benchmark,
- no fechamento de RQ2,
- no fechamento de RQ3,
- no paragrafo final de synthesis.

### 8) `appendix_a_body.tex`
Status: revisao obrigatoria.

Criticidade: baixa.

Trecho a revisar:
- `appendix_a_body.tex:70`

Problema:
- A legenda ainda usa `Delta Gap(T_eval)`. No estado atual do plano sincronizado, o horizonte longo de fairness deve ser referido como `T_eval_metrics`.

Direcao da futura alteracao:
- Atualizar a legenda para `Delta Gap(T_eval_metrics)`.
- Se o nome da figura permanecer legado (`...Teval...`), o texto deve desambiguar explicitamente que se trata do horizonte de fairness/metricas.

Revisao de frases:
- O apendice esta bem orientado, mas a legenda de `appendix_a_body.tex:70` precisa acompanhar a semantica nova dos horizontes para nao reintroduzir ambiguidade depois de corrigir o corpo principal.

Revisao de conceitos:
- Como o apendice serve de auditoria, ele nao pode carregar nomes legados sem explicacao. Isso e especialmente importante aqui porque o leitor mais tecnico pode usar o apendice para questionar a consistencia do paper.

Revisao de conclusoes:
- Nenhum problema forte de conclusao foi encontrado. O apendice esta prudente.

Justificativas a incluir:
- Basta uma justificativa curta na legenda ou na frase adjacente explicando que o nome do arquivo preserva `Teval`, mas a leitura correta no manuscrito e `T_eval_metrics`.

### 9) `appendix_b_body.tex`
Status: revisao obrigatoria.

Criticidade: media.

Trechos a revisar:
- `appendix_b_body.tex:24`
- `appendix_b_body.tex:25`

Problemas identificados:
- O apendice de reproducibilidade ainda lista os parametros com nomenclatura antiga (`W=2`, `delta_shock=0.2`).
- Falta amarrar os artefatos reprodutiveis que o plano travou como obrigatorios para escrita/auditoria:
  - `table_policy_spec.csv`
  - `table_policy_scenario_params.csv`
  - `table_policy_horizons_dual.csv`
  - `table_feature_set_provenance.csv`

Direcao da futura alteracao:
- Atualizar a descricao dos parametros para os nomes canonicamente exportados.
- Acrescentar os artefatos de reproducibilidade que carregam ancora, parametros compactos e dual horizons.

Revisao de frases:
- A secao esta funcional, mas ainda genérica demais para o nivel de auditabilidade que o resto do paper pretende defender.
- A frase atual sobre reproducibilidade fica melhor se explicitar que o pacote exportado nao guarda apenas figuras e tabelas, mas tambem policy contract, horizon semantics e feature provenance.

Revisao de conceitos:
- Reproducibilidade, neste paper, nao e so rerodar notebook. E reconstruir o mesmo evidence package com mesma semantica de policy e horizontes.
- Esse conceito ainda nao esta totalmente visivel no apendice B.

Revisao de conclusoes:
- A conclusao do apendice B deve implicar “replication of the evidence package under the same protocol”, nao apenas “same code + same inputs”.

Justificativas a incluir:
- Justificar por que o manuscrito faz questao de listar artefatos como `table_policy_spec.csv`, `table_policy_horizons_dual.csv` e `table_feature_set_provenance.csv`: eles sao parte do contrato de auditabilidade, nao anexos opcionais.
- Justificar que a presenca de nomes canonicos exportados e importante porque o manuscrito precisa bater com o pacote reprodutivel final.

### 10) `ref.bib`
Status: revisao obrigatoria.

Criticidade: media.

Problemas identificados:
- O arquivo ja contem varias referencias relevantes, mas a revisao atual ainda nao formaliza uma auditoria completa de integridade bibliografica.
- A entrada `KayBostock2023PowerNudge` existe e parece correta, mas o manuscrito ainda nao a usa para ancorar nem o gatilho nem a inspiracao qualitativa do impacto em covariaveis de engajamento.
- Ainda faltam checagens sistematicas de consistencia entre: referencias presentes em `ref.bib`, referencias efetivamente citadas nos `.tex`, e referencias metodologicas que continuam faltando no corpus.
- A narrativa metodologica continua sem ancora explicita para alguns blocos importantes ja mapeados nesta revisao: `Platt1999ProbabilisticOutputs`, literatura de leakage/validation, fundacao de hazard discreto e referencia metodologica de `Random Survival Forest`.

Direcao da futura alteracao:
- Fazer uma revisao total de `ref.bib`, cobrindo integridade dos metadados, chaves BibTeX, consistencia de autores/titulo/ano/DOI/URL e aderencia ao que o manuscrito realmente cita.
- Verificar se todas as referencias citadas nos `.tex` existem em `ref.bib` e se todas as referencias mantidas em `ref.bib` continuam justificadas pelo escopo do paper.
- Conferir explicitamente a ancoragem bibliografica da policy em `KayBostock2023PowerNudge`, inclusive para a parte de logica de participacao/reengajamento inspiradora das covariaveis mechanism-aware.
- Incluir a referencia metodologica de `Random Survival Forest` se esse baseline permanecer relevante na narrativa final.

Revisao de conceitos:
- A revisao bibliografica final nao deve ser tratada como limpeza editorial secundaria; ela faz parte do contrato de auditabilidade do paper.
- O manuscrito precisa distinguir com clareza entre referencias que ancoram protocolo/metodo, referencias que ancoram inspiracao substantiva e referencias que apenas contextualizam o problema.

Revisao de conclusoes:
- O paper nao deve deixar implito que certos componentes estao “ancorados na literatura” se a citacao correspondente ainda nao aparece no texto.
- Tambem nao convem manter referencias no `ref.bib` apenas por acumulacao historica se elas nao tiverem funcao defensavel no argumento final.

Justificativas a incluir:
- Justificar por que a revisao total do `ref.bib` e necessaria nesta etapa: o risco agora nao e apenas de estilo, mas de desalinhamento entre claims metodologicos e ancoragem bibliografica real.
- Justificar por que a entrada de `KayBostock2023PowerNudge` precisa sustentar tanto o trigger quanto a inspiracao qualitativa das covariaveis de engajamento, sem inflar o grau de equivalencia empirica entre estudo-fonte e simulacao.

## Revisao transversal de frases, conceitos e conclusoes

### Frases que hoje soam fortes demais

- Em Results, qualquer formula que sugira confirmacao ampla do benchmark temporal.
- Em RQ2, qualquer frase que trate o contraste como ganho estabelecido, em vez de contraste estrutural dependente de regime.
- Em RQ3, qualquer frase que possa ser lida como afirmacao normativa de fairness, e nao como mudanca de gap sob simulacao.

### Conceitos que precisam ficar mais claros no manuscrito inteiro

1. `Dual horizons`:
- nao e apenas um detalhe tecnico; e uma regra de leitura do paper.

2. `Policy contract`:
- precisa aparecer como conjunto estruturado de trigger + effect schedule + horizon semantics + provenance.

3. `Benchmark contribution`:
- o paper nao vende “melhor classificador universal”; vende framework temporal auditavel para comparacao estrutural de regimes.

4. `RQ3 fairness framing`:
- o paper demonstra uma capacidade analitica em subgrupo observado, nao uma teoria completa de fairness operacional.

### Conclusoes que devem ser reescritas com mais cuidado

1. Conclusao de RQ2:
- hoje precisa ser reformulada para aceitar explicitamente que o mechanism-aware atual nao entrega ganho positivo relevante no setup default.

2. Conclusao de benchmark:
- precisa reconhecer a dominancia do `non_temporal_rsf` em `AUC_event_by_T_*` sem desmontar a contribuicao metodologica do trabalho.

3. Conclusao de RQ3:
- precisa combinar robustez de sinal com prudencia sobre a magnitude absoluta observada.

### Lugares onde o manuscrito precisa de justificativas curtas e explicitas

1. Methodology:
- por que a ancora de policy vem de `Kay & Bostock (2023)`.
- por que as covariaveis mechanism-aware devem ser descritas como proxies de participacao/reengajamento inspiradas em `Kay & Bostock (2023)`, com linguagem compativel com o evidence package exportado.

2. Experimental Design:
- por que existem dois horizontes longos.
- por que `g_min = 0.05` e o ponto operacional de estabilidade.
- por que `gender` e o demonstration case atual.

3. Results:
- por que a qualidade de hazard sustenta policy/fairness downstream.
- por que `T_eval_policy = 38` pode aparecer para trajectory report, mas nao para IPCW metrics.
- por que benchmark ainda importa mesmo sem lideranca universal do temporal.
- por que um `Delta Gap` pequeno, mas com CI fora de zero, deve ser lido com prudencia.

4. Bibliography:
- por que o paper precisa de uma revisao total de `ref.bib` antes do fechamento final.
- por que referencias-chave presentes no projeto, mas ainda nao citadas, nao podem continuar apenas como potencial de uso futuro.

## Sugestao de uso deste relatorio na proxima etapa

Quando comecarmos a editar os `.tex`, a ordem mais eficiente nao e apenas por arquivo, mas por nivel de risco narrativo:

1. Corrigir primeiro a semantica de horizontes em `results_body.tex` e `experimental_design_body.tex`.
2. Corrigir em seguida o framing de benchmark e de RQ2 em `results_body.tex`.
3. Depois alinhar policy contract e ancora em `methodology_body.tex` e `appendix_b_body.tex`.
4. Por fim, fazer o polimento de consistencia de subgrupo entre `introduction_body.tex`, `results_body.tex` e `appendix_a_body.tex`.

## Auditoria especifica das referencias que ancoram parametros e escolhas de modelo

Objetivo desta auditoria:
- verificar se os parametros e escolhas metodologicas centrais estao de fato ancorados por referencias adequadas no manuscrito;
- separar o que ja esta corretamente referenciado do que hoje depende apenas de descricao implementacional;
- indicar onde a ancoragem deve ser bibliografica e onde basta justificativa textual/protocolo.

### A) O que esta ok hoje

1. Dataset base
- `Kuzilek2017OULAD` esta presente em `ref.bib` e e citado corretamente no manuscrito.
- Uso atual adequado em:
  - `experimental_design_body.tex:41`
  - `introduction_body.tex:16`
  - `methodology_body.tex:312`

2. Benchmark Cox
- `Cox1972RegressionModels` esta presente em `ref.bib` e e citado corretamente para ancorar a familia de modelo Cox.
- Uso atual adequado em:
  - `experimental_design_body.tex:136`
  - `methodology_body.tex:285`

3. IPCW Brier sob censura
- `Graf1999AssessmentSurvivalPrediction` esta presente em `ref.bib` e e usado corretamente para introduzir o Brier sob dados de sobrevivencia/censura.
- Uso atual adequado em:
  - `experimental_design_body.tex:235`

Leitura:
- Essa ancoragem e aceitavel para o texto atual.
- Se a versao final quiser ficar metodologicamente mais precisa no tratamento do Brier esperado sob censura, `Gerds2006ConsistentBrier` poderia complementar `Graf1999`, mas isso e refinamento, nao bloqueio.

### B) O que existe em `ref.bib`, mas ainda nao esta sendo usado no `.tex`

1. Policy anchor reference
- `KayBostock2023PowerNudge` existe em `ref.bib`, mas nao aparece citado em nenhum `.tex`.

Implicacao:
- Hoje o manuscrito declara os parametros da policy (`r^*`, `W`, schedule, regime shock/mechanism-aware), mas nao ancora explicitamente no texto a referencia externa que deveria sustentar o gatilho e sua inspiracao operacional.

Conclusao:
- Este e o principal gap de ancoragem bibliografica no estado atual.
- A referencia esta correta e disponivel, mas ainda nao esta cumprindo seu papel no manuscrito.

Onde deve entrar depois:
- `methodology_body.tex`, na subsecao de policy.
- `experimental_design_body.tex`, no bloco `Scenario specification`.
- Opcionalmente `appendix_b_body.tex`, no bloco de reproducibilidade/artefatos.

2. Platt scaling / sigmoid calibration
- `Platt1999ProbabilisticOutputs` existe em `ref.bib`, mas nao esta citado em nenhum `.tex`.

Implicacao:
- O texto menciona repetidamente `sigmoid calibration` / `Platt scaling`, por exemplo em:
  - `experimental_design_body.tex:146`
  - `experimental_design_body.tex:190`
  - `experimental_design_body.tex:347`
- Hoje isso esta metodologicamente descrito, mas nao referenciado.

Conclusao:
- Nao e um erro grave, mas vale citar `Platt1999ProbabilisticOutputs` quando a calibracao for apresentada como parte do protocolo do modelo principal.

3. Leakage / split rationale / structured CV
- Existem em `ref.bib`:
  - `KapoorNarayanan2023LeakagePatterns`
  - `RobertsEtAl2017CrossValidationEcography`
  - `BergmeirBenitez2012TimeSeriesCV`
  - `CerqueiraTorgoMozetic2020TSPerfEstimation`
- Nenhuma delas esta citada no `.tex` atual.

Implicacao:
- O manuscrito fala de leakage prevention, GroupKFold e split temporal estratificado, mas faz isso hoje apenas como protocolo interno, sem ancora metodologica externa.

Conclusao:
- Se a meta for deixar o argumento de leakage/split mais defensavel, vale usar ao menos uma referencia de leakage e uma de validacao estruturada.
- Isso e especialmente util em:
  - `methodology_body.tex:169-185`
  - `experimental_design_body.tex:75-113`

4. Fundacao metodologica de hazard discreto
- Existem em `ref.bib`:
  - `SingerWillett1993DiscreteTimeSurvival`
  - `Allison1982DiscreteTimeMethods`
  - `SureshSevernGhosh2022DiscreteTimeIntro`
  - `KvammeBorgan2019ContinuousDiscreteSurvivalNN`
- Nenhuma esta citada no `.tex` atual.

Implicacao:
- O paper descreve corretamente o discrete-time hazard framework, mas sem citar uma referencia fundacional/metodologica para essa familia de modelos.

Conclusao:
- Isso nao invalida o texto, mas enfraquece a ancoragem academica da secao Methodology.
- Recomenda-se citar pelo menos uma referencia classica e, se desejado, uma referencia mais recente de sintese.

### C) O que hoje NAO precisa necessariamente de referencia externa, mas precisa de justificativa textual clara

Estes pontos parecem mais `design choices` ou `implementation choices` do que parametros que precisem de lastro bibliografico forte:

1. Hiperparametros de implementacao do modelo principal
- `solver=liblinear`
- `max_iter=4000`
- `class_weight=balanced`
- `random_state=42`

Leitura:
- Nao precisam necessariamente de citacao externa.
- Precisam ser apresentados como escolhas de implementacao reprodutiveis, nao como valores “derivados da literatura”.

2. Hiperparametros de benchmark / treino
- `penalizer=0.01` no Cox
- `max_depth=6`, `learning_rate=0.05`, `max_iter=300` no HistGradientBoosting

Leitura:
- Tambem nao exigem citacao bibliografica por si so.
- O manuscrito deve trata-los como configuracoes fixadas do experimento.

3. Parametros operacionais do protocolo sob censura
- `g_min = 0.05`
- truncamento/cap de peso em `20`
- `q = 4` buckets no split temporal

Leitura:
- Esses pontos pedem justificativa metodologica curta no texto.
- Eles nao precisam, necessariamente, de uma referencia unica “autorizando” o valor numerico, mas precisam de racional explicito.

Conclusao:
- O erro aqui nao e “falta de referencia”, e sim “falta de justificativa local no manuscrito”.

### D) Ponto intermediario: deveria haver referencia metodologica, mas hoje nao ha

1. Random Survival Forest
- O manuscrito descreve o baseline como `Random Survival Forest from the scikit-survival package` em `experimental_design_body.tex:155`.

Problema:
- Ha ancora de pacote/ferramenta, mas nao ha ancora metodologica da familia RSF no `ref.bib` nem no `.tex`.

Leitura:
- Se o baseline nao-temporal continuar central na narrativa de benchmark, vale adicionar uma referencia metodologica de RSF na etapa de revisao.

2. GroupKFold como mecanismo de calibracao/validacao agrupada
- O texto cita `GroupKFold` como procedimento, mas sem ancora bibliografica.

Leitura:
- Nao e obrigatorio citar a API, mas vale citar literatura de validacao estruturada/leakage prevention para sustentar o racional, ja que esse passo e importante na narrativa do paper.

### E) Parecer final desta checagem

#### Referencias que estao corretas e adequadas
- `Kuzilek2017OULAD`
- `Cox1972RegressionModels`
- `Graf1999AssessmentSurvivalPrediction`

#### Referencias importantes que JA existem no projeto, mas ainda nao estao cumprindo o papel de ancoragem no manuscrito
- `KayBostock2023PowerNudge`
- `Platt1999ProbabilisticOutputs`
- `KapoorNarayanan2023LeakagePatterns`
- `RobertsEtAl2017CrossValidationEcography`
- `SingerWillett1993DiscreteTimeSurvival` ou `Allison1982DiscreteTimeMethods`

#### Referencias / ancoragens que faltam conceitualmente
- uma referencia metodologica para Random Survival Forest, se esse baseline permanecer central.

#### Julgamento pratico
- A ancoragem bibliografica dos parametros da policy ainda NAO esta ok no manuscrito atual, porque a referencia-chave `KayBostock2023PowerNudge` existe, mas nao esta sendo usada.
- A ancoragem bibliografica da calibracao (`Platt`) e da logica de leakage/split tambem esta incompleta.
- Ja os hiperparametros numericos de treino e configuracao parecem aceitaveis como escolhas de implementacao, desde que o texto nao tente vendê-los como parametros “referenciados na literatura”.

## Ordem sugerida de revisao futura

1. `results_body.tex`
2. `experimental_design_body.tex`
3. `methodology_body.tex`
4. `ref.bib`
5. `introduction_body.tex`
6. `appendix_b_body.tex`
7. `appendix_a_body.tex`

## Arquivos que podem ficar como estao, salvo ajuste editorial fino

- `TCM-Student-Dropout.tex`
- `abstract_body.tex`
- `background_body.tex`

## Fechamento

Conclusao desta etapa:
- A revisao cruzada foi feita sem alterar os `.tex`.
- O principal bloco de retrabalho esta concentrado em `results_body.tex` e `experimental_design_body.tex`.
- O problema dominante nao e de estrutura LaTeX; e de sincronizacao semantica com policy params congelados, dual horizons e leitura honesta do benchmark.