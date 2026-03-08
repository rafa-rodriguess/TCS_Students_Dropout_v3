# PARTE 1 — Diagnóstico global do manuscrito

## Visão geral da coesão argumentativa do paper

O manuscrito tem uma espinha argumentativa clara e intelectualmente reconhecível: partir de um problema real de dropout, justificar a necessidade de uma modelagem temporal, formalizar uma estrutura de hazard em tempo discreto, acoplar uma camada de simulação de regimes e, por fim, testar se essa estrutura também permite leitura por subgrupos. Em nível macro, o paper é coerente e tem um objeto identificável.

O principal problema não é ausência de tema nem falta de arquitetura. O principal problema é que, em pontos decisivos, o manuscrito afirma mais coesão empírica do que o texto e os artefatos efetivamente sustentam. Há uma tensão recorrente entre:

- a cautela metodológica declarada em Methodology e Experimental Design;
- a forma mais assertiva com que Results responde “Yes” às RQs;
- e algumas inconsistências factuais entre Abstract, Introduction, Experimental Design, Results e os artefatos exportados.

Em outras palavras: o paper tem desenho intelectual, mas ainda não está plenamente disciplinado na passagem de “framework plausível” para “framework empiricamente demonstrado”.

## Principais pontos fortes

- O manuscrito tem uma tese global clara: risco temporal pode ser convertido em uma estrutura de comparação de regimes simulados sem reivindicar identificação causal.
- Methodology e Experimental Design, em especial, delimitam bem o estatuto não causal da análise e isso reduz um risco comum de superinterpretação em trabalhos de policy simulation.
- O paper tem boa consciência de desenho: leakage, censoring, dual-horizon rule, ablation, OOD e endpoint sensitivity não são acessórios, mas parte da lógica central do estudo.
- A distinção entre $T_{policy}$, $T_{eval\_metrics}$ e $T_{eval\_policy}$ é uma contribuição real de clareza interpretativa e está, em geral, bem distribuída entre Methodology, Experimental Design e Results.
- O manuscrito está melhor quando formula capacidades do framework do que quando formula benefícios substantivos da política.

## Principais riscos argumentativos

- O paper oscila entre duas teses diferentes sem sempre separá-las com nitidez:
  - “o framework consegue gerar contrastes estruturais auditáveis”;
  - “a política simulada produz um sinal favorável e útil”.
  O texto sustenta razoavelmente a primeira, mas apenas parcialmente a segunda.
- RQ2 é apresentada como resposta positiva, mas os resultados mostram que a positividade depende fortemente de um ramo shock parametrizado exogenamente, enquanto o ramo mechanism-aware é nulo a negativo. Isso enfraquece a leitura substantiva de “policy benefit”.
- RQ3 é apresentada como evidência robusta, mas a robustez é essencialmente de sinal estatístico sobre magnitudes muito pequenas. O texto sustenta “directional robustness”, mas não sustenta com a mesma força relevância substantiva ou fairness relevance em sentido forte.
- RQ1 reivindica utilidade para “decision timing”, mas a evidência mostrada é mais forte para discriminação global do que para calibração operacional em regiões de maior risco, justamente onde a decisão seria mais sensível.

## Principais pontos de over-claiming

- O Abstract e a Introduction sugerem que o framework “supports structural policy evaluation” e “consistent structural policy evaluation across observable groups”. Isso é defensável apenas em sentido restrito: o framework produz comparações internas consistentes sob um contrato paramétrico. Não está demonstrado que ele sustente avaliação de política em sentido mais robusto, nem que a consistência se mantenha além do caso ilustrativo atual.
- Results responde “Yes” para RQ2 e RQ3 com linguagem mais forte do que o balanço empírico justifica. Em RQ2, o sinal positivo é cenário-dependente e não se reproduz na rama mechanism-aware. Em RQ3, a robustez é de direção, não de importância substantiva.
- Há linguagem que aproxima “calibration + discrimination” de prontidão para “decision timing”, mas o manuscrito não mostra avaliação operacional de decisão nem demonstra estabilidade forte nas caudas de risco.
- A noção de “benchmark coherence” e “robustness” está mais forte no texto do que nos resultados reportados, especialmente porque o benchmark narrado ao longo do manuscrito não coincide integralmente com o benchmark efetivamente apresentado em Results.

## Principais tensões ou contradições entre seções

- **Abstract vs Results vs artefatos exportados**: o Abstract reporta AUCs de 0.8359/0.8396; Results reporta 0.8350/0.8405; o artefato `table_calibration_metrics.csv` traz 0.8358500885/0.8395774756. Isso é uma inconsistência factual central, não apenas de arredondamento.
- **Abstract/Methodology/Experimental Design vs Results**: o manuscrito continua descrevendo um benchmark Cox como parte do benchmark principal, mas Results já não apresenta o Cox na comparação principal atualmente reportada.
- **Introduction vs Experimental Design vs artefatos de RQ3**: a Introduction usa “disability status” como exemplo aplicado, mas Experimental Design e os artefatos exportados mostram claramente `gender` como variável operacional do RQ3. Results evita nomear explicitamente o grupo e fala em “group coding used in this run”, o que mascara a inconsistência em vez de resolvê-la.
- **Experimental Design vs Results/Appendix A**: Experimental Design afirma que métricas por grupo e calibration curves por grupo são reportadas; o manuscrito principal não as mostra, e Appendix A tampouco as incorpora.
- **Methodology/Experimental Design caution vs Results assertiveness**: as seções de método e desenho são relativamente cautelosas; Results é mais conclusivo e às vezes formula a resposta como se o benefício empírico da política estivesse estabelecido, quando o texto anterior havia enquadrado tudo como contraste estrutural dependente de cenário.

## Avaliação geral da robustez do manuscrito do ponto de vista de coerência interna

O manuscrito é **intelectualmente promissor e estruturalmente sólido**, mas **ainda não está totalmente robusto em coerência interna**. A lógica do framework existe e está bem articulada; o problema está na disciplina de alinhamento entre:

- o que o paper diz que faz;
- o que realmente mostra;
- e o grau de força com que interpreta o que mostra.

Como diagnóstico global: o paper está mais forte como **artigo de framework metodológico com demonstração empírica localizada** do que como **artigo que já sustenta claims substantivos fortes sobre utilidade de policy simulation ou fairness-sensitive evaluation**.


# PARTE 2 — Análise seção por seção

## abstract_body.tex / Abstract

### 1. Função argumentativa da seção

Condensar a contribuição, o desenho empírico, os principais resultados e o enquadramento inferencial do paper.

### 2. Tese ou argumento central da seção

O paper propõe um framework temporal com simulação contrafactual de política e análise por subgrupos, e esse framework produziria evidência suficiente para avaliação estrutural de regimes sem reivindicar causalidade.

### 3. Coesão interna

**Classificação: Adequada**

O Abstract tem progressão clara: problema, método, resultados, enquadramento interpretativo. O encadeamento é bom. A fragilidade não está na organização, mas na fidelidade ao restante do manuscrito.

### 4. Qualidade da argumentação

**Classificação: Frágil**

O argumento é claro, mas comprime demais distinções importantes. O texto desliza de “framework com policy simulation” para “evidência de que o framework supports structural policy evaluation”, o que já é uma interpretação mais forte do que uma mera descrição do que foi feito. Para um Abstract, isso pode ser aceitável se o restante do paper sustentar exatamente essa formulação. No estado atual, não sustenta integralmente.

### 5. Suporte por evidência / resultados / referências

**Classificação: Problemática**

O maior problema é factual: os AUCs do Abstract não coincidem com os de Results nem com os números atualmente usados na narrativa de RQ1. Isso compromete a confiabilidade do resumo. Além disso, o Abstract ainda anuncia Cox como robustness check relevante, mas a seção de Results já não o materializa como benchmark central.

### 6. Alinhamento com tabelas e figuras

**Classificação: Frágil**

O Abstract resume resultados que o corpo do manuscrito não apresenta exatamente da mesma forma. A incompatibilidade numérica com a parte de Results é suficiente para classificar o alinhamento como frágil.

### 7. Over-claiming ou extrapolação

**Classificação: Moderado**

Há over-claiming em “supports structural policy evaluation and consistent scenario comparison” porque:

- “consistent scenario comparison” é plausível;
- “supports structural policy evaluation” é mais forte e sugere suficiência empírica mais ampla do que a demonstração localizada entrega.

Também há compressão excessiva entre “counterfactual simulation” e “evaluation” sem deixar totalmente claro que a segunda é apenas interna ao modelo.

### 8. Contradições internas ou com outras seções

**Classificação: Contradições claras**

- Contradição com `results_body.tex` nos valores de AUC.
- Tensão com `results_body.tex`, `methodology_body.tex` e `experimental_design_body.tex` no papel do benchmark Cox.

### 9. Principais problemas encontrados

- **[Consistência factual]** Números de AUC não batem com Results e com os artefatos exportados.
- **[Consistência entre seções]** O benchmark descrito no Abstract não coincide com o benchmark efetivamente reportado.
- **[Over-claiming]** A frase final sugere sustentação mais forte para policy evaluation do que a evidência permite.

### 10. Ações corretivas recomendadas

- Alinhar os números do Abstract aos artefatos exportados efetivamente adotados no manuscrito final.
- Atualizar ou remover a menção ao Cox se ele não for resultado central apresentado.
- Reescrever a conclusão do Abstract com linguagem mais calibrada: “supports internal scenario comparison under a model-implied structural framework” ou equivalente.


## introduction_body.tex / Introduction

### 1. Função argumentativa da seção

Apresentar o problema, justificar por que um enquadramento temporal é necessário, introduzir a passagem de predição para política e antecipar a contribuição do paper.

### 2. Tese ou argumento central da seção

A literatura foca demais em quem está em risco; este paper propõe um framework temporal que acrescenta quando agir, como comparar regimes simulados e como observar diferenças entre grupos.

### 3. Coesão interna

**Classificação: Adequada**

A seção progride de modo relativamente natural do problema geral para a proposta do paper. O ponto menos coeso é que ela combina, em pouco espaço, motivação substantiva, preview metodológico, preview empírico e qualificação inferencial. Não chega a ser desorganizada, mas mistura funções.

### 4. Qualidade da argumentação

**Classificação: Adequada**

O argumento central é identificável: a passagem de static prediction para temporal/policy evaluation. O raciocínio é inteligível. O problema é que a seção já vende algumas capacidades do framework antes de mostrar por que elas foram demonstradas de forma suficiente. A argumentação é boa como posicionamento de agenda, menos boa como promessa de demonstração.

### 5. Suporte por evidência / resultados / referências

**Classificação: Adequada**

A motivação para temporalidade e limitação dos dados observacionais é bem ancorada. O suporte enfraquece quando a seção afirma que o framework é “transferable” e que a análise por grupos “demonstrates” consistência estrutural. Essas formulações não derivam diretamente de uma base empírica mostrada aqui nem depois.

### 6. Alinhamento com tabelas e figuras

**Classificação: Não aplicável**

A seção não depende diretamente de tabelas/figuras próprias para sustentar seu argumento central.

### 7. Over-claiming ou extrapolação

**Classificação: Moderado**

Os principais pontos são:

- “The framework is designed to be transferable” é mais forte do que a evidência do paper permite. O paper não testa transferência entre instituições, datasets ou contextos.
- “This analysis ... demonstrates that the mathematical framework supports consistent structural policy evaluation across observable groups” está forte demais para um caso demonstrativo único, ainda por cima com magnitudes muito pequenas em RQ3.

### 8. Contradições internas ou com outras seções

**Classificação: Contradições claras**

- A Introduction usa “disability status” como exemplo de aplicação de subgrupos, mas `experimental_design_body.tex` e os artefatos de RQ3 mostram `gender` como variável efetivamente usada.
- A promessa de subgroup analysis aparece com mais nitidez do que a evidência efetivamente mostrada em `results_body.tex`.

### 9. Principais problemas encontrados

- **[Consistência entre seções]** Exemplo de grupo inconsistente com o experimento real.
- **[Over-claiming]** Transferability e consistência across observable groups estão formuladas de forma ampla demais.
- **[Argumentação]** A seção pré-vende capacidades empíricas que depois aparecem de forma mais limitada.

### 10. Ações corretivas recomendadas

- Substituir “disability status” pela variável realmente usada ou explicitar que era apenas exemplo abstrato, não a aplicação do paper.
- Recalibrar “transferable” para algo como “potentially portable under similar data conditions”.
- Reescrever o trecho de subgroup analysis para enfatizar demonstração localizada, não validação ampla.


## background_body.tex / Background

### 1. Função argumentativa da seção

Contextualizar a relevância do dropout, o papel dos ambientes online/LMS e a lacuna entre prática de intervenção e avaliação causal.

### 2. Tese ou argumento central da seção

O problema do dropout é relevante, a educação online gera traços temporais ricos, e o contexto observacional real é ruim para avaliação causal forte de intervenções, o que justifica uma abordagem estrutural e temporal.

### 3. Coesão interna

**Classificação: Forte**

As três subseções cumprem funções complementares e bem alinhadas: importância do problema, mudança estrutural do contexto, limitações da prática de intervenção. Não há mudanças bruscas nem trechos evidentemente deslocados.

### 4. Qualidade da argumentação

**Classificação: Adequada**

O Background prepara bem o terreno. Ele é particularmente bom em justificar por que dados observacionais de intervenção em educação superior tendem a ser insuficientes para avaliação causal limpa. O ponto menos desenvolvido é a transição da lacuna causal para a forma específica de simulação adotada pelo paper. O texto justifica a necessidade de cautela, mas não justifica com a mesma força por que esta arquitetura de scenario simulation é a resposta mais adequada.

### 5. Suporte por evidência / resultados / referências

**Classificação: Forte**

As afirmações substantivas centrais têm suporte bibliográfico. A seção usa referências como sustentação real, não só ornamentação.

### 6. Alinhamento com tabelas e figuras

**Classificação: Não aplicável**

Não depende de tabelas/figuras próprias.

### 7. Over-claiming ou extrapolação

**Classificação: Leve**

Há pouco over-claiming aqui. O principal risco é implícito: o texto constrói uma boa justificativa para não-identificação causal, mas isso não basta por si só para validar a solução proposta. Esse é um problema de ponte argumentativa mais do que de exagero lexical.

### 8. Contradições internas ou com outras seções

**Classificação: Possíveis tensões**

- A seção é mais cautelosa e epistemicamente disciplinada do que alguns trechos de `results_body.tex`, que respondem às RQs de forma mais assertiva.

### 9. Principais problemas encontrados

- **[Argumentação]** A seção justifica bem o problema e a limitação causal, mas menos bem por que a solução específica do paper é a melhor resposta.
- **[Consistência global]** O grau de cautela aqui é mais alto do que em partes de Results.

### 10. Ações corretivas recomendadas

- Inserir uma ponte mais explícita entre “dados observacionais inadequados para causalidade” e “por que uma framework de contraste estrutural parametrizado é a resposta metodológica adotada”.
- Harmonizar o nível de cautela desta seção com as formulações de Results.


## methodology_body.tex / Methodology

### 1. Função argumentativa da seção

Definir formalmente o framework, seus objetos matemáticos, seus estimandos e seus limites interpretativos.

### 2. Tese ou argumento central da seção

É possível construir um pipeline temporal com hazard discreto, contrastes estruturais de regime e análise por subgrupos, mantendo clara a fronteira entre contraste model-implied e efeito causal.

### 3. Coesão interna

**Classificação: Forte**

A seção é a mais organizada do manuscrito. Os blocos de definição, estimandos, tracks, assumptions e interpretive scope se articulam bem. A progressão é lógica e há boa unidade conceitual.

### 4. Qualidade da argumentação

**Classificação: Adequada**

Como formulação de framework, a seção funciona bem. O ponto mais frágil é que algumas “propositions” soam mais normativas do que demonstradas. Por exemplo, a proposição do “Temporal bridge” sugere que ausência de leakage e disponibilidade de $\hat h_{it}$ implicam janelas úteis para decisão. Isso não decorre apenas da definição matemática; depende também de performance e calibração empírica.

### 5. Suporte por evidência / resultados / referências

**Classificação: Adequada**

As partes conceituais e metodológicas têm bom suporte bibliográfico. A fragilidade está menos na citação e mais no vínculo com o que depois é realmente mostrado. A seção descreve um benchmark Cox e um fairness track com naturalidade, mas Results não entrega o mesmo grau de materialização para esses elementos.

### 6. Alinhamento com tabelas e figuras

**Classificação: Não aplicável**

O papel principal aqui é formal/conceitual, não demonstrativo via tabelas/figuras.

### 7. Over-claiming ou extrapolação

**Classificação: Leve**

O texto é relativamente comedido. O ponto principal é a proposição do “Temporal bridge”, que ainda formula utilidade decisória de forma mais forte do que uma seção metodológica deveria afirmar sem mediação empírica.

### 8. Contradições internas ou com outras seções

**Classificação: Possíveis tensões**

- Tensão com `results_body.tex` quanto ao benchmark: Methodology mantém Cox como parte do benchmark track, enquanto Results já opera com outro pacote de benchmark na prática reportada.
- Tensão com `results_body.tex` e `introduction_body.tex` sobre o grau de materialização do fairness track.

### 9. Principais problemas encontrados

- **[Argumentação]** Algumas proposições misturam definição do framework com antecipação de utilidade empírica.
- **[Consistência entre seções]** O benchmark descrito não coincide integralmente com o benchmark atualmente mostrado em Results.
- **[Consistência entre seções]** A seção sugere um fairness track mais plenamente desenvolvido do que o manuscrito efetivamente exibe.

### 10. Ações corretivas recomendadas

- Reformular as proposições para distingui-las entre definição formal e condição empírica a ser testada.
- Atualizar o benchmark track para refletir exatamente o benchmark final reportado.
- Explicitar que o subgroup track é demonstrativo e limitado ao caso operacional corrente, se essa for a intenção real.


## experimental_design_body.tex / Experimental Design

### 1. Função argumentativa da seção

Especificar protocolo, coorte, split, métricas, horizontes, regras de leitura, robustness battery e mapeamento claim-evidence.

### 2. Tese ou argumento central da seção

O paper segue um protocolo pré-definido, com horizontes, métricas e regras de interpretação suficientemente claros para sustentar leitura disciplinada dos resultados.

### 3. Coesão interna

**Classificação: Forte**

É uma seção bem encadeada e com boa unidade. A passagem de coorte para split, modelos, horizontes, métricas, policy, subgroup, robustness e limits é lógica.

### 4. Qualidade da argumentação

**Classificação: Adequada**

O desenho é bem apresentado. O problema não é falta de rigor interno, mas o fato de que o texto promete uma disciplina probatória que o restante do manuscrito nem sempre cumpre. A seção funciona quase como um contrato de leitura; por isso, qualquer desalinhamento posterior pesa mais.

### 5. Suporte por evidência / resultados / referências

**Classificação: Adequada**

Como desenho, a seção é detalhada. A fragilidade surge porque alguns itens “reportados” aqui não são realmente reportados de forma visível no manuscrito final. Isso enfraquece o valor da seção como protocolo cumprido.

### 6. Alinhamento com tabelas e figuras

**Classificação: Adequada**

As tabelas e figuras internas da seção ajudam a estruturar o protocolo. O problema maior não está nelas, mas no fato de que alguns produtos prometidos pelo desenho não aparecem depois com o mesmo peso.

### 7. Over-claiming ou extrapolação

**Classificação: Leve**

O texto é cauteloso. O único risco está no uso de “robust” em RQ3 e “robustness” nos comparison rules, porque a robustez definida é predominantemente direcional, não substantiva.

### 8. Contradições internas ou com outras seções

**Classificação: Contradições claras**

- `experimental_design_body.tex` afirma `gender` como variável operacional de demonstração em RQ3, enquanto `introduction_body.tex` fala em disability status como exemplo aplicado.
- A seção descreve benchmark com Cox, HGB e RSF, mas `results_body.tex` já não apresenta um benchmark principal coerente com toda essa narrativa.
- A seção afirma que métricas por grupo e calibration curves por grupo são reportadas; o manuscrito principal não as mostra de forma correspondente.

### 9. Principais problemas encontrados

- **[Consistência entre seções]** Desenho de benchmark desatualizado em relação aos resultados finais.
- **[Evidência]** Parte do que é prometido como “reported” não está realmente incorporado ao manuscrito.
- **[Interpretação]** O conceito de robustez é operacional, mas pode ser lido como mais forte do que realmente é.

### 10. Ações corretivas recomendadas

- Alinhar a narrativa do benchmark àquilo que Results realmente reporta.
- Se fairness diagnostics por grupo são parte do contrato, incorporá-los ao menos no apêndice, não apenas no pacote de artefatos.
- Reescrever “robustness” sempre que o resultado for apenas estabilidade de sinal, não convergência substantiva mais ampla.


## results_body.tex / Results

### 1. Função argumentativa da seção

Responder empiricamente às RQs e mostrar como os artefatos sustentam as principais conclusões do paper.

### 2. Tese ou argumento central da seção

Os resultados mostrariam que o modelo temporal é suficiente para timing, que a policy simulation produz contrastes interpretáveis e que o mesmo framework pode auditar diferenças entre grupos com direção robusta.

### 3. Coesão interna

**Classificação: Adequada**

Em termos de ordem, a seção é clara. Ela progride de RQ1 para censura, RQ2, benchmark/robustez e RQ3. O problema de coesão não é estrutural; é probatório. A seção se apresenta como fecho empírico disciplinado, mas em alguns pontos usa linguagem de fechamento mais forte do que a sustentação exibida.

### 4. Qualidade da argumentação

**Classificação: Frágil**

Os maiores problemas argumentativos são:

- as respostas “Yes” no quadro inicial são mais fortes do que a própria seção sustenta;
- RQ2 mistura “o framework consegue gerar contrastes” com “a política melhora o resultado”;
- RQ3 mistura robustez direcional com relevância substantiva;
- RQ1 avança de AUC/calibração para “timing signals” sem mostrar avaliação operacional de decisão.

O texto distingue parcialmente descrição, evidência e conclusão, mas não o faz de modo suficientemente estrito.

### 5. Suporte por evidência / resultados / referências

**Classificação: Frágil**

Pontos críticos:

- Os AUCs de RQ1 não estão alinhados de forma limpa com os artefatos exportados principais.
- A calibração apresentada é ambígua: o ECE global é baixo, mas os bins de risco mais altos têm baixíssimo suporte e grandes gaps absolutos. Isso sustenta cautela, não conforto amplo para decisão.
- Em RQ2, a leitura positiva depende do braço shock, que é paramétrico e exógeno; o braço mechanism-aware não confirma o ganho.
- Em RQ3, a evidência sustenta sinal não nulo, mas com magnitudes muito pequenas. A seção apoia “there is a detectable directional difference”, não necessariamente “there is a substantively meaningful subgroup-sensitive policy signal”.

### 6. Alinhamento com tabelas e figuras

**Classificação: Frágil**

Problemas principais:

- A figura/tabela de calibração não sustentam de forma forte a passagem para “decision timing” porque as caudas são muito mal suportadas.
- A tabela de benchmark mostra RSF melhor do que os modelos temporais nas AUCs por horizonte reportadas; isso enfraquece qualquer leitura implícita de superioridade do pipeline temporal como benchmark principal. O texto contorna isso dizendo que o benchmark é “anchored” em outra leitura, mas a tabela continua sendo potencialmente desconfortável para a narrativa central.
- A figura de RQ2 sustenta que existem contrastes por cenário, mas não sustenta integralmente a formulação simplificada de resposta positiva se o leitor considerar o braço mechanism-aware parte substantiva da tese.
- A tabela de RQ3 sustenta robustez de sinal, mas não sustenta sozinha a importância substantiva do efeito.

### 7. Over-claiming ou extrapolação

**Classificação: Alto**

Os pontos mais críticos são:

- “calibration diagnostics support the use of hazard trajectories for decision timing” está forte demais para o material exibido;
- “The answer is yes” em RQ2 simplifica excessivamente um resultado que é cenário-dependente e negativamente contradito no braço mechanism-aware;
- “The framework answers RQ3 positively” pode ser defendido apenas em sentido mínimo, porque a evidência mostra direção robusta, não uma diferença substantiva robusta;
- “The main interpretation remains directionally coherent under benchmark and robustness axes” é mais forte do que a seção prova, especialmente porque o benchmark em Results já está narrativamente reconfigurado e não coincide com o benchmark prometido em outras seções.

### 8. Contradições internas ou com outras seções

**Classificação: Contradições claras**

- Com `abstract_body.tex`: números de AUC divergentes.
- Com `experimental_design_body.tex` e `methodology_body.tex`: benchmark Cox descrito como componente do benchmark track, mas ausente do benchmark efetivamente reportado.
- Com `experimental_design_body.tex`: prometem-se métricas de fairness por grupo e calibration curves por grupo; Results não as mostra.
- Com `introduction_body.tex`: Introduction sugere aplicação por disability status; Results evita nomear o grupo efetivo, o que aumenta a opacidade da consistência entre seções.

### 9. Principais problemas encontrados

- **[Consistência factual]** Métricas de RQ1 inconsistentes com artefatos e Abstract.
- **[Interpretação de tabela/figura]** Calibração e bins de cauda não suportam leitura forte de prontidão decisória.
- **[Over-claiming]** RQ2 e RQ3 são respondidas de forma excessivamente afirmativa.
- **[Evidência]** O manuscrito não materializa parte importante dos diagnostics prometidos para fairness.
- **[Consistência entre seções]** Benchmark narrado e benchmark mostrado não coincidem.

### 10. Ações corretivas recomendadas

- Corrigir e unificar todos os números de RQ1 com base em um único artefato final explicitado.
- Reescrever RQ1 para separar “boa discriminação global” de “calibração operacional suficiente para decisão”, deixando claro que as caudas são frágeis.
- Reescrever a resposta de RQ2 como: o framework produz contrastes interpretáveis; o sinal benéfico aparece apenas no ramo shock e é fortemente cenário-dependente; o ramo mechanism-aware não confirma ganho.
- Reescrever a resposta de RQ3 para destacar “directional robustness with very small magnitude”.
- Ou incorporar fairness diagnostics por grupo ao manuscrito, ou retirar de Experimental Design a promessa de que eles são reportados.
- Explicitar no benchmark por que a comparação principal mudou e o que ainda resta da promessa original de Cox benchmark.


## appendix_a_body.tex / Appendix A

### 1. Função argumentativa da seção

Fornecer diagnósticos suplementares diretamente ligados às conclusões centrais, especialmente para incerteza de RQ3.

### 2. Tese ou argumento central da seção

O apêndice mostra apenas diagnósticos realmente necessários para auditar as conclusões principais, em especial a incerteza do RQ3.

### 3. Coesão interna

**Classificação: Adequada**

A seção é enxuta e coerente. Não há dispersão temática.

### 4. Qualidade da argumentação

**Classificação: Adequada**

Ela cumpre o que promete. O problema é de suficiência, não de construção: o apêndice é tão contido que não absorve várias fragilidades que o texto principal empurra para “artifact package”.

### 5. Suporte por evidência / resultados / referências

**Classificação: Frágil**

Para a função que o manuscrito lhe atribui, o apêndice está subdimensionado. Se RQ3 depende fortemente de bootstrap uncertainty e se fairness diagnostics por grupo são parte do argumento de validade, o apêndice atual oferece suporte parcial demais.

### 6. Alinhamento com tabelas e figuras

**Classificação: Adequada**

A figura de bootstrap é pertinente ao que o texto diz que ela faz. O problema não é interpretação errada da figura; é a estreiteza do conjunto de evidências suplementares mostradas.

### 7. Over-claiming ou extrapolação

**Classificação: Leve**

O tom é comedido. Não há exagero relevante no próprio apêndice.

### 8. Contradições internas ou com outras seções

**Classificação: Possíveis tensões**

- Tensão com `experimental_design_body.tex`: o desenho fala em calibration-by-group e fairness diagnostics por grupo, mas o apêndice não os incorpora.
- Tensão com `results_body.tex`: o corpo principal apoia RQ3 em uma base relativamente enxuta, e o apêndice não amplia isso tanto quanto seria desejável.

### 9. Principais problemas encontrados

- **[Escopo da seção]** O apêndice complementa, mas complementa menos do que o manuscrito precisaria para fortalecer RQ3.
- **[Evidência]** Material importante segue fora do manuscrito e apenas no pacote de artefatos.

### 10. Ações corretivas recomendadas

- Incluir ao menos uma tabela curta de métricas por grupo ou uma curva/calibration summary por grupo.
- Se o apêndice continuará mínimo, enfraquecer em Results a impressão de que a parte de fairness está amplamente auditada no próprio paper.


## appendix_b_body.tex / Appendix B

### 1. Função argumentativa da seção

Documentar reprodutibilidade, disponibilidade de código e rastreabilidade dos artefatos.

### 2. Tese ou argumento central da seção

O paper é reprodutível no nível do pipeline especificado e dos artefatos exportados.

### 3. Coesão interna

**Classificação: Forte**

A seção é clara, focada e funcional. Cumpre bem sua finalidade documental.

### 4. Qualidade da argumentação

**Classificação: Adequada**

Como seção de reprodutibilidade, ela é sólida. Sua limitação é que rastreabilidade operacional não resolve, por si só, inconsistências narrativas entre manuscrito e resultados reportados.

### 5. Suporte por evidência / resultados / referências

**Classificação: Adequada**

A seção informa de modo convincente onde estão código, notebook e artefatos. O problema não é falta de documentação, mas o fato de que os números narrados no paper nem sempre coincidem com os artefatos mencionados.

### 6. Alinhamento com tabelas e figuras

**Classificação: Não aplicável**

Não depende diretamente de tabelas/figuras argumentativas.

### 7. Over-claiming ou extrapolação

**Classificação: Nenhum relevante**

O texto é bastante sóbrio.

### 8. Contradições internas ou com outras seções

**Classificação: Possíveis tensões**

- Se o manuscrito insiste que a evidência deriva do mesmo pacote exportado, as inconsistências numéricas entre Abstract/Results e os artefatos tornam-se mais graves, porque a seção de reprodutibilidade implicitamente promete alinhamento exato.

### 9. Principais problemas encontrados

- **[Consistência global]** A seção fortalece a exigência de consistência com os artefatos, e isso torna mais visíveis as discrepâncias presentes em outras seções.
- **[Escopo]** Não há problema interno relevante, mas ela não compensa fragilidades argumentativas do corpo principal.

### 10. Ações corretivas recomendadas

- Garantir que todas as métricas citadas no manuscrito correspondam exatamente ao pacote exportado descrito aqui.
- Se houver múltiplos runs/exports, explicitar isso no manuscrito e não apenas no repositório.


# PARTE 3 — Matriz transversal de consistência

## Seções bem alinhadas entre si

- **Background + Methodology**: forte alinhamento. O Background prepara a necessidade de cautela causal, e Methodology incorpora isso explicitamente no interpretive scope.
- **Methodology + Experimental Design**: bom alinhamento geral. As duas seções compartilham a mesma ontologia do paper: hazard temporal, dual horizons, contrastes estruturais e limites inferenciais.
- **Appendix B + Experimental Design**: bom alinhamento no plano da rastreabilidade e da ideia de pipeline especificado.

## Seções com tensão de linguagem ou escopo

- **Introduction vs Experimental Design/Results**: a Introduction amplia o alcance do subgroup track e usa um exemplo de grupo que não coincide com o experimento efetivamente conduzido.
- **Methodology/Experimental Design vs Results**: Methodology e Experimental Design são mais cautelosos; Results transforma essa cautela em respostas mais assertivas às RQs.
- **Experimental Design vs Appendix A**: o desenho promete uma camada de fairness diagnostics que o apêndice não entrega de forma proporcional.

## Seções com diferentes níveis de cautela

- **Mais cautelosas**: Background, Methodology, Experimental Design.
- **Intermediárias**: Appendix A, Appendix B.
- **Menos cautelosa**: Results, especialmente no quadro de respostas diretas às RQs e nas conclusões de RQ2/RQ3.

## Onde o texto principal afirma mais do que os resultados mostram

- **RQ1**: o texto principal aproxima discriminação + calibração de utilidade para “decision timing”, mas a evidência de calibração nas caudas é fraca e escassamente suportada.
- **RQ2**: o texto sugere resposta positiva do policy track, mas o sinal favorável depende da parametrização shock; o branch mechanism-aware não sustenta a mesma conclusão.
- **RQ3**: o texto principal sustenta robustez de direção, mas facilmente pode ser lido como robustez substantiva ou fairness relevance mais forte do que a magnitude observada justifica.
- **Benchmark/robustness**: a narrativa de coerência benchmark é mais limpa do que o material efetivamente mostrado, especialmente porque o benchmark final já não corresponde ao benchmark prometido em outras seções.

## Onde apêndices, tabelas ou figuras enfraquecem ou qualificam conclusões do corpo principal

- **Tabela de calibração por bins**: qualifica e enfraquece uma leitura forte de calibração operacional; os bins superiores têm suporte muito baixo e gaps grandes.
- **Tabela de benchmark por horizonte**: qualifica qualquer leitura implícita de superioridade ou centralidade inequívoca do modelo temporal, pois o RSF aparece melhor nesses horizontes.
- **Tabela de RQ3**: qualifica o entusiasmo sobre fairness/policy relevance ao mostrar que o efeito é pequeno, ainda que estatisticamente direcionalmente estável.
- **Appendix A**: mostra que a incerteza de RQ3 existe e foi auditada, mas também evidencia que o manuscrito carrega muito da sustentação para fora do corpo principal e, ainda assim, de forma limitada.


# PARTE 4 — Lista final de riscos acadêmicos

## 1. Riscos graves de rejeição por incoerência ou excesso de claim

1. **Inconsistência factual de métricas entre Abstract, Results e artefatos exportados.** Isso compromete credibilidade básica do manuscrito.
2. **Desalinhamento entre benchmark prometido e benchmark reportado.** O texto continua estruturando a lógica do benchmark em torno de Cox/HGB/RSF, mas Results já opera outra configuração.
3. **Resposta excessivamente afirmativa para RQ2.** O paper não mostra “benefício de política” de modo estável; mostra contrastes paramétricos cenário-dependentes, com ramo mechanism-aware negativo.
4. **Uso potencialmente excessivo de RQ3 como evidência de subgroup/fairness capability.** O resultado sustenta direção robusta, mas com magnitude muito pequena e sem o conjunto de diagnostics prometido no desenho.

## 2. Riscos moderados

1. **Claim de calibration/timing em RQ1 mais forte do que a evidência de cauda permite.**
2. **Introduction com escopo mais amplo do que o experimento final, inclusive no exemplo do grupo analisado.**
3. **Diferença de cautela entre seções metodológicas e Results.** Um revisor pode ler isso como opportunistic framing.
4. **Appendix A insuficiente para sustentar toda a carga suplementar deslocada para o “artifact package”.**

## 3. Ajustes finos desejáveis

1. Recalibrar lexicalmente termos como “supports”, “robust”, “auditable” e “decision timing” quando a evidência for apenas parcial.
2. Tornar explícita, no corpo do texto, a diferença entre robustez direcional e relevância substantiva.
3. Harmonizar o uso do exemplo de subgroup ao longo de todo o paper.
4. Se houver múltiplos runs ou múltiplas tabelas candidatas para RQ1, explicitar isso em vez de deixar números concorrentes coexistirem.


# PARTE 5 — O que PRECISAMOS MUDAR

## 1. Unificar todas as métricas centrais de RQ1

**O que**

Unificar os valores de AUC e demais métricas centrais de RQ1 em todas as seções do manuscrito.

**Por que**

Hoje há inconsistência factual entre Abstract, Results e artefatos exportados. Isso é um problema grave de credibilidade acadêmica e pode ser interpretado como falta de controle sobre a versão final dos resultados.

**Onde**

- `abstract_body.tex`
- `results_body.tex`
- eventualmente `introduction_body.tex`, se algum número ou formulação resumida depender desses resultados
- qualquer tabela/síntese do manuscrito principal que resuma RQ1

**Sugestão**

Escolher um único artefato final como fonte oficial de RQ1, preferencialmente o export mais diretamente alinhado ao run final do paper, e atualizar todas as menções numéricas a partir dele. Se houver diferença entre métricas “raw” e métricas “reportable/robust”, isso deve ser explicitado, não apenas corrigido silenciosamente.

## 2. Resolver o desalinhamento do benchmark

**O que**

Reconciliar a narrativa do benchmark entre Methodology, Experimental Design, Results e Abstract.

**Por que**

O manuscrito descreve um benchmark em torno de Cox/HGB/RSF, mas Results já reporta outra configuração como benchmark principal. Isso gera contradição entre seções e enfraquece a confiabilidade da interpretação de robustez e comparação entre famílias.

**Onde**

- `abstract_body.tex`
- `methodology_body.tex`
- `experimental_design_body.tex`
- `results_body.tex`

**Sugestão**

Há duas rotas possíveis:

1. Reintroduzir no Results o benchmark coerente com o desenho metodológico, se os artefatos finais realmente o sustentarem.
2. Mais provavelmente, reescrever Methodology, Experimental Design e Abstract para refletir o benchmark efetivamente reportado no manuscrito final.

Se o Cox deixar de ser benchmark central, ele deve aparecer apenas como tentativa auxiliar, análise secundária ou elemento histórico do pipeline, não como parte do eixo comparativo principal.

## 3. Recalibrar a resposta de RQ2

**O que**

Reescrever a resposta de RQ2 para separar claramente:

- capacidade do framework de gerar contrastes estruturais;
- e suposto ganho da política simulada.

**Por que**

No estado atual, o texto responde “Yes” como se o achado central fosse um benefício de política relativamente claro. Mas os resultados mostram que o ganho aparece no branch shock e depende fortemente do cenário, enquanto o branch mechanism-aware é negativo. A leitura atual simplifica excessivamente a evidência.

**Onde**

- `results_body.tex`
- `abstract_body.tex`
- possivelmente `introduction_body.tex`, se a contribuição estiver formulada como benefício de policy relevance muito assertivo

**Sugestão**

Reformular RQ2 em termos de duas conclusões:

1. O framework produz contrastes estruturais interpretáveis sob cenários explícitos.
2. O sinal favorável, nesta execução, é restrito ao branch shock e fortemente cenário-dependente; o branch mechanism-aware não confirma ganho.

Isso preserva a contribuição metodológica e reduz over-claiming substantivo.

## 4. Recalibrar a resposta de RQ3

**O que**

Reescrever a leitura de RQ3 para explicitar que a robustez observada é direcional e de pequena magnitude.

**Por que**

O texto atual pode ser lido como se o paper tivesse demonstrado uma fairness-sensitive capability robusta em sentido mais amplo. O que a evidência realmente mostra é um $\Delta Gap(T)$ pequeno, porém com intervalo excluindo zero. Isso sustenta detecção direcional, não um efeito substantivo forte.

**Onde**

- `results_body.tex`
- `abstract_body.tex`
- `introduction_body.tex`
- qualquer conclusão geral do manuscrito que use RQ3 como pilar de contribuição

**Sugestão**

Trocar formulações do tipo “answers RQ3 positively” por algo como “provides directional subgroup-sensitive contrasts with bootstrap-supported sign stability in the reported scenario, at very small magnitude”.

## 5. Resolver a inconsistência sobre qual grupo foi usado em RQ3

**O que**

Alinhar todas as seções quanto ao grupo efetivamente usado na análise de subgrupos.

**Por que**

Hoje Introduction menciona disability status como exemplo aplicado, enquanto Experimental Design e os artefatos mostram gender. Isso cria uma contradição clara e desnecessária sobre o objeto empírico do RQ3.

**Onde**

- `introduction_body.tex`
- `experimental_design_body.tex`
- `results_body.tex`
- eventualmente `abstract_body.tex`, se houver formulações genéricas demais que possam ser lidas como outra aplicação

**Sugestão**

Se `gender` é o grupo real do experimento final, assumir isso explicitamente no manuscrito. Se o objetivo for manter generalidade, dizer que o framework é group-agnostic, mas que a demonstração empírica desta versão usa `gender`.

## 6. Ajustar RQ1 para não confundir discriminação global com prontidão decisória

**O que**

Reescrever os trechos de RQ1 que aproximam AUC e calibração global de “decision timing” ou uso operacional suficientemente sustentado.

**Por que**

Os bins superiores de calibração têm suporte muito baixo e gaps absolutos relevantes. Isso não invalida o modelo, mas enfraquece uma leitura forte de calibração operacional em regiões de maior risco, que seriam justamente as mais importantes para decisão.

**Onde**

- `results_body.tex`
- `abstract_body.tex`
- possivelmente `methodology_body.tex`, na proposição do “Temporal bridge”

**Sugestão**

Separar explicitamente:

- boa discriminação semanal global;
- calibração agregada razoável;
- fragilidade de suporte nas caudas de risco;
- e implicação prática mais comedida: o modelo oferece sinais temporais exploratórios/estruturais, não validação plena de decisão operacional.

## 7. Cumprir ou reduzir a promessa de fairness diagnostics por grupo

**O que**

Ou incluir no manuscrito os principais diagnostics por grupo, ou remover do desenho a afirmação de que eles são parte reportada da argumentação.

**Por que**

Experimental Design afirma que métricas por grupo e calibration curves por grupo são reportadas, mas o texto principal não as mostra e o Appendix A também não. Isso é problema de evidência e de consistência entre seções.

**Onde**

- `experimental_design_body.tex`
- `results_body.tex`
- `appendix_a_body.tex`

**Sugestão**

Se quiser manter a ambição do RQ3, incluir ao menos uma tabela compacta com AUC/Brier/ECE por grupo no apêndice e fazer referência a ela no corpo principal. Se não quiser expandir o manuscrito, reduzir a promessa em Experimental Design e modular a força da conclusão em Results.


# PARTE 6 — SERIA BOM / ÓTIMO

## 1. Tornar mais explícita a diferença entre “framework validation” e “policy benefit”

**O que**

Refinar o manuscrito para diferenciar constantemente validação do framework e benefício substantivo da política simulada.

**Por que**

Essa distinção já existe implicitamente, mas nem sempre está estável entre seções. Torná-la explícita deixaria o paper intelectualmente mais disciplinado e menos vulnerável a acusações de over-claiming.

**Onde**

- `introduction_body.tex`
- `methodology_body.tex`
- `results_body.tex`
- conclusão geral, se houver fora dos arquivos analisados

**Sugestão**

Usar formulações recorrentes como:

- “the framework supports internal structural comparison”
- “the reported positive signal is scenario-dependent”
- “the study validates the comparison machinery more strongly than the substantive desirability of the simulated intervention”

## 2. Fortalecer a ponte entre Background e a solução metodológica escolhida

**O que**

Adicionar uma transição mais argumentativa entre a limitação causal do contexto observacional e a adoção específica da policy simulation estrutural parametrizada.

**Por que**

Hoje o Background justifica muito bem a cautela causal, mas a passagem para a solução metodológica ainda depende de inferência do leitor. Fortalecer essa ponte melhoraria a unidade argumentativa global.

**Onde**

- `background_body.tex`
- `introduction_body.tex`
- eventualmente o início de `methodology_body.tex`

**Sugestão**

Inserir um parágrafo curto explicando que, diante da ausência de logging confiável de intervenção e da inviabilidade prática de identificação causal neste dataset, o paper opta por um desenho explicitamente estrutural e scenario-based como estratégia de análise disciplinada, e não como substituto da inferência causal.

## 3. Expandir moderadamente o Appendix A

**O que**

Adicionar 1 ou 2 artefatos suplementares que sustentem melhor RQ1 ou RQ3.

**Por que**

O apêndice atual está correto, mas econômico demais para a carga inferencial que o manuscrito quer sustentar. Pequena expansão pode produzir ganho desproporcional de credibilidade.

**Onde**

- `appendix_a_body.tex`

**Sugestão**

Prioridade prática:

1. tabela curta com métricas por grupo;
2. resumo de calibração reportable/robust para RQ1;
3. se couber, nota curta explicando a diferença entre robustez de sinal e magnitude substantiva em RQ3.

## 4. Harmonizar o léxico de força epistêmica ao longo do manuscrito

**O que**

Revisar termos como “supports”, “robust”, “auditable”, “decision-relevant”, “policy-relevant”, “transferable” e “answers positively”.

**Por que**

O manuscrito já melhorou de tom, mas ainda oscila entre seções mais cautelosas e Results mais assertivo. Um léxico mais homogêneo reduziria tensão interna e melhoraria percepção de rigor.

**Onde**

- sobretudo `abstract_body.tex`, `introduction_body.tex` e `results_body.tex`

**Sugestão**

Adotar uma regra simples:

- usar linguagem forte apenas quando houver demonstração direta;
- usar linguagem calibrada quando houver evidência parcial, cenário-dependente ou interpretativamente limitada.

## 5. Explicitar melhor o estatuto do branch mechanism-aware

**O que**

Definir com mais clareza, no texto principal, qual o papel epistemológico do branch mechanism-aware dentro do paper.

**Por que**

Hoje ele aparece como parte importante da contribuição, mas empiricamente seu resultado é fraco a negativo. Sem explicitação, o leitor pode entendê-lo como falha substantiva do policy layer ou como incoerência entre proposta e evidência.

**Onde**

- `methodology_body.tex`
- `experimental_design_body.tex`
- `results_body.tex`

**Sugestão**

Explicitar que ele funciona como ramo mais exigente e mecanisticamente interpretável, mas que, na configuração atual, não produziu ganho positivo. Isso transforma um possível problema narrativo em resultado informativo sobre dependência de parametrização e canal de intervenção.

## 6. Indicar explicitamente se houve múltiplos runs ou consolidação tardia de exports

**O que**

Esclarecer se as discrepâncias atuais decorrem de múltiplos exports/runs e qual deles é a versão canônica do paper.

**Por que**

Mesmo que você resolva as inconsistências numéricas, um revisor cuidadoso pode perceber rastros de reprocessamento e ficar em dúvida sobre qual run fundamenta as conclusões.

**Onde**

- possivelmente `appendix_b_body.tex`
- nota de reprodutibilidade ou nota metodológica curta

**Sugestão**

Uma nota sucinta basta: indicar que o manuscrito final referencia o export `outputs_v2` como versão canônica e que todos os números reportados foram harmonizados a partir dele.
