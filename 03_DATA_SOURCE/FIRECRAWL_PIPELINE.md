# Firecrawl Pipeline

## Role

Firecrawl is the production web collection layer.

## Cost / Quota Principle

The project intentionally minimizes requests because Firecrawl quota is limited.

Rules:
- cache first
- one request per date/source by default
- persistent request ledger
- no blind retries
- preserve raw response before parsing
- allow controlled incomplete-request resume only for research recovery

## Collection Contract

Expected Firecrawl response:
- success = true
- data.markdown exists
- metadata source URL equals the exact expected 10027s URL
- HTTP status is valid

## Pacing

Research long-window collection added controlled pacing and 429 retry handling.

Historical notes:
- minimum interval was increased to around 2.5s
- retry policy focused on 429 cases
- cache resume logic was added for incomplete requests

## Production Principle

Firecrawl is a transport layer only.
It must not interpret football meaning.
