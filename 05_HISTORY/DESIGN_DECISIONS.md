# Design Decisions

## D001 — Chat-native execution
User should be able to trigger work with one command from phone or desktop. Avoid requiring manual server maintenance.

## D002 — Firecrawl as official collector
Firecrawl is the supported collection path for HH520 pages.

## D003 — No long-term database
Historical data may be cached or processed for research, but the architecture should not depend on a permanent database.

## D004 — Canonical source moved to 10027s
Legacy source 10023s was superseded by 10027s as the official production source.

## D005 — Result labels are isolated
10027s base-table halftime/fulltime scores are actual results and must never leak into pre-match prediction.

## D006 — Probability owns direction
Probability Layer selects home/draw/away. Value Layer cannot override direction.

## D007 — Decision Filter is a gate
Decision Filter may PASS or allow a candidate, but should not silently rewrite the direction.

## D008 — Research and Stable are separate
Research has no authority to automatically mutate Stable.

## D009 — Candidate promotion requires evidence
Rule lifecycle:
Discovery → Historical Shadow → Forward Test → Manual Review.

## D010 — No automatic training/tuning
Offline research can compare candidates, but cannot auto-change production parameters.

## D011 — Derived outputs must be labeled
If score, HTFT or goals are inferred rather than raw HH520 fields, their source must be explicit.

## D012 — Cost-aware collection
Cache and request ledger are first-class architecture features because collection quota matters.

## D013 — Knowledge is externalized
Project context belongs in this GitHub knowledge base rather than relying on ChatGPT account memory.
