# Aider 官方参考手册

最后核验日期：2026-04-07  
本机版本：`aider 0.86.3.dev+less`

## 1. 官方来源

### A 级：官方文档

- `https://aider.chat/docs/usage.html`
- `https://aider.chat/docs/usage/commands.html`
- `https://aider.chat/docs/git.html`
- `https://aider.chat/docs/faq.html`
- `https://aider.chat/2023/10/22/repomap.html`

### A 级：本机 CLI

- `aider --version`
- `aider --help`

## 2. 产品边界

写 `round1` 时先分层：

- `Aider` 是终端工具
- `--model` 选择的才是模型
- `repo map`、`architect`、`/undo`、`--read` 是工作机制

不要把 Aider 写成某个固定模型，也不要把它描述成“抽象 IDE agent”。

## 3. 会话与使用方式

本机帮助可确认：

- 交互式聊天是默认模式
- `--message` / `--message-file` 是非交互式路径
- 可以显式指定可编辑文件 `--file`
- 可以指定只读文件 `--read`
- 支持恢复 chat history：`--restore-chat-history`

这意味着：

- Aider 既可以连续对话，也可以做单次批处理
- 它并不是“只有一次性 prompt”的工具

## 4. Repository Map

### 4.1 正式术语就是 repo map

官方 FAQ、博客和本机帮助共同使用：

- `repository map`
- `repo map`
- `--show-repo-map`
- `--map-tokens`
- `--map-refresh`

### 4.2 不能偷换成 AST 图谱

这点是 `round1` 最容易写错的地方。

安全写法：

- “Aider 有 repository map / repo map 机制，用于把大仓的结构压缩进上下文”

不安全写法：

- “Aider 在后台构建整个代码库的 AST 图谱”

官方从来没有把它定义成 AST 图谱。

### 4.3 可确认的 repo map 配置项

本机帮助直接给出：

- `--map-tokens`
- `--map-refresh {auto,always,files,manual}`
- `--map-multiplier-no-files`
- `--show-repo-map`

这说明 repo map 是正式、可调、可关闭的机制。

## 5. 只读探索与规划

### 5.1 `--read` 是正式入口

本机帮助明确支持：

- `--read FILE`

官方 FAQ 也提到：

- 可以用 `/read` 增加只读文件

### 5.2 `/ask` 是正式命令

官方 commands 文档明确列出：

- `/ask`

所以 Aider 完全可以做只读分析型工作流，不能写成“只能直接改代码”。

### 5.3 `architect` 是正式模式

本机帮助和官方命令文档共同确认：

- `--architect`
- `--auto-accept-architect`
- `/architect`

因此：

- 可以把 Aider 的规划/执行分层落在 `--read` + `/ask` + `architect`
- 不需要虚构一个“Plan Mode”

## 6. 命令体系

官方 commands 文档明确列出一批关键命令，足以支撑 `round1` 写法：

- `/add`
- `/ask`
- `/architect`
- `/clear`
- `/reset`
- `/undo`

本机帮助也可确认大量工作流参数：

- `--lint`
- `--test`
- `--show-prompts`
- `--show-repo-map`
- `--apply`

## 7. Git 绑定是 Aider 的核心特征

### 7.1 默认是 git-first

官方 git 文档和本机帮助共同确认：

- Aider 编辑后默认自动提交
- `/undo` 基于 git commit 回退
- 支持：
  - `--auto-commits`
  - `--dirty-commits`
  - `--commit`
  - `--no-git`

### 7.2 `/undo` 的真实语义

官方命令文档的含义是：

- `/undo` 撤销 Aider 做的上一次 git commit

因此不要写：

- “/undo 就是撤回上一条输出”

### 7.3 对 `round1` 的意义

如果要讲“编辑不是追加”“如何回退”：

- Aider 的 grounding 应落在 git 边界
- 不能拿 Cursor checkpoint 或 Claude compact 的逻辑来套它

## 8. 模型与角色分工

本机帮助可确认：

- 主模型：`--model`
- 弱模型：`--weak-model`
- 编辑模型：`--editor-model`
- 编辑格式：`--edit-format`
- 推理强度：`--reasoning-effort`

这说明：

- Aider 允许把提交信息、历史摘要、编辑执行拆给不同模型
- 但这不是“多 agent 系统”，不要和 Claude/Codex 的 subagents 混为一谈

## 9. 历史、缓存与自动化

### 9.1 历史文件

本机帮助明确给出：

- `--input-history-file`
- `--chat-history-file`
- `--llm-history-file`
- `--restore-chat-history`

### 9.2 prompt caching

本机帮助明确给出：

- `--cache-prompts`
- `--cache-keepalive-pings`

### 9.3 非交互式与调试入口

本机帮助明确给出：

- `--message`
- `--message-file`
- `--apply`
- `--show-repo-map`
- `--show-prompts`

所以如果 `round1` 要讲“自动化”“批处理”“脚本化”，Aider 是有正式入口的。

## 10. 可以安全写成事实的

- Aider 的正式术语是 repository map / repo map
- Aider 有正式只读入口：`--read` / `/read`
- `/ask`、`/architect`、`/clear`、`/reset`、`/undo` 都是正式命令
- Aider 的编辑和撤销高度依赖 git commit 边界
- Aider 支持非交互式消息模式和 prompt caching

## 11. 必须避免的错误写法

- “Aider 会在后台构建 AST 图谱”
- “Aider 没有只读规划方式”
- “/undo 只是撤销上一条文本输出”
- “Aider 的 repo map 就等于 IDE 的完整代码索引”

## 12. 给 `round1` 的推荐话术

### 规划

“Aider 更稳的规划工作流是 `--read` + `/ask`，需要结构化设计时再切到 `architect`。”

### 大仓理解

“Aider 依赖 repo map 压缩大仓结构，它不是 AST 图谱，但足以在有限上下文里保持代码库轮廓。”

### 回退

“Aider 的 `/undo` 依赖它自己的 git 提交边界，因此它的撤销语义与 Cursor checkpoint 或纯聊天压缩完全不同。”
