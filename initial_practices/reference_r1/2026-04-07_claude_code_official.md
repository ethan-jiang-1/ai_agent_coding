# Claude Code 官方参考手册

最后核验日期：2026-04-07  
本机版本：`Claude Code 2.1.92`

## 1. 参考定位

这份文件用于约束 `round1` 的写法，尤其是：

- 不把 Claude Code 的术语和其他工具混用
- 不把记忆、规则、session persistence 混成一层
- 不把某个 UI 按钮或快捷键的印象写成“产品本质”

证据等级：

- `A 级`：Anthropic 官方文档正文或本机 CLI 帮助直接核验
- `B 级`：官方搜索摘要或官方页面标题能确认方向，但当前抓取未逐行展开

Claude Code 当前大部分关键结论都能做到 `A 级`。

## 2. 官方来源

### A 级：Anthropic 官方文档

- `https://docs.anthropic.com/en/docs/claude-code`
- `https://code.claude.com/docs/en/memory`
- `https://code.claude.com/docs/en/permission-modes`
- `https://docs.anthropic.com/en/docs/claude-code/settings`
- `https://docs.anthropic.com/en/docs/claude-code/sub-agents`
- `https://docs.anthropic.com/en/docs/claude-code/common-workflows`

### A 级：本机 CLI

- `claude --version`
- `claude --help`
- `claude agents --help`
- `claude mcp --help`

## 3. 产品边界

写作时先分层：

- `Claude Code` 是工具
- `Sonnet / Opus / Haiku` 是模型或模型别名
- `permission mode / subagent / memory / worktree` 是 Claude Code 的工作机制

不要把 `Claude Code` 当模型名。

## 4. 会话与持久化

### 4.1 默认就是交互式会话

本机 `claude --help` 明确写着：

- 默认启动 interactive session
- `-p/--print` 才是非交互式输出

### 4.2 会话可以延续、恢复、分叉

本机帮助直接提供：

- `-c, --continue`
- `-r, --resume`
- `--fork-session`
- `--session-id`

所以 Claude Code 不是“只会从头开新对话”的工具。

### 4.3 非交互式路径也是正式能力

本机帮助可确认：

- `-p/--print`
- `--output-format text/json/stream-json`
- `--input-format text/stream-json`
- `--json-schema`
- `--no-session-persistence`

这意味着 Claude Code 既能做交互式 coding，也能做脚本化自动化。

## 5. Memory / 指令体系

### 5.1 `CLAUDE.md` 是正式机制，不是民间约定

官方 memory 文档明确写到：

- `CLAUDE.md`
- `CLAUDE.local.md`
- `.claude/rules/`
- auto memory

因此：

- `CLAUDE.md` 必须被当作正式指令入口
- 它不是“社区推荐的 README 别名”

### 5.2 auto memory 有明确路径和加载上限

官方 memory 文档可直接确认：

- auto memory 路径：`~/.claude/projects/<project>/memory/`
- 入口文件：`MEMORY.md`
- 每次 session 启动只加载 `MEMORY.md` 的前 200 行或前 25KB

这很重要，因为它说明：

- auto memory 不是无限上下文
- 它是受边界约束的持久记忆层

### 5.3 worktree / 子目录共享同一项目 auto memory

官方 memory 文档明确指出：

- 同一 git 仓库内的 worktree 和子目录共享同一 auto memory 目录

所以如果正文谈“跨 worktree 的经验延续”，这是可以写成事实的。

### 5.4 `/memory` 和 `/compact` 的真实语义

官方 memory 文档明确：

- `/memory` 可以查看当前 session 加载了哪些 `CLAUDE.md` / local / rules / memory
- `/memory` 还能开关 auto memory、打开 memory 目录
- `/compact` 后 `CLAUDE.md` 会从磁盘重新读取并重新注入

因此不能写：

- “/compact 只是简单压缩历史，规则也许会丢”

## 6. `.claude/rules/` 与规则分层

官方 settings / memory 文档可确认：

- `.claude/rules/` 是正式规则目录
- 规则支持 YAML frontmatter
- 可以用 `paths` 做路径作用域

这意味着 Claude Code 的规则体系不是只有单一 `CLAUDE.md`：

- 全局或项目总则可以写在 `CLAUDE.md`
- 目录或文件模式相关的精细约束可以写到 `.claude/rules/`

对 `round1` 的直接影响：

- 如果要讲“规则层级”，Claude Code 必须与 Codex 的 `AGENTS.md`、Cursor 的 `.cursor/rules/*.mdc` 区分开写

## 7. Permission Modes

### 7.1 `plan` 是正式 mode

本机 `claude --help` 直接给出：

- `--permission-mode <mode>`
- 可选值包括 `plan`

所以“Claude Code 有 Plan 模式”这个判断本身是对的，但要写对它的层级：

- 它是 permission mode
- 不是随便猜的 marketing 说法

### 7.2 当前可见的 mode 枚举

本机帮助当前给出：

- `acceptEdits`
- `auto`
- `bypassPermissions`
- `default`
- `dontAsk`
- `plan`

写 `round1` 时：

- 可以说 `plan` 是正式 mode
- 不要随便编更多未核验的 mode 含义

### 7.3 `plan` 对写作的意义

安全写法：

- “规划阶段可用 `--permission-mode plan` 把 Claude Code 限定在先分析、先拟方案的工作流中”

不安全写法：

- “Claude Code 的 Plan Mode 就是一个固定 UI 标签，所有版本都长一样”

## 8. Worktree

### 8.1 工作树是正式工作流，不是民间技巧

本机帮助明确有：

- `-w, --worktree [name]`

官方 common workflows 也明确推荐：

- 用 Git worktree 跑并行 Claude Code 会话

### 8.2 写作时可以安全表达的事实

- Claude Code 官方支持 worktree 工作流
- worktree 适合并行任务和隔离实验

但不要写死：

- 某个具体 UI 文案
- 某个固定目录名是否永远不变

## 9. Subagents

### 9.1 子智能体是正式能力

官方 sub-agents 文档和本机 CLI 共同确认：

- 子智能体存在
- `claude agents` 是正式子命令
- `--agents <json>` 是正式 CLI 入口

### 9.2 官方可直接确认的关键点

官方 sub-agents 文档明确：

- 子智能体有独立 context window
- 可配置工具权限
- 项目级位置：`.claude/agents/`
- 用户级位置：`~/.claude/agents/`

这些点足以支撑 `round1` 里关于“多 agent 不是共享一个窗口硬切 prompt”的论述。

### 9.3 对 `round1` 的直接约束

可以写：

- “Claude Code 的 subagents 具备独立上下文与可配置工具权限”

不要写：

- “Claude Code 的子 agent 只是同一个大上下文里分段回复”

## 10. MCP

本机 `claude mcp --help` 明确支持：

- `add`
- `add-from-claude-desktop`
- `add-json`
- `get`
- `list`
- `remove`
- `reset-project-choices`
- `serve`

这说明：

- MCP 不是旁门插件
- 它是 Claude Code 的正式集成机制

如果 `round1` 要谈外部工具扩展，Claude Code 这里应当落在 MCP，而不是杜撰私有插件配置键。

## 11. 可以直接写成事实的内容

- `CLAUDE.md`、`CLAUDE.local.md`、`.claude/rules/`、auto memory 都是正式机制
- `plan` 是正式 permission mode
- `/memory`、`/compact` 是正式命令
- Git worktree 是官方支持的并行工作流
- Claude Code subagents 有独立 context window 和可配置工具权限
- MCP 有正式 CLI 管理命令

## 12. 必须避免的错误写法

- “Claude Code 会自动记住一切”
- “/compact 只是压缩聊天记录”
- “Plan Mode 只是网友起的名字”
- “subagent 只是同一个 session 里切几次角色扮演”

## 13. 给 `round1` 的推荐话术

### 规划

“Claude Code 可通过 `--permission-mode plan` 先做只读规划，再切到可编辑模式执行。”

### 记忆

“Claude Code 的长期记忆不是单一机制，而是 `CLAUDE.md`、`.claude/rules/` 和 auto memory 的组合。”

### 多 agent

“Claude Code 的 subagents 有独立上下文和独立工具权限，因此适合做并行探索和角色拆分。”

### 持久化

“Claude Code 既支持交互式持续会话，也支持 `--print` 非交互式运行；两者不要混为一谈。”
