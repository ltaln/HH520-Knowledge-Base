# HH520 Project Memory

## 1. 项目目标

HH520 是一个以 HH520 结构化网页数据为基础的足球预测与研究系统。核心目标：

- 手机/电脑一句命令完成预测。
- 尽量减少复杂基础设施。
- 正式预测链稳定、可复现。
- Research 与 Stable 完全隔离。
- 通过历史验证、压力验证和前向验证提高最终决策质量。
- 不依赖单个 ChatGPT 账号记忆。

## 2. 当前正式版本

**HH520 Stable V3.5.1**

代码仓库：`ltaln/HH520-stable-V2`

知识仓库：`ltaln/HH520-Knowledge-Base`

## 3. 当前正式数据源

正式数据源：**10027s**

固定形式：

```
https://www.hh520.com/tx/10027s.php
?riqi_start=YYYY-MM-DD
&riqi_end=YYYY-MM-DD
&threshold=1
&bankroll=5000
```

Firecrawl 是正式采集层。

## 4. Stable V3.5.1 正式架构

```
10027s
↓
Probability Layer
↓
Market Failure Detector
↓
Formal Draw Resolver
↓
Decision Filter V3.5.1
↓
Independent Score Layer
↓
Independent HT/FT Layer
↓
Consistency Layer
↓
Locked Prediction
↓
Strict 6-column Output
```

## 5. Formal Draw Resolver

用途：解决原模型极少正式输出平局的问题。

正式模型：

`CROSS_FIT_LOGISTIC_V1_FORMAL`

规则：
- 仅低置信均衡区
- `draw_score >= 0.38`
- `pmax <= 0.45`
- 永不覆盖 `>=55%` 已授权主/客方向

验证：
- Dev1 61.5%
- Dev2 41.4%
- Sep Stress 55.6%

## 6. HT/FT

正式模型：

`INDEPENDENT_POISSON_SPLIT_HTFT_V3_EXISTING_DATA`

只利用已采集历史半场/全场数据反推。

冻结参数：
- Home first-half share = 0.36
- Away first-half share = 0.44

历史标签：
- 538 + 592 + 302 = 1432 场

正式预测不再采集任何新 Goal Timing 数据。

## 7. Score

比分保持独立：

`HDA_POISSON_V1`

不再被 FT 方向硬锁，也不被 HT/FT 模板反向锁定。

## 8. 当前关键研究结论

平局：
- 简单阈值规则在不同窗口不稳定。
- 最终采用交叉验证 Logistic 高特异性平局覆盖器。

HT/FT：
- 不需要新增 Goal Timing。
- 使用已有真实 HT/FT 标签可稳定得到约 32–34% Top1、49–53% Top2 的历史表现。

## 9. 当前运行偏好

- 直接执行，不逐步询问。
- 输出简洁。
- 手机/电脑一句命令。
- 不建立长期数据库。
- Firecrawl 成本要控制。
- 已知信息不要反复询问。
- 遇到问题优先自行验证、修复、反复测试，最终只给结果。

## 10. 当前知识库职责

代码仓库回答“系统怎么运行”。

知识库回答：
- 为什么这么设计
- 当前版本是什么
- 历史做过什么
- 哪些规则被冻结
- 什么不能改
- 换账号后怎么恢复
