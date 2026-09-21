# Prediction Workflow

## User command

`预测 YYYY-MM-DD 全部比赛`

## Current Stable V2.1 sequence

```
Command Router
→ collect_date()
→ 10027s URL
→ Cache / Firecrawl
→ parse_10027s_markdown()
→ analyze_match()
→ Probability Layer
→ Value Layer
→ Decision Filter V2
→ build_predictions()
→ GPT (optional/final)
→ format_prediction()
```

## PASS behavior

A match can be PASS because:
- invalid/missing probability
- Decision Filter hard block
- no validated positive evidence
- negative evidence cancels positive evidence
- GPT conflicts with direction
- score/HTFT/goals inconsistent
- supporting evidence insufficient

PASS is a valid output. The system must not force a prediction.

## Finished-match handling

If an actual result is present, the production pre-match builder skips the match.

## Output

Current target format includes:
- teams
- score x2
- HTFT x2
- total goals
- direction
- confidence
- Decision Filter decision
- Decision Filter version/score
- status
- reason

## A/B evaluation concept

Keep original Stable direction visible internally and compare it against the final Stable V2.1 gate so forward evaluation can answer:
- how many matches were filtered
- how many errors were removed
- how much coverage was lost
- whether conditional accuracy improved
