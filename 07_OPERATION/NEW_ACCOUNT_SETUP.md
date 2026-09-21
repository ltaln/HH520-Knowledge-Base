# New Account Setup

## Goal

A new ChatGPT/OpenAI account should be able to recover the HH520 project without access to the old account's memory.

## Minimum required access

The new account needs read access to:
- HH520-Knowledge-Base
- HH520-stable-V2

Write access is needed only if the new account will modify code/docs.

## First instruction to give the new account

```
请读取 GitHub 仓库 ltaln/HH520-Knowledge-Base。
先读取 00_PROJECT_MEMORY/WAKE_CODE.md，
再按照里面的顺序恢复 HH520 项目上下文。
然后检查 ltaln/HH520-stable-V2 当前 main 分支，
以代码为最终事实来源。
不要重新设计架构，也不要自动修改 Stable。
```

## What should be recovered

The new account should know:
- project goal
- production architecture
- 10027s source contract
- Stable V2.1
- Decision Filter V2
- Research isolation
- historical chronology
- key metrics
- current forward-validation task
- user command style
- operational constraints

## Important

Do not rely on “memory imported from another ChatGPT account.”
The GitHub knowledge base is the portable source of context.
