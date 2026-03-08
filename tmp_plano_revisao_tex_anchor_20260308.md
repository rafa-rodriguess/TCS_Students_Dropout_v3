# Plano de revisao TEX (ancoragem e hipoteses por parametro)

Data: 2026-03-08
Escopo: `methodology_body.tex` e `experimental_design_body.tex`
Base: alinhado ao plano de revisao e ao contrato exportado em `outputs_v2`.

## 1) Objetivo editorial desta revisao

Fortalecer, em ambos os arquivos, a separacao entre:
- o que esta ancorado externamente na literatura;
- o que e parametro de cenario (hipotese de simulacao);
- o que e inspirado de forma analogica (nao replicacao literal).

Referencias-alvo:
- `KayBostock2023PowerNudge` (regra operacional e classe de intervencao).
- `SneyersDeWitte2018InterventionsMeta` (ancoragem conservadora de intensidade, `delta=0.08`).

## 2) Pre-condicao bibliografica (necessaria antes de citar no TEX)

Adicionar no `ref.bib` (se ainda nao existir):

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

## 3) Mapa de declaracao por parametro (ancora vs hipotese)

Aplicar a mesma leitura em `methodology_body.tex` e `experimental_design_body.tex`.

| Parametro | Valor(es) atual(is) | Status | Como declarar no texto | Fonte de ancora |
|---|---:|---|---|---|
| Trigger operacional (`r*`) | `r*=1` (`~7 dias sem LMS`) | Ancorado (regra) | `r*=1` e usado como proxy semanal da regra de 7 dias sem engajamento LMS | Kay & Bostock 2023 |
| Frequencia de checagem | semanal | Ancorado (protocolo operacional) | regra monitorada semanalmente | Kay & Bostock 2023 |
| Janela ativa (`W`) | `W=2` | Ancorado em classe/intervencao curta + congelado no contrato | janela curta de 2 semanas, coerente com short-term digital nudge | Kay & Bostock 2023 |
| `alpha_week0` | `0.35` | Hipotese de cenario (nao efeito causal identificado) | intensidade de cenario mechanism-aware, nao estimativa causal observada | (coerencia com narrativa Kay; nao "estimado por Kay") |
| `alpha_week1` | `0.10` | Hipotese de cenario | residual de cenario na semana 1, nao estimativa causal identificada | idem |
| `decay_type` | `kb2023_step_2w` | Hipotese estrutural inspirada | schedule de 2 semanas inspirado no padrao de nudge curto | Kay & Bostock 2023 (inspiracao estrutural) |
| `delta_shock_ablation` | `0.08`, `0.20`, `0.60` | Misto: 0.08 ancorado conservador; 0.20/0.60 hipoteticos | `delta` e parametro de intensidade de cenario | `0.08` com ancora qualitativa em Sneyers & De Witte 2018 |
| `reference_scenario_id` | `hypothetical_A` (`delta=0.20`) | Convencao de reprodutibilidade | referencia tecnica para compatibilidade de artefatos | Interno (nao claim substantivo) |

## 4) Alteracoes planejadas em `methodology_body.tex`

### 4.1 Subsecao de policy (RQ2)

Alterar o bloco atual para declarar explicitamente 3 camadas:
1. ancora operacional (trigger, frequencia, janela curta) em Kay & Bostock 2023;
2. intensidade de choque como parametro de cenario;
3. distincao entre cenario ancorado conservador e cenarios hipoteticos.

Insercoes propostas:
- frase de contrato:
  - "The intervention rule is anchored to Kay and Bostock (2023), while effect intensity is treated as a scenario parameter under observational non-identification."
- frase de cenarios `delta`:
  - "We report three `delta_shock_ablation` settings: 0.08 (anchored conservative scenario, qualitatively aligned with Sneyers and De Witte, 2018), 0.20 (hypothetical A), and 0.60 (hypothetical B stress-test)."
- frase de limite inferencial:
  - "These settings define structural scenario intensities and must not be read as causally identified effect sizes from OULAD."

### 4.2 Bloco mechanism-aware (covariantes)

Adicionar declaracao explicita de analogia para `total_clicks`:
- "The mechanism-aware perturbation is implemented primarily through click-based engagement (`total_clicks`) as an LMS proxy analogous to participation/re-engagement dynamics discussed by Kay and Bostock (2023), not as a literal replication of their measured variables."

Adicionar frase de escopo empirico:
- "In the exported run, the strongest material perturbation is concentrated on click-based engagement, so interpretation of broader covariate propagation remains conservative."

### 4.3 Reproducibilidade

Expandir referencia de artefatos para citar o pacote multicenario:
- `table_policy_scenarios_main.csv`
- `table_policy_deltaS_by_week_by_scenario.csv`
- `table_policy_deltaS_at_horizons_by_scenario.csv`

## 5) Alteracoes planejadas em `experimental_design_body.tex`

### 5.1 Reescrever "Scenario specification"

Trocar tabela unica de parametro fixo por duas tabelas:

Tabela A: "Operational anchor"
- Trigger rule: 7 dias sem LMS (`r*=1` como proxy semanal)
- Checking frequency: weekly
- Active window: `W=2`
- Intervention class: short-term digital nudge
- Source: Kay & Bostock 2023

Tabela B: "Scenario intensities"
- `anchored_conservative`: `delta=0.08` (anchored, Sneyers & De Witte 2018)
- `hypothetical_A`: `delta=0.20` (hypothetical, legacy baseline)
- `hypothetical_B`: `delta=0.60` (hypothetical, stress test)
- manter `alpha_week0=0.35`, `alpha_week1=0.10`, `decay_type=kb2023_step_2w` como effect schedule compartilhado

### 5.2 Ajustar texto dos regimes

No "Shock regime":
- explicitar que shock e avaliado por cenario (`delta` variando por `scenario_id`), nao por unico valor fixo.

No "Mechanism-aware regime":
- explicitar que uplift em clicks e principal canal modelado.
- incluir frase de analogia com participacao/re-engajamento de Kay & Bostock 2023.

### 5.3 Ajustar saida/horizontes em RQ2

Atualizar linguagem de output para:
- reportar `Delta S` por cenario em `T_policy` e `T_eval_policy`.
- manter separacao de horizonte tecnico de metricas (`T_eval_metrics`) sem misturar com narrativa de trajetoria.

## 6) Checklist de implementacao (ordem recomendada)

1. Inserir entrada `SneyersDeWitte2018InterventionsMeta` em `ref.bib`.
2. Revisar `methodology_body.tex` (policy contract + analogia em clicks + cenarios `delta`).
3. Revisar `experimental_design_body.tex` (duas tabelas: ancora operacional e intensidades de cenario).
4. Revisar citacoes no TEX:
   - `\cite{KayBostock2023PowerNudge}` nos blocos de ancora operacional e analogia de participacao.
   - `\cite{SneyersDeWitte2018InterventionsMeta}` no bloco de `delta=0.08`.
5. Conferir consistencia terminologica:
   - usar sempre "anchored conservative" para `0.08`.
   - usar sempre "hypothetical" para `0.20` e `0.60`.
6. Conferir consistencia com artefatos exportados em `outputs_v2`.

## 7) Frases-guia prontas para colar/adaptar

### 7.1 Methodology (anchor + hypothesis)

"We anchor the intervention rule to Kay and Bostock (2023): a weekly digital re-engagement nudge triggered after 7 days without LMS activity and applied over a two-week action window. Because OULAD does not identify intervention exposure or causal magnitude, effect intensity is treated as a scenario parameter."

"We therefore report three shock-intensity settings: `delta=0.08` (anchored conservative scenario, qualitatively aligned with Sneyers and De Witte, 2018), `delta=0.20` (hypothetical A), and `delta=0.60` (hypothetical B stress test)."

### 7.2 Methodology/Experimental Design (click analogy)

"In the mechanism-aware regime, the main covariate perturbation is implemented on click-based engagement (`total_clicks`) as an LMS proxy analogous to participation/re-engagement dynamics discussed by Kay and Bostock (2023), rather than as a literal variable-level replication of that study."

## 8) Criterio de aceite desta revisao

A revisao estara pronta quando:
- cada parametro estiver explicitamente classificado como ancora externa ou hipotese de cenario;
- `delta=0.08` estiver declarado como ancora conservadora com citacao de Sneyers & De Witte (2018);
- `delta=0.20` e `delta=0.60` estiverem declarados como hipoteticos;
- a inspiracao analogica em `total_clicks` (participacao/re-engajamento) estiver textual e inequivoca;
- os dois arquivos (`methodology_body.tex` e `experimental_design_body.tex`) estiverem semanticamente alinhados com `outputs_v2`.
