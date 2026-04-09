# 2026-04-09 Cursor Baseline Audit

Run ID: `2026-04-09_additional_practices_r1`
Phase: `Phase 4. Tool Loop: Cursor`
Step: `Step 2A. Tool Baseline`

## 目标

先确认 `practice_cursor/` 已经讲了什么、还没讲什么，以及哪些内容已经落在 `practices/`、`commands/`、`reference/`，避免把已有表达误判为新增缺口。

## 本次复核范围

- `practice_cursor/README.md`
- `practice_cursor/practices/`
- `practice_cursor/commands/`
- `practice_cursor/reference/`

## 当前工具的既有覆盖骨架

| Layer | 数量 | 结论 |
| --- | ---: | --- |
| `README.md` | 1 | 已明确 Cursor 以 tabs / Ask / Agent / custom modes / Background Agents 为主线，而不是硬套 CLI session 语义 |
| `practices/` | 13 | 已覆盖原始 `11` 个习惯，并补入 `12` 交互命令层、`13` 高级 agentic 协调 |
| `commands/` | 13 | 已把 quick commands、custom commands、rules、history、Background Agents、web/mobile、Slack/GitHub/Linear、API 等落文 |
| `reference/` | 2 | 已对 editor command surface 和 advanced agentic features 做 grounding |

## 已在 practice 层明确表达的内容

| 主题 | 既有锚点 | Step 2A 判断 |
| --- | --- | --- |
| tabs / history / 任务边界 | `ba.cursor.practice.session_lifecycle` | 已明确 Cursor 的主会话单位是 tab 和任务边界，不是 CLI 式 resume 语义 |
| Ask 先读、Agent 再写、Custom Modes 固化流程 | `ba.cursor.practice.plan_before_code` | 已明确 Cursor 没有固定名称的 Plan Mode，但有稳定的 plan-before-execution 组合 |
| 规则层分工 | `ba.cursor.practice.rule_hierarchy` | 已明确 `.cursor/rules/*.mdc`、`AGENTS.md`、legacy `.cursorrules` 的边界 |
| history / memories / handoff | `ba.cursor.practice.memento_workflow` | 已明确 tabs、`@Past Chats`、Project Memories、chat export、handoff 文件的分层 |
| tabs 与 Background Agents 的分流 | `ba.cursor.practice.multi_agent` | 已明确前台轻分流与后台异步分流不是同一层 |
| 跨入口高级编排 | `ba.cursor.practice.advanced_agentic_coordination` | 已明确 IDE 后台、web/mobile、Slack/GitHub/Linear、API 的四层分工 |
| 交互命令层 | `ba.cursor.practice.interactive_command_layer` | 已明确 quick commands / project commands / global commands 与 rules/prompt 的分层 |

## 已在 commands / reference 层明确表达的内容

| 主题 | 既有锚点 | Step 2A 判断 |
| --- | --- | --- |
| editor-side command surface | `ba.cursor.reference.command_surface_mapping` | 已明确 quick commands、`/summarize`、`/Reset Context`、`.cursor/commands`、`~/.cursor/commands` 的边界 |
| Ask / Agent / Custom Modes | `ba.cursor.command.ask_and_custom_modes` | 已明确 Ask 是只读探索，Agent 是执行，Custom Modes 是流程固化 |
| 规则层与规则生成入口 | `ba.cursor.command.rules_and_context` | 已明确 `.mdc`、`alwaysApply`、`globs`、`New Cursor Rule` 等动作 |
| histories / memories / export / handoff | `ba.cursor.command.memories_history_handoff` | 已明确旧对话、Project Memories、导出聊天与 handoff 的分层 |
| Background Agents | `ba.cursor.command.background_agents` | 已明确 remote machine、separate branch、follow-up、take over 的主边界 |
| 高级跨入口编排 | `ba.cursor.command.advanced_agentic_coordination` | 已明确 web/mobile、Slack、GitHub、Linear、API 的入口分工 |

## 当前工具相对全局基线的特殊点

### 1. Cursor 的核心不是 CLI 式 session，而是 tab-centric workflow

Cursor 当前基线最稳定的组织单元是：

- chat tabs
- history / old chats
- `@Past Chats`

这意味着：

- `session_lifecycle` 在 Cursor 上要避免被写成 CLI resume/fork 叙事

### 2. 计划控制面是 Ask / Agent / Custom Modes 的组合，不是单个 plan mode

Cursor 当前已明确：

- Ask 适合只读探索
- Agent 适合执行
- Custom Modes 适合把常见工作流固化

这说明：

- Cursor 的计划控制面更分散，也更依赖团队自己收口

### 3. 命令面厚度来自 quick commands 与 custom commands 的组合

Cursor 当前 command richness 的重点不是超长内建 slash 清单，而是：

- quick commands
- project commands
- global commands
- command palette 内的相关动作

这使得：

- `command_surface_fluency` 在 Cursor 上可能成立
- 但其形态更像“可扩展控制层”，而不是 Claude Code 式 built-in control plane

### 4. 就 Background Agents 本体看，原生隔离重点是 remote branch worker

当前基线已经清楚：

- Background Agents 运行在 isolated remote machine
- 在 separate branch 上工作
- 可 push 回 repo 并回到 IDE 接管

但没有稳定证据显示：

- Cursor editor 有 Claude Code 那种第一方本地 `--worktree` 命令面

这一点只适用于当时已核实的 Background Agents 本体。

后续 validation 已进一步扩大为 Cursor 3 的多环境 handoff 判断：

- local
- worktrees
- cloud
- remote SSH

所以这里不能再外推成“Cursor 整体只有 remote branch 叙事”。

### 5. long horizon 天生是跨入口问题

Cursor 当前已明显分出：

- IDE 内 Background Agents
- web / mobile
- Slack / GitHub / Linear
- Background Agents API

这意味着：

- Cursor 的 long horizon 重点不是“继续当前对话”，而是“如何在多入口远程 worker 之间接续、follow-up 和收口”

## 当前工具的薄弱点与待验证区

### `plan_first_kickoff`

- 当前已写 Ask -> plan file -> human review -> Agent execution
- 但需要验证这在 `2026-01-01` 到 `2026-04-09` 的外部采用层是否已足够稳定

### `command_surface_control_plane`

- 当前已写 quick commands / custom commands / command palette
- 但需要判断这到底是稳定行为变化，还是只是可扩展 convenience layer

### `remote_branch_isolation_threshold`（后续 validation 已扩成 `local_cloud_worktree_handoff_threshold`）

- 当前基线对 Background Agents 本体已经很强，因为它是正式 remote branch worker
- 需要验证 2026 窗口里，用户是否真的把它当成默认异步隔离主路径

### `long_horizon_cross_surface_layering`

- 当前基线已经显式分层
- 需要外部证据判断 IDE、web/mobile、Slack/GitHub/Linear、API 的真实使用阈值

### `memory_hierarchy_and_handoff`

- 当前已写 tabs/history、Project Memories、chat export、handoff
- 需要验证这些层次是否已在高信号工作流里形成明确 discipline

## Step 2A 结论

- `practice_cursor/` 同样已经是成熟基线，不存在大块未覆盖的问题。
- Cursor 下一步外部研究不该再问“有没有这些能力”，而该问：
  - 哪些能力在 `2026-01-01` 到 `2026-04-09` 之间真正改变了行为
  - 哪些只是 editor command surface / beta extensibility 变厚
  - 哪些点已经足以进入跨工具候选池，哪些仍只是 Cursor-specific 叙事

## 建议进入 Step 2B 时优先验证的方向

1. `plan_first_kickoff`
2. `command_surface_control_plane`
3. `remote_branch_isolation_threshold`（后续 validation 已扩成 `local_cloud_worktree_handoff_threshold`）
4. `long_horizon_cross_surface_layering`
5. `memory_hierarchy_and_handoff`

当前仍未分配任何 `tool_candidate_id`。这一步只负责确认 baseline，不提前进入候选卡片。
