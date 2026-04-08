# OpenCode Practice Set

这个目录把 `overall/round2_detailed` 里的 11 个习惯，整理成了适配 OpenCode 的一套实践材料。

- `reference/`：这轮迁移依赖的官方事实边界
- `practices/`：按 11 个习惯组织的主阅读内容
- `commands/`：支撑这些习惯的 OpenCode 命令、TUI slash commands、agent、permission、rules 与 handoff 做法

这套内容保留了 round2 的骨架，但收紧到 OpenCode 当前官方 docs 能确认的真实能力边界，重点包括：

- 默认 TUI，会话继续与分叉
- `opencode run`、`opencode session list`、`opencode export`、`opencode import`、`opencode stats`
- `build` / `plan` primary agents，以及 `general` / `explore` subagents
- `AGENTS.md`、`instructions`、`CLAUDE.md` fallback
- `permission`、`share`、`snapshot`、`custom commands`
- `/compact`、`/undo`、`/redo`、`/sessions`、`/share`、`/models` 这些 TUI 入口

需要明确的是：

- 当前环境里没有安装 `opencode`
- 所以这套材料是 `官方 docs grounded`
- 不是 `本机 opencode --help grounded`

建议阅读顺序：

1. 先读 `practices/01_session_lifecycle.md`
2. 再读 `practices/03_plan_before_code.md`
3. 接着看 `practices/04_rule_hierarchy.md`
4. 然后看 `practices/08_tool_permission.md`
5. 需要跨会话协作时，再看 `practices/09_memento_workflow.md`
6. 需要具体命令时，再去对应的 `commands/` 文件

如果要先看这轮 grounding 的事实底座，先读：

- `reference/2026-04-09_opencode_official_reference.md`
