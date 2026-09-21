# Data Flow

## 1. Input Command

Typical user command:

`预测 YYYY-MM-DD 全部比赛`

Command router resolves the date.

## 2. Source URL

```
https://www.hh520.com/tx/10027s.php
?riqi_start=YYYY-MM-DD
&riqi_end=YYYY-MM-DD
&threshold=1
&bankroll=5000
```

## 3. Collection

Firecrawl retrieves markdown.

Rules:
- same date/source cache first
- no blind force refresh
- preserve request ledger
- avoid repeated paid requests
- source URL must exactly match expected URL

## 4. Parsing

10027s parser separates:
- settlement/base table
- fusion summary tables

Important:
- do not parse fusion rows as base rows
- actual HT/FT result fields are labels, not predictions

## 5. Probability

Priority:
1. complete page_probability
2. de-vigged 1X2 market baseline

Direction is the top probability.

## 6. Value

EV / Kelly / page signal are retained.

They do not determine direction.

## 7. Decision Filter V2

Takes validated factors from 10027s and outputs:
- BET_CANDIDATE
- PASS

It may block a prediction but does not redefine Probability Layer direction.

## 8. GPT

Only candidates allowed by Decision Filter proceed to final GPT output.

GPT output must pass consistency checks:
- direction
- two distinct scores
- HTFT alignment
- total goals compatible with scores
- evidence availability

## 9. Output

Fixed user-facing structure:
- match
- score x2
- HTFT x2
- total goals
- direction
- confidence
- Decision Filter result
- status
- reason
