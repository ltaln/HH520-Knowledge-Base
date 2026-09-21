# System Architecture

## Production Path

```
User
 ↓
ChatGPT / CLI
 ↓
GitHub Actions or local execution
 ↓
Firecrawl
 ↓
HH520 10027s
 ↓
collector/hh520_10027_parser.py
 ↓
Market Baseline
 ↓
Probability Layer
 ↓
Value Layer
 ↓
Decision Filter V2
 ↓
GPT Final Analysis
 ↓
Formatter / JSON Result
```

## Research Path

```
10027s Historical Data
 ↓
Historical Sanitizer
 ↓
Prediction Snapshot Layer
 ↓
Result Label Layer
 ↓
Error Attribution
 ↓
Hidden Model Reverse
 ↓
League DNA / Team DNA
 ↓
Candidate Rule Engine
 ↓
Historical Shadow
 ↓
Forward Test
 ↓
Manual Review
```

## Isolation Rule

Production and Research share source semantics but not authority.

- Production may consume only approved Stable rules.
- Research may inspect historical production-compatible inputs.
- Research may never directly overwrite Stable rules or weights.
- Any promotion requires explicit manual review.

## Infrastructure Philosophy

Avoid:
- permanent server dependency
- long-term database
- complex queues
- automatic retraining
- hidden state that cannot be reconstructed

Prefer:
- GitHub as code/rule source of truth
- Firecrawl for source retrieval
- cache for cost control
- JSON/Markdown artifacts
- chat-native command entry
