# 采集1-1 — FootyStats 全字段增强首轮验证结果

## 1. 定位

“采集1-1”是“采集一”方案的首轮完整验证结果。

目标：

- 使用 2026-09-01 至 2026-09-20 的既有 HH520 比赛
- 对已成功映射的 FootyStats 球队做完整 profile 采集
- 将外部字段按功能分组
- 对胜平负、半全场、比分三个方向进行多轮组合测试
- 与当前 Stable V3.5.1 正式结果对比
- 不自动修改 Stable

---

## 2. 数据采集结果

原始 FootyStats 映射球队：

- mapped_teams: 159
- unique_profile_urls: 150

完整 profile 采集：

- profiles_scraped: 150
- profiles_available: 145
- unique_profiles_available: 136
- Firecrawl credits consumed: 136
- backup credits: 226 -> 90

快照日期：

- 2026-09-25

重要限制：

- historical_point_in_time = false
- 这是 9月25日 当前赛季累计快照
- 因此本轮只能作为 retrospective feature-value research
- 不能当作严格 leakage-clean historical OOS

---

## 3. 数据质量门

用于正式组合测试的比赛必须满足：

- 主客双方 FootyStats profile 都可用
- 球队映射 score >= 0.85
- 歧义 profile URL 排除
- 错误共享 profile URL 排除

最终：

- accepted matches: 81
- rejected mapping_low_confidence: 195
- rejected profile_unavailable: 15
- rejected ambiguous_profile_url: 11
- ambiguous_profile_url_groups: 9

实验切分：

- selection: 2026-09-01..2026-09-14，共 50 场
- holdout: 2026-09-15..2026-09-20，共 31 场

---

## 4. 字段分组

### XG

- xG For
- xG Against
- 主客场 xG 拆分
- matchup xG 组合特征

### SHOTS

- Shots
- Shots On Target
- Shots Off Target
- Shot Conversion
- Shots Per Goal
- Shots On Target Per Goal

### GOAL_RATE

- Scored Per Match
- Conceded Per Match
- Clean Sheet
- Failed To Score

### RESULT_STABILITY

- Wins
- Draws
- Losses

### BTTS_TOTALS

- BTTS
- BTTS & Win
- BTTS & Draw
- Over 0.5
- Over 1.5
- Over 2.5
- Over 3.5

### HALF_TIMING

- Scored 1H
- Scored 2H
- Failed To Score 1H
- Failed To Score 2H
- Scored Both Halves
- Conceded Average 1H
- Conceded Average 2H
- Clean Sheet 1H
- Clean Sheet 2H
- 15分钟六段进球/失球时间结构

### POSSESSION

- Possession

总分组：

7 组

非空组合：

127 种

---

## 5. 测试规模

在所有字段组合基础上继续叠加不同模型参数：

### 胜平负 FT

- 381 个候选

### 半全场 HTFT

- 381 个候选

### 比分 SCORE

- 1143 个候选

测试方式：

- 9月1–14日内部做 blocked cross-validation
- 9月15–20日 holdout 不参与前置选择
- 最后再做 all-candidate holdout robustness audit

---

## 6. 胜平负结果

最终最有价值组合：

**SHOTS + RESULT_STABILITY + HALF_TIMING**

最终 holdout 31 场：

当前 Stable：

- 19 / 31
- 61.29%

增强候选：

- 21 / 31
- 67.74%

净提升：

- +2 场
- +6.45 个百分点

该组合在 selection 内部交叉验证中没有主要指标负增益。

### 单组观察

- XG 单独表现不稳定，最终 holdout 明显下降
- SHOTS 单独不如组合稳定
- RESULT_STABILITY 单独接近 baseline
- HALF_TIMING 单独不足以稳定提升
- 真正有效的是组合结构，不是单字段

结论：

> 胜平负方向最值得继续研究：SHOTS + RESULT_STABILITY + HALF_TIMING

---

## 7. 半全场结果

目前没有组合满足“Top1 / Top2 / Top3 同时稳定不退化”的严格升级要求。

较有价值组合：

**RESULT_STABILITY**

holdout 31 场：

当前 Stable：

- Top1: 13 / 31 = 41.94%
- Top2: 15 / 31 = 48.39%
- Top3: 20 / 31 = 64.52%

增强后：

- Top1: 12 / 31 = 38.71%
- Top2: 18 / 31 = 58.06%
- Top3: 21 / 31 = 67.74%

变化：

- Top1: -1
- Top2: +3
- Top3: +1

结论：

- 对 Top2 / Top3 有明显信号
- 但会损伤 Top1
- 当前不进入正式 HTFT

其他观察：

- SHOTS
- GOAL_RATE
- BTTS_TOTALS

也存在提高 Top2 的局部信号，但同样不能保护 Top1。

---

## 8. 比分结果

最终最有价值组合：

**SHOTS + RESULT_STABILITY**

holdout 31 场：

当前 Stable：

- Top1: 6 / 31 = 19.35%
- Top2: 7 / 31 = 22.58%
- Top3: 11 / 31 = 35.48%
- Top5: 17 / 31 = 54.84%

增强后：

- Top1: 8 / 31 = 25.81%
- Top2: 14 / 31 = 45.16%
- Top3: 17 / 31 = 54.84%
- Top5: 17 / 31 = 54.84%

净变化：

- Top1: +2
- Top2: +7
- Top3: +6
- Top5: 0

这是本轮最强信号。

结论：

> 比分方向优先继续研究 SHOTS + RESULT_STABILITY

---

## 9. 核心发现

### 发现一

FootyStats 外部原始能力数据，整体上比 HH520 内部二次加工字段更有独立增益潜力。

### 发现二

xG/xGA 并没有在首轮成为最强单独特征。

XG 单独在部分 holdout 中反而明显下降。

### 发现三

目前最稳定的外部信号主要来自：

- 射门
- 射正
- 射门效率
- 胜平负稳定性
- 部分半场 / 时间结构

### 发现四

比分模型是本轮获益最大的模块。

### 发现五

半全场仍然最难提升：

- Top2 可以提高
- 但 Top1 容易下降

所以半全场不能因为 Top2 好看就直接升级。

---

## 10. 当前建议

### 胜平负

继续第二轮验证：

**SHOTS + RESULT_STABILITY + HALF_TIMING**

### 比分

继续第二轮验证：

**SHOTS + RESULT_STABILITY**

### 半全场

不升级。

继续研究：

- RESULT_STABILITY
- SHOTS
- GOAL_RATE
- BTTS_TOTALS

但必须增加“保护 Top1”约束。

---

## 11. Stable 状态

当前：

**Stable V3.5.1 保持不变**

本轮状态：

**RESEARCH_ONLY_NOT_PROMOTED_AUTOMATICALLY**

没有修改：

- 正式胜平负核心
- 正式比分模型
- 正式 HTFT 模型
- Draw Resolver
- Decision Filter

---

## 12. 调用名称

以后在 HH520 项目中：

**“采集1-1”**

默认指：

> 2026-09-01 至 2026-09-20，基于 FootyStats Full Profile 的首轮全字段组合验证结果。核心结论：胜平负最优研究组合为 SHOTS + RESULT_STABILITY + HALF_TIMING；比分最优研究组合为 SHOTS + RESULT_STABILITY；半全场暂不升级；Stable V3.5.1 保持不变。
