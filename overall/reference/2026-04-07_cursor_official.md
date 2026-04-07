# Cursor 官方参考手册

最后核验日期：2026-04-07

## 1. 证据状态说明

Cursor 是当前四个工具里证据最不对称的一项。

原因不是资料少，而是：

- `docs.cursor.com` 在当前环境会触发 Vercel Security Checkpoint
- 正文 HTML、`llms.txt`、`sitemap.xml` 都被拦截

所以本文件采用两层证据：

- `A 级`：官方搜索结果摘要，来源页面仍是 `docs.cursor.com`
- `B 级`：官方页面标题、路径结构、同一主题多条官方摘要相互印证

这意味着：

- 能确认很多“产品层事实”
- 不适合写死某个按钮文案、细节快捷键、某个 beta UI 是否默认出现

写 `round1` 时必须尊重这个边界。

## 2. 官方来源

### 官方页面目标

- `https://docs.cursor.com/context/rules-for-ai`
- `https://docs.cursor.com/chat/manual`
- `https://docs.cursor.com/chat/overview`
- `https://docs.cursor.com/en/background-agents`
- `https://docs.cursor.com/agent/tools`
- `https://docs.cursor.com/agent/checkpoints`
- `https://docs.cursor.com/agent/custom-modes`

### 当前可直接确认的方式

- 官方搜索结果摘要
- 官方页面标题
- 官方 URL 路径和主题名称

## 3. 产品边界

写 `round1` 时先分层：

- `Cursor` 是编辑器/产品
- `Agent / Ask / Manual / Custom` 是工作模式
- `Background Agents` 是远程异步 agent 系统
- `.cursor/rules/*.mdc`、`AGENTS.md`、`.cursorrules` 是规则载体

不要把其中任何一层错写成另一层。

## 4. Rules 体系

### 4.1 当前主规则体系不是 `.cursorrules`

官方搜索摘要明确指出：

- Project Rules：`.cursor/rules`
- User Rules
- `AGENTS.md`
- `.cursorrules`（Legacy）

因此：

- `.cursor/rules/*.mdc` 才是当前主体系
- `.cursorrules` 必须被标注为 legacy

### 4.2 `.mdc` / `alwaysApply` / `globs` 是核心机制

官方搜索摘要可确认：

- 规则文件是 MDC 格式
- 常见元数据包括：
  - `description`
  - `globs`
  - `alwaysApply`

可安全推论：

- Cursor 的规则体系支持基于路径模式自动附着
- 它不是一个只有单文件全局 prompt 的系统

### 4.3 规则类型

官方摘要指出规则类型包括：

- `Always`
- `Auto Attached`
- `Agent Requested`
- `Manual`

所以写规则层级时，Cursor 适合被描述为“显式支持不同附着机制的规则系统”。

### 4.4 嵌套规则目录

官方摘要明确提到：

- 子目录可以有自己的 `.cursor/rules`
- 引用该目录文件时会自动附着相关规则

这足以支持“目录级局部规则”的写法。

### 4.5 `AGENTS.md` 在 Cursor 里也是官方支持项

这一点很重要，因为 `round1` 很容易误写成：

- “只有 Codex 才读 `AGENTS.md`”

更稳妥的表达是：

- Cursor 也支持 `AGENTS.md`
- 但它的主规则体系仍是 `.cursor/rules/*.mdc`

## 5. Modes

### 5.1 不要把 Cursor 简化成单一固定 mode 清单

官方页面标题与摘要共同显示：

- 有 `Agent`
- 有 `Ask`
- 有 `Manual`
- 有 `Custom modes`

但不同页面强调重点略有不同：

- 有的页面强调 `Agent / Ask / Custom`
- 有的页面明确点出 `Manual`

安全写法：

- “Cursor 当前官方模式体系围绕 Agent、Ask、Manual、Custom 展开，但不同文档页的叙事重点略有差异”

不安全写法：

- “Cursor 只有三种模式”
- “Cursor 内建标准 Plan Mode”

### 5.2 Ask 的官方定位

官方搜索摘要明确：

- `Ask` 是 read-only exploration
- 不自动改文件
- 适合 learning / planning / questions

所以如果 `round1` 要写“先规划、后执行”，Cursor 这里最稳的 grounding 是：

- `Ask` 负责只读分析
- 如需更专门的流程，再用 `Custom Modes`

### 5.3 Custom Modes

官方搜索摘要可确认：

- Custom modes 是 beta
- 需要在设置里开启
- 可以给 mode 定工具、提示词和工作方式
- 官方示例里确实会构造 `Plan` 之类的自定义模式

因此：

- 可以说“Cursor 可以通过 Custom Modes 实现 Plan/Debug/Refactor 流程”
- 不要写“Cursor 有一个固定、内建、官方标准名称的 Plan Mode”

## 6. Agent Tools

官方搜索摘要给出了相当多工具层信息，可用来避免写空话。

### 6.1 搜索 / 读取类

官方摘要能确认：

- Search
- Read File
- List Directory
- Codebase
- Grep / Search Files
- Web
- Fetch Rules

### 6.2 编辑类

官方摘要能确认：

- Edit
- Reapply
- Delete File

### 6.3 命令执行类

官方摘要能确认：

- Terminal
- Auto-run with guardrails
- Auto-fix errors

### 6.4 扩展类

官方摘要能确认：

- MCP 支持

写 `round1` 时，这比空泛地写“Cursor 很会改代码”更扎实。

## 7. Background Agents

### 7.1 它不是“本地多开一个聊天窗口”

官方搜索摘要明确说明：

- Background agents 在远程环境异步运行
- 默认运行在隔离的 Ubuntu 机器
- 有 internet access
- 通过 GitHub 克隆仓库并在独立 branch 上工作

这和“本地前台 agent 再开个 tab”完全不是一回事。

### 7.2 远程环境的进一步事实

官方摘要还能确认：

- 可以安装依赖
- 可以同时开多个 `tmux` terminal
- 有基于 `environment.json` 的环境设置能力

### 7.3 权限模型不同于前台 agent

官方摘要明确指出：

- Background agents 会自动运行 terminal commands
- 这与 foreground agent 需要用户审批每条命令不同

因此：

- 如果 `round1` 要谈权限模型，必须区分前台 agent 与 background agent

### 7.4 风险与边界

官方摘要还能确认：

- Background agents 支持从 GitHub issue / PR 触发
- 有 prompt injection 风险提示
- 数据保留不是无限期

所以如果正文要给实践建议，可以合理强调：

- 远程 agent 更强，但也更需要边界和安全意识

## 8. Checkpoints

官方摘要可确认：

- Cursor 有正式的 checkpoints
- checkpoints 不等同于 git
- 只跟踪 agent 产生的更改
- 不回滚手工编辑
- 可以从历史 checkpoints 恢复

这意味着：

- Cursor 的“撤销”不应被写成“自动撤销之前所有修改”
- 更准确的写法是“可以恢复到 agent 变更边界”

## 9. 对 `round1` 的直接约束

### 9.1 可以安全写成事实的

- `.cursor/rules/*.mdc` 是当前主 project rules 体系
- `AGENTS.md` 是官方支持项
- `.cursorrules` 是 legacy
- `Ask` 是只读探索模式
- Cursor 支持 Custom Modes
- Background Agents 是远程异步 agent，不是本地多开聊天窗
- Cursor 有正式 checkpoints 机制

### 9.2 应避免写成确定事实的

- “Cursor 内建固定名为 Plan Mode 的标准模式”
- “`.cursorrules` 是首选规则文件”
- “Background Agent 只是本地后台线程”
- “Cursor 会自动撤销所有之前的改动”

## 10. 给 `round1` 的推荐话术

### 规划

“Cursor 更稳的官方工作流是用 `Ask` 做只读探索，再用 `Custom Modes` 定义计划型或调试型流程，而不是把某个 `Plan Mode` 名称写成普适事实。”

### 规则

“Cursor 当前应优先使用 `.cursor/rules/*.mdc`；`AGENTS.md` 可作为简单全局指令；`.cursorrules` 属于 legacy。”

### 多 agent

“Cursor 的 Background Agents 是远程异步 agent，具备独立环境、GitHub 分支和更自动化的命令执行能力，不等同于本地多开聊天窗口。”

## 11. 本文件的盲区

由于 Cursor 官方正文在当前环境被 Vercel checkpoint 挡住，本文件不建议写死以下内容：

- 快捷键
- 某个菜单按钮名称
- 某个 beta 开关的默认位置
- 某个页面当前是否默认展示 `Manual`

这些内容如果以后需要写进 `round1`，应单独做一次人工浏览器核验。
