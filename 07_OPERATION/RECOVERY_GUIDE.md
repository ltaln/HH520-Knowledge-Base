# Recovery Guide

Use this when the old ChatGPT account, old chat history, computer or local environment is unavailable.

## Step 1 — Connect GitHub

Ensure the new AI/account can read:
- `ltaln/HH520-Knowledge-Base`
- `ltaln/HH520-stable-V2`

## Step 2 — Load project memory

Read in this order:

1. `00_PROJECT_MEMORY/WAKE_CODE.md`
2. `00_PROJECT_MEMORY/PROJECT_MEMORY.md`
3. `00_PROJECT_MEMORY/CURRENT_STATE.md`
4. `01_ARCHITECTURE/SYSTEM_ARCHITECTURE.md`
5. `05_HISTORY/DESIGN_DECISIONS.md`
6. `04_WORKFLOW/PREDICTION_WORKFLOW.md`
7. task-specific documentation

## Step 3 — Verify code truth

Knowledge base describes intended/current state, but executable truth is the current code repository.

Verify:
- `config/stable.yaml`
- `collector/service.py`
- `collector/hh520_10027_parser.py`
- `engine/decision_filter.py`
- `prediction/builder.py`
- `.github/workflows/`
- latest CI status

## Step 4 — Restore behavior

The new assistant should understand:

- source = 10027s
- Stable = V2.1
- Decision Filter V2 is active
- Research is isolated
- no auto-promotion
- forward validation begins 2026-09-21
- do not use post-match result fields for pre-match reasoning

## Step 5 — Continue

Use the user's preferred style:
- execute directly
- do not repeatedly ask for known information
- keep output compact
- preserve one-command workflow

## Recovery test

Ask the new assistant:

“告诉我 HH520 当前正式数据源、Stable 版本、Decision Filter 状态、Discovery/Shadow/Forward 时间线和禁止自动执行的事项。”

A correct recovery should match this knowledge base.
