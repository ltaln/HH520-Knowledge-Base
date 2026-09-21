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


---

# GPT 配置方案与迁移代码

> 本节用于 **更换 ChatGPT/OpenAI 账号后完整恢复 HH520 的 GPT 配置**。  
> 这里不保存任何真实密钥、Token 或 API Key；新账号只需要重新授权并按本节配置即可。

## 1. GPT 在 HH520 中的角色

当前 GPT 不是数据采集器，也不是自动训练器。

GPT 的正式职责：

```
10027s 结构化赛前数据
        ↓
Probability Layer
        ↓
Value Layer
        ↓
Decision Filter V2
        ↓
GPT Final Analysis
        ↓
比分 ×2
半全场 ×2
总进球
方向
置信度
原因
```

GPT 必须遵守：

- Probability Layer 决定方向
- EV / Kelly 不能单独决定方向
- Decision Filter V2 输出 PASS 时，GPT 不得绕过
- 不得补充未采集的伤停、天气、首发、球员等事实
- 不得把 10027s 真实赛果字段当赛前输入
- 不得把研究派生比分冒充 HH520 原始预测
- Research 不得自动修改 Stable

---

## 2. 新账号推荐 GPT 配置

### 名称

推荐：

```
HH520 Stable V2.1
```

### GPT Instructions / 系统指令

新账号创建 GPT 后，把下面内容作为核心 Instructions：

```
你是 HH520 Stable V2.1 的执行与最终分析入口。

正式代码仓库：
ltaln/HH520-stable-V2

长期知识库：
ltaln/HH520-Knowledge-Base

每次接管项目时：
1. 先读取 HH520-Knowledge-Base/00_PROJECT_MEMORY/WAKE_CODE.md
2. 再读取 CURRENT_STATE.md
3. 以 HH520-stable-V2 当前 main 分支代码作为最终事实来源

正式预测数据源：
HH520 10027s

正式预测链：
10027s
→ Probability Layer
→ Value Layer
→ Decision Filter V2
→ GPT Final Analysis
→ Final Output

用户命令：
预测 YYYY-MM-DD 全部比赛

Research 命令：
研究 YYYY-MM-DD
研究 YYYY-MM-DD至YYYY-MM-DD

验证命令：
Shadow Test YYYY-MM-DD至YYYY-MM-DD
Forward Test YYYY-MM-DD
Forward Test YYYY-MM-DD至YYYY-MM-DD

执行原则：
- 不逐步询问已知信息，能直接执行就直接执行
- Stable 与 Research 必须隔离
- Research 只能生成 Candidate
- 不自动训练
- 不自动修改 Stable
- 不自动晋级规则
- 历史真实比分只能作为 Result Label
- Decision Filter PASS 时不得强制预测
- 10027s 是当前唯一正式采集源
- Firecrawl 是正式采集层
- GitHub Actions 是主要执行桥
- 结果优先简洁输出

正式预测输出：
比赛
比分预测 ×2
半全场 ×2
总进球
方向
置信度
Decision Filter
状态
原因
```

完整预测 Prompt 的代码事实来源：

```
HH520-stable-V2/
prompts/HH520_Stable_V2_Prediction_Prompt.md
```

---

## 3. GPT Action 配置

当前采用 **GitHub Actions Bridge**。

OpenAPI Schema 的唯一正式来源：

```
HH520-stable-V2/
integration/chatgpt-action.openapi.json
```

当前 Schema 版本：

```
2.5
```

Server：

```
https://api.github.com
```

认证方式：

```
HTTP Bearer Token
```

OpenAPI security scheme：

```json
{
  "githubToken": {
    "type": "http",
    "scheme": "bearer"
  }
}
```

### 新账号重新配置 Action 时

1. 在新的 GPT / Action 配置中导入：
   `integration/chatgpt-action.openapi.json`
2. Authentication 选择 Bearer / API Token。
3. 使用新的 GitHub Fine-grained PAT 完成授权。
4. **不要把 Token 写入知识库、README 或聊天消息。**

推荐 GitHub Token 权限：

```
Repository:
ltaln/HH520-stable-V2

Actions:
Read and write

Contents:
Read
```

如果未来 Action 需要新增 GitHub 写接口，再按最小权限原则增加权限。

---

## 4. 当前 Action operationId

```
startHH520Prediction
getHH520PredictionResult

startHH520Research
getHH520ResearchResult
getHH520ResearchDetailManifest
getHH520ResearchDetailPage

startHH520ShadowTest
startHH520ForwardTest
```

### Prediction Dispatch

底层请求结构：

```json
{
  "ref": "main",
  "inputs": {
    "request_id": "hh520-20260921-a1b2c3d4",
    "command": "预测 2026-09-21 全部比赛"
  }
}
```

对应 workflow：

```
.github/workflows/hh520-predict.yml
```

### Research Dispatch

```json
{
  "ref": "main",
  "inputs": {
    "request_id": "hh520-research-20260901-20260920-a1b2c3d4",
    "start_date": "2026-09-01",
    "end_date": "2026-09-20"
  }
}
```

对应：

```
.github/workflows/hh520-research.yml
```

### Shadow Dispatch

```json
{
  "ref": "main",
  "inputs": {
    "request_id": "hh520-shadow-20260901-20260920-a1b2c3d4",
    "discovery_request_id": "hh520-research-20260801-20260831-a3f7c9d2",
    "validation_start": "2026-09-01",
    "validation_end": "2026-09-20"
  }
}
```

对应：

```
.github/workflows/hh520-shadow.yml
```

### Forward Dispatch

```json
{
  "ref": "main",
  "inputs": {
    "request_id": "hh520-forward-20260921-20260927-a1b2c3d4",
    "shadow_request_id": "hh520-shadow-20260901-20260920-c7f5a2d9",
    "validation_start": "2026-09-21",
    "validation_end": "2026-09-27"
  }
}
```

对应：

```
.github/workflows/hh520-forward.yml
```

---

## 5. Action 结果读取机制

### Prediction

分支：

```
action-results
```

路径：

```
results/<request_id>.json
```

状态：

```
PENDING
READY
FAILED
```

如果返回 404，表示结果暂未发布：

- 继续读取同一个 request_id
- 更换 poll 参数
- **不要重复 dispatch 同一任务**

### Research / Shadow / Forward

分支：

```
research-results
```

快速结果：

```
results/<request_id>.json
```

完整结果：

```
archive/<request_id>.json
```

详情索引：

```
pages/<request_id>/manifest.json
```

详情页：

```
pages/<request_id>/error-N.json
pages/<request_id>/signals-N.json
```

---

## 6. GPT API 直连方案

除了聊天中的 GitHub Actions Bridge，代码仓库还保留可选的 **OpenAI Responses API** 模式。

代码来源：

```
prediction/gpt.py
```

需要环境变量：

```bash
OPENAI_API_KEY=<new-account-api-key>
OPENAI_MODEL=<account-available-model>
```

不要把真实 Key 写进 GitHub。

调用入口：

```bash
python main.py "预测 2026-09-21 全部比赛" --gpt
```

### Responses API 核心请求代码

当前实现等价于：

```python
response = requests.post(
    "https://api.openai.com/v1/responses",
    headers={
        "Authorization": f"Bearer {OPENAI_API_KEY}",
        "Content-Type": "application/json",
    },
    json={
        "model": OPENAI_MODEL,
        "input": json.dumps({
            "config": stable_config,
            "matches": matches
        }, ensure_ascii=False),
        "instructions": prediction_prompt,
        "store": False,
        "max_output_tokens": 6000,
        "text": {
            "format": {
                "type": "json_schema",
                "name": "hh520_predictions",
                "strict": True,
                "schema": HH520_OUTPUT_SCHEMA
            }
        }
    },
    timeout=90,
    allow_redirects=False
)
```

重要：

```
store = False
自动重试 = 禁止
相同 GPT 输入 = 缓存
请求前先写 .requested 防止重复扣费
```

---

## 7. GPT Structured Output Schema

GPT 最终每场必须返回：

```json
{
  "match_id": "1",
  "score1": "2:0",
  "score2": "2:1",
  "htft1": "主/主",
  "htft2": "平/主",
  "total_goals": "2—3球",
  "direction": "主胜",
  "reason": "依据结构化赛前证据",
  "confidence": 70,
  "status": "GPT"
}
```

字段要求：

```
match_id      string
score1        string
score2        string
htft1         string
htft2         string
total_goals   string
direction     string
reason        string
confidence    integer 0-99
status        GPT | PASS
```

完整 Schema 在：

```
prediction/gpt.py
```

### 验证规则

- GPT 返回场次数量必须与输入一致
- match_id 和顺序必须一致
- confidence 必须 0–99
- direction 必须与 Probability Layer 一致
- 两个比分必须不同
- 两个比分必须符合方向
- 两个 HTFT 必须不同
- HTFT 最终方向必须一致
- 总进球必须与两个比分兼容
- GPT confidence 不得超过结构化 evidence confidence
- 数据不足时返回 PASS

---

## 8. Stable V2.1 GPT Config

正式配置文件：

```
config/stable.yaml
```

当前等价配置：

```yaml
project:
  name: HH520 Stable V2.1
  version: "2.1"

source:
  page: "10027s"
  base_url: "https://www.hh520.com/tx/10027s.php"
  date_params: ["riqi_start", "riqi_end"]
  threshold: 1
  bankroll: 5000

firecrawl:
  endpoint: "https://api.firecrawl.dev/v2/scrape"
  formats: ["markdown"]
  only_main_content: true
  proxy: "basic"
  store_in_cache: true
  max_age_ms: 0
  one_request_per_date: true

prediction:
  use_probability_layer: true
  use_value_layer: true
  use_decision_filter: true
  decision_filter_version: "2.0"
  decision_filter_mode: "stable_v2_1"
  allow_pass: true

research:
  enabled: true
  auto_tune_stable: false
```

---

## 9. Stable V2.1 Prediction Prompt

正式 Prompt 文件：

```
prompts/HH520_Stable_V2_Prediction_Prompt.md
```

核心内容：

```
你是 HH520 Stable V2.1 的最终分析引擎。

正式预测只使用 HH520 10027s 结构化赛前数据。

执行顺序：
1. Market Baseline
2. Probability Layer
3. Value Layer
4. Decision Filter V2
5. Final Prediction

Probability Layer 决定比赛方向。
EV / Kelly 只描述价值。
Decision Filter V2 输出 PASS 时不得强行预测。
不得绕过 Hard PASS。
Research 不得自行修改 Stable 权重。

10027s 基础表中的半场/全场比分属于真实赛果标签，
历史比赛不得把这些字段进入赛前推理。

10027s 没有原始预测比分/总进球时，
不得声称页面提供了这些值。

最终输出：
比分 ×2
半全场 ×2
总进球
方向
Decision Filter
置信度
```

> 新账号恢复时优先直接读取仓库中的 Prompt 文件，不要仅依赖 README 摘要。

---

## 10. 可选旧式 WSGI Action Bridge

仓库仍保留：

```
controller/chatgpt_action.py
```

它提供：

```
POST /prepare
GET /prepared/YYYY-MM-DD
GET /health
```

需要：

```
HH520_ACCESS_TOKEN
```

但当前正式推荐方式是：

```
ChatGPT
→ GitHub Actions Bridge
→ Firecrawl
→ Stable V2.1
```

不要把旧 WSGI Bridge 当作当前主要部署依赖。

---

## 11. 新账号完整迁移清单

更换账号后，按下面清单即可恢复：

```
[ ] 连接 GitHub
[ ] 能读取 HH520-Knowledge-Base
[ ] 能读取 HH520-stable-V2
[ ] 读取 WAKE_CODE.md
[ ] 创建/配置 HH520 GPT
[ ] 写入本 README 中的 GPT Instructions
[ ] 导入 integration/chatgpt-action.openapi.json
[ ] 配置新的 GitHub Bearer Token
[ ] 确认 Actions Read/Write
[ ] 确认 Contents Read
[ ] 不复制旧 Token
[ ] 如使用 API 模式，重新配置 OPENAI_API_KEY
[ ] 设置新账号可用的 OPENAI_MODEL
[ ] 测试：预测一个日期
[ ] 测试：读取 prediction result
[ ] 测试：Research
[ ] 确认 Decision Filter V2 生效
[ ] 确认数据源为 10027s
```

### 新账号最终恢复指令

```
读取 ltaln/HH520-Knowledge-Base 的 README.md 和
00_PROJECT_MEMORY/WAKE_CODE.md。

恢复 HH520 Stable V2.1 全部上下文。

然后检查 ltaln/HH520-stable-V2 当前 main 分支中的：
config/stable.yaml
prompts/HH520_Stable_V2_Prediction_Prompt.md
integration/chatgpt-action.openapi.json
prediction/gpt.py
engine/decision_filter.py

以代码仓库为最终事实来源。

恢复 GPT Action、预测、Research、Shadow、Forward 能力。
不要修改 Stable 规则。
不要自动训练或自动晋级 Research 规则。
```

---

## 12. GPT 配置代码事实来源总表

| 内容 | 正式文件 |
|---|---|
| GPT Prediction Prompt | `prompts/HH520_Stable_V2_Prediction_Prompt.md` |
| GPT Responses API | `prediction/gpt.py` |
| GPT 输入构造/验证 | `prediction/builder.py` |
| Stable 配置 | `config/stable.yaml` |
| ChatGPT Action OpenAPI | `integration/chatgpt-action.openapi.json` |
| GitHub Prediction Workflow | `.github/workflows/hh520-predict.yml` |
| Research Workflow | `.github/workflows/hh520-research.yml` |
| Shadow Workflow | `.github/workflows/hh520-shadow.yml` |
| Forward Workflow | `.github/workflows/hh520-forward.yml` |
| 可选 WSGI Bridge | `controller/chatgpt_action.py` |
| Decision Filter | `engine/decision_filter.py` |

**恢复原则：README 用于快速恢复；以上源文件才是 GPT 配置与代码的最终事实来源。**
