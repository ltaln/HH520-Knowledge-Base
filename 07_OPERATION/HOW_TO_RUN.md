# How To Run

## Normal prediction command

`预测 YYYY-MM-DD 全部比赛`

Expected production path:
- source: 10027s
- Stable version: V2.1
- Decision Filter V2 enabled

## GitHub Actions

Production workflow:
`.github/workflows/hh520-predict.yml`

Research workflow:
`.github/workflows/hh520-research.yml`

Historical Shadow:
`.github/workflows/hh520-shadow.yml`

Forward Test:
`.github/workflows/hh520-forward.yml`

## Local CLI

Canonical entry:
`python main.py "预测 YYYY-MM-DD 全部比赛"`

Optional GPT API mode is controlled by the production CLI flags/config in the code repository.

## Output expectations

Production should return fixed prediction fields and Decision Filter status.

Do not treat PASS as a failure; PASS is an intentional decision.

## Operational rule

Before changing any production logic:
1. read CURRENT_STATE
2. read DESIGN_DECISIONS
3. inspect current main branch in HH520-stable-V2
4. run CI after edits
5. update this knowledge base if architecture/behavior changes
