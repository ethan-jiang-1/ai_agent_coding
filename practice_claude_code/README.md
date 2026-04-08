# Claude Code Practice Set

这个目录把 `overall/round2_detailed` 里的 11 个习惯，整理成了适配 Claude Code 的一套实践材料。

- `practices/`：按习惯组织的主阅读内容
- `commands/`：支撑这些习惯的 Claude Code 命令、参数、规则载体和工作流

这套内容保留了 round2 的骨架，但收紧到当前 Claude Code 的真实能力边界，重点包括：

- 默认交互式会话，以及 `--continue` / `--resume` / `--fork-session` 的分工
- `--permission-mode plan`、`--allowedTools`、`--tools` 这类工具级控制
- `CLAUDE.md` / `.claude/CLAUDE.md`、`.claude/rules/*.md`、`CLAUDE.local.md` 的规则层
- auto-memory、subagents、worktree、`--print` / `--max-budget-usd`

建议阅读顺序：

1. 先读 `practices/01_session_lifecycle.md`
2. 再读 `practices/03_plan_before_code.md`
3. 接着看 `practices/08_tool_permission.md`
4. 需要落地命令时，再去对应的 `commands/` 文件
