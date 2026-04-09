# 2026-04-09 Canonical Landing Index

Run ID: `2026-04-09_additional_practices_r1`

## 目的

给后续 `Merge`、`apply_matrix` 和最终落章设计提供稳定的 `baseline_anchor_id`，避免后面只靠“好像在某个文件里提过”这种口头描述。

## Anchor 规则

- `baseline_anchor_id` 固定格式：`ba.<tool>.<layer>.<slug>`
- `tool` 取值：`codex_cli`、`claude_code`、`cursor`、`opencode`
- `layer` 取值：`readme`、`practice`、`command`、`reference`
- `slug` 取当前文件稳定语义名，而不是行号或临时标题

补充说明：

- 当前索引以“文件级锚点”为主
- `practices/` 的大多数文件遵循稳定模板：`核心习惯 -> 在工具里怎么做 -> 反例/边界 -> 验收标准 -> 对应支撑`
- `commands/` 的大多数文件遵循稳定模板：`入口/命令 -> 映射/边界 -> 易错点`
- 如果后续 `Step 5` 需要更细粒度落位，可在引用这些文件级锚点的基础上，再追加具体章节标题

## Codex CLI

| baseline_anchor_id | Layer | Path | 当前作用 |
| --- | --- | --- | --- |
| `ba.codex_cli.readme.root` | readme | `practice_codex_cli/README.md` | 总入口、阅读顺序、现实边界摘要 |
| `ba.codex_cli.practice.session_lifecycle` | practice | `practice_codex_cli/practices/01_session_lifecycle.md` | 会话边界与继续/分叉判断 |
| `ba.codex_cli.practice.edit_not_append` | practice | `practice_codex_cli/practices/02_edit_not_append.md` | 纠错重试与修改原意图 |
| `ba.codex_cli.practice.plan_before_code` | practice | `practice_codex_cli/practices/03_plan_before_code.md` | 读写分离与计划先行 |
| `ba.codex_cli.practice.rule_hierarchy` | practice | `practice_codex_cli/practices/04_rule_hierarchy.md` | `AGENTS.md` 驱动的规则分层 |
| `ba.codex_cli.practice.progressive_feed` | practice | `practice_codex_cli/practices/05_progressive_feed.md` | 渐进式补材料与信息外化 |
| `ba.codex_cli.practice.kv_cache` | practice | `practice_codex_cli/practices/06_kv_cache.md` | 稳定前缀、慢变规则与快变任务拆分 |
| `ba.codex_cli.practice.model_routing` | practice | `practice_codex_cli/practices/07_model_routing.md` | 按复杂度选模型 |
| `ba.codex_cli.practice.tool_permission` | practice | `practice_codex_cli/practices/08_tool_permission.md` | 最小权限与按阶段抬升 |
| `ba.codex_cli.practice.memento_workflow` | practice | `practice_codex_cli/practices/09_memento_workflow.md` | resume 与 handoff 双层状态传递 |
| `ba.codex_cli.practice.multi_agent` | practice | `practice_codex_cli/practices/10_multi_agent.md` | 职责隔离后再并行 |
| `ba.codex_cli.practice.billing_tips` | practice | `practice_codex_cli/practices/11_billing_tips.md` | 成本、额度与形态控制 |
| `ba.codex_cli.practice.interactive_only` | practice | `practice_codex_cli/practices/12_interactive_only.md` | interactive-only slash commands 的使用边界 |
| `ba.codex_cli.practice.advanced_agentic_coordination` | practice | `practice_codex_cli/practices/13_advanced_agentic_coordination.md` | 角色隔离、文件隔离、运行时隔离 |
| `ba.codex_cli.command.session_commands` | command | `practice_codex_cli/commands/01_session_commands.md` | 交互式、一次性 exec、分步 exec 与 resume |
| `ba.codex_cli.command.retry_and_branch_commands` | command | `practice_codex_cli/commands/02_retry_and_branch_commands.md` | 回滚后重试与交互式分叉 |
| `ba.codex_cli.command.plan_and_execute_commands` | command | `practice_codex_cli/commands/03_plan_and_execute_commands.md` | 只读规划、基于计划执行、链式推进 |
| `ba.codex_cli.command.agents_and_rules_commands` | command | `practice_codex_cli/commands/04_agents_and_rules_commands.md` | `AGENTS.md` 布局、发现顺序与验证 |
| `ba.codex_cli.command.io_and_handoff_commands` | command | `practice_codex_cli/commands/05_io_and_handoff_commands.md` | `stdin`、文件外化与 handoff 输入 |
| `ba.codex_cli.command.stable_prefix_commands` | command | `practice_codex_cli/commands/06_stable_prefix_commands.md` | 用户级规则、`CODEX_HOME` 与指令链稳定性 |
| `ba.codex_cli.command.model_selection_commands` | command | `practice_codex_cli/commands/07_model_selection_commands.md` | 显式选模型与模型路由现实 |
| `ba.codex_cli.command.sandbox_and_approval_commands` | command | `practice_codex_cli/commands/08_sandbox_and_approval_commands.md` | sandbox、approval 与多代理权限现实 |
| `ba.codex_cli.command.resume_and_handoff_commands` | command | `practice_codex_cli/commands/09_resume_and_handoff_commands.md` | 交互/非交互续跑与 handoff 模板 |
| `ba.codex_cli.command.subagents_and_parallel_commands` | command | `practice_codex_cli/commands/10_subagents_and_parallel_commands.md` | 内置 subagents、自定义 agent 与并行深度控制 |
| `ba.codex_cli.command.cost_and_auth_commands` | command | `practice_codex_cli/commands/11_cost_and_auth_commands.md` | 登录方式、成本形态与认证 |
| `ba.codex_cli.command.interactive_only_commands` | command | `practice_codex_cli/commands/12_interactive_only_commands.md` | 会话内高价值 slash commands |
| `ba.codex_cli.command.advanced_agentic_coordination_commands` | command | `practice_codex_cli/commands/13_advanced_agentic_coordination_commands.md` | subagents、外部 worktree、运行时观察面 |
| `ba.codex_cli.reference.slash_commands_reference` | reference | `practice_codex_cli/reference/2026-04-09_codex_cli_slash_commands_reference.md` | Codex CLI slash command grounding |
| `ba.codex_cli.reference.advanced_agentic_features_reference` | reference | `practice_codex_cli/reference/2026-04-09_codex_cli_advanced_agentic_features_reference.md` | subagents、worktree、cloud/long horizon grounding |

## Claude Code

| baseline_anchor_id | Layer | Path | 当前作用 |
| --- | --- | --- | --- |
| `ba.claude_code.readme.root` | readme | `practice_claude_code/README.md` | 总入口、阅读顺序、slash/advanced 概览 |
| `ba.claude_code.practice.session_lifecycle` | practice | `practice_claude_code/practices/01_session_lifecycle.md` | 会话边界与继续/分叉判断 |
| `ba.claude_code.practice.edit_not_append` | practice | `practice_claude_code/practices/02_edit_not_append.md` | 纠错重试与修改原意图 |
| `ba.claude_code.practice.plan_before_code` | practice | `practice_claude_code/practices/03_plan_before_code.md` | 读写分离与计划先行 |
| `ba.claude_code.practice.rule_hierarchy` | practice | `practice_claude_code/practices/04_rule_hierarchy.md` | `CLAUDE.md`、rules 与上下文层级 |
| `ba.claude_code.practice.progressive_feed` | practice | `practice_claude_code/practices/05_progressive_feed.md` | 先给锚点，再补材料 |
| `ba.claude_code.practice.kv_cache` | practice | `practice_claude_code/practices/06_kv_cache.md` | 稳定前缀与历史压缩 |
| `ba.claude_code.practice.model_routing` | practice | `practice_claude_code/practices/07_model_routing.md` | 强模型做重活、轻模型做轻活 |
| `ba.claude_code.practice.tool_permission` | practice | `practice_claude_code/practices/08_tool_permission.md` | 工具白名单与权限控制 |
| `ba.claude_code.practice.memento_workflow` | practice | `practice_claude_code/practices/09_memento_workflow.md` | 会话、memory、handoff 三层分工 |
| `ba.claude_code.practice.multi_agent` | practice | `practice_claude_code/practices/10_multi_agent.md` | 探索、实现、审查隔离 |
| `ba.claude_code.practice.billing_tips` | practice | `practice_claude_code/practices/11_billing_tips.md` | 成本与批处理节奏 |
| `ba.claude_code.practice.interactive_only_session_ops` | practice | `practice_claude_code/practices/12_interactive_only_session_ops.md` | 会话内 slash 动作层 |
| `ba.claude_code.practice.advanced_agentic_coordination` | practice | `practice_claude_code/practices/13_advanced_agentic_coordination.md` | subagents、worktree、agent teams、schedule |
| `ba.claude_code.command.session_commands` | command | `practice_claude_code/commands/01_session_commands.md` | `--continue`、`--resume`、`--fork-session` |
| `ba.claude_code.command.retry_and_branch_commands` | command | `practice_claude_code/commands/02_retry_and_branch_commands.md` | 回滚后重试、分叉与 slash 对应 |
| `ba.claude_code.command.plan_and_execute_commands` | command | `practice_claude_code/commands/03_plan_and_execute_commands.md` | 规划、执行与 plan mode |
| `ba.claude_code.command.rules_and_context_commands` | command | `practice_claude_code/commands/04_rules_and_context_commands.md` | `CLAUDE.md`、`.claude/rules` 与上下文载体 |
| `ba.claude_code.command.io_and_handoff_commands` | command | `practice_claude_code/commands/05_io_and_handoff_commands.md` | `--print`、资源输入与 handoff 模板 |
| `ba.claude_code.command.compact_and_stable_prefix_commands` | command | `practice_claude_code/commands/06_compact_and_stable_prefix_commands.md` | compact、稳定前缀与历史治理 |
| `ba.claude_code.command.model_selection_commands` | command | `practice_claude_code/commands/07_model_selection_commands.md` | 模型选择与路由现实 |
| `ba.claude_code.command.permission_and_tools_commands` | command | `practice_claude_code/commands/08_permission_and_tools_commands.md` | permission mode、tool control、白名单 |
| `ba.claude_code.command.memory_resume_handoff_commands` | command | `practice_claude_code/commands/09_memory_resume_handoff_commands.md` | memory、resume、handoff |
| `ba.claude_code.command.subagents_and_worktree_commands` | command | `practice_claude_code/commands/10_subagents_and_worktree_commands.md` | 内置/临时 subagents 与 worktree |
| `ba.claude_code.command.cost_and_batch_commands` | command | `practice_claude_code/commands/11_cost_and_batch_commands.md` | 成本、budget 与批处理 |
| `ba.claude_code.command.interactive_only_session_ops` | command | `practice_claude_code/commands/12_interactive_only_session_ops.md` | 高价值 interactive-only slash commands |
| `ba.claude_code.command.advanced_agentic_coordination_commands` | command | `practice_claude_code/commands/13_advanced_agentic_coordination_commands.md` | agent teams、schedule、长程协调 |
| `ba.claude_code.reference.command_surface_mapping` | reference | `practice_claude_code/reference/2026-04-09_claude_code_command_surface_mapping.md` | command surface 与 slash mapping grounding |
| `ba.claude_code.reference.advanced_agentic_features_reference` | reference | `practice_claude_code/reference/2026-04-09_claude_code_advanced_agentic_features_reference.md` | 高级 agentic 能力 grounding |

## Cursor

| baseline_anchor_id | Layer | Path | 当前作用 |
| --- | --- | --- | --- |
| `ba.cursor.readme.root` | readme | `practice_cursor/README.md` | 总入口、阅读顺序、IDE 行为模型摘要 |
| `ba.cursor.practice.session_lifecycle` | practice | `practice_cursor/practices/01_session_lifecycle.md` | tab 边界就是任务边界 |
| `ba.cursor.practice.edit_not_append` | practice | `practice_cursor/practices/02_edit_not_append.md` | 先回退 agent 改动，再重写提示 |
| `ba.cursor.practice.plan_before_code` | practice | `practice_cursor/practices/03_plan_before_code.md` | `Ask` 先读，`Agent` 再写 |
| `ba.cursor.practice.rule_hierarchy` | practice | `practice_cursor/practices/04_rule_hierarchy.md` | rules、memories 与上下文层级 |
| `ba.cursor.practice.progressive_feed` | practice | `practice_cursor/practices/05_progressive_feed.md` | 先给锚点，再补材料 |
| `ba.cursor.practice.kv_cache` | practice | `practice_cursor/practices/06_kv_cache.md` | 稳定前缀与长对话管理 |
| `ba.cursor.practice.model_routing` | practice | `practice_cursor/practices/07_model_routing.md` | 模型与 Max Mode 的成本/能力分配 |
| `ba.cursor.practice.tool_permission` | practice | `practice_cursor/practices/08_tool_permission.md` | mode、guardrails 与边界控制 |
| `ba.cursor.practice.memento_workflow` | practice | `practice_cursor/practices/09_memento_workflow.md` | tab、memories、handoff 三层分工 |
| `ba.cursor.practice.multi_agent` | practice | `practice_cursor/practices/10_multi_agent.md` | tabs 与 Background Agents 的隔离 |
| `ba.cursor.practice.billing_tips` | practice | `practice_cursor/practices/11_billing_tips.md` | usage 与不同消耗类型 |
| `ba.cursor.practice.interactive_command_layer` | practice | `practice_cursor/practices/12_interactive_command_layer.md` | 高频动作沉淀为 custom commands |
| `ba.cursor.practice.advanced_agentic_coordination` | practice | `practice_cursor/practices/13_advanced_agentic_coordination.md` | IDE 后台、跨入口编排与 API 编排 |
| `ba.cursor.command.session_and_tabs` | command | `practice_cursor/commands/01_session_and_tabs.md` | tabs、history 与任务边界 |
| `ba.cursor.command.retry_and_checkpoint_commands` | command | `practice_cursor/commands/02_retry_and_checkpoint_commands.md` | checkpoint 回退与重试顺序 |
| `ba.cursor.command.ask_and_custom_modes_commands` | command | `practice_cursor/commands/03_ask_and_custom_modes_commands.md` | `Ask`、`Agent`、`Manual`、`Custom Modes` |
| `ba.cursor.command.rules_and_context_commands` | command | `practice_cursor/commands/04_rules_and_context_commands.md` | `.cursor/rules`、`AGENTS.md`、memories |
| `ba.cursor.command.context_injection_commands` | command | `practice_cursor/commands/05_context_injection_commands.md` | `@` 引用、代码库检索与 handoff |
| `ba.cursor.command.context_window_and_reset_commands` | command | `practice_cursor/commands/06_context_window_and_reset_commands.md` | 长上下文、summarization、重开与 Max Mode |
| `ba.cursor.command.model_selection_commands` | command | `practice_cursor/commands/07_model_selection_commands.md` | 模型选择与 Max Mode |
| `ba.cursor.command.tools_and_guardrails_commands` | command | `practice_cursor/commands/08_tools_and_guardrails_commands.md` | 工具、guardrails、MCP 与 auto-run |
| `ba.cursor.command.memories_history_handoff_commands` | command | `practice_cursor/commands/09_memories_history_handoff_commands.md` | history、Project Memories、handoff |
| `ba.cursor.command.background_agents_commands` | command | `practice_cursor/commands/10_background_agents_commands.md` | tabs 与 Background Agents 协作 |
| `ba.cursor.command.cost_and_usage_commands` | command | `practice_cursor/commands/11_cost_and_usage_commands.md` | usage、额度与成本观察 |
| `ba.cursor.command.interactive_only_commands` | command | `practice_cursor/commands/12_interactive_only_commands.md` | custom commands 的可复用动作层 |
| `ba.cursor.command.advanced_agentic_coordination_commands` | command | `practice_cursor/commands/13_advanced_agentic_coordination_commands.md` | 后台 agent、跨设备、Slack/API 编排 |
| `ba.cursor.reference.command_surface_mapping` | reference | `practice_cursor/reference/2026-04-09_cursor_command_surface_mapping.md` | Cursor command surface grounding |
| `ba.cursor.reference.advanced_agentic_features_reference` | reference | `practice_cursor/reference/2026-04-09_cursor_advanced_agentic_features_reference.md` | Background Agents 等高级能力 grounding |

## OpenCode

| baseline_anchor_id | Layer | Path | 当前作用 |
| --- | --- | --- | --- |
| `ba.opencode.readme.root` | readme | `practice_opencode/README.md` | 总入口、阅读顺序、TUI/CLI grounding 摘要 |
| `ba.opencode.practice.session_lifecycle` | practice | `practice_opencode/practices/01_session_lifecycle.md` | 会话边界与继续/分叉判断 |
| `ba.opencode.practice.edit_not_append` | practice | `practice_opencode/practices/02_edit_not_append.md` | 回退并重说，而不是无限追加 |
| `ba.opencode.practice.plan_before_code` | practice | `practice_opencode/practices/03_plan_before_code.md` | 用 `plan` agent 做读写分离 |
| `ba.opencode.practice.rule_hierarchy` | practice | `practice_opencode/practices/04_rule_hierarchy.md` | `AGENTS` 稳定、`instructions` 按需补 |
| `ba.opencode.practice.progressive_feed` | practice | `practice_opencode/practices/05_progressive_feed.md` | 先给锚点，再补文件和命令输出 |
| `ba.opencode.practice.kv_cache` | practice | `practice_opencode/practices/06_kv_cache.md` | 慢变规则与长历史压缩 |
| `ba.opencode.practice.model_routing` | practice | `practice_opencode/practices/07_model_routing.md` | 主模型和小模型分工 |
| `ba.opencode.practice.tool_permission` | practice | `practice_opencode/practices/08_tool_permission.md` | 默认全开不代表默认合理 |
| `ba.opencode.practice.memento_workflow` | practice | `practice_opencode/practices/09_memento_workflow.md` | session、export、share、handoff 分层 |
| `ba.opencode.practice.multi_agent` | practice | `practice_opencode/practices/10_multi_agent.md` | 探索、规划、执行隔离 |
| `ba.opencode.practice.billing_tips` | practice | `practice_opencode/practices/11_billing_tips.md` | `stats`、模型与自动化成本控制 |
| `ba.opencode.practice.interactive_only_session_ops` | practice | `practice_opencode/practices/12_interactive_only_session_ops.md` | slash commands 与 keybinds 的会话内控制层 |
| `ba.opencode.practice.advanced_agentic_coordination` | practice | `practice_opencode/practices/13_advanced_agentic_coordination.md` | subtask、共享 backend、handoff 层 |
| `ba.opencode.command.session_commands` | command | `practice_opencode/commands/01_session_commands.md` | 新会话、继续、恢复、分叉与 `/sessions` |
| `ba.opencode.command.retry_and_undo_commands` | command | `practice_opencode/commands/02_retry_and_undo_commands.md` | 回退、重试、分叉与 `/undo` |
| `ba.opencode.command.plan_and_execute_commands` | command | `practice_opencode/commands/03_plan_and_execute_commands.md` | `plan`、执行与工作流切换 |
| `ba.opencode.command.rules_and_instructions_commands` | command | `practice_opencode/commands/04_rules_and_instructions_commands.md` | `AGENTS`、`instructions`、`CLAUDE.md` fallback |
| `ba.opencode.command.context_and_io_commands` | command | `practice_opencode/commands/05_context_and_io_commands.md` | 文件引用、命令输出与 handoff 输入 |
| `ba.opencode.command.compact_and_stable_prefix_commands` | command | `practice_opencode/commands/06_compact_and_stable_prefix_commands.md` | `/compact` 与稳定前缀 |
| `ba.opencode.command.model_and_agent_routing_commands` | command | `practice_opencode/commands/07_model_and_agent_routing_commands.md` | 模型、variants、agent 路由 |
| `ba.opencode.command.permissions_and_tools_commands` | command | `practice_opencode/commands/08_permissions_and_tools_commands.md` | `permission` 与工具边界 |
| `ba.opencode.command.sessions_share_handoff_commands` | command | `practice_opencode/commands/09_sessions_share_handoff_commands.md` | session、share、export/import、server/web/attach |
| `ba.opencode.command.agents_and_parallel_commands` | command | `practice_opencode/commands/10_agents_and_parallel_commands.md` | agents、subtasks 与并行分流 |
| `ba.opencode.command.stats_and_cost_commands` | command | `practice_opencode/commands/11_stats_and_cost_commands.md` | `stats`、模型成本与自动化控制 |
| `ba.opencode.command.interactive_only_session_ops` | command | `practice_opencode/commands/12_interactive_only_session_ops.md` | 交互式专用 session ops |
| `ba.opencode.command.advanced_agentic_coordination_commands` | command | `practice_opencode/commands/13_advanced_agentic_coordination_commands.md` | shared backend、多客户端与高级协调 |
| `ba.opencode.reference.official_reference` | reference | `practice_opencode/reference/2026-04-09_opencode_official_reference.md` | OpenCode 官方事实边界 grounding |
| `ba.opencode.reference.command_surface_mapping` | reference | `practice_opencode/reference/2026-04-09_opencode_command_surface_mapping.md` | CLI/TUI 命令面和 built-ins grounding |
| `ba.opencode.reference.advanced_agentic_features_reference` | reference | `practice_opencode/reference/2026-04-09_opencode_advanced_agentic_features_reference.md` | subagents、worktree 缺失、long horizon 边界 grounding |

## Step 1 用法约束

- 后续 `Merge` 默认先引用这里的 `baseline_anchor_id`
- 如果候选最后只适合进入 `commands/` 或 `reference/`，也仍然要先指向这里的既有锚点
- 不允许在 `apply_matrix` 里只写“并到第 9 章附近”这种模糊描述；必须写出具体 `baseline_anchor_id`
