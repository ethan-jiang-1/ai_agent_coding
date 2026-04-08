# OpenCode Official Reference

核验日期：2026-04-09

## 目的

这份 reference 用来支撑 `practice_opencode/` 这一轮首版迁移：

- 先把 OpenCode 当前官方能确认的事实边界写清楚
- 再据此把 `overall/round2_detailed` 的 11 个习惯迁移到 OpenCode 语境
- 避免把 Codex CLI、Claude Code 或 Cursor 的能力硬套到 OpenCode 上

## 证据边界

这次主要依赖 OpenCode 官方文档：

- `https://opencode.ai/docs/tui/`
- `https://opencode.ai/docs/cli/`
- `https://opencode.ai/docs/config/`
- `https://opencode.ai/docs/agents/`
- `https://opencode.ai/docs/permissions/`
- `https://opencode.ai/docs/rules/`
- `https://opencode.ai/docs/models/`
- `https://opencode.ai/docs/share/`
- `https://opencode.ai/docs/commands/`

本机核验结果：

- 当前环境里 `opencode` 未安装
- `command -v opencode` 返回 `command not found`

所以这次整理是：

- `官方 docs 已确认` 的事实
- 不是 `本机 CLI --help 已逐项核验`

凡是涉及具体交互行为、UI 细节、默认 keybind 以外的体验，这次都按官方文档边界保守落文。

## 官方术语

当前官方文档里，最稳的术语有这些：

- `TUI`：默认启动的交互式终端界面
- `CLI`：`opencode run`、`opencode session list`、`opencode stats` 这一层命令面
- `slash commands`：TUI 里以 `/...` 触发的 built-in commands
- `agents`：primary agents 和 subagents
- `permission`：审批与阻断规则
- `AGENTS.md`：项目或全局规则载体
- `instructions`：额外指令文件数组

## 当前能确认的核心事实

### 1. 入口与会话

- 直接运行 `opencode` 默认进入 TUI
- `opencode run [message..]` 是非交互式入口
- TUI 启动参数和 `run` 都支持：
  - `--continue` / `-c`
  - `--session` / `-s`
  - `--fork`
  - `--model` / `-m`
  - `--agent`
- TUI 内置 `/new`，别名是 `/clear`
- TUI 内置 `/sessions`，别名是 `/resume`、`/continue`
- CLI 还有 `opencode session list` 用来列出会话

这意味着 OpenCode 不是“天然每次都独立新会话”的工具。它有明确的继续、切换和分叉机制。

### 2. 文件引用与命令输出注入

- TUI 支持 `@file` 文件引用；官方写法是用 `@` 做模糊文件搜索并自动把文件内容加入上下文
- TUI 支持以 `!` 开头执行 shell command，并把结果作为工具输出加入对话
- CLI `opencode run` 支持 `--file` / `-f` 给消息附加文件
- OpenCode 的 custom commands 也支持：
  - `$ARGUMENTS`
  - `!` 命令输出注入
  - `@file` 文件引用

这说明 OpenCode 在“用指针代替粘贴”这件事上，有比较完整的官方支持面。

### 3. agents 能力边界

官方 docs 当前明确写到：

- OpenCode 有两类 agents：`primary agents` 和 `subagents`
- 内建 primary agents：
  - `build`
  - `plan`
- 内建 subagents：
  - `general`
  - `explore`
- 内建隐藏 system agents：
  - `compaction`
  - `title`
  - `summary`

已确认的职责边界：

- `build`：默认 primary agent，工具全开，适合正常开发执行
- `plan`：受限 primary agent，默认对 `file edits` 和 `bash` 都走 `ask`
- `general`：通用 subagent，可做复杂研究和多步任务，可修改文件
- `explore`：快速、只读的 subagent，不修改文件

调用方式当前官方能确认：

- primary agents 可在会话中切换
- 也可以通过 `--agent` 指定
- 文档写明可以用 `@` mention 调用 agent
- subagents 既可自动被 primary agent 调用，也可手动 `@general`、`@explore`

配置位置：

- JSON：`opencode.json`
- Markdown：
  - `~/.config/opencode/agents/`
  - `.opencode/agents/`

### 4. 规则与说明文件

官方当前明确支持：

- 项目规则：`AGENTS.md`
- 全局规则：`~/.config/opencode/AGENTS.md`
- `/init`：引导式创建或改进 `AGENTS.md`

与 Claude Code 的兼容回退也有官方说明：

- 如果项目里没有 `AGENTS.md`，会回退到 `CLAUDE.md`
- 如果没有 `~/.config/opencode/AGENTS.md`，会回退到 `~/.claude/CLAUDE.md`
- 还支持 `~/.claude/skills/`

规则优先级当前官方文档能确认：

1. 从当前目录向上遍历本地规则文件（`AGENTS.md`、`CLAUDE.md`）
2. `~/.config/opencode/AGENTS.md`
3. `~/.claude/CLAUDE.md`

并且：

- 同一类别内 first matching file wins
- 如果同时有 `AGENTS.md` 和 `CLAUDE.md`，只用 `AGENTS.md`

额外指令支持：

- `instructions` 可在 `opencode.json` 中配置
- 支持路径、glob、远程 URL
- 这些 instruction files 会和 `AGENTS.md` 一起组合进上下文

### 5. 模型与变体

当前官方能确认：

- `/models` 用于在 TUI 里列可用模型
- CLI 有 `opencode models`
- CLI/TUI 都支持 `--model` / `-m`
- 模型 ID 格式是 `provider/model`
- 配置层支持：
  - `model`
  - `small_model`
- `small_model` 用于 title generation 这类轻量任务；如果 provider 有更便宜模型会优先用，否则回退到主模型
- agent config 可单独覆盖 model
- OpenCode 支持 variants，且有 built-in variants
- 官方 docs 还写了 `variant_cycle` keybind 用来切换 variants

需要保守理解的点：

- 官方推荐模型清单明确说“not exhaustive”且“not necessarily up to date”
- 所以实践文档里不把某个推荐模型列表写成长期稳定真理

### 6. permission 与 tools

当前官方能确认：

- `permission` 控制动作是 `allow`、`ask` 还是 `deny`
- 默认情况下，OpenCode 允许所有操作而不要求显式审批
- `permission` 可以：
  - 整体设成一个值
  - 按工具名配置
  - 对 `bash`、`edit` 等做更细粒度的 pattern 匹配
- pattern matching 支持例如：
  - `git *`
  - `npm *`
  - `rm *`
- 规则按 pattern match 计算，`last matching rule wins`

已确认的版本边界：

- 官方 `Permissions` 页写明：自 `v1.1.1` 起，legacy top-level `tools` boolean config 已并入 `permission`
- 但 `Agents` 页示例里仍然出现 agent 内部的 `tools` 配置，用来收窄某个 agent 的可用工具

因此更稳的理解是：

- 会话级或项目级审批策略，以 `permission` 为主
- agent 级能力塑形，官方示例仍允许通过 agent config 里的 `tools` / 权限组合来表达

### 7. snapshot、undo、redo

官方当前能确认：

- snapshots 默认开启
- snapshots 用来跟踪 agent 操作期间的文件变化
- 这让你可以在 session 内做 undo / revert
- `snapshot: false` 会关闭这套能力
- 关闭后，agent 产生的修改不能再通过 UI 回滚

TUI 页还明确写到：

- `/undo` 会撤销最近一次用户消息、后续响应以及相关文件改动
- `/redo` 只在 `/undo` 之后可用
- `/undo` / `/redo` 的文件恢复内部依赖 Git
- 项目需要是 Git repository

这组信息说明 OpenCode 的“回退并重试”确实有正式工具支持，但不应把它理解成无限层级、无限可靠的历史版本系统。

### 8. share、export、import

当前官方能确认：

- `/share`：分享当前会话
- `/unshare`：取消分享，并删除该会话相关分享数据
- `share` 配置模式：
  - `manual`：默认
  - `auto`
  - `disabled`
- 被分享的会话：
  - 会生成公开链接
  - 链接格式是 `opncd.ai/s/<share-id>`
  - 任何拿到链接的人都可以访问
  - 会一直保留到你显式 `unshare`

CLI 还支持：

- `opencode export [sessionID]`
- `opencode import <file>`
- `opencode import https://opncd.ai/s/abc123`

所以 OpenCode 的跨会话 / 跨机器状态传递至少有三层：

- session 本身
- export/import
- public share link

### 9. stats 与成本可观测性

CLI 当前官方能确认：

- `opencode stats`
- 可以看 token usage 和 cost statistics
- 支持：
  - `--days`
  - `--tools`
  - `--models`
  - `--project`

另外：

- `opencode models --verbose` 会包含 metadata like costs

当前这批官方文档里，没有看到类似 Claude Code `--max-budget-usd` 那种显式单次预算上限参数。

更稳的结论是：

- OpenCode 当前更强调 `stats`、model choice、session discipline 这类事前事后控制
- 不应凭空写出一个“官方硬预算护栏”

### 10. custom commands

OpenCode 官方有单独的 `Commands` 页面，确认支持：

- `.opencode/commands/`
- `~/.config/opencode/commands/`
- `command` config
- command frontmatter 可带：
  - `description`
  - `agent`
  - `subtask`
  - `model`

尤其重要的是：

- custom commands 可以绑定到某个 agent
- 如果绑定的是 subagent，默认会触发 subagent invocation
- `subtask: true` 可以强制以 subtask 方式运行，减少主上下文污染

这对第 03 章和第 10 章很有价值：OpenCode 不只是“有 agent”，而且能把重复工作流抽成 agent-aware 的 `/command`。

## 本次迁移时的落文策略

基于上面的事实边界，这次 `practice_opencode/` 采取这些写法：

1. `practices/` 仍以 11 个习惯为主体
2. `commands/` 负责放 OpenCode 真实的命令、TUI slash command、config 写法和 agent 工作流
3. 只把当前官方 docs 能稳定确认的内容写成肯定句
4. 凡是没有本机 CLI 可核验的点，不把 UI 细节和隐式行为写死

## 这次不写死的内容

为了避免“硬补”，下面这些点这次只做保守表达：

- 某些 keybind 的完整交互体验
- 所有 built-in slash commands 的细粒度副作用
- 各 provider 的真实成本与模型推荐是否持续有效
- custom commands 在复杂项目里的最佳实践细节
- share 在企业版 / 自托管环境中的完整策略细节

## 结论

OpenCode 当前最适合被理解成：

- 默认是 TUI 的 coding agent
- 有明确的 `run`、`session`、`export/import`、`stats` 这些 CLI 面
- 有内建 `build/plan` primary agents 和 `general/explore` subagents
- 有 `AGENTS.md` + `instructions` 这一套规则层
- 有正式的 `permission`、`share`、`snapshot`、`custom commands` 支撑工作流

这足够支撑把 `round2_detailed` 的 11 个习惯做一版稳妥的 OpenCode 适配。
