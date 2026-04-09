# 2026-04-09 Cross-Tool Candidate Convergence

Run ID: `2026-04-09_additional_practices_r1`
Phase: `Phase 6. Cross-Tool Convergence`
Effective Window: `2026-01-01` to `2026-04-09`

## 目标

在四个工具都完成 `Step 2F` 后，对近义候选做聚类，并为每个仍保留的 canonical 候选分配稳定 `global_candidate_id`。

## 收敛原则

- 不为了整齐而强行统一 vendor-specific 差异
- 允许一个 `global_candidate_id` 只聚合 1 个工具候选
- 如果两个候选只是在同一大主题下互相启发，但判断门槛不同，优先分开
- 这一轮主要做聚类与命名收敛，不在这里直接写最终落章

## Canonical Candidate Set

### `global__plan_first_kickoff`

Canonical meaning:

- 复杂任务默认先进入 plan / research / review loop，再执行；不把“边改边想”当默认起手式。

Source tool candidates:

- `codex_cli__session_kickoff_contract`
- `claude_code__plan_first_kickoff`
- `cursor__plan_first_kickoff`
- `opencode__plan_first_kickoff`

Cross-tool notes:

- 四个工具都成立
- 强度最高的原生支点分别是：
  - Claude: plan mode / built-in commands
  - OpenCode: `plan` primary agent
  - Cursor: Ask + approval before long-running agents
  - Codex: kickoff contract / steer + plan discipline

### `global__command_surface_control_plane`

Canonical meaning:

- 把控制动作、环境调整、状态查询、review / handoff trigger 从自然语言主线程里分层出来，用正式 command surface 完成。

Source tool candidates:

- `codex_cli__command_surface_fluency`
- `claude_code__command_surface_control_plane`
- `cursor__command_surface_control_plane`
- `opencode__command_surface_control_plane`

Cross-tool notes:

- 四个工具都成立
- 但形态不同：
  - Claude / OpenCode 更偏 built-in control plane
  - Cursor 更偏 mode + command extensibility
  - Codex 介于 built-in slash 与 cloud / apply / status commands 之间

### `global__interactive_to_durable_handoff_threshold`

Canonical meaning:

- 要明确区分交互式会话续跑、共享状态延续、后台异步执行与 durable automation；不要把所有“继续跑”混成一种东西。

Source tool candidates:

- `codex_cli__cloud_detach_threshold`
- `claude_code__long_horizon_layering`
- `cursor__long_horizon_cross_surface_layering`
- `cursor__automations_workflow_control_plane`
- `opencode__shared_backend_long_horizon_layering`
- `opencode__external_runner_durable_automation_threshold`

Cross-tool notes:

- 四个工具都成立，但切层点差异很大
- Cursor / OpenCode 在这一组里最容易被写错：
  - Cursor 因为 native agents + automations + multi-entry
  - OpenCode 因为 native shared backend 与 external runner durable automation 并存

### `global__environment_isolation_threshold`

Canonical meaning:

- 并行执行与独立审查不该只靠“多开对话”，而应在合适时切到更清晰的环境隔离层。

Source tool candidates:

- `claude_code__first_party_worktree_isolation`
- `cursor__local_cloud_worktree_handoff_threshold`

Cross-tool notes:

- 这是当前最明显的非全工具对齐候选
- Claude 的事实层最强
- Cursor 在 `2026-04-02` 后开始形成多环境 handoff 叙事，但 adoption 仍早期
- Codex / OpenCode 当前没有等强的第一方环境隔离主叙事

### `global__memory_hierarchy_and_handoff`

Canonical meaning:

- 长期规则、项目记忆、会话状态、分享/导出、handoff 文档要分层，不靠单一记忆桶硬撑。

Source tool candidates:

- `claude_code__memory_hierarchy_and_handoff`
- `cursor__memory_hierarchy_and_handoff`
- `opencode__memory_hierarchy_and_handoff`

Cross-tool notes:

- 三个工具都有强信号
- Codex 本轮没有独立抽成候选，但其相关部分可在后续 apply 时受启发

### `global__bounded_delegation_guardrails`

Canonical meaning:

- 多智能体拆分应保持浅层、有界、职责清楚；主线程负责收口，不把所有 agent 能力都写成 fan-out 默认路径。

Source tool candidates:

- `codex_cli__shallow_delegation_with_guardrails`
- `claude_code__agent_teams_threshold`
- `opencode__bounded_subtask_orchestration`

Cross-tool notes:

- 三个工具都成立
- 但 guardrail 的对象不同：
  - Codex：限制 subagent 深度与职责
  - Claude：不要轻易升级到 agent teams
  - OpenCode：不要把 subtask 误写成 detached worker

### `global__layered_context_packaging`

Canonical meaning:

- 稳定合同、按需加载知识、重复 workflow 与当前任务材料要分层；不要把一切都塞进单一长文件。

Source tool candidates:

- `codex_cli__skills_over_agents_bloat`
- `opencode__skills_as_lazy_context_layer`

Cross-tool notes:

- 当前主要由 Codex 与 OpenCode 提供最强事实锚点
- Claude 对这一组有启发，但本轮更接近 memory/rules 层，而非 skills 候选本身

### `global__agent_specific_permission_profiles`

Canonical meaning:

- 记忆化授权与预设 approval profile 是真实工作流放大器，但当前更像单工具特性，不应强行泛化。

Source tool candidates:

- `codex_cli__approval_profiles_and_remembered_grants`

Cross-tool notes:

- 当前只在 Codex 上有足够清晰的独立候选强度
- 先保留为 agent-specific global candidate

## Current Dedup Decisions

- `cursor__automations_workflow_control_plane` 并入 `global__interactive_to_durable_handoff_threshold`
- `opencode__external_runner_durable_automation_threshold` 并入 `global__interactive_to_durable_handoff_threshold`
- `claude_code__agent_teams_threshold` 不单独保留为 teams-only canonical 候选，而并入 `global__bounded_delegation_guardrails`
- `opencode__skills_as_lazy_context_layer` 与 `codex_cli__skills_over_agents_bloat` 收敛到 `global__layered_context_packaging`

## Candidate Count After Convergence

- pre-convergence active tool candidates: `25`
- post-convergence canonical candidates: `8`

## Next Step

- 进入 `Phase 7`
- 对这 `8` 个 `global_candidate_id` 做 `Promote / Merge / Hold / Reject`
- 生成 decision memos，并落到 review queue
