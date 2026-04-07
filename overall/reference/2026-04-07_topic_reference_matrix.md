# 主题参考矩阵

最后核验日期：2026-04-07

## 1. 用法

这份矩阵按“写作主题”而不是按“工具”组织，方便逐个修 `round1` 文件时查。

## 2. 会话生命周期

| 工具 | 正确 grounding |
|---|---|
| Codex CLI | 有交互式 session，也有 `exec`、`resume`、`fork` |
| Claude Code | 默认交互式；支持 `--continue`、`--resume`、`--fork-session` |
| Cursor | 有聊天 tab / mode / history，不建议本轮写死 CLI 级 session 语义 |
| Aider | 默认交互式；可 `--message`；可恢复 chat history |

## 3. 先规划再执行

| 工具 | 正确 grounding |
|---|---|
| Codex CLI | `read-only sandbox` 或只读 `exec` 先规划 |
| Claude Code | `--permission-mode plan` |
| Cursor | `Ask` 做只读探索；更复杂流程用 `Custom Modes` |
| Aider | `--read` + `/ask`；复杂设计再进 `architect` |

## 4. 规则层级

| 工具 | 正确 grounding |
|---|---|
| Codex CLI | `AGENTS.md`，目录层级覆盖 |
| Claude Code | `CLAUDE.md` + `.claude/rules/` + auto memory |
| Cursor | `.cursor/rules/*.mdc` + `AGENTS.md`，`.cursorrules` 是 legacy |
| Aider | 无对等的多层规则树，不要硬套 |

## 5. 编辑不是追加

| 工具 | 正确 grounding |
|---|---|
| Codex CLI | 重跑 `exec --ephemeral`，或 `fork` / `resume` 新分支处理 |
| Claude Code | 依赖 mode / session / 版本控制，不应写成“自动清空旧改动” |
| Cursor | checkpoints 只覆盖 agent 变更，不覆盖手工编辑 |
| Aider | git commit 边界最重要，`/undo` 回退 aider 的上一次提交 |

## 6. 多 agent

| 工具 | 正确 grounding |
|---|---|
| Codex CLI | 官方支持 subagents / multi-agent，但 UX 细节版本相关 |
| Claude Code | subagents 有独立 context 和工具权限 |
| Cursor | Background Agents 是远程异步环境 |
| Aider | 不要写成同类子 agent 系统 |

## 7. 权限与沙箱

| 工具 | 正确 grounding |
|---|---|
| Codex CLI | sandbox / approval / web search 都是正式配置项 |
| Claude Code | permission mode、tool allow/deny、dangerous skip permissions |
| Cursor | 前台审批与后台自动运行差异明显 |
| Aider | 主要是 git、shell、文件范围；不是同一套沙箱系统 |

## 8. 需要删掉的典型误写

- “Codex 的规则文件是 `codex.md`”
- “Cursor 内建标准 Plan Mode”
- “Aider 构建 AST 图谱”
- “Claude Code `/compact` 只是压缩历史”
- “Codex 没有持续会话”
- “Background Agents 只是本地额外聊天窗”
