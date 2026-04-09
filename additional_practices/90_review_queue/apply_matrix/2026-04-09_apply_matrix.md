# 2026-04-09 Apply Matrix

Run ID: `2026-04-09_additional_practices_r1`
Phase: `Phase 8. Apply Matrix Design`

## 目标

把 `Merge / Hold` 判断翻译成未来落章方式，精确到工具内文件。

## Merge Candidates

| Global Candidate | Tool | Primary Landing | Secondary Landing | Notes |
| --- | --- | --- | --- | --- |
| `global__plan_first_kickoff` | Codex CLI | `practice_codex_cli/practices/01_session_lifecycle.md` | `practice_codex_cli/practices/03_plan_before_code.md`, `practice_codex_cli/commands/03_plan_and_execute_commands.md` | 强化 kickoff contract 与 plan review loop |
| `global__plan_first_kickoff` | Claude Code | `practice_claude_code/practices/03_plan_before_code.md` | `practice_claude_code/practices/01_session_lifecycle.md`, `practice_claude_code/commands/03_plan_and_execute_commands.md` | 补强 plan mode 不是一次性 feature，而是开局纪律 |
| `global__plan_first_kickoff` | Cursor | `practice_cursor/practices/03_plan_before_code.md` | `practice_cursor/commands/03_ask_and_custom_modes_commands.md` | 明确 Ask / approval / Agent 执行的节奏 |
| `global__plan_first_kickoff` | OpenCode | `practice_opencode/practices/03_plan_before_code.md` | `practice_opencode/commands/03_plan_and_execute_commands.md`, `practice_opencode/practices/01_session_lifecycle.md` | 强调 `plan` primary agent 的默认起手位 |
| `global__command_surface_control_plane` | Codex CLI | `practice_codex_cli/practices/12_interactive_only.md` | `practice_codex_cli/commands/12_interactive_only_commands.md`, relevant command refs | 把 slash / cloud / apply / status 视为控制层 |
| `global__command_surface_control_plane` | Claude Code | `practice_claude_code/practices/12_interactive_only_session_ops.md` | `practice_claude_code/commands/12_interactive_only_session_ops.md` | 强化 built-in commands 是正式控制面 |
| `global__command_surface_control_plane` | Cursor | `practice_cursor/practices/12_interactive_command_layer.md` | `practice_cursor/commands/12_interactive_only_commands.md`, `practice_cursor/commands/03_ask_and_custom_modes_commands.md` | 强调 modes + commands 的柔性控制层 |
| `global__command_surface_control_plane` | OpenCode | `practice_opencode/practices/12_interactive_only_session_ops.md` | `practice_opencode/commands/12_interactive_only_session_ops.md`, `practice_opencode/commands/01_session_commands.md` | 强化 multi-layer command surface |
| `global__interactive_to_durable_handoff_threshold` | Codex CLI | `practice_codex_cli/practices/13_advanced_agentic_coordination.md` | `practice_codex_cli/practices/09_memento_workflow.md`, `practice_codex_cli/commands/13_advanced_agentic_coordination_commands.md` | 明确 local resume vs cloud detach |
| `global__interactive_to_durable_handoff_threshold` | Claude Code | `practice_claude_code/practices/13_advanced_agentic_coordination.md` | `practice_claude_code/practices/09_memento_workflow.md`, `practice_claude_code/commands/13_advanced_agentic_coordination_commands.md` | 明确 resume / loop / schedule / GHA layering |
| `global__interactive_to_durable_handoff_threshold` | Cursor | `practice_cursor/practices/13_advanced_agentic_coordination.md` | `practice_cursor/commands/13_advanced_agentic_coordination_commands.md`, `practice_cursor/commands/10_background_agents_commands.md` | 加入 automations 与 multi-entry durable threshold |
| `global__interactive_to_durable_handoff_threshold` | OpenCode | `practice_opencode/practices/13_advanced_agentic_coordination.md` | `practice_opencode/practices/09_memento_workflow.md`, `practice_opencode/commands/13_advanced_agentic_coordination_commands.md` | 区分 native shared backend 与 external runner automation |
| `global__environment_isolation_threshold` | Claude Code | `practice_claude_code/practices/13_advanced_agentic_coordination.md` | `practice_claude_code/practices/10_multi_agent.md`, `practice_claude_code/commands/10_subagents_and_worktree_commands.md` | 强化 worktree 作为默认并行隔离原语 |
| `global__environment_isolation_threshold` | Cursor | `practice_cursor/practices/13_advanced_agentic_coordination.md` | `practice_cursor/commands/10_background_agents_commands.md`, `practice_cursor/commands/13_advanced_agentic_coordination_commands.md` | 校正到 Cursor 3 的 local/worktree/cloud/remote SSH |
| `global__environment_isolation_threshold` | Codex CLI | `no-op` | `no-op` | 当前不强行补成等价能力 |
| `global__environment_isolation_threshold` | OpenCode | `no-op` | `no-op` | 当前不补不存在的第一方环境隔离面 |
| `global__memory_hierarchy_and_handoff` | Claude Code | `practice_claude_code/practices/09_memento_workflow.md` | `practice_claude_code/practices/04_rule_hierarchy.md`, `practice_claude_code/commands/09_memory_resume_handoff_commands.md` | 强化 rules / memory / handoff layering |
| `global__memory_hierarchy_and_handoff` | Cursor | `practice_cursor/practices/09_memento_workflow.md` | `practice_cursor/commands/09_memories_history_handoff_commands.md` | 明确 history / memories / background chat / handoff 分层 |
| `global__memory_hierarchy_and_handoff` | OpenCode | `practice_opencode/practices/09_memento_workflow.md` | `practice_opencode/commands/09_sessions_share_handoff_commands.md`, `practice_opencode/practices/13_advanced_agentic_coordination.md` | 明确 session / shared backend / export / share / handoff 分层 |
| `global__memory_hierarchy_and_handoff` | Codex CLI | `optional inspiration only` | `practice_codex_cli/practices/09_memento_workflow.md` | 本轮无独立候选，但可吸收语义收紧 |
| `global__bounded_delegation_guardrails` | Codex CLI | `practice_codex_cli/practices/10_multi_agent.md` | `practice_codex_cli/practices/13_advanced_agentic_coordination.md`, `practice_codex_cli/commands/10_subagents_and_parallel_commands.md` | 强化 shallow delegation 与主线程收口 |
| `global__bounded_delegation_guardrails` | Claude Code | `practice_claude_code/practices/13_advanced_agentic_coordination.md` | `practice_claude_code/practices/10_multi_agent.md`, `practice_claude_code/commands/10_subagents_and_worktree_commands.md` | 强化 teams 是高门槛升级，不是默认入口 |
| `global__bounded_delegation_guardrails` | OpenCode | `practice_opencode/practices/10_multi_agent.md` | `practice_opencode/practices/13_advanced_agentic_coordination.md`, `practice_opencode/commands/10_agents_and_parallel_commands.md` | 强化 subtask bounded orchestration |
| `global__bounded_delegation_guardrails` | Cursor | `optional inspiration only` | `practice_cursor/practices/10_multi_agent.md` | 当前无强等价候选，先不硬并 |

## Hold Candidates

| Global Candidate | Current Status | Likely Future Landing | Reason To Wait |
| --- | --- | --- | --- |
| `global__layered_context_packaging` | `Hold` | `04_rule_hierarchy`, related rules/skills docs in Codex/OpenCode | 事实层强，但跨工具 adoption 层仍不足，容易误写成 “skills 替代 rules/AGENTS” |
| `global__agent_specific_permission_profiles` | `Hold` | Codex-specific `08_tool_permission` and auth/approval commands | 当前只有 Codex 上成立，不适合提前泛化 |

## No New Chapter Recommendation

当前所有 `Merge` 候选都应并入现有章节；这轮没有需要单独新增正式 practice 章节的候选。
