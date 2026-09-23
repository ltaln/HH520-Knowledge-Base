# Current State

更新时间：**2026-09-23**

## 正式版本

**HH520 Stable V3.4**

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
- ✅ Decision Filter V3.4
- ✅ FT-conditioned HT/FT Layer
- ✅ HTFT-conditioned Score Layer
- ✅ GPT Explanation Only 模式
- ✅ GitHub Actions Prediction Pipeline
- ✅ Research Lab 独立架构
- ✅ Historical Shadow / Forward 验证框架
- ✅ Stable V3.4 CI 通过

## Stable V3.4 正式模型链

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
Risk Tier
(CONFIRM / BALANCED / TAIL_ALERT / PASS)
↓
Value Diagnostic
↓
FT-conditioned HT/FT
↓
HTFT-conditioned Score
↓
Consistency Check
↓
Locked Prediction
```

## 模型规则冻结

### Probability Layer

平局：

`PD = 0.789 × PD_market + 0.211 × 25.74%`

主客分配：

`HomeShare = (1/OH) / ((1/OH)+(1/OA))`

### 基本面作用

控球、进攻、防守、交锋、状态：

- 不直接修改胜平负概率。
- 只进入 Market Failure Detector。
- 只影响风险等级，不翻转方向。

### Value Layer

仅用于：
- EV
- Kelly
- 价值诊断

禁止：
- 修改胜平负
- 修改半全场
- 修改比分

### HT/FT 与比分

正式链：

`FT → HT/FT → Score`

已退出正式链：

- Pooled Poisson
- 外部 Goal Timing 正式干预
- 外部页面概率覆盖 FT

## Research 状态

Research 与 Stable 完全隔离：

- Stable = READ ONLY
- Research = Candidate Only
- Research 不自动修改 Stable

当前发现：

- Research Action 路由存在待修复问题。
- Prediction Action 已与 Stable V3.4 同步。
- Research 结果读取路径需要进一步隔离，避免与 Prediction Result 路由冲突。

## ChatGPT 角色

GPT 不负责重新预测。

正式职责：

- 触发任务
- 读取结果
- 解释结果

角色：

`EXPLANATION_ONLY`

## 最近关键提交

Stable V3.4：

- `1bf7eb301f872c394e8cacecc4284198ff545608` Action/文档同步
- `130578d` 生产链同步修复
- `25957659f2ce91067ef080e50acbe4a42067d0da` 测试修正完成

CI：

- Stable V3.4 测试通过

## 当前下一任务

1. 修复 Research Action 结果读取路径隔离问题。
2. 验证 Research 端到端 request_id 生命周期。
3. 保持 Stable V3.4 不变，进入真实未来样本验证。
4. 持续更新知识库 CURRENT_STATE。
