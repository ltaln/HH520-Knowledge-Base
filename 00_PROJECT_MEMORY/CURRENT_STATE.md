# Current State

更新时间：**2026-09-21**

## 正式版本

**HH520 Stable V2.1**

## 正式代码仓库

`ltaln/HH520-stable-V2`

## 当前完成状态

- ✅ 10027s 已成为正式默认采集源
- ✅ Firecrawl 采集
- ✅ 10027s Parser
- ✅ Probability Layer
- ✅ Value Layer
- ✅ Decision Filter V2
- ✅ GPT Prompt 已升级为 Stable V2.1
- ✅ 输出增加 Decision Filter 结果
- ✅ GitHub Actions
- ✅ Research Lab
- ✅ Hidden Model Reverse
- ✅ Error Attribution
- ✅ League DNA / Team DNA 研究框架
- ✅ Historical Shadow
- ✅ Forward Test 框架
- ✅ Stable V2.1 CI：59 passed

## Decision Filter V2

正式接入预测主链。

当前核心正向证据：
- away_odds_bucket <1.50
- probability_concentration >=60%
- structure=强优
- pattern=🔶风控赔率
- home_odds_bucket <1.50
- handicap=客让半一低水/一球高水
- rating=B+
- risk=低

Hard PASS：
- probability_concentration <40%
- pattern=⚡ 极端

## Research 状态

August Discovery：
- 29 Candidate Rules

September Shadow：
- 22 PASS
- 7 HOLD

Forward：
- 从 2026-09-21 起
- 规则冻结
- 不自动调参
- 不自动晋级 Stable

## 最近关键提交

Stable V2.1 接入阶段：
- `e0083eb1` Decision Filter V2 接入 Stable
- `11d60fc5` Prediction Pipeline 加入 Filter Gate
- `af20f081` 正式数据源配置切换 10027s
- `f1746c99` 输出增加 Filter 信息
- `da49f556` Cache 默认源 10027s
- `1cc2ee6e` 修复 10027s Base/Fusion 误读
- `162960a4` Prompt 升级
- `fb8e19eb` 测试修正

CI：
- Run `35601017542`
- 59 passed

## 当前下一任务

1. 用 2026-09-21 以后真实未来样本验证 Decision Filter V2。
2. 对比 Stable 原始方向与 Stable V2.1 最终放行效果。
3. 累积 Forward 数据，不重新筛同一批规则。
4. 定期更新本知识库 CURRENT_STATE。