# Module Map

Canonical code repository: `ltaln/HH520-stable-V2`

## collector/

- `service.py` — collection orchestration; default source 10027s
- `url_builder.py` — 10027s / legacy URL builders
- `hh520_10027_parser.py` — official 10027s parser
- `firecrawl_client.py` — Firecrawl access
- `cache_manager.py` — source/date cache and request ledger

## engine/

- `market_baseline.py` — de-vig 1X2
- `probability_layer.py` — final probability and direction
- `value_layer.py` — EV/Kelly/value fields
- `decision_filter.py` — Stable V2.1 Decision Filter V2

## analysis/

- `match_analysis.py` — combines Probability, Value, Filter and confidence
- confidence related logic

## prediction/

- `builder.py` — production prediction pipeline and GPT handoff
- `gpt.py` — optional GPT API final inference

## controller/

- command router
- execution manager
- ChatGPT action integration

## formatter/

- final output formatting

## research/

Major research modules:
- lab / runner
- score inference
- HTFT inference
- error attribution
- hidden model reverse
- DNA analysis
- candidate rules
- shadow test
- shadow runner
- chronology
- forward test
- forward runner
- decision_filter_v2 candidate research module

## .github/workflows/

- `hh520-predict.yml`
- `hh520-research.yml`
- `hh520-shadow.yml`
- `hh520-forward.yml`
- `ci.yml`

## integration/

- ChatGPT Action OpenAPI schema

## config/

- `stable.yaml` — Stable V2.1 canonical config

## prompts/

- Stable V2.1 prediction prompt
