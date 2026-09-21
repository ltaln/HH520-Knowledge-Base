# Decision Filter V2

## Status

Active in **HH520 Stable V2.1**.

Research origin:
- August 2026 Discovery
- September 1–20 Historical Shadow
- ~704 evaluable result labels across the two periods

## Purpose

Reduce low-quality predictions by adding a final gate after Probability and Value.

It outputs:
- `BET_CANDIDATE`
- `PASS`

It does not redefine the Probability Layer direction.

## Positive WDL Evidence

Cross-window factors used by the candidate design:

| Factor | Aug | Sep | Pooled |
|---|---:|---:|---:|
| away odds <1.50 | 84.21% | 72.41% | 79.10% / 67 |
| probability concentration >=60% | 79.17% | 70.59% | 75.61% / 82 |
| structure=强优 | 85.29% | 70.00% | 79.63% / 54 |
| pattern=🔶风控赔率 | 71.67% | 68.29% | 70.30% / 101 |
| home odds <1.50 | 71.05% | 66.67% | 69.29% / 127 |
| 客让半一低水/一球高水 | 75.00% | 58.33% | 68.33% / 60 |
| rating=B+ | 66.30% | 58.11% | 62.65% / 166 |
| risk=低 | 63.83% | 61.19% | 62.73% / 161 |

These are factor-level conditional accuracies, not the combined filter accuracy.

## Negative WDL Evidence

Persistent negative regions include:

- probability concentration <40%
- pattern=⚡极端
- pattern=⚠️边缘
- home/away odds 1.80–2.99
- risk=中高
- weak home handicap bucket

## Hard PASS

Current hard blocks:
- probability concentration <40%
- pattern=⚡ 极端

## Scoring

The implementation uses conservative weights based on the weaker cross-window uplift and a minimum decision score.

A positive signal is required.
Negative evidence can reduce the total score.
Hard PASS always blocks.

## Limitation

The final combined filter's real accuracy must be measured on forward data. Factor-level pooled rates cannot be added together because factors overlap.
