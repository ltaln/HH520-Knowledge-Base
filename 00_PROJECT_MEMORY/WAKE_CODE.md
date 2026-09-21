# HH520 Wake Code

你正在接管 **HH520 足球预测项目**。

## 接管规则

不要重新设计架构，不要根据旧聊天猜测，不要自动修改 Stable。先把本知识库作为唯一项目记忆入口。

必须先读取：

1. `PROJECT_MEMORY.md`
2. `CURRENT_STATE.md`
3. `../01_ARCHITECTURE/SYSTEM_ARCHITECTURE.md`
4. `../05_HISTORY/DESIGN_DECISIONS.md`
5. `../07_OPERATION/RECOVERY_GUIDE.md`

## 当前正式版本

**HH520 Stable V2.1**

正式数据源：

`https://www.hh520.com/tx/10027s.php?riqi_start=YYYY-MM-DD&riqi_end=YYYY-MM-DD&threshold=1&bankroll=5000`

正式代码仓库：

`ltaln/HH520-stable-V2`

## 正式预测链

```
用户命令
  ↓
GitHub Actions / 本地 CLI
  ↓
Firecrawl
  ↓
HH520 10027s
  ↓
10027s Parser
  ↓
Probability Layer
  ↓
Value Layer
  ↓
Decision Filter V2
  ↓
GPT Final Analysis
  ↓
固定输出
```

## 不可破坏规则

- Stable 核心模型不可被 Research 自动修改。
- Research 只能生成 Candidate。
- 10027s 基础表中的半场/全场比分是赛果标签，历史预测时必须隔离。
- 10027s 未提供原始预测比分/总进球时，不得把派生值冒充 HH520 原始值。
- 不建立长期数据库。
- 不自动训练、自动调参、自动晋级 Stable。
- Forward Test 规则必须冻结后再验证。

## 当前继续方向

1. 使用 Stable V2.1 正式预测。
2. 持续 Forward Test Decision Filter V2。
3. 继续积累 10027s 数据。
4. Research 只产生候选规则。
5. 所有晋级均需 Manual Review。

如果用户只说“继续 HH520”，先读取 CURRENT_STATE，再执行当前未完成工作。