# Value Layer

## Purpose

Preserve value information without allowing value fields to become an unverified direction engine.

Inputs may include:
- EV
- Kelly
- page value/signal

Output marks:
- `used_for_direction: false`

## Rule

High EV or Kelly alone does not mean a match should be selected.

The Decision Filter receives value-layer output, but direction remains owned by Probability Layer.

## Design reason

Earlier project stages showed that page recommendation/value signals could differ materially from actual accuracy. The project therefore separates:

- Probability = what direction looks most likely
- Value = market/value context
- Decision Filter = whether the opportunity should be allowed
