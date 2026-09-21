# HH520 Project Memory

## 1. 项目目标

HH520 是一个以网页结构化数据为基础的足球预测与研究系统。核心目标：

- 手机/电脑一句命令完成预测。
- 尽量减少复杂基础设施。
- 正式预测链稳定、可复现。
- Research 与 Stable 完全隔离。
- 通过历史验证和前向验证提高最终决策质量。
- 不依赖单个 ChatGPT 账号记忆。

## 2. 当前正式版本

**HH520 Stable V2.1**

代码仓库：

`ltaln/HH520-stable-V2`

知识仓库：

`ltaln/HH520-Knowledge-Base`

## 3. 当前数据源

正式数据源已从 10023s 迁移到 **10027s**。

固定形式：

```
https://www.hh520.com/tx/10027s.php
?riqi_start=YYYY-MM-DD
&riqi_end=YYYY-MM-DD
&threshold=1
&bankroll=5000
```

Firecrawl 是正式采集层。

## 4. 10027s 数据语义

基础表：
- 比赛身份
- 联赛
- 时间
- 1X2赔率
- EV / Kelly / 建议字段
- 半场比分
- 全场比分

其中半场比分、全场比分是 **真实赛果标签**。

融合表：
- 单选
- 融合真实概率
- 半全场
- structure
- consistency
- pattern
- rating
- risk
- handicap
- advantage / draw 等结构化因素

研究发现：
- 10027s 页面没有稳定可确认的“原始预测比分”和“原始总进球”字段。
- 比分/总进球如由模型推导，必须标注 RESEARCH_DERIVED 或模型输出。

## 5. 正式架构

```
10027s
↓
Probability Layer
↓
Value Layer
↓
Decision Filter V2
↓
GPT
↓
Prediction Output
```

### Probability Layer
优先使用 10027s 页面融合概率；没有完整页面概率时使用去水 1X2 市场概率。

### Value Layer
保留 EV / Kelly / 页面信号，但不允许单独改变预测方向。

### Decision Filter V2
基于 2026-08 Discovery + 2026-09 Historical Shadow 的跨时间窗证据进行最终放行。

### GPT
负责最终预测表达、比分/半全场/总进球生成与一致性检查；不得绕过 PASS。

## 6. Stable / Research 隔离

Stable：
- 正式预测
- 不允许 Research 自动修改
- 不自动训练

Research：
- 只读 Stable
- 可以反推隐藏模型
- 可以生成 Candidate Rule
- Candidate 不等于 Stable Rule

晋级流程：

```
Discovery
→ Candidate Rule
→ Historical Shadow
→ Forward Test
→ Manual Review
→ Stable Candidate
```

## 7. 研究时间线

Canonical chronology：

- History Start：2026-08-01
- Discovery：2026-08-01 ～ 2026-08-31
- Historical Shadow：2026-09-01 ～ 2026-09-20
- Forward：2026-09-21 起

## 8. 关键样本

August Discovery：
- 403 collected
- 402 result labels matched
- WDL 217/402 = 53.98%
- Score Exact 58/402 = 14.43%
- Score Top2 101/402 = 25.12%
- HTFT 132/402 = 32.84%
- HTFT Top2 190/402 = 47.26%
- Goals 86/402 = 21.39%
- 29 Candidate Rules

September Historical Shadow：
- 302/302 matched
- 29 frozen rules
- 22 SHADOW_PASS
- 7 SHADOW_HOLD

## 9. 当前运行偏好

用户偏好：
- 直接执行，不逐步询问。
- 输出简洁。
- 手机/电脑一句命令。
- 不建立长期数据库。
- Firecrawl 成本要控制。
- 已知信息不要反复询问。

## 10. 当前知识库职责

代码仓库回答“系统怎么运行”。

知识库回答：
- 为什么这么设计
- 当前版本是什么
- 历史做过什么
- 哪些规则被冻结
- 什么不能改
- 换账号后怎么恢复
