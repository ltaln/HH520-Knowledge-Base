# HH520 Stable V3.5.1

## Status

Current formal production model.

## Production source

HH520 10027s only.

## Core rules

- Formal probability base: de-vig 1X2 market.
- Draw Anchor: PD = 0.789 × PD_market + 0.211 × 25.74%.
- Home/Away split: HomeShare-based.
- Page probability: diagnostic only.
- Value Layer: diagnostic only.
- GPT: EXPLANATION_ONLY.

## FT calibration

Authorized side thresholds:

- STANDARD: >=55%
- STRONG: >=60%
- HIGH: >=65%

Below 55%, the system does not force a HOME/AWAY direction.

## Formal draw resolver

Model: CROSS_FIT_LOGISTIC_V1_FORMAL

Scope:

- balanced-side zone only
- pmax <= 0.45
- draw score >= 0.38
- cannot override authorized >=55% HOME/AWAY

Historical validation:

- Dev1: 61.5% (8/13)
- Dev2: 41.4% (12/29)
- Sep Stress: 55.6% (5/9)

## Formal HT/FT

Model: INDEPENDENT_POISSON_SPLIT_HTFT_V3_EXISTING_DATA

Uses already-collected half/full result labels only.

Frozen first-half shares:

- HOME: 0.36
- AWAY: 0.44

Labels:

- Dev1: 538
- Dev2: 592
- Stress: 302
- Total: 1432

Performance:

- Dev1 Top1 32.2%, Top2 53.3%
- Dev2 Top1 34.3%, Top2 52.7%
- Stress Top1 31.1%, Top2 48.7%

No new Goal Timing collection is required.

## Score model

HDA_POISSON_V1

Independent from FT hard direction and HT/FT.

## Output

Strict 6-column contract:

- 球队对阵
- 胜平负场景
- 市场概率
- 比分×2及概率
- 半全场×2及概率
- 总进球及概率

## Validation

- CI run 36093520930: success
- E2E run 36093576607: success
- request hh520-20260925-drawhtft-final: READY
- Formal HT/FT output confirmed with timing_used=false
