# Claude Code Command Surface Mapping Reference

核验日期：2026-04-09  
本机版本：`2.1.96 (Claude Code)`

## 目的

这份 reference 用来支撑 `practice_claude_code/` 的第二轮整理：

- 把 Claude Code 的 external command surface 和 in-session command surface 放到同一张图里
- 先确认官方术语和官方清单
- 再决定哪些应融合进现有 11 章，哪些要进入最后一个补充兜底章

## 官方术语

Claude Code 官方把这类 `/...` 入口称为 `Built-in commands`。

但真实使用时，输入 `/` 后看到的列表不只包含 built-in commands，还可能混入：

- bundled skills
- MCP prompts
- 其他环境相关入口

所以这次整理时要区分：

1. built-in commands：Claude Code 官方内建 slash commands
2. external commands：`claude ...` 命令行参数和子命令
3. 其他 slash-like entries：技能、插件或 MCP 暴露出来的额外入口

## 来源

- Anthropic 官方 built-in commands 页面：`https://code.claude.com/docs/en/commands`
- Anthropic 官方 memory 页面：`https://docs.anthropic.com/en/docs/claude-code/memory`
- 本机 `claude --help`
- 本机 `claude agents --help`

## 映射类型

- Type A：exact mapping。外部命令和 slash command 基本等价，只是入口不同。
- Type B：combined mapping。需要多个外部参数或多个会话内动作组合。
- Type C：approximate mapping。没有严格等价物，但工作流作用足够接近。
- Type D：no mapping。没有合理外部对应，或只适合留在交互会话内。

## 当前官方 built-in commands 清单

### 编码工作流高相关

| Slash command | 主要用途 | 外部命令对应 | 映射类型 | 建议落位 |
|---|---|---|---|---|
| `/add-dir [path]` | 给当前会话增加可访问目录 | `claude --add-dir <path>` | A | `commands/05` |
| `/agents` | 查看和管理 agents | `claude agents`、`--agent`、`--agents` | B | `commands/10` |
| `/branch [name]` | 从当前会话分叉新分支 | `--resume/--continue --fork-session` | C | `commands/02` |
| `/btw [question]` | 插入一个旁路问题，不污染主线 | 无稳定外部等价 | D | `commands/12` |
| `/bug` | 上报问题 | 无实践层核心对应 | D | `commands/12` |
| `/clear` | 清空当前会话历史 | 新开会话或重启交互流程 | C | `commands/01` |
| `/compact [instructions]` | 压缩历史并保留规则层 | 无稳定外部等价 | D | `commands/06` |
| `/config` | 打开配置界面 | `--settings`、settings 文件 | C | `commands/12` |
| `/context` | 查看上下文窗口占用等信息 | 无稳定外部等价 | D | `commands/06` |
| `/copy [N]` | 复制最近消息 | 无稳定外部等价 | D | `commands/12` |
| `/cost` | 看当前对话成本 | 无严格 CLI 等价 | D | `commands/11` |
| `/doctor` | 诊断安装和环境 | `claude doctor` | A | `commands/12` |
| `/effort [level]` | 调整当前会话 effort | `claude --effort <level>` | A | `commands/07` |
| `/exit` `/quit` | 退出交互会话 | 结束当前 CLI 进程 | C | `commands/01` |
| `/export [filename]` | 导出会话 | shell 重定向、handoff 文件 | C | `commands/05` |
| `/help` | 查看命令列表 | `claude --help` 不等价 | D | `commands/12` |
| `/hooks` | 管理 hooks | 无单一 CLI flag | D | `commands/08` |
| `/init [instructions]` | 初始化项目文档和上下文入口 | 手工创建 `CLAUDE.md` / `.claude/CLAUDE.md` | C | `commands/04` |
| `/mcp` | 管理 MCP servers | `claude mcp`、`--mcp-config`、`--strict-mcp-config` | B | `commands/08` |
| `/memory` | 查看/编辑 memory、开关 auto-memory | 无单一 CLI flag | D | `commands/04`、`commands/09` |
| `/model [model]` | 切换模型 | `claude --model <model>` | A | `commands/07` |
| `/permissions` | 调整权限和审批规则 | `--permission-mode`、`--allowedTools`、`--disallowedTools`、`--tools` | B | `commands/08` |
| `/plan [description]` | 在会话内进入规划流 | `--permission-mode plan` | C | `commands/03` |
| `/resume [conversationId]` | 恢复历史会话 | `claude --resume [id]`、`--continue` | A | `commands/01`、`commands/09` |
| `/rewind [messageId]` | 回退到较早节点 | 无真正 CLI 一键等价 | D | `commands/02` |
| `/security-review` | 审查待提交改动的安全风险 | 无稳定外部等价 | D | `commands/12` |
| `/status` | 查看当前会话和环境状态 | 无单一 CLI 等价 | D | `commands/12` |
| `/stats` | 使用/统计信息 | 无单一 CLI 等价 | D | `commands/11` |
| `/tasks` | 查看后台任务 | 无稳定外部等价 | D | `commands/12` |
| `/usage` | 看 usage 数据 | 无单一 CLI 等价 | D | `commands/11` |
| `/ultraplan [description]` | 更重型规划流 | 无单一 CLI 等价 | D | `commands/12` |

### 环境、账户、平台相关

这些命令是官方 built-in commands，但不属于这套 coding practice 的核心命令面。可以记录在 reference 里，不强行写进主章节。

- `/chrome`
- `/desktop`
- `/ide [command]`
- `/insights`
- `/login`
- `/logout`
- `/migrate-installer`
- `/mobile`
- `/passes`
- `/plugin [command]`
- `/plugins [command]`
- `/privacy-settings`
- `/remote-env`
- `/settings`
- `/setup-token`
- `/terminal-setup`
- `/upgrade`
- `/voice`
- `/fast [on|off]`

## 需要明确区分的几组关系

### 1. `/resume` vs `--resume`

- 这是最接近 Type A 的一组
- 外部入口适合在启动时决定恢复哪条会话
- 会话内入口适合在交互中直接切换历史会话

### 2. `/branch` vs `--fork-session`

- 作用都和“保留历史、走另一条路径”有关
- 但 `/branch` 是交互内分叉，`--fork-session` 是启动时分叉
- 因此更适合作为 Type C 近似映射，而不是硬说完全等价

### 3. `/plan` vs `--permission-mode plan`

- 二者都服务“先规划再执行”
- 但外部命令更适合批处理、落盘、审查后再实施
- slash command 更适合交互式会话内临时切到规划流

### 4. `/permissions` vs CLI 权限参数

- `/permissions` 是交互内调整权限策略的入口
- CLI 侧则分散在 `--permission-mode`、`--allowedTools`、`--disallowedTools`、`--tools`
- 这是典型 Type B combined mapping

### 5. `/memory` vs memory 文件

- `/memory` 是交互内查看和编辑 memory 的入口
- CLI 没有一个单一 flag 能完全替代
- 外部做法更像“直接改 `CLAUDE.md` / `.claude/CLAUDE.md` / `MEMORY.md` 文件”

## 对这次融合最重要的 no-mapping 命令

这些命令很有用，但很难自然归到已有外部命令章节里，所以应考虑进入最后一个补充兜底章：

- `/btw`
- `/config`
- `/copy`
- `/doctor`
- `/help`
- `/security-review`
- `/status`
- `/tasks`
- `/ultraplan`

## 官方页面里的状态变化

官方 built-in commands 页面还明确标了几条变化：

- `/pr-comments` 已在 `v2.1.91` 移除
- `/review [since]` 已废弃
- `/vim` 已在 `2.1.92` 移除

所以这次融合时不应把这些命令当成当前稳定能力写进主实践。

## 这次对 `practice_claude_code/` 的落地策略

1. 先把高相关 slash commands 融入已有的 11 个 `commands/` 章节
2. 再把这些 in-session 入口补回对应的 `practices/`
3. 对 interactive-only 且重要、又难并入原章节的内容，新增第 12 个补充章

## 待确认或不写死的点

- slash command 列表会随 Claude Code 版本持续变化
- 某些命令可能受平台、订阅、桌面端或插件环境影响
- `/plan`、`/ultraplan`、`/tasks` 等交互命令的内部实现细节，不在这次实践文档里写死
