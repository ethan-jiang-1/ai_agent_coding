# Claude Code Practice Set

这个目录把 `overall/round2_detailed` 里的 11 个习惯，整理成了适配 Claude Code 的一套实践材料，并补入了 Claude Code 的 built-in commands 这一层 slash command 命令面。

- `reference/`：这轮 external command surface + slash command surface 的官方核验和映射材料
- `practices/`：按习惯组织的主阅读内容
- `commands/`：支撑这些习惯的 Claude Code 命令、参数、规则载体和工作流；现已补入 slash command 映射

这套内容保留了 round2 的骨架，但收紧到当前 Claude Code 的真实能力边界，重点包括：

- 默认交互式会话，以及 `--continue` / `--resume` / `--fork-session` 的分工
- built-in commands 这一层 slash commands，以及它们和外部命令面的对应关系
- `--permission-mode plan`、`--allowedTools`、`--tools` 这类工具级控制
- `CLAUDE.md` / `.claude/CLAUDE.md`、`.claude/rules/*.md`、`CLAUDE.local.md` 的规则层
- auto-memory、subagents、worktree、`--print` / `--max-budget-usd`
- 追加第 `13` 章，把 `agent teams / scheduled automation / long horizon` 放到同一层处理

建议阅读顺序：

1. 先读 `practices/01_session_lifecycle.md`
2. 再读 `practices/03_plan_before_code.md`
3. 接着看 `practices/08_tool_permission.md`
4. 想看 slash command 全景，先读 `reference/2026-04-09_claude_code_command_surface_mapping.md`
5. 需要落地命令时，再去对应的 `commands/` 文件
6. 如果任务进入并行编排、定时自动化或长程运行，再读 `practices/13_advanced_agentic_coordination.md`
7. 需要看会话控制层，再看 `practices/12_interactive_only_session_ops.md`

如果要先看这轮 grounding 的事实底座，先读：

- `reference/2026-04-09_claude_code_command_surface_mapping.md`
- `reference/2026-04-09_claude_code_advanced_agentic_features_reference.md`
