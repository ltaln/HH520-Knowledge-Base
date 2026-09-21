# Forward Test

## Purpose

Determine whether frozen rules remain useful on genuinely future data.

## Canonical start

2026-09-21 onward.

## Eligibility

Only rules with `SHADOW_PASS` may enter Forward Test.

## Forward implementation

Code repository includes:
- `research/forward_test.py`
- `research/forward_runner.py`
- `.github/workflows/hh520-forward.yml`

ChatGPT Action schema includes:
- `startHH520ForwardTest`

## Forward statuses

- FORWARD_PASS
- FORWARD_HOLD

## Rules

- starts after the historical shadow period
- frozen rules only
- no regeneration
- no tuning against forward results
- no automatic Stable promotion
- final transition requires Manual Review

## What to measure

For Decision Filter V2 and frozen candidate rules:
- sample count
- coverage
- hits
- accuracy
- baseline
- delta vs baseline
- change vs discovery/shadow
- date count
- league count

Forward validation is the primary protection against historical overfitting.
