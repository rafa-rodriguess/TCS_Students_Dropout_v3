# Parametros Principais da Simulacao Contrafactual

## Objetivo

Este arquivo consolida os principais parametros atualmente usados na simulacao contrafactual de intervencao contra dropout e associa, para cada parametro, referencias candidatas que podem servir como ancora empirica ou como faixa plausivel de calibracao.

O foco aqui nao e listar todos os hiperparametros do pipeline, mas apenas os parametros centrais da simulacao de intervencao em P12-P14.

## Fonte dos valores atuais

Os valores abaixo refletem o cenario principal atualmente congelado nos artefatos exportados:

- `outputs_v2/tables/table_policy_spec.csv`
- `outputs_v2/tables/table_policy_activation_summary.csv`
- `outputs_v2/tables/table_feature_set_provenance.csv`

Importante:

- O cenario principal usado na simulacao nao e o mesmo que o `grid_top1` da sensibilidade.
- O `grid_top1` aparece apenas como melhor ponto do grid de RQ2, nao como contrato principal da politica.

## Tabela-sintese

| Parametro | Valor atual | Modelo / bloco | Significado operacional | Status de ancoragem | Referencias candidatas | Observacao tecnica |
|---|---:|---|---|---|---|---|
| `r_star` | `1` | Trigger comum aos dois regimes | A intervencao dispara quando `recency >= 1`, isto e, cerca de 7 dias sem engajamento no LMS | Ancora indireta plausivel | Foster & Siddle (2019); Adnan et al. (2021); Mubarak et al. (2020) | No notebook, a regra exportada e `flag if no LMS engagement in last 7 days` |
| `W_weeks` | `2` | Shock-only e mechanism-aware | Janela ativa da intervencao apos o trigger | Calibrado por faixa plausivel | Kizilcec et al. (2020); Rodriguez, Guerrero-Roldan, & Baneres (2022); Oliveira et al. (2021) | No cenario principal, a janela ativa dura 2 semanas |
| `alpha_week0` | `0.35` | Mechanism-aware | Aumento imediato de engajamento na semana 0 da intervencao | Calibrado por faixa plausivel | Kizilcec et al. (2020); Rodriguez et al. (2022); Sonderlund, Hughes, & Smith (2018) | No codigo, isso entra como `boost_factor = 1 + alpha_week0`, isto e, cerca de +35% em `total_clicks` nas linhas ativas |
| `alpha_week1` | `0.10` | Mechanism-aware | Efeito residual de engajamento na semana 1 apos o trigger | Calibrado por faixa plausivel | Kizilcec et al. (2020); Rodriguez et al. (2022) | No codigo, isso entra como `boost_factor = 1 + alpha_week1`, isto e, cerca de +10% em `total_clicks` na segunda semana ativa |
| `decay_type` | `kb2023_step_2w` | Mechanism-aware | Efeito em degrau: mais forte na semana 0, menor na semana 1, sem efeito constante longo | Ancora indireta plausivel | Kizilcec et al. (2020); Rodriguez et al. (2022) | O notebook implementa um decaimento discreto de 2 semanas, nao uma funcao continua |
| `delta_shock_ablation` | `0.20` | Shock-only ablation | Reducao proporcional direta do hazard semanal nas linhas ativas | Calibrado por faixa plausivel | Sonderlund et al. (2018); Oliveira et al. (2021); Foster & Siddle (2019) | No codigo, entra como `hazard_1_shock = hazard_0 * (1 - 0.20)` |
| `T_policy` | `18` | Horizonte do contraste | Horizonte principal em que o ganho da politica e reportado | Parametro de avaliacao, nao de literatura de intervencao | Nao aplicavel como ancora de efeito | E um horizonte operacional do estudo, nao um efeito comportamental |

## Leitura por modelo

### Shock-only

Parametros principais:

- `r_star = 1`
- `W_weeks = 2`
- `delta_shock_ablation = 0.20`
- `T_policy = 18`

Interpretacao:

- A politica e ativada quando o aluno entra em estado de inatividade curta.
- Durante a janela ativa de 2 semanas, o hazard e reduzido diretamente em 20% nas linhas afetadas.
- Esse regime funciona como ablation simples: ele nao reescreve a dinamica comportamental, apenas aplica um choque exogeno no risco.

### Mechanism-aware

Parametros principais:

- `r_star = 1`
- `W_weeks = 2`
- `alpha_week0 = 0.35`
- `alpha_week1 = 0.10`
- `decay_type = kb2023_step_2w`
- `T_policy = 18`

Interpretacao:

- A politica e ativada pelo mesmo trigger de inatividade.
- O efeito e modelado como aumento temporario de engajamento, principalmente em `total_clicks`.
- Na semana 0, o aumento e de aproximadamente 35% nas linhas ativas.
- Na semana 1, o efeito residual cai para cerca de 10%.
- O hazard contrafactual nao e ajustado manualmente; ele e recalculado a partir das covariaveis alteradas.

## Distincao critica: cenario principal versus grid top-1

O notebook contem tambem um resultado de sensibilidade cujo melhor ponto e:

- `grid_top1_r_star = 2`
- `grid_top1_W = 3`
- `grid_top1_decay_type = flat`
- `grid_top1_alpha_week0 = 0.50`
- `grid_top1_alpha_week1 = 0.15`

Esse bloco nao deve ser confundido com o cenario principal atualmente usado na simulacao. O cenario principal congelado nos artefatos e:

- `r_star = 1`
- `W_weeks = 2`
- `alpha_week0 = 0.35`
- `alpha_week1 = 0.10`
- `decay_type = kb2023_step_2w`
- `delta_shock_ablation = 0.20`

## Quais estudos baixar primeiro

### Prioridade 1: mais uteis para conferir os valores centrais

1. **Kizilcec et al. (2020)**
   - Melhor candidato para sustentar `W_weeks = 2`, `alpha_week0`, `alpha_week1` e `decay_type`.
   - E o estudo mais importante para checar se a narrativa de "efeito forte na primeira semana e menor na segunda" se sustenta bem.

2. **Rodriguez, Guerrero-Roldan, & Baneres (2022)**
   - Bom complemento para efeito de nudge personalizado e persistencia curta do efeito.
   - Especialmente util para reforcar `W_weeks = 2` e o padrao de decaimento.

3. **Foster & Siddle (2019)**
   - Principal candidato para discutir trigger baseado em no-engagement / inatividade operacional.
   - Deve ser baixado para verificar quao proximo o estudo chega de um limiar de 7 dias.

### Prioridade 2: consolidacao de faixa plausivel

4. **Sonderlund, Hughes, & Smith (2018)**
   - Relevante para discutir a faixa plausivel de impacto de intervencoes de learning analytics.
   - Bom candidato para dar contexto ao `delta_shock_ablation = 0.20`.

5. **Oliveira, Sobral, Ferreira, & Moreira (2021)**
   - Revisao sistematica util para enquadrar a simulacao como coerente com a literatura de prevencao de dropout via learning analytics.
   - Ajuda principalmente na justificativa geral da faixa de impacto, nao no valor pontual.

6. **Adnan et al. (2021)**
   - Util para reforcar granularidade semanal de monitoramento e intervencao.
   - Mais util para o trigger e para a defesa da unidade temporal que para os efeitos em si.

### Prioridade 3: apoio contextual adicional

7. **Mubarak, Cao, & Zhang (2020)**
   - Complementa a defesa da granularidade semanal e da deteccao precoce.
   - Deve ser usado como apoio conceitual, nao como ancora quantitativa do valor.

## Recomendacao por parametro

### `r_star = 1`

Melhor estrategia de escrita:

- tratar como parametro operacional calibrado a partir da literatura de early warning baseada em ciclos curtos de inatividade;
- nao afirmar que 7 dias e um limiar universalmente estimado;
- usar Foster & Siddle (2019) como principal candidato de verificacao.

### `W_weeks = 2`

Melhor estrategia de escrita:

- tratar como janela curta de efeito, coerente com evidencias de impacto concentrado nas primeiras 1-2 semanas;
- principal estudo para conferir: Kizilcec et al. (2020);
- Rodriguez et al. (2022) funciona como reforco.

### `alpha_week0 = 0.35` e `alpha_week1 = 0.10`

Melhor estrategia de escrita:

- tratar como parametros de cenario plausivel calibrados em faixa empirica, nao como constantes identificadas diretamente;
- principal estudo para conferir: Kizilcec et al. (2020);
- estudo complementar: Rodriguez et al. (2022).

### `decay_type = kb2023_step_2w`

Melhor estrategia de escrita:

- descrever como simplificacao estrutural baseada em evidencia de pico inicial e atenuacao rapida;
- principal estudo para conferir: Kizilcec et al. (2020);
- complementar com Rodriguez et al. (2022).

### `delta_shock_ablation = 0.20`

Melhor estrategia de escrita:

- tratar como cenario plausivel alinhado a faixas de impacto de intervencoes de retencao e learning analytics;
- nao apresentar como efeito causal identificado no seu dado;
- principais estudos para conferir: Sonderlund et al. (2018) e Oliveira et al. (2021).

## Cenarios alternativos de simulacao

Esta secao propoe cenarios alternativos que podem ser usados na simulacao contrafactual sem sair das faixas plausiveis sugeridas pela literatura revisada acima.

Principio geral:

- os cenarios abaixo nao devem ser apresentados como "valores verdadeiros";
- eles funcionam como especificacoes alternativas, academicamente defensaveis, para analise de robustez;
- a melhor leitura e: cenario conservador, cenario central e cenario intensificado.

## Tabela de cenarios alternativos

| Cenario | `r_star` | `W_weeks` | `alpha_week0` | `alpha_week1` | `decay_type` | `delta_shock_ablation` | Leitura substantiva | Estudos mais uteis |
|---|---:|---:|---:|---:|---|---:|---|---|
| Conservador | `1` | `1` | `0.20` | `0.00` | `step_1w` | `0.10` | Nudge curto, efeito pequeno e concentrado apenas na semana imediata | Kizilcec et al. (2020); Sonderlund et al. (2018); Oliveira et al. (2021) |
| Central atual | `1` | `2` | `0.35` | `0.10` | `kb2023_step_2w` | `0.20` | Nudge com pico imediato e residual curto na semana seguinte | Kizilcec et al. (2020); Rodriguez et al. (2022); Foster & Siddle (2019) |
| Intensificado plausivel | `1` | `2` | `0.40` | `0.15` | `step_2w` | `0.25` | Nudge forte, mas ainda dentro de faixa plausivel de efeito curto e moderado | Kizilcec et al. (2020); Rodriguez et al. (2022); Sonderlund et al. (2018) |
| Janela prolongada | `1` | `3` | `0.35` | `0.10` | `step_3w` | `0.20` | Mesmo pico inicial, mas assumindo persistencia operacional um pouco maior | Rodriguez et al. (2022); Oliveira et al. (2021) |
| Trigger mais estrito | `2` | `2` | `0.35` | `0.10` | `kb2023_step_2w` | `0.20` | Intervencao so com inatividade mais persistente, reduzindo cobertura e aumentando seletividade | Foster & Siddle (2019); Adnan et al. (2021); Mubarak et al. (2020) |

## Interpretacao e uso recomendado

### 1. Cenario conservador

Valores:

- `r_star = 1`
- `W_weeks = 1`
- `alpha_week0 = 0.20`
- `alpha_week1 = 0.00`
- `delta_shock_ablation = 0.10`

Quando usar:

- se voce quiser mostrar que o ganho da politica persiste mesmo sob uma hipotese modesta de efeito;
- se a banca ou o paper exigir parametros mais prudentes;
- se os estudos baixados mostrarem efeitos pequenos e muito curtos.

Justificativa:

- revisoes como Sonderlund et al. (2018) e Oliveira et al. (2021) sustentam efeitos positivos, mas heterogeneos e frequentemente modestos;
- esse cenario minimiza o risco de superestimar impacto.

### 2. Cenario central atual

Valores:

- `r_star = 1`
- `W_weeks = 2`
- `alpha_week0 = 0.35`
- `alpha_week1 = 0.10`
- `decay_type = kb2023_step_2w`
- `delta_shock_ablation = 0.20`

Quando usar:

- como cenario principal do artigo, se os estudos centrais confirmarem uma dinamica curta, com pico imediato e decaimento rapido.

Justificativa:

- e o melhor compromisso atual entre efeito de curto prazo, decaimento rapido e impacto moderado sobre risco.

### 3. Cenario intensificado plausivel

Valores:

- `r_star = 1`
- `W_weeks = 2`
- `alpha_week0 = 0.40`
- `alpha_week1 = 0.15`
- `delta_shock_ablation = 0.25`

Quando usar:

- como teste de sensibilidade superior;
- para verificar se conclusoes de fairness ou ganho de politica mudam sob uma intervencao mais forte;
- se a literatura encontrada reportar efeitos mais altos em programas de outreach mais ativos.

Justificativa:

- permanece dentro de uma faixa ainda crivel para intervencoes bem implementadas, sem migrar para o extremo do `grid_top1`.

### 4. Cenario de janela prolongada

Valores:

- `r_star = 1`
- `W_weeks = 3`
- `alpha_week0 = 0.35`
- `alpha_week1 = 0.10`
- semana adicional residual implicita de baixa persistencia
- `delta_shock_ablation = 0.20`

Quando usar:

- se voce quiser testar se a duracao do efeito importa mais que a intensidade inicial;
- se os estudos baixados sugerirem que parte dos nudges produz efeito perceptivel por ate 3 semanas.

Justificativa:

- a literatura aponta efeitos de curta duracao, mas algumas intervencoes personalizadas podem sustentar resposta um pouco alem de 2 semanas.

### 5. Cenario com trigger mais estrito

Valores:

- `r_star = 2`
- `W_weeks = 2`
- `alpha_week0 = 0.35`
- `alpha_week1 = 0.10`
- `delta_shock_ablation = 0.20`

Quando usar:

- se voce quiser comparar cobertura versus precisao operacional;
- se a narrativa do artigo quiser diferenciar intervencoes precoces e intervencoes mais seletivas;
- se a literatura de early warning parecer mais confortavel com janelas de 2 semanas do que com 7 dias.

Justificativa:

- Foster & Siddle (2019) e a literatura correlata tendem a apoiar mais confortavelmente janelas de 1-2 semanas do que exatamente 7 dias;
- esse cenario ajuda a mostrar robustez a uma especificacao de trigger menos agressiva.

## Cenarios recomendados para rodar primeiro

Se a ideia for manter a analise enxuta, os tres cenarios mais uteis seriam:

1. **Central atual**
   - serve como cenario principal.

2. **Conservador**
   - testa se a conclusao principal persiste sob efeito menor.

3. **Trigger mais estrito**
   - testa se a conclusao depende de intervir cedo demais.

Essa combinacao e boa porque varia tres dimensoes substantivas diferentes:

- magnitude do efeito;
- duracao do efeito;
- agressividade operacional do trigger.

## Estudos a baixar para validar os cenarios alternativos

### Para efeito e decaimento do mechanism-aware

1. **Kizilcec et al. (2020)**
   - mais importante para sustentar cenarios conservador, central e intensificado.

2. **Rodriguez, Guerrero-Roldan, & Baneres (2022)**
   - mais util para verificar persistencia curta e personalizacao do nudge.

### Para trigger e seletividade da intervencao

3. **Foster & Siddle (2019)**
   - principal para comparar trigger de 7 dias versus trigger mais estrito.

4. **Adnan et al. (2021)**
   - ajuda a sustentar granularidade semanal para deteccao e intervencao.

### Para faixa plausivel de impacto no shock-only

5. **Sonderlund et al. (2018)**
   - principal para verificar se `delta = 0.10`, `0.20` e `0.25` permanecem plausiveis.

6. **Oliveira et al. (2021)**
   - complementa a defesa de cenarios por faixa plausivel, especialmente na escrita do paper.

## Referencias candidatas em APA

Adnan, M., Habib, A., Ashraf, J., Mussadiq, S., Raza, A., Abid, M., Bashir, M., & Khan, S. U. (2021). Predicting at-risk students at different percentages of course length for early intervention using machine learning models. IEEE Access, 9, 140534-140548. https://doi.org/10.1109/ACCESS.2021.3049446

Foster, E., & Siddle, R. (2019). The effectiveness of learning analytics for identifying at-risk students in higher education. Assessment & Evaluation in Higher Education, 45(6), 842-854. https://doi.org/10.1080/02602938.2019.1691955

Kizilcec, R. F., Reich, J., Yeomans, M., Dann, C., Brunskill, E., Lopez, G., Turkay, S., Williams, J. J., & Tingley, D. (2020). Scaling up behavioral science interventions in online education. Proceedings of the National Academy of Sciences, 117(26), 14900-14905. https://doi.org/10.1073/pnas.1921417117

Mubarak, A. A., Cao, H., & Zhang, W. (2020). Prediction of students' early dropout based on their interaction logs in online learning environment. Interactive Learning Environments, 28(1), 1-20. https://doi.org/10.1080/10494820.2019.1709212

Oliveira, C. F. de, Sobral, S. R., Ferreira, M. J., & Moreira, F. (2021). How does learning analytics contribute to prevent students' dropout in higher education: A systematic literature review. Big Data and Cognitive Computing, 5(4), 55. https://doi.org/10.3390/bdcc5040055

Rodriguez, M. E., Guerrero-Roldan, A.-E., & Baneres, D. (2022). An intelligent nudging system to guide online learners. The International Review of Research in Open and Distributed Learning, 23(4), 74-95. https://doi.org/10.19173/irrodl.v23i4.6279

Sonderlund, A. L., Hughes, E., & Smith, J. (2018). The efficacy of learning analytics interventions in higher education: A systematic review. British Journal of Educational Technology, 49(5), 971-984. https://doi.org/10.1111/bjet.12663

## Conclusao pratica

Se o objetivo e defender academicamente os parametros principais da simulacao contrafactual, a combinacao mais eficiente para baixar e conferir agora e:

1. Kizilcec et al. (2020)
2. Rodriguez et al. (2022)
3. Foster & Siddle (2019)
4. Sonderlund et al. (2018)
5. Oliveira et al. (2021)

Essa pilha cobre, com melhor custo-beneficio:

- trigger por inatividade curta;
- janela curta de efeito;
- magnitude imediata e residual do efeito sobre engajamento;
- forma de decaimento;
- faixa plausivel para impacto sobre risco de dropout.