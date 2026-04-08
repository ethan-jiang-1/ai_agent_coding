# Cursor Command Surface Mapping Reference

核验日期：2026-04-09

## 目的

这份 reference 用来支撑 `practice_cursor/` 的 command-surface 补强：

- 先确认 Cursor 官方在编辑器内到底有哪些 `/` 和交互式命令入口
- 再把这些入口映射回现有的 `commands/` 章节
- 最后把这些做法反映回 `practices/`

这次整理的目标是 `Cursor` 编辑器产品，不是 `Cursor CLI`。

## 证据边界

本次主要依赖：

- Cursor 官方文档 URL 与官方搜索结果摘要
- 已有本地参考：`overall/reference_r1/2026-04-07_cursor_official.md`

需要明确说明的是：

- `docs.cursor.com` 在当前环境里直接抓取正文会命中 Vercel Security Checkpoint
- 因此这份 reference 更适合确认“官方术语、入口类型、关键样例、工作流边界”
- 不适合写死某个按钮的全部 UI 细节

## 官方术语

Cursor 官方这里最稳的术语不是单独写死成 `slash commands`，而是：

- `Commands`
- `quick commands`
- `project commands`
- `global commands`

对应关系可以这样理解：

- `quick commands`：在聊天里输入 `/command` 直接调用的内建快捷命令
- `project commands`：项目级自定义命令，来自 `.cursor/commands`
- `global commands`：用户级自定义命令，来自 `~/.cursor/commands`

所以对 Cursor 来说，`/` 只是入口形式；更稳的官方命名中心其实是 `Commands` 页面本身。

## 官方来源

- `https://docs.cursor.com/en/agent/chat/commands`
- `https://docs.cursor.com/en/context/@-symbols/slash-commands`
- `https://docs.cursor.com/agent/chats`
- `https://docs.cursor.com/agent/chat/history`
- `https://docs.cursor.com/en/context/memories`
- `https://docs.cursor.com/context/rules`

边界参考，但这次不作为 `practice_cursor/` 主体依据：

- `https://docs.cursor.com/en/cli/reference/slash-commands`
- `https://docs.cursor.com/en/cli/using-cursor-cli/compress`

## 为什么会觉得 Cursor 的 slash 很丰富

这个直觉并不奇怪，但要先把两张命令面拆开：

### 1. Cursor editor

editor 侧的 richness 主要来自：

- 5 个已能稳定确认的 built-in quick commands
- `.cursor/commands`
- `~/.cursor/commands`

也就是说，editor 的丰富度更多来自“可扩展命令面”，不是很长的一串官方内建 slash。

### 2. Cursor CLI

Cursor CLI 官方也有单独的 slash commands 页面，而且明显更像传统 CLI/TUI 的内建命令面，例如搜索摘要中能确认：

- `/help`
- `/model`
- `/new-chat`
- `/resume`
- `/auto-run`

所以如果你脑子里想的是“Cursor 的 slash 应该很多”，很可能混入了 CLI 那条命令面。

这次 `practice_cursor/` 仍然以 editor 产品为主，不把 CLI slash commands 混进主体章节。

## 这次映射的基本判断

`practice_cursor/commands/` 之前已经不是“shell 外部命令清单”，而是 Cursor 产品能力和操作入口的章节化整理。

所以这次工作不是把 editor `/` 命令硬翻译成 CLI flags，而是：

1. 先承认 Cursor editor 本身存在一层真实的 in-session command surface
2. 再把这些 `/` 命令和相关交互入口融入现有章节
3. 对没有稳定外部对应、也难自然并入的部分，单独补一个最后兜底章

## 映射类型

- Type A：exact mapping。现有章节已经描述了同一动作，只差把 `/` 入口补上。
- Type B：combined mapping。需要 slash command、UI 动作和已有章节一起理解。
- Type C：approximate mapping。作用接近，但不是严格等价。
- Type D：no mapping。没有稳定外部对应，只适合留在交互层。

## 当前能稳定确认的 editor-side command surface

### 1. 内建 quick commands

官方 `Commands` 页面搜索摘要可确认，Cursor 支持通过 `/command` 调用 quick commands；当前至少能确认这些样例：

| In-session command | 主要用途 | 映射类型 | 建议落位 |
|---|---|---|---|
| `/Generate Cursor Rules` | 生成规则草稿 | C | `commands/04` |
| `/Add Open Files to Context` | 把当前打开文件加入上下文 | A | `commands/05` |
| `/Add Active Files to Context` | 把当前活跃文件加入上下文 | A | `commands/05` |
| `/Reset Context` | 重置当前对话上下文 | C | `commands/06` |
| `/Disable Iterate on Lints` | 关闭 lint 自动迭代类行为 | C | `commands/06` |

这些例子足以说明：

- Cursor editor 的 `/` 入口是真实能力，不是民间叫法
- 其中一部分确实能直接支撑当前 practice
- 但官方搜索摘要没有保证这就是完整清单，所以不要把这五条写成“全部 slash commands”

### 2. 自定义 commands

官方 `Commands` 页面还能确认两类自定义命令来源：

- `.cursor/commands`
- `~/.cursor/commands`

这两类命令的价值在于：

- 把重复性的 chat-side workflow 做成可复用命令
- 让团队级和个人级的交互动作分层管理

它们和规则层不同：

- 规则层负责稳定约束
- custom commands 负责重复性会话动作

这一组更适合作为 Type D / interactive-only 内容，进入最后补充章。

官方搜索摘要还能确认这组事实：

- commands 当前属于 beta
- 语法未来可能变化
- command 文件本身是 markdown
- 可以带参数

这意味着它们不只是“提示词收藏夹”，而是 Cursor 官方认可的可扩展交互层。

### 2.1 官方示例命令库

官方 `Commands` 页搜索摘要还能确认一批示例命令名，这些例子对 practice 融合很有价值：

| Example command | 更接近的章节 | 说明 |
|---|---|---|
| `setup-new-feature` | `commands/03` | 适合把规划型问题收束成固定入口 |
| `run-all-tests-and-fix` | `commands/02` `commands/08` | 适合做失败后迭代修复与验证 |
| `code-review-checklist` | `commands/08` | 适合把 review/checklist 动作标准化 |
| `security-audit` | `commands/08` | 适合把安全审查独立成固定命令 |
| `light-review-existing-diffs` | `commands/02` `commands/08` | 适合先审 diff 再决定是否返工 |
| `address-github-pr-comments` | `commands/02` `commands/10` | 适合围绕 PR comments 做一轮定向修改 |
| `create-pr` | `commands/10` | 更接近前后台 agent 之后的 PR 交接动作 |
| `onboard-new-developer` | `commands/12` | 更像专项工作流，不强塞进原 11 章 |

### 3. 规则、历史与 handoff 的交互入口

除 `/...` 之外，Cursor 还有几类重要的会话内入口，也应纳入 command surface 理解：

- Command Palette 里的 `New Cursor Rule`
- history / old chats 检索
- `@Past Chats`
- chat export to markdown

这些动作不一定带 slash 前缀，但都属于“交互会话里真正会用到的操作层”。

## 几组关键边界

### 1. `/Generate Cursor Rules` 不是规则存储层本身

它更像规则草稿生成入口。

真正稳定落地时，仍然应该回到：

- `.cursor/rules/*.mdc`
- `AGENTS.md`

必要时通过 `New Cursor Rule` 或手工编辑把结果持久化。

### 2. `/Add Open Files to Context` / `/Add Active Files to Context` 不是“把一切都塞进去”

它们解决的是：

- 已经打开或正在看的文件，如何快速补入上下文

它们不意味着：

- 应该把所有打开标签一口气全塞给当前任务

### 3. `/Reset Context` 不是 Cursor 版 `/compact`

Cursor 的重点仍然是：

- 规则层稳定
- 当前任务状态外化到文件
- 历史污染严重时直接开新 tab

`/Reset Context` 是会话内重置动作，不应被误写成和 Claude Code `/compact` 对等。

### 4. custom commands 不是 rules 的替代品

更稳的分工是：

- 稳定长期约束：`AGENTS.md`、`.cursor/rules/*.mdc`
- 重复性会话动作：`.cursor/commands`、`~/.cursor/commands`
- 单次任务意图：当前 prompt / 当前 chat

## 对这次融合最重要的 no-mapping 内容

这次最值得进入最后补充章的，是这类 interactive-only command surface：

- project commands：`.cursor/commands`
- global commands：`~/.cursor/commands`
- 其他更偏会话操作层、又不适合硬塞进前 11 章的 command palette / slash 使用习惯

## 逐章复核结论

这次重新深挖后，更稳的章节判断是：

| 章节 | 结论 |
|---|---|
| `01` | 没发现 editor 内建 `/new-chat` 或 `/resume`；这一章仍以 tab / history 为主 |
| `02` | 应补 custom command 视角，例如 `run-all-tests-and-fix`、`light-review-existing-diffs`、`address-github-pr-comments` |
| `03` | 应补 `setup-new-feature` 这类规划型 custom command |
| `04` | 已有 `/Generate Cursor Rules`，并且适合继续保留 |
| `05` | 已有 `/Add Open Files to Context`、`/Add Active Files to Context` |
| `06` | 已有 `/Reset Context`、`/Disable Iterate on Lints` |
| `07` | 没发现 editor 内建 `/model`；模型选择仍以 model picker / custom modes 为主 |
| `08` | 应补 `security-audit`、`code-review-checklist`、`run-all-tests-and-fix` |
| `09` | history / export / `@Past Chats` 已补入 |
| `10` | 没发现 background-agent 专用 built-in slash；但 `create-pr`、`address-github-pr-comments` 适合补成前后台交接动作 |
| `11` | 没发现 editor 侧成本 / usage slash command；这一章仍以 dashboard / spending limit 为主 |
| `12` | 继续承接 project/global commands 这层 interactive-only surface |

## 对 `practice_cursor/` 的落地策略

1. 先把高相关 quick commands 融入 `commands/04`、`05`、`06`
2. 再把 history / export / `@Past Chats` 的交互入口补强到 `commands/09`
3. 新增第 12 章，承接 project/global custom commands 这类 interactive-only 内容
4. 把这些内容反映回对应的 `practices/`

## 待保持谨慎的点

- Cursor 的 `Commands` 页面当前能确认的是入口类型和关键样例，不宜冒充完整枚举
- command palette 中某些按钮名、位置或 beta 能力，后续可能变化
- Cursor CLI 的 slash commands 与 Cursor editor 的 commands 是两张不同命令面，不应混写
