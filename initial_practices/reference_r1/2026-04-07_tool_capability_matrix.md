# AI Coding 工具能力矩阵

最后核验日期：2026-04-07

## 1. 使用说明

这个矩阵不是“谁更强”的排名，而是防止 `round1` 把不同工具的机制写串。

证据等级：

- `A`：官方正文和/或本机 CLI 可直接核验
- `B`：官方搜索摘要或官方仓库可确认

## 2. 核心矩阵

| 维度 | Codex CLI | Claude Code | Cursor | Aider |
|---|---|---|---|---|
| 工具层身份 | 本地/终端 coding agent 工具 | 本地/终端 coding agent 工具 | 编辑器与 agent 产品 | 终端 pair programming 工具 |
| 主要证据等级 | A/B | A | A/B | A |
| 交互式会话 | 有 | 有 | 有 | 有 |
| 非交互式入口 | `codex exec` | `claude --print` | 有官方模式/页面，但本轮未本机核验 CLI | `--message` / `--message-file` |
| 可恢复会话 | `resume` | `--resume` / `--continue` | 聊天 tab / 历史存在，但本轮不写死 CLI 级 session 语义 | 可恢复历史，但不是同类 session UX |
| 可分叉会话 | `fork` | `--fork-session` | 未在本轮写成确定事实 | 未作为官方核心叙事 |
| 正式规则入口 | `AGENTS.md` | `CLAUDE.md` + `.claude/rules/` | `.cursor/rules/*.mdc` + `AGENTS.md` | 无同等级规则树，主要靠命令与文件范围 |
| legacy 规则入口 | 未确认 | 不适用 | `.cursorrules` | 不适用 |
| 层级规则作用域 | 目录层级 AGENTS 覆盖 | `CLAUDE.md` + scoped rules | `.cursor/rules` 支持 globs / nested rules | 不适用 |
| 只读/规划机制 | `exec -s read-only` 或 session 约束 | `--permission-mode plan` | `Ask` + `Custom Modes` | `--read` + `/ask` |
| 编辑/执行机制 | 交互式或 `exec` | mode 切换后执行 | Agent / Manual / Custom | 默认对已加入文件进行编辑 |
| 回退语义 | 依赖 git/worktree/外部流程，本轮不虚构内建 undo | 不是 `/compact`，需按会话/文件/版本控制处理 | checkpoints 只覆盖 agent 更改 | `/undo` 回退 aider 的上一次 git commit |
| 多 agent / 子 agent | 有，`multi_agent stable true` | 有 subagents | 有 Background Agents | 无官方“子 agent”主叙事 |
| 子 agent 独立上下文 | 有官方支持 | 官方明确有 | Background agent 为独立远程环境 | 不适用 |
| 权限/沙箱 | sandbox + approval + web search | permission mode + tool allow/deny | 前台审批与后台自动运行分离 | 主要是 git / shell / 文件范围，不是同一套沙箱体系 |
| 网络默认叙事 | `web_search = "cached"`；sandbox 默认拦出站网络 | 依配置/模式而定 | Background agents 有 internet access | 可用网页抓取能力，非统一沙箱叙事 |
| Git worktree | 可配合使用 | 官方明确支持 | 可并行，但本轮不写死 worktree 细节 | 可配合 git 使用 |
| MCP | 有 | 有 | 有 | 非核心主叙事 |
| 大仓索引机制 | 官方有 rules/skills/agents 等分层，不写成 AST | 长期记忆 + rules + subagents | codebase / rules / checkpoints / remote agents | repo map |

## 3. 最容易写串的点

### 3.1 规则文件

- Codex CLI：`AGENTS.md`
- Claude Code：`CLAUDE.md`、`CLAUDE.local.md`、`.claude/rules/`
- Cursor：`.cursor/rules/*.mdc`、`AGENTS.md`、`.cursorrules`（legacy）
- Aider：没有与前三者同构的官方规则树

### 3.2 “规划模式”

- Codex CLI：更接近 `read-only sandbox + exec/session discipline`
- Claude Code：`plan` 是正式 permission mode
- Cursor：官方更稳的说法是 `Ask + Custom Modes`
- Aider：更稳的说法是 `--read` + `/ask` + `architect`

### 3.3 “撤销”

- Claude Code：`/compact` 不是撤销
- Cursor：checkpoint 不是 git
- Aider：`/undo` 是 git commit 级回退
- Codex CLI：不要随便虚构统一内建 undo 语义

### 3.4 “多 agent”

- Codex CLI：官方支持 subagents / multi-agent，但 UX 细节版本相关
- Claude Code：subagents 有独立 context 和工具权限
- Cursor：Background Agents 是远程异步环境
- Aider：不要硬说它有同级子 agent 系统

## 4. 对 `round1` 的统一要求

- 不要把工具名和模型名混成一列。`Codex CLI`、`Claude Code`、`Cursor`、`Aider` 都是工具层。
- 不要把别家术语借过来。`plan`、`Ask`、`architect`、`Background Agent` 不是同一个东西。
- 不要用一个工具的撤销语义去描述另一个工具。
- 不要把 Cursor 或 Codex 某个版本的具体 UI 暴露形态写成跨版本永恒事实。
