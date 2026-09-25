# Current State

更新时间：**2026-09-25**

## 正式版本

**HH520 Stable V3.5.1**

## 正式代码仓库

`ltaln/HH520-stable-V2`

## 当前完成状态

- ✅ 正式数据源：HH520 10027s
- ✅ Firecrawl 采集链
- ✅ 10027s Parser
- ✅ Probability Layer
- ✅ Draw Anchor
- ✅ HomeShare Side Layer
- ✅ Value Layer（Diagnostic Only，不改变方向）
- ✅ Market Failure Detector
- ✅ Decision Filter V3.5.1
- ✅ 正式平局判定规则
- ✅ 新 HT/FT 正式模型
- ✅ 独立比分模型
- ✅ GPT Explanation Only 模式
- ✅ GitHub Actions Prediction Pipeline
- ✅ Plugin 一次命令触发 + request_id 生命周期
- ✅ Research Lab 独立架构
- ✅ Historical Shadow / Forward 验证框架
- ✅ Stable V3.5.1 CI 全量通过
- ✅ 2026-09-25 端到端 READY 验证通过

## Stable V3.5.1 正式模型链

```text
10027s
↓
Market De-vig
↓
Draw Anchor
↓
HomeShare Side Layer
↓
FT Probability
↓
Market Failure Detector
↓
Formal Draw Resolver
↓
Decision Filter
↓
Risk Tier
(CONFIRM / BALANCED / TAIL_ALERT / PASS)
↓
Independent Score Model
↓
Independent HT/FT Model
↓
Consistency Check
↓
Locked Prediction
↓
Strict 6-column Output
```

## Probability Layer

平局锚点：

`PD = 0.789 × PD_market + 0.211 × 25.74%`

主客分配：

`HomeShare = (1/OH) / ((1/OH)+(1/OA))`

页面融合概率仅作诊断，不允许覆盖正式 FT 方向。

## 正式平局判定规则

当前正式规则：

- 类型：`CROSS_FIT_LOGISTIC_V1_FORMAL`
- 只在低置信、均衡侧向区工作
- `draw_score >= 0.38`
- `pmax <= 0.45`
- 永不覆盖已经授权的 `>=55%` 主胜/客胜方向

输入特征：

- pd
- side_gap
- draw_top_gap
- pmax
- market_pd
- home_share_dev
- lambda_total
- lambda_gap
- attack_gap
- defense_gap
- h2h_gap
- form_gap
- possession_gap

历史验证：

- Dev1：8/13 = **61.5%**
- Dev2：12/29 = **41.4%**
- Sep 1–20 Stress：5/9 = **55.6%**
- 三个阶段相对原始 argmax 均为净增命中

## 新 HT/FT 正式模型

模型：

`INDEPENDENT_POISSON_SPLIT_HTFT_V3_EXISTING_DATA`

数据来源：

- 2026-05-01～06-30：538 场
- 2026-07-01～08-31：592 场
- 2026-09-01～09-20：302 场
- 合计：**1432 场**

冻结参数：

- 主队上半场强度份额：`0.36`
- 客队上半场强度份额：`0.44`

历史表现：

- Dev1 Top1：32.2%，Top2：53.3%
- Dev2 Top1：34.3%，Top2：52.7%
- Sep Stress Top1：31.1%，Top2：48.7%

正式策略：

- 不采集新的 Goal Timing
- 不要求比赛日额外分时进球数据
- `timing_used = false`
- 正式链只使用已有 FT 概率 + 冻结的历史半场强度份额

## HT/FT 与比分

正式链已改为：

```text
FT Probability
├─→ Independent Score Model
└─→ Independent HT/FT Model
```

不再使用：

- FT-conditioned HT/FT 模板
- HTFT-conditioned Score
- 新增 Goal Timing 采集

## Plugin / GitHub Action 状态

正式流程：

```text
手机/电脑
↓
Plugin
↓
create request once
↓
same request_id polling
↓
GitHub Action
↓
Stable V3.5.1
↓
action-results
↓
READY
↓
output_contract.display_rows
```

最新验证：

- CI Run：`36093520930` → success
- E2E Prediction Run：`36093576607` → success
- Request ID：`hh520-20260925-drawhtft-final` → READY
- 半全场正式输出成功
- Goal Timing 正式链关闭

## ChatGPT 角色

GPT 不负责重新预测。

正式职责：

- 触发任务
- 读取结果
- 展示严格 6 列结果
- 解释结果

角色：

`EXPLANATION_ONLY`

## 当前下一任务

1. 继续真实未来样本验证平局正式规则。
2. 继续监测 HT/FT Top2 的前向表现。
3. 不新增半全场 Goal Timing 数据源。
4. 保持 Stable 3.5.1 核心概率层不变，只有经过验证的候选规则才允许升级。
5. 持续同步知识库与代码仓库。

---
