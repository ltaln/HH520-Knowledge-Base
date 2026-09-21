# HH520 Knowledge Base

这是 HH520 足球预测项目的长期知识库（Project Memory / AI Handoff Repository）。

目标：让任何新的 ChatGPT 账号、AI 助手、Codex 会话或开发环境，在不依赖旧聊天记录和账号记忆的情况下，通过读取本仓库即可恢复项目上下文、当前架构、历史决策、研究结论、运行方式和后续工作。

## 当前正式状态

- 正式预测版本：**HH520 Stable V2.1**
- 正式数据源：**HH520 10027s**
- 正式代码仓库：`ltaln/HH520-stable-V2`
- 采集：Firecrawl
- 主链：10027s → Probability Layer → Value Layer → Decision Filter V2 → GPT → Output
- Research：独立、只读 Stable、Candidate Only
- Historical Shadow：29 条候选规则中 22 PASS / 7 HOLD
- Forward Test：框架已建立，从 2026-09-21 起验证
- Stable V2.1 CI：**59 passed**

## 新账号恢复

新账号或新 AI 第一次接手时，按顺序读取：

1. `00_PROJECT_MEMORY/WAKE_CODE.md`
2. `00_PROJECT_MEMORY/PROJECT_MEMORY.md`
3. `00_PROJECT_MEMORY/CURRENT_STATE.md`
4. `01_ARCHITECTURE/SYSTEM_ARCHITECTURE.md`
5. `05_HISTORY/DESIGN_DECISIONS.md`
6. `07_OPERATION/NEW_ACCOUNT_SETUP.md`

然后再根据任务读取对应模块文档。

## 核心原则

1. 10027s 是当前唯一正式采集源。
2. Stable 与 Research 必须隔离。
3. Research 不得自动修改 Stable。
4. 不建立长期数据库。
5. 不把真实赛果字段混入赛前预测。
6. Decision Filter 是最终放行层，不改变 Probability Layer 的方向定义。
7. 所有新规则必须经过 Discovery → Shadow → Forward → Manual Review。
8. 代码事实以 `HH520-stable-V2` 当前 main 分支为准；本知识库记录“为什么、当前状态、如何继续”。

最后更新：2026-09-21
