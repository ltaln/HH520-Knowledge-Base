# Research Workflow

## Purpose

Research exists to learn from historical data without contaminating production Stable.

## Main flow

```
Historical 10027s
→ Sanitizer
→ Prediction Snapshot
→ Result Labels
→ Backtest Metrics
→ Error Attribution
→ Hidden Model Reverse
→ DNA
→ Candidate Rule Engine
→ Shadow
→ Forward
```

## Research modules

- Backtest / metrics
- Hidden Model Reverse
- Risk Analysis
- League DNA
- Team DNA
- Causal-style descriptive analysis
- Confidence analysis
- Candidate Rules
- Shadow Test
- Forward Test

## Rule status

Research conclusions are:
- RESEARCH_ONLY
- RESEARCH_SIGNAL_ONLY
- CANDIDATE_ONLY

They are not Stable rules until manual approval.

## Candidate thresholds used in V3.1

- MIN_SAMPLE = 20
- MIN_DATES = 3
- MIN_DELTA = 0.08
- MIN_POSITIVE_DATE_RATIO = 0.60
- MIN_LEAGUES_FOR_GLOBAL = 2

Common rejection reasons:
- SAMPLE_LT_20
- DATES_LT_3
- DELTA_LT_8PP
- DATE_STABILITY_LT_60PCT
- GLOBAL_RULE_SINGLE_LEAGUE

## Promotion rule

No auto-promotion.
Final policy: `MANUAL_REVIEW_REQUIRED`.
