# Final Recommendation

Status: completed `Step 6`
Run ID: `2026-04-09_additional_practices_r1`
Supersedes Run ID: none

## 总结结论

这轮调研的答案是：

- 现有 4 个工具目录并不是“明显缺一批全新章节”
- 真正的缺口主要是：
  - 旧章节还不够明确地表达新的行为阈值
  - 某些原本写在 `commands/`、`reference/` 的事实，已经值得上升成更清晰的 practice 叙述
  - 部分工具的能力边界在 `2026-01-01` 到 `2026-04-09` 之间已经变化，旧基线需要校正

因此，本轮最合理的结论不是“新增很多 practice 章节”，而是：

- 以 `Merge` 为主
- 以 `Hold` 为辅
- 暂不新增正式章节

## 当前最高优先级的补充方向

优先级最高的 `Merge` 候选共有 `6` 组：

1. `global__plan_first_kickoff`
2. `global__command_surface_control_plane`
3. `global__interactive_to_durable_handoff_threshold`
4. `global__environment_isolation_threshold`
5. `global__memory_hierarchy_and_handoff`
6. `global__bounded_delegation_guardrails`

其中最该优先 apply 的是前 `4` 组，因为它们最直接改变：

- 任务如何起步
- 控制动作如何从主提示词中分层
- 长任务何时切到 detached / durable surface
- 并行执行何时切到更强隔离层

## 当前不建议直接 promote 的方向

本轮 `Hold` 的候选有 `2` 组：

1. `global__layered_context_packaging`
   - 方向正确，但跨工具 adoption 还不够强
   - 需要防止误写成“skills 替代 rules / AGENTS”
2. `global__agent_specific_permission_profiles`
   - 当前主要成立于 Codex
   - 更像 agent-specific workflow amplifier

## 对后续正式 apply 的建议

这轮 apply 应按下面顺序推进：

1. 先补 `03_plan_before_code`、`12_interactive_*`、`13_advanced_agentic_coordination`
2. 再补 `09_memento_workflow`
3. 然后才处理 `10_multi_agent` 与 `04_rule_hierarchy`
4. 对 `Cursor` 与 `Claude Code`，优先修正环境隔离与长程分层的旧边界
5. 对 `OpenCode`，优先修正“只有 shared backend、没有 durable automation 路径”的过窄表述
6. 对 `Codex CLI`，优先把 kickoff / cloud threshold / shallow delegation 写得更像行为纪律，而不是 feature list

## 本轮不做的事

- 不新开正式 practice 章节
- 不把单工具特性强行升格成 canonical 通用实践
- 不把单个 command 名称表直接翻译成 practice 目录结构

## 可直接作为施工输入的文件

- 跨工具收敛：
  - `01_global/cross_tool/2026-04-09_cross_tool_candidate_convergence.md`
- review queue：
  - `90_review_queue/merge/`
  - `90_review_queue/hold/`
- apply matrix：
  - `90_review_queue/apply_matrix/2026-04-09_apply_matrix.md`

## 最终判断

我们还有缺 practice 吗？

- 有，但主要是“现有 practice 表达不够新”，不是“目录里缺新章”。

哪些方向最值得补？

- 开局契约
- 控制面分层
- 交互式会话到 durable automation 的切层阈值
- 环境隔离阈值
- memory / handoff 分层
- bounded delegation guardrails

哪些方向现在先不要补成正式 practice？

- skills / lazy context packaging
- agent-specific approval profile amplifiers
