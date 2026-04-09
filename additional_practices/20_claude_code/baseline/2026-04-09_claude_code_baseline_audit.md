# 2026-04-09 Claude Code Baseline Audit

Run ID: `2026-04-09_additional_practices_r1`
Phase: `Phase 3. Tool Loop: Claude Code`
Step: `Step 2A. Tool Baseline`

## 目标

先确认 `practice_claude_code/` 已经讲了什么、还没讲什么，以及哪些内容已经落在 `practices/`、`commands/`、`reference/`，避免把已有表达误判为新增缺口。

## 本次复核范围

- `practice_claude_code/README.md`
- `practice_claude_code/practices/`
- `practice_claude_code/commands/`
- `practice_claude_code/reference/`

## 当前工具的既有覆盖骨架

| Layer | 数量 | 结论 |
| --- | ---: | --- |
| `README.md` | 1 | 已明确阅读顺序、slash command 层和 advanced agentic 能力入口 |
| `practices/` | 13 | 已覆盖原始 `11` 个习惯，并补入 `12` 交互式会话操作层、`13` 高级 agentic 协调 |
| `commands/` | 13 | 已把外部命令、built-in commands、permission / rules / memory / worktree 等落文 |
| `reference/` | 2 | 已对 command surface mapping 和 advanced agentic features 做 grounding |

## 已在 practice 层明确表达的内容

| 主题 | 既有锚点 | Step 2A 判断 |
| --- | --- | --- |
| 会话续跑 / 分叉 | `ba.claude_code.practice.session_lifecycle` | 已明确 `--continue`、`--resume`、`--fork-session` 的分工 |
| 读写分离与 plan mode | `ba.claude_code.practice.plan_before_code` | 已明确 `--permission-mode plan` 和计划落盘 |
| 权限 / 工具 / MCP 边界 | `ba.claude_code.practice.tool_permission` | 已把 permission mode、工具白名单和 MCP 来源写成 workflow |
| 会话 / memory / handoff 分层 | `ba.claude_code.practice.memento_workflow` | 已明确 auto-memory、resume、handoff、`/loop` 的不同层次 |
| subagents / worktree / teams / schedule | `ba.claude_code.practice.advanced_agentic_coordination` | 已明确四层协调能力和轻重顺序 |
| 交互式会话旁路动作 | `ba.claude_code.practice.interactive_only_session_ops` | 已把 slash command 作为控制动作层，而不是提示词装饰 |

## 已在 commands / reference 层明确表达的内容

| 主题 | 既有锚点 | Step 2A 判断 |
| --- | --- | --- |
| built-in commands 清单与 mapping | `ba.claude_code.reference.command_surface_mapping` | 已对 `/agents`、`/permissions`、`/plan`、`/tasks`、`/ultraplan` 等做映射分类 |
| 规则层 / auto-memory / `--bare` | `ba.claude_code.command.rules_and_context_commands` | 已明确 `CLAUDE.md`、`.claude/rules`、`CLAUDE.local.md`、`--bare` |
| permission modes / tool control / MCP | `ba.claude_code.command.permission_and_tools_commands` | 已明确 plan / auto / default / tools / strict MCP / hooks |
| memory / resume / export / handoff | `ba.claude_code.command.memory_resume_handoff_commands` | 已明确 auto-memory 路径、读入边界和 handoff 文件必要性 |
| subagents / `--worktree` / teams | `ba.claude_code.command.subagents_and_worktree_commands` | 已明确 built-in subagents、frontmatter、`--worktree` 和隔离写入 |
| `/loop` / `/schedule` / durable automation | `ba.claude_code.command.advanced_agentic_coordination_commands` | 已明确 session-scoped scheduling 与 durable scheduling 的区别 |

## 当前工具相对全局基线的特殊点

### 1. 交互命令层已经非常厚

Claude Code 的 built-in command 面已经明显不只是补充层，而是：

- 会话控制面
- permission / MCP / hooks 调整面
- agent / task / schedule 观察面
- 更重型交互式 planning 面

### 2. `--worktree` 是第一方 CLI 能力

和 Codex CLI 不同，Claude Code 当前的 worktree 不是外部 Git 技巧，而是正式 CLI surface。

### 3. long horizon 明显分成三层

当前基线已经清楚分出：

- `--continue / --resume / --fork-session`
- `/loop`
- Cloud / Desktop / GitHub Actions durable scheduling

### 4. memory 层比 Codex CLI 更显式

Claude Code 当前基线已经把：

- `CLAUDE.md`
- `.claude/rules/*.md`
- auto-memory
- `MEMORY.md`
- handoff 文件

做成了更完整的多层结构。

## 当前工具的薄弱点与待验证区

### `session_kickoff_discipline`

- 当前已写 plan mode、会话切换和权限模式
- 但还没有一个特别明确的“Claude Code 开局契约”统一叙述

### `command_surface_fluency`

- 当前已经有大量 built-in command 映射
- 但需要判断这到底只是 command richness，还是已经稳定改变行为

### `workspace_worktree_isolation`

- 当前基线已经很强，因为 `--worktree` 是第一方能力
- 需要验证 2026 窗口里，它是否真的推动了更稳定的实践升级

### `long_horizon_control`

- 当前基线已经显式分层
- 需要外部证据判断 `/loop`、`/schedule`、GitHub Actions、Cloud 是否真的改变了使用习惯

### `agent_teams_threshold`

- 当前基线已明确 agent teams 是 experimental 且更贵
- 需要验证高信号实践是否把它当作真实工作流，还是更多仍停留在文档层

## Step 2A 结论

- `practice_claude_code/` 同样已经是成熟基线，不存在“大块未覆盖”的问题。
- Claude Code 下一步外部研究不该再问“有没有这些能力”，而该问：
  - 哪些能力在 `2026-01-01` 到 `2026-04-09` 之间真正改变了行为
  - 哪些是 `12` / `13` 章的强化
  - 哪些只是 command surface 变厚，并不足以升格为 practice

## 建议进入 Step 2B 时优先验证的方向

1. `command_surface_fluency`
2. `workspace_worktree_isolation`
3. `long_horizon_control`
4. `session_kickoff_discipline`
5. `agent_teams_threshold`
6. `context_packaging`

当前仍未分配任何 `tool_candidate_id`。这一步只负责确认 baseline，不提前进入候选卡片。
