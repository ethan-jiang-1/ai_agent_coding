# Claude Code 官方核验摘要

最后核验日期：2026-04-07
工具版本：`Claude Code 2.1.92`
核验方式：Anthropic 官方文档 + 本机 `claude --help`

## 官方来源

- Claude Code 文档总入口：`https://docs.anthropic.com/en/docs/claude-code`
- Memory：`https://docs.anthropic.com/en/docs/claude-code/memory`
- Settings：`https://docs.anthropic.com/en/docs/claude-code/settings`
- Subagents：`https://docs.anthropic.com/en/docs/claude-code/sub-agents`
- Common workflows：`https://docs.anthropic.com/en/docs/claude-code/common-workflows`
- 本机帮助：
  - `claude --help`
  - `claude agents --help`
  - `claude mcp --help`

## 已确认事实

### 1. `CLAUDE.md`、`.claude/rules/`、auto memory 都是正式能力

- 官方 memory 文档明确有：
  - `CLAUDE.md`
  - `.claude/rules/`
  - auto memory
- `CLAUDE.md` 是 Claude Code 的正式指令文件，不是社区约定。

### 2. auto memory 有明确存储路径和加载边界

- 官方文档给出项目级 memory 路径：
  - `~/.claude/projects/<project>/memory/`
- 目录入口文件为：
  - `MEMORY.md`
- 官方文档写明：
  - 每次会话启动时会加载 `MEMORY.md` 的前 200 行或前 25KB
- 同一 git 仓库内的 worktree 和子目录共享同一 auto memory 目录。

### 3. `/memory` 是正式命令

- 官方 memory 文档写明 `/memory` 可：
  - 列出当前 session 已加载的 `CLAUDE.md` / `CLAUDE.local.md` / rules
  - 开关 auto memory
  - 打开 memory 目录

### 4. `/compact` 的官方语义比“纯压缩历史”更具体

- 官方 memory 文档写明：
  - `CLAUDE.md` 在 `/compact` 后会从磁盘重新读取并重新注入
- 所以“compact 后规则会丢失”的说法不成立。

### 5. Plan 是正式 permission mode

- 本机 `claude --help` 显示：
  - `--permission-mode <mode>`
  - 可选值包含 `plan`
- 所以“Plan”在 Claude Code 里不是完全虚构的术语。
- 但如果正文涉及快捷键、UI 切换入口，仍应以官方文档当前界面描述为准，不要只靠印象写。

### 6. Worktree 是正式工作流

- 本机帮助有：
  - `-w, --worktree [name]`
- 官方 common workflows 也明确推荐 Git worktree 做并行会话。

### 7. Subagents 是正式能力

- 官方 subagents 文档明确说明：
  - 子智能体有独立 context window
  - 可配置工具权限
  - 项目级位置：`.claude/agents/`
  - 用户级位置：`~/.claude/agents/`
- 本机帮助也有：
  - `agents` 子命令
  - `--agents <json>`

### 8. MCP 是正式能力，不是旁门插件

- 本机帮助有 `mcp` 子命令。
- `claude mcp --help` 支持：
  - `add`
  - `list`
  - `get`
  - `remove`
  - `serve`

## 对 round1 的直接修正建议

### 可以安全写成事实的

- `CLAUDE.md`、`.claude/rules/`、`.claude/agents/`、auto memory 都是官方支持的机制。
- `plan` 是正式 permission mode。
- `/memory` 和 `/compact` 是官方内置命令。
- Git worktree 是官方推荐的并行工作流之一。

### 需要谨慎写的

- 某个具体快捷键是否切换 Plan Mode
- 某个 UI 按钮文案
- auto memory 会“记录哪些具体类型的信息”的边界

### 不应继续写成空泛比喻的

- “Claude Code auto-memory 会自动记住一切”
- “/compact 只是把历史压缩一下”

## 建议在正文里采用的表述

- 若讲状态传递：
  - “Claude Code 同时提供 `CLAUDE.md`、`.claude/rules/` 和 auto memory 三层长期记忆机制”
- 若讲多智能体：
  - “Claude Code 的 subagents 有独立上下文和可配置工具权限”
- 若讲并行：
  - “官方明确支持用 Git worktree 跑并行 Claude Code 会话”

