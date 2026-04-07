# round2 计划

日期：2026-04-07
前置：round1 已完成工具 grounding（见 round1_tool_grounding_audit_plan.md）

---

## 1. round1 现在是什么

round1 是"原则 + 已核验工具事实"层：
- 每章有核心习惯、正确做法、错误可能、原理、延伸习惯
- 工具操作已按官方文档校正（`A/B` 标注）
- 工作流建议标注为推论（`I` 标注）
- 每章结尾有 4 个"延伸调查方向"——这些是 round2 的研究议题

round1 的局限：
- 原理部分多为推论，缺乏量化支撑
- 场景举例是构造的，不是真实案例
- "延伸调查方向"提出了问题，但没有回答

---

## 2. round2 要解决什么

round1 能告诉读者"应该怎么做"，但说服力不够——因为缺少：
1. **真实场景**：这个习惯在实际项目里是什么样的，错误的后果有多具体
2. **量化证据**：节省了多少 Token、减少了多少返工、成本差异有多大
3. **可直接用的东西**：模板、配置片段、命令序列，拿来就能用

round2 的目标：给 round1 的每个核心论点配上"证据层"和"工具层"。

---

## 3. round2 的三类文档

### 类型 A：场景深挖（Scenario Deep-Dives）

每份文档聚焦一个高价值场景，展示：
- 错误做法的完整过程（不是一句话带过）
- 正确做法的完整过程（含具体命令序列）
- 可量化的结果差异（Token 消耗、返工次数、时间）
- 真实社区案例或工具行为记录

**不是**对 round1 原则的重复，而是把原则"演出来"。

### 类型 B：证据文档（Evidence Docs）

回答 round1 各章"延伸调查方向"里的具体问题。
每份文档聚焦一个可研究的问题，给出：
- 已有的公开数据（官方文档、基准测试、社区报告）
- 数据的局限性（什么情况下不适用）
- 对 round1 相关章节的影响（是否需要修订）

### 类型 C：实用模板（Practical Templates）

可以直接复制使用的东西：
- 规则文件模板（CLAUDE.md、AGENTS.md 的精简版）
- 会话交接文档模板
- 提示词模板（按任务类型）
- 工具配置片段（Aider、Codex CLI、Claude Code 的常用配置）

---

## 4. 具体文档列表

### 类型 A：场景深挖（6 份）

| 文件名 | 对应 round1 章节 | 核心场景 |
|--------|----------------|---------|
| `A1_context_rot_in_practice.md` | 第一章 | 一个真实的"大杂烩会话"从正常到崩溃的完整过程，含 Token 消耗曲线 |
| `A2_plan_mode_real_workflow.md` | 第三章 | 一次跨 5 个文件的重构，有规划 vs 无规划的完整对比（含 diff 示例） |
| `A3_rule_file_token_math.md` | 第四章 | 11,000 Token 规则文件 vs 800 Token 精简版的实际成本计算，含团队年度节省估算 |
| `A4_api_integration_failure.md` | 第五章 | AI 通过 Web Search 生成错误 API 调用的完整案例，含 MCP 替代方案对比 |
| `A5_model_routing_cost_calc.md` | 第七章 | 一个开发者一周的任务分布，全用旗舰 vs 分级路由的成本对比（含具体数字） |
| `A6_sandbox_workflow.md` | 第八章 | Codex CLI 沙箱在 Bug 修复验证场景的完整工作流，含 approval mode 选择逻辑 |

### 类型 B：证据文档（4 份）

| 文件名 | 回答的问题 | 来源 |
|--------|-----------|------|
| `B1_context_rot_evidence.md` | Context Rot 的临界点在哪里？不同工具的抵抗能力有差异吗？ | 官方文档、社区报告、工具行为记录 |
| `B2_kv_cache_real_numbers.md` | KV Cache 的实际命中率和节省额是多少？Aider `--cache-prompts` 的实测数据 | Anthropic/Google 官方定价文档、Aider 文档 |
| `B3_model_quality_gap.md` | Sonnet vs Opus 在日常编码任务上的质量差距有多大？本地模型的可用性边界在哪里？ | 公开基准测试、社区对比报告 |
| `B4_session_handoff_quality.md` | AI 生成的会话快照 vs 人工描述，启动新会话的效果差异有多大？ | 社区案例、工具文档 |

### 类型 C：实用模板（3 份）

| 文件名 | 内容 |
|--------|------|
| `C1_rule_file_templates.md` | CLAUDE.md、AGENTS.md、.cursor/rules/*.mdc 的精简模板，含注释说明每行存在的理由 |
| `C2_session_handoff_templates.md` | 会话交接文档模板（通用版 + Claude Code 版 + Codex CLI 版），含填写指南 |
| `C3_prompt_templates.md` | 按任务类型整理的提示词模板：规划阶段、执行阶段、审查阶段、调试阶段 |

---

## 5. 优先级与执行顺序

### 第一批（最高价值，先做）

1. `C1_rule_file_templates.md` — 最直接可用，读者拿来就能改
2. `A3_rule_file_token_math.md` — 数字最具说服力，计算逻辑清晰
3. `A5_model_routing_cost_calc.md` — 成本对比最容易量化
4. `B2_kv_cache_real_numbers.md` — 官方定价文档可直接引用

### 第二批（场景说服力）

5. `A1_context_rot_in_practice.md` — 最常见的问题，场景最有共鸣
6. `A2_plan_mode_real_workflow.md` — 第三章是最重要的习惯之一
7. `C2_session_handoff_templates.md` — 直接可用

### 第三批（补充证据）

8. `A4_api_integration_failure.md`
9. `A6_sandbox_workflow.md`
10. `B1_context_rot_evidence.md`
11. `B3_model_quality_gap.md`
12. `B4_session_handoff_quality.md`
13. `C3_prompt_templates.md`

---

## 6. 写作约束

### 场景文档（类型 A）的要求
- 每个场景必须有完整的命令序列（可复现）
- 错误路径和正确路径都要写完整，不能只写结论
- 量化数字必须有来源（官方定价 × 估算用量，或社区实测报告）
- 不能把"可能发生"写成"一定发生"

### 证据文档（类型 B）的要求
- 明确区分"官方文档可证"和"社区报告/估算"
- 对每个数据点说明适用条件和局限性
- 如果某个问题目前没有可靠数据，直接说"目前无可靠公开数据"，不要凑数字

### 模板文档（类型 C）的要求
- 每个模板必须是可以直接复制使用的
- 每行注释说明"为什么这行在这里"
- 提供"最小版"（只有必须的内容）和"完整版"（含常见扩展）

---

## 7. round2 与 round1 的关系

round2 不修改 round1。如果 round2 的研究发现 round1 有需要修订的地方，记录在对应的证据文档里，由人工决定是否回头改 round1。

round2 文档可以独立阅读，也可以作为 round1 对应章节的"配套材料"。

---

## 8. 不做什么

- 不重复 round1 的原则陈述
- 不写没有来源的量化数字
- 不把工具的未来规划当成当前事实
- 不写超出当前工具能力的"理想工作流"
- 不做 round1 的润色或重写（那是 round3 的事）
