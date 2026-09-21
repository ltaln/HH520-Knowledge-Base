# Troubleshooting

## Prediction uses wrong source

Check:
- `collector/service.py` default source
- `config/stable.yaml`
- cache source token

Expected: 10027s.

## 10027 parser produces strange teams/odds

Check that base-table scanning stops before fusion-table rows.

Known fix:
base parser must break when it sees `FUSION_PREFIX`.

## Historical prediction appears suspiciously accurate

Check for result leakage:
- actual half score
- actual full score
- settlement/result fields
- post-match stats

Historical prediction must use pre-match-only fields.

## Firecrawl 429

Use:
- cache
- pacing
- controlled 429 retry
- incomplete-request resume only where explicitly allowed

Do not repeatedly rerun the same date blindly.

## GitHub Action response too large

Use compact:
`results/{request_id}.json`

Use archive/detail only when needed:
- `archive/{request_id}.json`
- `pages/{request_id}/manifest.json`

## Rule looks strong in one month

Do not promote from one period.

Require:
Discovery → later Shadow → Forward → Manual Review.

## CI fails after architecture migration

Distinguish:
- real production failure
- stale tests expecting legacy 10023s behavior

Update tests only when the new contract is intentional and documented.
Never “fix” CI by weakening safety checks without justification.
