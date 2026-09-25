# Prediction Workflow

## User command

`预测 YYYY-MM-DD 全部比赛`

## Current Stable V3.5.1 sequence

```
Command Router
→ Plugin create request once
→ keep immutable request_id
→ GitHub Plugin Bridge
→ hh520-predict.yml
→ collect_date()
→ 10027s URL
→ Cache / Firecrawl
→ parse_10027s_markdown()
→ analyze_match()
→ Probability Layer
→ Market Failure Detector
→ Formal Draw Resolver
→ Decision Filter V3.5.1
→ Independent Score Layer
→ Independent HT/FT Layer
→ Consistency Layer
→ build_predictions()
→ format_prediction()
→ publish action-results
→ Plugin polling same request_id
→ READY
→ output_contract.display_rows
```

## Request lifecycle

1. Prediction request is created exactly once.
2. `request_id` is immutable.
3. PENDING / 404 never creates a second prediction request.
4. Poll the same result path until READY or FAILED.
5. READY output is authoritative.
6. User display reads only `output_contract.display_rows`.

## Formal draw behavior

Formal Draw Resolver can promote DRAW only when:
- FT raw direction is home/away
- FT grade is BALANCED
- `pmax <= 0.45`
- frozen draw logistic score `>= 0.38`

It cannot override an authorized `>=55%` side direction.

## HT/FT behavior

Formal model:

`INDEPENDENT_POISSON_SPLIT_HTFT_V3_EXISTING_DATA`

Rules:
- Uses formal H/D/A probabilities.
- Fits home/away goal intensities.
- Uses frozen historical first-half shares:
  - home = 0.36
  - away = 0.44
- Builds the 9-cell HT/FT distribution.
- Outputs Top2.
- No new Goal Timing collection is used or required.

## Score behavior

Score model:

`HDA_POISSON_V1`

It is independent from the locked FT direction and HT/FT model.

## PASS / downgrade behavior

A match can be downgraded because of:
- invalid/missing probability
- Market Failure Detector
- probability margin too small
- structural data incomplete
- FT × Score conflict
- other quality/risk conditions

PASS is a valid output state.

## Output contract

Strict 6 columns:

1. 球队对阵
2. 胜平负场景
3. 市场概率
4. 比分×2及概率
5. 半全场×2及概率
6. 总进球及概率

## Latest validation

- Full CI: success
- CI Run: `36093520930`
- E2E Run: `36093576607`
- Request ID: `hh520-20260925-drawhtft-final`
- Result: READY
- `timing_used = false`
- HT/FT Top2 output present in formal production result
