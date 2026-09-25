# Changelog

## 2026-09-25 — Stable V3.5.1 Formal Draw + HT/FT Final

- Current formal version: HH520 Stable V3.5.1.
- Formal draw resolver: CROSS_FIT_LOGISTIC_V1_FORMAL.
- Draw gate: score >= 0.38 and pmax <= 0.45.
- Draw validation: Dev1 61.5%, Dev2 41.4%, Sep stress 55.6%.
- Formal HT/FT model: INDEPENDENT_POISSON_SPLIT_HTFT_V3_EXISTING_DATA.
- HT/FT historical labels: 1432.
- Frozen first-half shares: HOME 0.36, AWAY 0.44.
- No new Goal Timing data is required; timing_used=false.
- CI run 36093520930 succeeded.
- E2E run 36093576607 succeeded and returned READY.

## 2026-09-23 — Stable V3.4
- Upgraded formal prediction architecture from Stable V2.1 to Stable V3.4.
- Confirmed HH520 10027s as the only formal production source.
- Added Draw Anchor and HomeShare Side Layer.
- Repositioned Value Layer as diagnostic-only; it cannot modify direction.
- Replaced old probability/conflict logic with Market Failure Detector risk filtering.
- Formal risk states: CONFIRM / BALANCED / TAIL_ALERT / PASS.
- HT/FT changed to FT-conditioned generation.
- Score generation changed to HTFT-conditioned templates.
- Removed Pooled Poisson from the formal production chain.
- Removed external Goal Timing from the formal prediction chain.
- GPT role changed to EXPLANATION_ONLY.
- Stable V3.4 Action contract uses locked prediction output and strict 6-column rendering.

## 2026-09-23 — Research Isolation Review
- Confirmed Research Lab remains independent from Stable.
- Identified Research Action result routing issue requiring later repair.
- Prediction Action remains unaffected.

## 2026-09-21 — Knowledge Base V1.0
- Created external project-memory repository.
- Added wake/recovery documentation.
- Captured Stable architecture, research chronology and current status.

## 2026-09-21 — Stable V2.1
- Canonical source changed to 10027s.
- Decision Filter integrated into production prediction pipeline.
- Config renamed/versioned as Stable V2.1.
- GPT prompt aligned to 10027s + Decision Filter.
- Output exposed filter decision and score.

## Source migration
Earlier project stages used:
- 10023s
- 10013
- xi.php
- 10016
- 10017

Current production does not use those as formal prediction sources.
