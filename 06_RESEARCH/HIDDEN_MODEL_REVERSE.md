# Hidden Model Reverse

## Purpose

Infer which 10027s pre-match factors are associated with stronger or weaker prediction performance without claiming access to HH520's proprietary internal formula.

This is descriptive reverse engineering only.

## Factors retained from 10027s

- odds_judgement
- draw_odds
- fusion_draw
- advantage_diff
- draw_composite_score
- advantage_side
- structure
- consistency
- pattern
- rating_score
- rating
- risk
- handicap
- ignore
- probability concentration bucket
- home odds bucket
- draw odds bucket
- away odds bucket
- league

## Output status

Signals are:
`RESEARCH_SIGNAL_ONLY`

They do not automatically alter Stable.

## Important findings across Aug + Sep

Positive WDL regions:
- away odds <1.50
- top probability >=60%
- structure=强优
- pattern=🔶风控赔率
- home odds <1.50
- risk=低
- rating=B+

Negative WDL regions:
- top probability <40%
- pattern=⚡极端
- pattern=⚠️边缘
- risk=中高
- home/away odds around 1.80–2.99

## Interpretation rule

Do not treat correlation as causation.
Do not sum factor accuracies because factors overlap.
Use later-window validation before promotion.
