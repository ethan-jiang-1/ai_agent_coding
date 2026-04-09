# 2026-04-09 OpenCode Baseline Audit

Run ID: `2026-04-09_additional_practices_r1`
Phase: `Phase 5. Tool Loop: OpenCode`
Step: `Step 2A. Tool Baseline`

## 目标

先确认 `practice_opencode/` 已经讲了什么、还没讲什么，以及哪些内容已经落在 `practices/`、`commands/`、`reference/`，避免把已有表达误判为新增缺口。

## 本次复核范围

- `practice_opencode/README.md`
- `practice_opencode/practices/`
- `practice_opencode/commands/`
- `practice_opencode/reference/`

## 当前工具的既有覆盖骨架

| Layer | 数量 | 结论 |
| --- | ---: | --- |
| `README.md` | 1 | 已明确 OpenCode 以默认 TUI、slash commands、plan/build agents、share/export/attach 为主线 |
| `practices/` | 13 | 已覆盖原始 `11` 个习惯，并补入 `12` 会话内操作层、`13` 高级 agentic 协调 |
| `commands/` | 13 | 已把 TUI built-ins、built-in commands、custom commands、permissions、share/export/import、serve/web/attach 等落文 |
| `reference/` | 3 | 已对官方事实、command surface、advanced agentic features 做 grounding，并补了本机 app-bundled CLI help 与本地注册点核验 |

## 已在 practice 层明确表达的内容

| 主题 | 既有锚点 | Step 2A 判断 |
| --- | --- | --- |
| 会话继续、切换与分叉 | `ba.opencode.practice.session_lifecycle` | 已明确 `--continue`、`--session`、`--fork` 与 TUI `/sessions` 的分工 |
| 规划与执行分离 | `ba.opencode.practice.plan_before_code` | 已明确 `plan` agent 与 `build` agent 的分工，不依赖虚构的内建 `/plan` |
| 规则层与 instructions | `ba.opencode.practice.rule_hierarchy` | 已明确 `AGENTS.md`、global `AGENTS.md`、`instructions`、`CLAUDE.md` fallback 的边界 |
| session / share / export / handoff 分层 | `ba.opencode.practice.memento_workflow` | 已明确当前 session、shared backend、export/import、share link、handoff 文件的不同用途 |
| subagent / subtask 分流 | `ba.opencode.practice.multi_agent` | 已明确 `plan`、`build`、`@general`、`@explore` 与 `subtask: true` 的使用边界 |
| 会话内 TUI 操作层 | `ba.opencode.practice.interactive_only_session_ops` | 已明确 slash commands、keybinds、command palette 与主提示词分层 |
| 高级协调 | `ba.opencode.practice.advanced_agentic_coordination` | 已明确 subtask、shared backend、share/export/handoff 和 absence of worktree / scheduler |

## 已在 commands / reference 层明确表达的内容

| 主题 | 既有锚点 | Step 2A 判断 |
| --- | --- | --- |
| TUI slash built-ins 与 built-in command layer | `ba.opencode.reference.command_surface_mapping` | 已明确 `/sessions`、`/new`、`/agents`、`/share`、`/compact`、`/undo`、`/editor`、`/init`、`/review` 等分层 |
| custom commands / MCP prompt commands / skills 边界 | `ba.opencode.reference.command_surface_mapping` | 已明确 custom commands 会覆盖 built-ins、可绑定 `agent`、`subtask`、`model` |
| permission / tools / pattern matching | `ba.opencode.command.permissions_and_tools` | 已明确 `permission` 主体规则、`last matching rule wins` 与 agent 级工具收窄 |
| shared backend 与多客户端接续 | `ba.opencode.command.advanced_agentic_coordination` | 已明确 `serve`、`web`、`attach`、`run --attach` 的作用 |
| share / export / import | `ba.opencode.command.sessions_share_handoff` | 已明确 share link、session export/import、handoff 模板的分层 |
| absence of worktree / detached worker / scheduler | `ba.opencode.reference.advanced_agentic_features` | 已明确 reviewed docs 里没有这些第一方能力 |

## 当前工具相对全局基线的特殊点

### 1. OpenCode 的计划控制面不是 mode，而是明确的 `plan` primary agent

当前基线已经清楚：

- `build` 是默认执行 agent
- `plan` 默认把 edits / bash 收成 `ask`
- 可以通过 `--agent`、会话切换或自定义 command 进入 `plan`

这意味着：

- `plan_first_kickoff` 在 OpenCode 上有非常明确的原生支点

### 2. OpenCode 的交互控制面是 TUI built-ins + built-in commands + custom commands

OpenCode 当前命令面不是单一 slash 列表，而是至少三层：

- TUI built-ins
- `/init`、`/review` 这种 built-in command layer
- project/global custom commands

再加上：

- keybind layer
- MCP prompt commands

这说明：

- `command_surface_fluency` 在 OpenCode 上不是弱命题

### 3. long horizon 的正式主轴是 shared backend，不是 detached worker

当前基线已经清楚：

- `serve`
- `web`
- `attach`
- `run --attach`

的正式意义是：

- 多客户端接同一 backend / session state

而不是：

- scheduler
- cloud job runner
- long-running autonomous worker

### 4. share / export / import / handoff 是不同层，不应混写

OpenCode 的跨时段协作层至少包括：

- 当前 session 继续
- shared backend
- export/import
- public share link
- handoff 文件

这比“继续会话”一条线更细，也更容易写错。

### 5. absence 本身是重要边界

当前 reviewed docs 范围里，没有稳定证据显示 OpenCode 当前有：

- 第一方 worktree
- detached background agent
- scheduler / cron / durable autonomous jobs

这意味着：

- `absence guardrail` 本身已经是正式写作约束

## 当前工具的薄弱点与待验证区

### `plan_first_kickoff`

- 当前已写 `plan` agent 开局
- 但需要验证 `2026-01-01` 到 `2026-04-09` 之间，官方与外部材料是否把这一路径进一步强化成稳定 workflow

### `command_surface_control_plane`

- 当前基线已经很强，因为 OpenCode 的 TUI / built-in commands / custom commands 是多层命令面
- 需要验证这些命令层到底只是 feature density，还是已经稳定改变行为

### `shared_backend_long_horizon_layering`

- 当前基线已经清楚 `serve` / `web` / `attach`
- 需要外部证据判断多客户端 shared state 是否真的改变了长程协作习惯

### `memory_hierarchy_and_handoff`

- 当前已写 session、share、export、handoff 分层
- 需要验证 adoption 层是否足够强，还是仍主要停在 capability facts

### `bounded_subtask_orchestration`

- 当前基线已明确 `plan/build/general/explore` 和 `subtask: true`
- 需要验证这是否足以形成强的多智能体 guardrail 候选

## Step 2A 结论

- `practice_opencode/` 同样已经是成熟基线，不存在大块未覆盖的问题。
- OpenCode 下一步外部研究不该再问“有没有这些能力”，而该问：
  - 哪些能力在 `2026-01-01` 到 `2026-04-09` 之间真正改变了行为
  - 哪些是 TUI command surface 变厚，但还没升格为 practice
  - 哪些 absence 本身就该进入 guardrail 型候选

## 建议进入 Step 2B 时优先验证的方向

1. `plan_first_kickoff`
2. `command_surface_control_plane`
3. `shared_backend_long_horizon_layering`
4. `memory_hierarchy_and_handoff`
5. `bounded_subtask_orchestration`

当前仍未分配任何 `tool_candidate_id`。这一步只负责确认 baseline，不提前进入候选卡片。
