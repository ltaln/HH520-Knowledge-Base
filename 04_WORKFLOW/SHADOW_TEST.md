# Historical Shadow Test

## Purpose

Validate frozen Discovery rules on a later historical period without regenerating them.

## Canonical windows

Discovery:
- 2026-08-01 → 2026-08-31

Historical Shadow:
- 2026-09-01 → 2026-09-20

## Frozen discovery source

Request:
`hh520-research-20260801-20260831-a3f7c9d2`

## Shadow run

Request:
`hh520-shadow-20260901-20260920-c7f5a2d9`

GitHub Actions run:
`35597510169`

Result:
- READY
- validation matched: 302/302
- frozen rules: 29
- SHADOW_PASS: 22
- SHADOW_HOLD: 7

The engine currently uses PASS/HOLD, not a separate FAIL state.

## Policy

- validation start must be strictly after discovery end
- no rule regeneration
- no tuning during shadow
- SHADOW_PASS proceeds toward forward/manual review
- SHADOW_HOLD stays out of production
- Stable access forbidden
