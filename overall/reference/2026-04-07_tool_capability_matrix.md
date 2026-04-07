# AI Coding 工具能力矩阵

最后核验日期：2026-04-07

| 维度 | Codex CLI | Claude Code | Cursor | Aider |
|---|---|---|---|---|
| 官方规则文件 | `AGENTS.md` + Rules 文档 | `CLAUDE.md` + `.claude/rules/` | `.cursor/rules/*.mdc` + `AGENTS.md` | 无正式分层规则体系，主要靠 `--read` / 命令 |
| legacy 规则文件 | 未确认 | 无 | `.cursorrules`（legacy） | 不适用 |
| 交互式会话 | 有 | 有 | 有 | 有 |
| 恢复/分叉会话 | 有：`resume` / `fork` | 有：`--resume` / `--continue` / `--fork-session` | 文档未在本轮重点核验 | 可恢复 chat history，但不是同类 session UX |
| 非交互式执行 | 有：`codex exec` | 有：`claude -p` | 有文档，但本轮未深挖 | 有：`--message` / `--message-file` |
| 只读探索模式 | 需用 prompt / 配置约束 | 有：`plan` permission mode | 有：`Ask` | 有：`/ask` + `--read` |
| 子智能体/多智能体 | 有：Subagents / `multi_agent` | 有：subagents | 有：background agents | 无官方“子智能体”叙事 |
| 子智能体独立上下文 | 官方能力存在 | 官方明确有 | Background agent 为远程独立环境 | 不适用 |
| 工作树/并行 worktree | 可配合 git 自行使用 | 官方明确推荐 | 可并行会话，但本轮未深挖 worktree 文档 | 可自行配合 git 使用 |
| MCP | 有 | 有 | 有 | 非核心叙事 |
| sandbox / approvals | 一等配置项 | 一等配置项 | 前后台 agent 权限分化明显 | 主要是 git + shell 控制，不是同一体系 |
| 默认联网状态 | `web_search = "cached"` | 取决于配置/权限 | 视模式与设置而定 | 有 `/web` 和网页抓取能力 |
| 多文件自动编辑 | 有 | 有 | 有 | 有 |

## 对 round1 的统一术语建议

- `Codex CLI`、`Claude Code`、`Cursor`、`Aider` 都是工具层，不要和 `GPT-5`、`Sonnet`、`Haiku` 混成一列。
- `Ask`、`plan`、`architect`、`background agent` 分别是不同产品的不同工作模式，不要互相借名。
- “规则文件”必须按工具区分：
  - Codex：`AGENTS.md`
  - Claude Code：`CLAUDE.md` / `.claude/rules/`
  - Cursor：`.cursor/rules/*.mdc` / `AGENTS.md`
  - Aider：`--read` / 命令式约束

