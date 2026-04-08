# overall/ — 跨工具通用习惯指南

面向已经在用 AI Coding 工具的开发者，研究如何通过调整工作习惯来最大化 AI 能力、减少返工、控制成本。

覆盖工具：Claude Code、Cursor、Codex CLI、Aider，以及 Cline、Windsurf、Gemini CLI 等。

---

## 内容层次

### round1_short/ — 原则层

11 个章节，每章一个核心习惯。工具操作已经过官方文档核验（标注 `A/B`），工程推论标注 `I`。

适合：第一次读，建立框架。

| 文件 | 内容 |
|------|------|
| [00_overview.md](round1_short/00_overview.md) | 导言：为什么上下文管理比模型选择更重要 |
| [01_session_lifecycle.md](round1_short/01_session_lifecycle.md) | 任务边界即会话边界 |
| [02_edit_not_append.md](round1_short/02_edit_not_append.md) | 修改原提示词，而非追加对话 |
| [03_plan_before_code.md](round1_short/03_plan_before_code.md) | 先规划，后执行 |
| [04_rule_hierarchy.md](round1_short/04_rule_hierarchy.md) | 规则文件的分层设计 |
| [05_progressive_feed.md](round1_short/05_progressive_feed.md) | 渐进式信息投喂 |
| [06_kv_cache.md](round1_short/06_kv_cache.md) | KV Cache 经济学 |
| [07_model_routing.md](round1_short/07_model_routing.md) | 按任务复杂度选模型 |
| [08_tool_permission.md](round1_short/08_tool_permission.md) | 工具权限克制 |
| [09_memento_workflow.md](round1_short/09_memento_workflow.md) | 跨会话状态传递 |
| [10_multi_agent.md](round1_short/10_multi_agent.md) | 多智能体分流 |
| [11_billing_tips.md](round1_short/11_billing_tips.md) | 订阅额度陷阱 |

### round2_detailed/ — 深化层

同样 11 个章节，面向已读 round1 的读者。补充工具级操作细节、修正 round1 的事实错误、回答 round1 留下的延伸问题。

适合：已经熟悉基本框架，想了解具体工具行为和更深的原理。

主要深化内容：
- Codex CLI 的 `resume`/`fork`/`exec --ephemeral` 三种会话模式
- Aider 的三模型分工（`--model`/`--editor-model`/`--weak-model`）和 repo map 机制
- Claude Code 的 `--permission-mode plan`、`--max-budget-usd`、subagents 存储路径
- sandbox + approval 两维权限矩阵（Codex CLI）

---

## 研究材料

### raw_dr/ — 原始研究资料

四篇深度研究报告，是 round1 内容的原始来源。

### reference_r1/ — round1 工具事实核验

官方文档摘要和工具能力矩阵，是 round1 grounding audit 的产出。

### reference_r2/ — round2 工具事实核验

本机 CLI 帮助输出和官方文档核验，是 round2 的 grounding truth。

### plan/ — 写作计划

- [guide_plan.md](plan/guide_plan.md) — 整体指南的规划
- [round1_tool_grounding_audit_plan.md](plan/round1_tool_grounding_audit_plan.md) — round1 工具事实核验计划
- [round2_plan.md](plan/round2_plan.md) — round2 深化层的规划

---

## 阅读建议

- **10 分钟**：读 round1 的第一章和第六章
- **完整阅读**：按顺序读 round1 的 11 章，每章 10-15 分钟
- **深入某个工具**：在 round2 找对应章节，有更多工具级细节
