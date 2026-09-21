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


---

# 常用命令速查

> 下面分为“聊天命令”和“底层执行命令”。日常使用优先直接在 ChatGPT 里发送聊天命令。

## 1. 正式预测

### 聊天命令

```
预测 YYYY-MM-DD 全部比赛
```

示例：

```
预测 2026-09-21 全部比赛
```

这是当前正式预测入口，执行：

```
10027s
→ Probability Layer
→ Value Layer
→ Decision Filter V2
→ GPT / Final Output
```

当前代码中的严格命令格式同时接受：

```
预测 YYYY-MM-DD
```

但推荐始终使用：

```
预测 YYYY-MM-DD 全部比赛
```

### 本地 CLI

```bash
python main.py "预测 2026-09-21 全部比赛"
```

如显式启用 GPT API：

```bash
python main.py "预测 2026-09-21 全部比赛" --gpt
```

保存 JSON：

```bash
python main.py "预测 2026-09-21 全部比赛" --output output.json
```

---

## 2. Research 研究

### 聊天命令

单日：

```
研究 YYYY-MM-DD
```

日期范围：

```
研究 YYYY-MM-DD至YYYY-MM-DD
```

示例：

```
研究 2026-09-21
研究 2026-09-01至2026-09-20
```

Research 必须保持：

```
Stable = READ_ONLY
Candidate = CANDIDATE_ONLY
```

研究结果不得自动修改 Stable。

### 底层 Research CLI

单日：

```bash
python -m research.runner 2026-09-21 --output research.json
```

范围：

```bash
python -m research.runner 2026-09-01 2026-09-20 --output research.json
```

离线输入：

```bash
python -m research.runner 2026-09-01 2026-09-20 --input input.json --output research.json
```

---

## 3. Historical Shadow Test

用途：使用后续历史区间验证已经冻结的 Discovery Candidate Rules。

### 聊天命令

推荐格式：

```
Shadow Test YYYY-MM-DD至YYYY-MM-DD
```

如果需要指定 Discovery：

```
Shadow Test YYYY-MM-DD至YYYY-MM-DD
使用 Discovery request_id: <request_id>
```

当前 canonical 示例：

```
Shadow Test 2026-09-01至2026-09-20
使用 Discovery request_id: hh520-research-20260801-20260831-a3f7c9d2
```

已完成的正式 Shadow：

```
Discovery:
hh520-research-20260801-20260831-a3f7c9d2

Shadow:
hh520-shadow-20260901-20260920-c7f5a2d9

结果:
22 SHADOW_PASS
7 SHADOW_HOLD
```

### 底层 CLI

```bash
python -m research.shadow_runner \
  discovery.json \
  2026-09-01 \
  2026-09-20 \
  --output shadow.json
```

---

## 4. Forward Test

用途：验证已经通过 Historical Shadow 的冻结规则在真正未来数据中的持续性。

### 聊天命令

单日：

```
Forward Test YYYY-MM-DD
```

范围：

```
Forward Test YYYY-MM-DD至YYYY-MM-DD
```

示例：

```
Forward Test 2026-09-21
Forward Test 2026-09-21至2026-09-27
```

如需明确指定 Shadow 源：

```
Forward Test 2026-09-21至2026-09-27
使用 Shadow request_id: hh520-shadow-20260901-20260920-c7f5a2d9
```

Forward 规则：

- 只允许 `SHADOW_PASS` 进入
- 规则冻结
- 不重新筛规则
- 不根据 Forward 结果自动调参
- 不自动升级 Stable

### 底层 CLI

```bash
python -m research.forward_runner \
  shadow.json \
  2026-09-21 \
  2026-09-27 \
  --output forward.json
```

---

## 5. 结果读取命令

### 预测结果

聊天中可直接说：

```
读取预测结果 <request_id>
```

或：

```
查看预测结果 <request_id>
```

底层结果分支：

```
action-results
results/<request_id>.json
```

### Research / Shadow / Forward 结果

聊天中可直接说：

```
读取研究结果 <request_id>
读取 Shadow 结果 <request_id>
读取 Forward 结果 <request_id>
```

Research 结果分支：

```
research-results
```

快速轮询：

```
results/<request_id>.json
```

完整报告：

```
archive/<request_id>.json
```

分页详情：

```
pages/<request_id>/manifest.json
```

---

## 6. 项目恢复命令

更换 ChatGPT 账号或新的 AI 会话时，直接发送：

```
请读取 GitHub 仓库 ltaln/HH520-Knowledge-Base。
先读取 00_PROJECT_MEMORY/WAKE_CODE.md，
再按照里面的顺序恢复 HH520 项目上下文。
然后检查 ltaln/HH520-stable-V2 当前 main 分支，
以代码为最终事实来源。
不要重新设计架构，也不要自动修改 Stable。
继续 CURRENT_STATE 中的未完成任务。
```

更短的版本：

```
读取 HH520-Knowledge-Base/00_PROJECT_MEMORY/WAKE_CODE.md
恢复 HH520 项目上下文并继续 CURRENT_STATE
```

---

## 7. 项目状态检查

聊天中推荐使用：

```
检查 HH520 当前状态
```

或：

```
检查 Stable V2.1
```

或：

```
检查 Research 当前状态
```

或：

```
检查 Forward Test 当前状态
```

状态检查时应同时核对：

- Knowledge Base 的 `CURRENT_STATE.md`
- `HH520-stable-V2` 当前 main
- 最新 GitHub Actions / CI
- 最新 Research / Shadow / Forward 结果

---

## 8. 知识库维护命令

项目发生正式架构变化后，可直接说：

```
更新 HH520 知识库
```

应同步更新至少：

```
00_PROJECT_MEMORY/CURRENT_STATE.md
05_HISTORY/CHANGELOG.md
05_HISTORY/DESIGN_DECISIONS.md
```

如果模型、数据源、工作流发生变化，还要同步对应模块文档。

---

# 命令总表

| 目的 | 推荐命令 |
|---|---|
| 正式预测 | `预测 YYYY-MM-DD 全部比赛` |
| 单日研究 | `研究 YYYY-MM-DD` |
| 区间研究 | `研究 YYYY-MM-DD至YYYY-MM-DD` |
| Historical Shadow | `Shadow Test YYYY-MM-DD至YYYY-MM-DD` |
| Forward 单日 | `Forward Test YYYY-MM-DD` |
| Forward 区间 | `Forward Test YYYY-MM-DD至YYYY-MM-DD` |
| 读取预测结果 | `读取预测结果 <request_id>` |
| 读取研究结果 | `读取研究结果 <request_id>` |
| 读取 Shadow | `读取 Shadow 结果 <request_id>` |
| 读取 Forward | `读取 Forward 结果 <request_id>` |
| 检查项目 | `检查 HH520 当前状态` |
| 恢复新账号 | `读取 HH520-Knowledge-Base/00_PROJECT_MEMORY/WAKE_CODE.md` |
| 更新知识库 | `更新 HH520 知识库` |

## 当前 ChatGPT Action operationId

| 功能 | operationId |
|---|---|
| 启动预测 | `startHH520Prediction` |
| 读取预测结果 | `getHH520PredictionResult` |
| 启动 Research | `startHH520Research` |
| 读取 Research | `getHH520ResearchResult` |
| Research 详情清单 | `getHH520ResearchDetailManifest` |
| Research 详情页 | `getHH520ResearchDetailPage` |
| 启动 Shadow | `startHH520ShadowTest` |
| 启动 Forward | `startHH520ForwardTest` |

> 注意：聊天里的“研究… / Shadow Test… / Forward Test…”是推荐的人类可读指令；底层由对应 GitHub Action / operationId 执行。正式预测命令 `预测 YYYY-MM-DD 全部比赛` 则同时是代码中的严格命令格式。
