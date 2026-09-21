# Problem / Fix History

## ResponseTooLarge
Problem:
Research reports became too large for direct Action polling.

Fix:
- compact polling file: `results/{request_id}.json`
- full archive: `archive/{request_id}.json`
- paged detail manifests under `pages/`

## Firecrawl 429 / long-window collection
Problem:
Long historical windows triggered rate limits or incomplete request markers.

Fixes:
- pacing around 2.5 seconds
- retry targeted at HTTP 429
- controlled incomplete request resume
- longer workflow timeout
- cache restore/save in GitHub Actions

## Chronology contamination risk
Problem:
September candidate generation could not be validated on the same September period.

Fix:
Canonical chronology reset:
- Aug Discovery
- Sep Historical Shadow
- Sep 21+ Forward

## 10027s parser base/fusion confusion
Problem:
Base-table scan could continue into later fusion tables and misread rows.

Fix:
Stop base scan when fusion header begins.

## Prediction-vs-result confusion
Problem:
10027s score columns could be interpreted as predicted scores.

Fix:
Explicit semantics:
- base HT/FT = actual labels
- raw predicted score/goals unavailable unless explicitly present
- research-derived outputs labeled separately

## Workflow result size
Problem:
Full research JSON unsuitable for simple polling.

Fix:
Compact summary + archive + paged details.
