# Experiment Log

## Score inference

Module:
`research/score_inference.py`

Method:
Independent Poisson model fitted from pre-match 1X2 probabilities.

Outputs:
- top 2 score candidates
- derived total goals

Labels:
- `score_source=RESEARCH_DERIVED`
- `total_goals_source=RESEARCH_DERIVED_FROM_SCORE`
- `research_score_model.source=RESEARCH_POISSON_1X2`

This is not raw HH520 score data.

## HTFT inference

Module:
`research/htft_inference.py`

Method:
Uses score-model lambdas and a fixed first-half share assumption.

Current assumption:
`HALF_SHARE = 0.45`

Status:
Uncalibrated assumption; do not claim improvement without validation.

Derived labels:
- `htft_source=RESEARCH_DERIVED_POISSON`
- `research_htft_model.source=RESEARCH_POISSON_HTFT`

## August Discovery result

Request:
`hh520-research-20260801-20260831-a3f7c9d2`

Results:
- collected 403
- matched 402
- WDL 53.98%
- exact score 14.43%
- score Top2 25.12%
- HTFT 32.84%
- HTFT Top2 47.26%
- total goals 21.39%
- 29 candidate rules

## September Research

Request:
`hh520-research-20260901-20260920-z9b4d6e2`

Results:
- 302 clean / matched
- WDL 47.02%
- exact score 12.91%
- score Top2 20.86%
- HTFT 29.47%
- HTFT Top2 46.36%
- goals 21.85%

## Historical Shadow

Request:
`hh520-shadow-20260901-20260920-c7f5a2d9`

Run:
`35597510169`

Outcome:
- 22 SHADOW_PASS
- 7 SHADOW_HOLD

## Stable V2.1 Decision Filter

Joint Aug/Sep evidence was used to create a conservative cross-window gate.
Final combined-filter accuracy is not yet established; Forward Test is responsible for that.
