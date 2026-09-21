# HH520 Stable V2.1

## Definition

Stable V2.1 = previous Stable prediction chain + Decision Filter V2 + canonical 10027s source contract.

It is not a newly trained model.

## Production Order

```
10027s
→ Probability Layer
→ Value Layer
→ Decision Filter V2
→ GPT
→ Final Output
```

## What changed from Stable V2

Changed:
- canonical source moved to 10027s
- Decision Filter V2 became an active production gate
- production output exposes filter decision, score and reason
- prompt/config/docs/tests aligned to V2.1

Unchanged principles:
- Probability determines direction
- Value fields do not override direction
- GPT cannot override PASS
- Research cannot auto-update Stable
- no automatic training

## Source Safety

10027s base-table result fields are historical labels.
Finished matches must be skipped for pre-match inference.

## Validation

Stable V2.1 integration CI:
- GitHub Actions Run: 35601017542
- Result: SUCCESS
- Tests: 59 passed
