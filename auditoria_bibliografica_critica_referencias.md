# Auditoria Bibliografica Critica do Manuscrito

## PARTE 1 - Visao geral

O manuscrito nao esta mal referenciado no conjunto, mas a distribuicao do suporte bibliografico e desigual. O Background esta razoavelmente bem sustentado para framing de evasao, ensino online e desafios de intervencao; a Metodologia tambem cobre bem alguns pilares tecnicos centrais, em especial discrete-time survival, leakage e IPCW/Brier. O problema principal nao e falta generalizada de referencias, e sim sub-referenciamento seletivo nos pontos em que o texto sobe de descricao do proprio pipeline para afirmacoes de campo, de metodo ou de enquadramento conceitual.

Em termos academicos, o manuscrito parece bibliograficamente adequado no pano de fundo sobre evasao, OULAD, discrete-time survival e avaliacao sob censoring, mas sub-referenciado em quatro eixos: (i) a ponte conceitual entre predicao e acao institucional; (ii) o enquadramento de "structural counterfactual policy simulation" e "simulated regime contrasts"; (iii) a camada de fairness/subgroup evaluation; e (iv) alguns nomes de metodos e metricas usados como se fossem autoevidentes, sobretudo Random Survival Forest, ECE e C-index. Em resumo: o texto nao parece superconcentrado em poucos pontos, mas a densidade bibliografica esta mais forte em Background e em partes da Metodologia do que na costura conceitual que justifica a contribuicao.

As areas em que faltam referencias com maior urgencia sao: traducao de early warning em decisao/intervencao; relacao entre cenario estrutural e nao-identificacao causal; fairness/subgroup auditing; e ancoragem metodologica de baseline/metrica. As areas em que o suporte ja parece suficiente sao: relevancia geral da evasao, transformacao do ensino online em termos amplos, definicao da base OULAD, discrete-time hazard como backbone, e uso de IPCW/Brier em avaliacao com censoring.

## PARTE 2 - Lista priorizada de lacunas bibliograficas

### Categoria 1 - TEMOS de referenciar

## Item 1
### Classificacao
- Necessario acrescentar referencia

### Local
- Arquivo: introduction_body.tex
- Secao: abertura da Introducao e bloco "From prediction to policy"
- Ponto: afirmacao de que abordagens operacionais ainda enfatizam o "quem" mais do que o "quando", de que isso leva a suporte reativo ou mal temporizado, e de que passar de predicao para acao requer temporalizar risco, explicitar regras e comparar regimes.

### Tipo de problema
- precisa de referencia complementar

### O que
- O enquadramento que transforma predicao em decisao operacional esta apresentado como necessidade reconhecida da area, mas aparece apenas parcialmente coberto por surveys de dropout prediction.

### Por que isso e um problema
- Este e o nucleo motivacional do paper. Sem ancoragem especifica em literatura de early warning, intervention timing, decision support ou learning analytics orientada a acao, o argumento fica excessivamente autoral e parece mais um programa normativo do que uma lacuna documentada. Os surveys citados sustentam bem a parte "predicao de evasao", mas nao sustentam sozinhos a tese sobre traducao para acao institucional auditavel.

### O que esse trecho parece precisar
- revisao ou survey sobre early warning systems e intervention timing em ensino superior
- estudo empirico comparavel sobre dificuldades de converter scores de risco em acao institucional
- referencia metodologica sobre decision support em learning analytics

### Prioridade
- Alta

### Citacoes escolhidas a partir de new_citations.txt
- Mcmahon and Sembiante (2020, Review of Education): melhor ancora conceitual para sustentar a critica ao foco estreito em identificar quem esta em risco sem conectar previsao a intervencao significativa.
- Ortigosa et al. (2019, IEEE Transactions on Learning Technologies): melhor apoio empirico para mostrar um EWS em operacao real, com previsoes periodicas e registro de acoes.
- Plak et al. (2021, Higher Education Quarterly): util para sustentar o uso do modelo como suporte a decisao institucional, nao como substituto do aconselhamento.

### Observacao de uso
- Essas tres referencias bastam para este ponto. Nao recomendaria adicionar mais do que isso aqui.

### Texto-guia para LLM especializada
Procure literatura sobre a transicao de modelos preditivos de evasao estudantil para acao institucional concreta em ensino superior, com foco em early warning systems, intervention timing, decision support e regras operacionais de acionamento. Interessa especialmente trabalho que discuta a limitacao de scores estaticos do tipo "who is at risk" e a necessidade de incorporar dimensao temporal, timing de intervencao e protocolos auditaveis de tomada de decisao.

## Item 2
### Classificacao
- Necessario acrescentar referencia

### Local
- Arquivos: introduction_body.tex, background_body.tex, methodology_body.tex
- Secoes: introducao do "structural counterfactual policy simulation"; encerramento de "Intervention Practice Gaps"; "Policy Simulation", "Structural Survival Contrast" e "Interpretive Scope"
- Ponto: uso recorrente de expressoes como "structural counterfactual policy simulation", "simulated regime contrasts" e "not a substitute for causal inference" sem ancora teorica explicita.

### Tipo de problema
- sem referencia

### O que
- O manuscrito usa uma linguagem conceitualmente carregada, proxima de tradicoes de causal inference, structural modeling e counterfactual reasoning, mas sem indicar de que tradicao metodologica essa terminologia esta sendo herdada ou adaptada.

### Por que isso e um problema
- Aqui a ausencia de referencia enfraquece a validade academica do framing. O leitor pode interpretar "structural", "counterfactual" e "regime contrast" como termos tecnicos consolidados, quando o texto nao mostra qual linhagem metodologica legitima esse uso. Como essa distincao entre contraste estrutural e efeito causal identificado e central para a contribuicao, ela precisa de suporte externo para nao soar apenas como blindagem retorica.

### O que esse trecho parece precisar
- referencia metodologica classica sobre structural/counterfactual modeling
- trabalho sobre simulacao de regimes ou decision-analytic scenario comparison sob nao-identificacao causal
- referencia conceitual sobre limites entre contraste model-implied e efeito causal identificado

### Prioridade
- Alta

### Citacoes escolhidas a partir de new_citations.txt
- Igelstrom et al. (2022, Journal of Epidemiology and Community Health): melhor referencia curta e clara para distinguir associacao/predicao de estimacao causal com dados observacionais.
- Yao et al. (2020, ACM TKDD): bom survey para ancorar a diferenca entre linguagem causal forte e comparacoes que nao demonstram identificacao.
- Doutreligne and Varoquaux (2025, GigaScience): a melhor ponte entre uso de modelos preditivos para decisao e a cautela necessaria para nao vendelos como inferencia causal.

### Observacao de uso
- O arquivo traz boas referencias para sustentar a distincao entre comparacao de cenarios de modelo e efeito causal identificado.
- O arquivo nao traz uma referencia perfeita para legitimar exatamente a expressao "structural counterfactual policy simulation" como termo consolidado. O melhor uso aqui e apoiar a distincao metodologica, nao tentar provar que o rotulo em si e padrao da area.

### Texto-guia para LLM especializada
Procure referencias metodologicas que ajudem a ancorar o uso de termos como structural simulation, counterfactual policy simulation, regime contrast e model-implied comparison em contexto observacional sem identificacao causal. O objetivo e encontrar literatura que diferencie claramente comparacao de cenarios simulados por modelo de estimacao causal de efeito de intervencao, preferencialmente em predictive modeling, decision analysis, causal inference ou policy simulation.

## Item 3
### Classificacao
- Necessario acrescentar referencia

### Local
- Arquivos: methodology_body.tex, experimental_design_body.tex
- Secoes: "Fairness and Subgroup Track (RQ3)" e "Subgroup Evaluation Protocol (RQ3)"
- Ponto: a camada de subgroup/fairness e apresentada como trilha metodologica legitima, baseada em change-in-gap, metricas por grupo e bootstrap, mas sem ancoragem bibliografica especifica.

### Tipo de problema
- sem referencia

### O que
- O texto introduz uma logica de auditoria por subgrupos como se o leitor ja aceitasse que essa e uma forma reconhecida de validacao fairness-aware, mas nao cita literatura de fairness auditing, subgroup evaluation ou mesmo trabalhos educacionais que justifiquem esse tipo de leitura.

### Por que isso e um problema
- Fairness e subgroup analysis sao temas tecnicos e normativamente sensiveis. Sem referencia, o leitor nao sabe se a escolha por change-in-gap em survival trajectories e uma adaptacao propria, uma pratica derivada da literatura ou apenas uma heuristica ad hoc. Como RQ3 e uma perna substantiva do paper, a trilha precisa ser ancorada para ganhar legitimidade metodologica.

### O que esse trecho parece precisar
- survey ou referencia metodologica sobre algorithmic fairness / subgroup performance evaluation
- trabalho sobre fairness auditing em prediction/risk models
- se possivel, trabalho empirico em educational data mining / learning analytics com avaliacao por subgrupos

### Prioridade
- Alta

### Citacoes escolhidas a partir de new_citations.txt
- Pessach and Shmueli (2022, ACM Computing Surveys): melhor ancora geral para fairness auditing e group fairness sem amarrar o texto a um unico dominio aplicado.
- Lu et al. (2022, Frontiers in Digital Health): boa referencia operacional para auditoria por subgrupos com metricas por grupo, calibracao e intervalos de confianca.
- Opoku et al. (2025, Journal of Learning Analytics): a melhor referencia do conjunto para aproximar fairness/subgroup auditing do contexto educacional.

### Observacao de uso
- Essas tres referencias cobrem bem: quadro geral de fairness, pratica de auditoria por subgrupo, e proximidade com learning analytics.
- Eu nao usaria Cherian and Candes aqui como fonte principal, porque no arquivo aparece como arXiv e nao e a opcao mais limpa diante da exigencia de nao exagerar.

### Texto-guia para LLM especializada
Procure literatura sobre subgroup evaluation e fairness auditing em modelos preditivos, idealmente em learning analytics, educational data mining, risk prediction ou survival modeling. O foco deve ser referencias que justifiquem o uso de metricas por grupo, diferencas de gap entre regimes, incerteza por bootstrap e leituras de directional stability quando o objetivo nao e estimar efeito causal, mas auditar sensibilidade de subgrupos sob um mesmo protocolo.

## Item 4
### Classificacao
- Necessario acrescentar referencia

### Local
- Arquivos: methodology_body.tex, experimental_design_body.tex
- Secoes: "Benchmark Track"; "Model Variants in the Experiment"; "Non-temporal baseline (Random Survival Forest)"
- Ponto: o baseline Random Survival Forest e nomeado e usado na comparacao principal sem citacao do metodo.

### Tipo de problema
- sem referencia

### O que
- O manuscrito cita Cox, mas nao cita Random Survival Forest, apesar de RSF ser um metodo especifico e nao apenas uma descricao generica de baseline arboreo.

### Por que isso e um problema
- Quando um metodo entra como baseline formal do experimento, a referencia do metodo e obrigatoria. Sem isso, a secao de desenho experimental fica tecnicamente incompleta e assimetrica: um baseline classico esta ancorado, o outro nao.

### O que esse trecho parece precisar
- paper seminal ou referencia metodologica classica de Random Survival Forest
- opcionalmente, referencia de implementacao/uso em survival ML se o texto quiser justificar a versao utilizada

### Prioridade
- Media-Alta

### Citacoes escolhidas a partir de new_citations.txt
- Mogensen et al. (2012, Journal of Statistical Software): melhor referencia metodologica realmente utilizavel entre as listadas para descrever RSF em survival analysis.
- Huang et al. (2023, BMC Medical Research Methodology): boa referencia complementar para posicionar RSF como baseline recorrente em survival machine learning.

### Observacao de uso
- O arquivo nao traz a referencia seminal canonica de Random Survival Forest. Para este ponto, isso importa.
- Portanto, o uso correto e: Mogensen et al. como apoio metodologico complementar e busca separada da referencia original de Ishwaran et al. para fechar a lacuna de forma adequada.
- Se a equipe quiser ser estrita e minimalista, este e um dos pontos em que ainda falta a fonte ideal.

### Texto-guia para LLM especializada
Procure a referencia seminal e/ou metodologica classica de Random Survival Forest aplicada a analise de sobrevivencia, para sustentar o baseline nao temporal do manuscrito. Se houver literatura de survey sobre survival machine learning que situe RSF como baseline comparativo recorrente, ela tambem pode ser util como complemento.

## Item 5
### Classificacao
- Necessario acrescentar referencia

### Local
- Arquivo: experimental_design_body.tex
- Secao: "Primary and Secondary Metrics"
- Ponto: a suite formal de metricas inclui C-index e ECE, mas so o IPCW Brier recebe citacao explicita.

### Tipo de problema
- referencia insuficiente

### O que
- O texto trata C-index e ECE como metricas metodologicamente estabelecidas no desenho experimental, mas nao as ancora bibliograficamente quando sao introduzidas como instrumentos formais de avaliacao.

### Por que isso e um problema
- Em secao de desenho experimental, metricas nao triviais devem ser definidas ou ao menos ancoradas. A ausencia nao inviabiliza a leitura para quem ja conhece a area, mas empobrece a defensabilidade metodologica do protocolo e passa a impressao de suporte bibliografico seletivo.

### O que esse trecho parece precisar
- referencia metodologica classica de C-index para survival/ranking
- referencia metodologica de calibration error / ECE
- opcionalmente, survey de metricas de calibracao e discriminacao em risk prediction

### Prioridade
- Media

### Citacoes escolhidas a partir de new_citations.txt
- Uno et al. (2011, Statistics in Medicine): melhor referencia para C-index sob censura, inclusive como ancora forte e classica.
- Longato et al. (2020, Journal of Biomedical Informatics): boa referencia complementar para interpretacao pratica do C-index em modelos de tempo-ate-evento.
- Fan et al. (2021, BioData Mining, ou BMC Medical Informatics and Decision Making): melhor opcao disponivel no arquivo para definir ECE como erro medio de calibracao em bins.

### Observacao de uso
- Para C-index, o conjunto esta bom e suficiente.
- Para ECE, o arquivo oferece apoio metodologico util, mas nao uma referencia survival-specific canonica tao limpa quanto a do C-index. Eu usaria Fan et al. apenas para ancorar a definicao operacional de ECE, sem forcar uma narrativa de que se trata de referencia classica de survival.

### Texto-guia para LLM especializada
Procure referencias metodologicas para duas metricas usadas no desenho experimental: C-index em contexto de survival/risk ranking e Expected Calibration Error (ECE) em contexto de calibracao probabilistica. Sao especialmente uteis trabalhos que expliquem o papel dessas metricas em avaliacao de modelos de risco ou survival prediction, de preferencia com interpretacao adequada para horizontes temporais.

## Item 6
### Classificacao
- Necessario acrescentar referencia

### Local
- Arquivo: background_body.tex
- Secao: "Intervention Practice Gaps: Documentation, Evaluation, and Experimental Frictions"
- Ponto: a frase sobre ausencia de politicas de intervencao estruturadas e de logging consistente, com consequente inviabilidade de avaliacao causal crivel em dados rotineiros.

### Tipo de problema
- precisa de referencia complementar

### O que
- Ha referencias em torno do paragrafo para experimentacao e desafios de intervencao, mas o enunciado mais forte sobre falta de logging, granularidade insuficiente e barreiras para avaliacao causal aparece pouco sustentado de forma direta.

### Por que isso e um problema
- Esse e um ponto de apoio decisivo para justificar por que o paper opta por comparacao estrutural de cenarios em vez de avaliacao causal de intervencoes reais. Se a premissa empirica sobre documentacao insuficiente nao estiver bem ancorada, a justificativa do enquadramento perde lastro.

### O que esse trecho parece precisar
- estudo empirico ou revisao sobre implementacao/registro de intervencoes em ensino superior
- literatura sobre limites de dados observacionais rotineiros para causal evaluation em learning analytics
- referencia metodologica sobre intervention logging e treatment observability

### Prioridade
- Media-Alta

### Citacoes escolhidas a partir de new_citations.txt
- Sonderlund et al. (2018, British Journal of Educational Technology): melhor ancora para a afirmacao de que intervencoes em learning analytics frequentemente sao pouco documentadas ou metodologicamente fracas.
- Page et al. (2020, AERA Open): melhor apoio para o problema de mecanismos de atribuicao ao tratamento mal observados em estudos educacionais observacionais.
- Kitto et al. (2023, British Journal of Educational Technology): util para reforcar que sem modelos causais explicitos e sem variaveis de intervencao adequadas os dados de analytics nao sustentam bem inferencia causal.

### Observacao de uso
- Esse trio apoia muito bem a justificativa do paper para nao tratar os contrastes simulados como efeitos causais observacionalmente identificados.
- Eu nao colocaria mais do que essas tres aqui.

### Texto-guia para LLM especializada
Procure literatura empirica ou metodologica mostrando que dados educacionais observacionais de rotina frequentemente nao registram de forma confiavel quando uma intervencao ocorreu, quem a recebeu e em que condicoes, dificultando avaliacao causal crivel. Sao especialmente relevantes trabalhos em learning analytics, educational interventions, program evaluation ou causal inference aplicada a ambientes educacionais reais.

### Categoria 2 - Seria bom/otimo referenciar

## Item 7
### Classificacao
- Bom acrescentar referencia

### Local
- Arquivo: background_body.tex
- Secao: "Structural Shifts in Delivery: From Distance to Online"
- Ponto: paragrafo que afirma institucionalizacao do online/hibrido, migracao ampla de programas, redesenho pedagogico e centralidade dos LMS como infraestruturas de ensino e engajamento.

### Tipo de problema
- sem referencia

### O que
- O paragrafo faz uma extensao interpretativa plausivel da literatura sobre COVID e educacao online, mas a conexao entre migracao institucional, redesign pedagogico e centralidade analitica dos LMS ficou sem ancora direta.

### Por que isso e um problema
- Nao compromete a tese central, mas enfraquece a densidade intelectual do Background. Uma citacao aqui deixaria mais claro que a transicao para ambientes mediados por LMS nao e apenas intuicao contextual, mas um fato documentado por pesquisa recente.

### O que esse trecho parece precisar
- revisao recente sobre ensino online/hibrido e redesign pedagogico
- estudo sobre LMS como infraestrutura central de ensino e learning analytics

### Prioridade
- Media

### Citacoes escolhidas a partir de new_citations.txt
- Rapanta et al. (2021, Postdigital Science and Education): melhor ancora conceitual para redesenho pedagogico e novo normal pos-pandemia.
- Otto et al. (2023, Education and Information Technologies): boa referencia de revisao para praticas digitais student-centered e consolidacao de ambientes mediados por tecnologia.

### Observacao de uso
- Este ponto ja esta parcialmente sustentado por referencias presentes no manuscrito e no `ref.bib`, como Turnbull (2021).
- Se for acrescentar algo novo, eu limitaria a uma ou duas dessas referencias; nao ha necessidade de expandir muito.

### Texto-guia para LLM especializada
Procure estudos ou revisoes que documentem como a expansao do ensino online e hibrido levou a redesenho pedagogico, institucionalizacao do LMS e consolidacao desses sistemas como infraestrutura central de ensino, engajamento e analytics em ensino superior. Interessa sobretudo literatura pos-COVID ou comparativa entre modalidades.

## Item 8
### Classificacao
- Bom acrescentar referencia

### Local
- Arquivo: background_body.tex
- Secao: ainda em "Structural Shifts in Delivery"
- Ponto: afirmacao de que analises comparativas entre disciplinas e niveis de estudo permanecem limitadas e de que boa parte da literatura se concentra em MOOCs.

### Tipo de problema
- referencia insuficiente

### O que
- O texto ja cita um survey, mas a afirmacao sobre lacuna comparativa e concentracao em MOOCs e suficientemente especifica para pedir apoio adicional, idealmente mais recente ou mais focado em escopo de corpus.

### Por que isso e um problema
- Nao chega a comprometer a validade da secao, mas o argumento de lacuna fica mais convincente se amparado por uma revisao recente ou mapeamento sistematico. Hoje a frase soa correta, mas levemente subdemonstrada.

### O que esse trecho parece precisar
- revisao sistematica recente
- mapping study sobre educational dropout prediction / learning analytics
- estudo comparativo sobre distribuicao de corpus entre MOOCs e higher education formal

### Prioridade
- Media

### Citacoes escolhidas a partir de new_citations.txt
- Sghir et al. (2022, Education and Information Technologies): melhor referencia ampla para heterogeneidade do campo e forte dependencia de dados de LMS.
- Alalawi et al. (2023, Engineering Reports): melhor apoio direto para a afirmacao de concentracao de modelos de dropout em ambientes online e MOOC.
- Moreno-Marcos et al. (2019, IEEE Transactions on Learning Technologies): a melhor referencia especifica se o texto quiser nomear explicitamente o foco em MOOCs.

### Observacao de uso
- Para nao exagerar, duas referencias ja bastam aqui: Sghir + Alalawi.
- Moreno-Marcos so vale a pena se a redacao mencionar MOOCs de forma explicita.

### Texto-guia para LLM especializada
Procure revisoes sistematicas ou mapping studies recentes sobre student dropout prediction em learning analytics ou educational data mining que discutam concentracao da literatura em MOOCs e limitada comparacao entre disciplinas, cursos ou niveis de estudo. O objetivo e sustentar a afirmacao de lacuna de cobertura e heterogeneidade do campo.

## Item 9
### Classificacao
- Bom acrescentar referencia

### Local
- Arquivo: introduction_body.tex
- Secao: paragrafo sobre OULAD e portabilidade potencial do framework
- Ponto: afirmacao de que o framework e potencialmente portavel sempre que houver rastros temporais e um endpoint tempo-ate-evento bem definido.

### Tipo de problema
- sem referencia

### O que
- Trata-se de uma afirmacao de escopo externo e condicoes de aplicabilidade, nao de um resultado medido no paper.

### Por que isso e um problema
- Nao e obrigatoria, porque a frase esta formulada com cautela. Ainda assim, uma citacao melhoraria o posicionamento do paper em dialogo com literatura sobre transferibilidade de pipelines de learning analytics, generalizacao entre LMS ou requisitos minimos de infraestrutura temporal para modelagem de risco.

### O que esse trecho parece precisar
- estudo comparativo ou survey sobre portabilidade/transferibilidade em learning analytics
- referencia metodologica sobre requisitos de dados para temporal risk modeling

### Prioridade
- Media-Baixa

### Citacoes escolhidas a partir de new_citations.txt
- Nenhuma nova citacao do arquivo e especialmente forte para este ponto no recorte exato do manuscrito.

### Observacao de uso
- Eu nao forçaria nova citacao aqui.
- Se a equipe quiser mesmo ancorar essa frase, as melhores opcoes continuam sendo as que ja estao no projeto: Arroyo-Barriguete (2021) e Lagus (2018), presentes em `ref.bib`.
- Entre as novas sugestoes, Knight et al. (2017) ajuda mais a falar de temporalidade em learning analytics do que de portabilidade propriamente dita.

### Texto-guia para LLM especializada
Procure literatura sobre portabilidade, transferibilidade ou generalizacao de pipelines de learning analytics baseados em logs temporais, especialmente quando o problema e modelado como risco temporal ou time-to-event. Interessa tambem referencia que explicite quais requisitos de dados tornam esse tipo de framework aplicavel em diferentes contextos institucionais.

## Item 10
### Classificacao
- Bom acrescentar referencia

### Local
- Arquivo: experimental_design_body.tex
- Secao: "Policy Evaluation Protocol (RQ2)"
- Ponto: frase que diz que a perturbacao em clicks e um proxy de LMS analogous to participation/re-engagement dynamics in Kay and Bostock (2023), mas sem comando de citacao no proprio trecho e sem apoio adicional para a traducao do mecanismo.

### Tipo de problema
- precisa de referencia complementar

### O que
- O texto faz um salto entre a ancora de intervencao e a operacionalizacao concreta do mecanismo no pipeline, mas essa traducao de "engagement intervention" para perturbacao em click-based activity merece suporte mais local.

### Por que isso e um problema
- Nao invalida o protocolo, porque a ancora geral de Kay e Bostock ja aparece antes. Ainda assim, uma referencia local ou complementar ajudaria a mostrar que o uso de clicks como proxy de reengajamento nao e arbitrario e conversa com literatura sobre participacao digital e LMS activity.

### O que esse trecho parece precisar
- estudo empirico sobre click activity como proxy de engagement/re-engagement
- artigo sobre nudges digitais ou participacao em LMS
- ou citacao local explicita da ancora ja usada no manuscrito

### Prioridade
- Media-Baixa

### Citacoes escolhidas a partir de new_citations.txt
- Ahmadi et al. (2023, IRRODL): melhor referencia para tratar logs e clicks de LMS como indicadores pragmaticos de engajamento.
- Nkomo et al. (2021, International Journal of Educational Technology in Higher Education): importante para qualificar o argumento e evitar exagero, deixando claro que clique e proxy parcial, nao engajamento pleno.
- Brown et al. (2023, Education Sciences): opcao complementar se o texto quiser ligar logs de engajamento a nudges/intervencoes leves.

### Observacao de uso
- Este e um ponto onde vale mais a qualidade do encaixe do que a quantidade.
- Eu usaria Ahmadi + Nkomo, e Brown apenas se o paragrafo realmente enfatizar nudge/re-engagement.

### Texto-guia para LLM especializada
Procure estudos que relacionem atividade em LMS, clicks, participacao digital ou re-engagement com intervencoes leves, nudges ou recuperacao de engajamento em ensino superior. O objetivo e sustentar a decisao de usar perturbacao em clicks como proxy mecanistico para um regime de suporte, mesmo quando nao ha replicacao literal das variaveis do estudo ancora.

## Item 11
### Classificacao
- Bom acrescentar referencia

### Local
- Arquivo: results_body.tex
- Secao: "Benchmark and Robustness Evidence"
- Ponto: duas leituras interpretativas: (i) que a queda maior sem Recency/Streak e consistente com o papel de regularidade temporal na qualidade do hazard; (ii) que a variacao de AUC entre runs revela heterogeneidade de curso-run e escala esperada de domain sensitivity.

### Tipo de problema
- sem referencia

### O que
- Os resultados em si sao do paper e nao pedem citacao. O ponto e a interpretacao substantiva dada a eles, que poderia dialogar melhor com literatura sobre temporal engagement regularity, domain shift e heterogeneidade entre cursos/runs em educational ML.

### Por que isso e um problema
- Nao compromete a validade imediata dos Resultados, mas deixa a secao um pouco isolada da literatura. Uma ou duas referencias fariam a interpretacao soar menos autoral e mais cumulativa.

### O que esse trecho parece precisar
- estudo empirico sobre regularidade temporal de engajamento e risco academico
- referencia sobre domain shift / portability / heterogeneidade entre course runs em educational prediction

### Prioridade
- Baixa

### Citacoes escolhidas a partir de new_citations.txt
- Chen and Cui (2020, Journal of Learning Analytics): melhor referencia para defender que series temporais simples de comportamento no LMS melhoram generalizacao e desempenho em relacao a agregados estaticos.
- Lee et al. (2025, Journal of Learning Analytics): melhor referencia para heterogeneidade entre cursos/runs e variacao da importancia de padroes temporais entre contextos.

### Observacao de uso
- Essas referencias sao suficientes se houver interesse em conectar a interpretacao dos resultados a literatura.
- Como este item e de baixa prioridade, eu so acrescentaria citacao aqui se o objetivo for deixar a secao de resultados mais dialogica.

### Texto-guia para LLM especializada
Procure estudos em learning analytics ou educational machine learning que liguem regularidade temporal de engajamento, recency/streak, padroes semanais de atividade ou heterogeneidade entre course runs ao desempenho de modelos preditivos. Sao uteis referencias que ajudem a interpretar tanto a importancia de features temporais quanto a variabilidade de AUC sob mudanca de curso ou run.

## PARTE 3 - Sintese por secao

### abstract_body.tex
O abstract nao me parece bibliograficamente problematico. Em abstracts, a ausencia de citacoes e uma convencao aceitavel, e aqui o texto esta focado em apresentar proposta, resultados e limites. Eu nao trataria esse arquivo como ponto de intervencao bibliografica.

### introduction_body.tex
E a secao mais bibliograficamente sensivel depois da Metodologia. Ela esta bem sustentada no enquadramento geral sobre evasao e sobre a predominancia de modelos estaticos, mas fica fragil na ponte conceitual entre predicao e acao, no uso de "structural counterfactual policy simulation" e na alegacao de portabilidade. Faltam referencias necessarias para o framing central do paper e cabem referencias adicionais para dialogar com literatura de early warning, decision support e transferibilidade.

### background_body.tex
No geral, e uma das secoes mais bem sustentadas do manuscrito. O problema nao e falta generalizada, e sim dois pontos localizados: o paragrafo sobre institucionalizacao do online/LMS, que esta sem citacao direta, e a justificativa sobre ausencia de logging/intervencao observavel, que precisaria de ancoragem mais forte por ser premissa do enquadramento nao-causal. Alem disso, a frase sobre concentracao da literatura em MOOCs pode ganhar apoio adicional recente.

### methodology_body.tex
Esta secao sustenta bem discrete-time survival, leakage e IPCW. As fragilidades aparecem quando ela deixa a camada tecnica padrao e passa para a camada conceitual propria: simulated regime contrast, structural framing e fairness/subgroup track. O baseline RSF tambem precisa de citacao do metodo. Portanto, a secao esta tecnicamente razoavel, mas bibliograficamente fragil naquilo que pretende ser mais original.

### experimental_design_body.tex
O desenho experimental esta operacionalmente claro, mas bibliograficamente um pouco seletivo. OULAD, Cox e IPCW Brier estao ancorados; RSF, ECE e C-index nao. A parte de subgroup evaluation tambem carece de ancoragem bibliografica mais explicita. Eu descreveria a secao como metodologicamente clara, porem parcialmente sub-referenciada em metricas e baselines.

### results_body.tex
Como secao de resultados, ela nao precisa ser densamente citada, e em geral nao esta fraca por falta de referencias. Ainda assim, duas interpretacoes poderiam ganhar reforco util: a leitura sobre recency/streak como sinal de regularidade temporal relevante e a leitura sobre variabilidade entre runs como evidência de domain sensitivity. Sao melhorias desejaveis, nao obrigatorias.

### appendix_a_body.tex
O apendice esta adequado sem novas referencias. Ele descreve diagnosticos suplementares e artefatos exportados, nao faz afirmacoes de estado da arte nem escolhas metodologicas novas que exijam suporte externo adicional.

### appendix_b_body.tex
Tambem esta adequado. E uma secao de reprodutibilidade e disponibilidade de codigo, focada em rastreabilidade do pipeline. Nao identifiquei lacunas bibliograficas relevantes aqui.

## Fechamento

Diagnostico sintetico: o manuscrito nao precisa de uma grande expansao bibliografica difusa. Ele precisa de algumas insercoes pontuais e estrategicas, sobretudo onde o texto converte uma proposta propria em afirmacao sobre pratica reconhecida, framing metodologico ou legitimidade de fairness/subgroup evaluation. Se esses poucos pontos forem reforcados, o ganho de robustez academica sera desproporcionalmente alto em relacao ao numero de novas citacoes.