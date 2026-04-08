# Codex CLI Practice Set

这个目录把 `overall/round2_detailed` 里的 11 个习惯，整理成了适配 Codex CLI 的一套实践材料。

- `practices/`：按习惯组织的主阅读内容
- `commands/`：支撑这些习惯的 Codex CLI 命令、参数、文件布局和配置片段

这套内容保留了 round2 的框架，但对 Codex CLI 的当前行为做了收紧和校正，重点包括：

- 区分交互式 `codex`、非交互式 `codex exec`、以及 `codex exec resume`
- 使用 `AGENTS.md` 作为规则载体，并按当前官方文档整理发现顺序
- 把权限、沙箱、自动化和信息外化写成 Codex CLI 可执行的工作流
- 对多智能体部分，纳入当前 Codex 的 subagents 能力，同时保留 `exec` 串联的保守做法

建议阅读顺序：

1. 先读 `practices/01_session_lifecycle.md`
2. 再读 `practices/03_plan_before_code.md`
3. 接着看 `practices/08_tool_permission.md`
4. 需要落地命令时，再去对应的 `commands/` 文件
