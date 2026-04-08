# OpenCode Command Surface Mapping Reference

核验日期：2026-04-09

## 目的

这份 reference 用来支撑 `practice_opencode/` 的 command-surface 补强：

- 先确认 OpenCode 官方对交互式 `/...` 命令面的叫法
- 再把 built-in slash commands 和 custom commands 整理出来
- 然后映射回现有 `commands/` 与 `practices/`

## 证据边界

本次主要依赖 OpenCode 官方文档：

- `https://opencode.ai/docs/tui/`
- `https://opencode.ai/docs/commands/`
- `https://opencode.ai/docs/cli/`
- `https://opencode.ai/docs/providers/`
- `https://opencode.ai/docs/themes/`

本机边界：

- 当前环境里没有安装 `opencode`
- 所以本次不能用本机 `opencode --help` 或真实 TUI 交互补核

这意味着这份 mapping reference 是：

- `官方 docs grounded`
- 不是 `本机命令面逐项实测 grounded`

## 官方术语

OpenCode 当前官方文档里，最稳的交互命令面术语有三层：

1. `slash commands`
2. `built-in commands`
3. `custom commands`

TUI 页的写法是：

- 在 TUI 里输入 `/` 后跟 command name 来执行动作

Commands 页的写法是：

- custom commands 也是在 TUI 里通过 `/my-command` 触发
- 它们是 built-in commands 之外的另一层可扩展命令面

另外，TUI 文档还明确提到：

- `/help` 会打开 help dialog
- `ctrl+x h` 或 `/help` 也被用作 command palette 入口

所以对 OpenCode 来说，真正的 interactive command surface 不只是 built-in slash commands，还包括：

- `.opencode/commands/`
- `~/.config/opencode/commands/`
- `command` config 里定义的 custom commands

## 映射类型

- Type A：exact mapping
- Type B：combined mapping
- Type C：approximate mapping
- Type D：no stable external mapping

## 当前能确认的 built-in slash commands

TUI 官方页当前列出的 built-in slash commands 有：

- `/connect`
- `/compact`
- `/details`
- `/editor`
- `/exit`
- `/export`
- `/help`
- `/init`
- `/models`
- `/new`
- `/redo`
- `/sessions`
- `/share`
- `/thinking`
- `/undo`
- `/unshare`

已确认的 alias：

- `/compact` = alias `/summarize`
- `/exit` = aliases `/quit`、`/q`
- `/new` = alias `/clear`
- `/sessions` = aliases `/resume`、`/continue`

## 当前能确认的 custom commands 面

Commands 官方页当前确认：

- custom commands 用 `/name` 的形式在 TUI 中触发
- 定义位置：
  - `.opencode/commands/`
  - `~/.config/opencode/commands/`
  - `opencode.json` 的 `command` 配置
- 支持：
  - `description`
  - `agent`
  - `subtask`
  - `model`
  - `$ARGUMENTS` / `$1` / `$2`
  - `!` shell output 注入
  - `@file` 文件引用

一个重要边界：

- custom commands can override built-in commands

所以不要随便把自定义命令命名成 `share`、`undo`、`models` 这类会覆盖内建行为的名字。

## 映射总表

| Interactive command | 主要用途 | 外部命令 / 配置对应 | 映射类型 | 建议落位 |
|---|---|---|---|---|
| `/connect` | 在 TUI 里添加 provider 凭证 | `opencode auth login` + provider config | B | `commands/11` |
| `/compact` `/summarize` | 压缩当前会话 | 无稳定外部等价物 | D | `commands/06` |
| `/details` | 切换 tool execution details | 无稳定外部等价物 | D | `commands/12` |
| `/editor` | 打开外部编辑器写长提示词 | 手工用 `$EDITOR` 写 prompt 再送入 `run` | C | `commands/05` |
| `/exit` `/quit` `/q` | 退出当前 TUI | 结束当前 `opencode` 进程 | C | `commands/01` |
| `/export` | 导出当前会话到 Markdown | `opencode export [sessionID]` 只导 JSON | C | `commands/09` |
| `/help` | 打开 help dialog / command palette | 无单一外部等价物 | D | `commands/12` |
| `/init` | 生成或改进 `AGENTS.md` | 手工编辑 `AGENTS.md` | C | `commands/04` |
| `/models` | 在 TUI 中查看并选择模型 | `opencode models` + `-m/--model` + config | B | `commands/07` |
| `/new` `/clear` | 新开当前 TUI 会话 | 重新启动 `opencode` 或不用 `--continue` | C | `commands/01` |
| `/redo` | 恢复刚刚被 `/undo` 的消息与改动 | 无稳定外部等价物 | D | `commands/02` |
| `/sessions` `/resume` `/continue` | 列会话并切换 | `opencode session list` + `--continue` / `--session` | B | `commands/01`、`commands/09` |
| `/share` | 分享当前 TUI 会话 | `opencode run --share` 只覆盖 run 场景 | C | `commands/09` |
| `/thinking` | 只切换 reasoning block 可见性 | 无稳定外部等价物 | D | `commands/07`、`commands/12` |
| `/undo` | 撤销最近消息和相关改动 | `git restore` 只能近似回退文件 | C | `commands/02` |
| `/unshare` | 取消当前会话分享 | 无直接外部命令对应 | D | `commands/09` |

## 当前 docs 里值得特别注意的几组边界

### 1. `/connect` 不是单纯等价于 `opencode auth login`

更稳的理解是：

- `opencode auth login` 是外部 CLI 的凭证入口
- `/connect` 是 TUI 内的 provider onboarding 入口
- 它常和 `/models` 连着用

所以写作时更适合作为 Type B combined mapping，而不是硬说一模一样。

### 2. `/models` 也不是单独等于 `opencode models`

官方 docs 里：

- `opencode models` 更像外部枚举和检查模型列表
- `/models` 在 TUI 里还承担选择当前会话模型的作用

所以更稳的对应关系是：

- 发现模型：`opencode models`
- 指定模型：`-m/--model` 或 config
- 会话内切换：`/models`

### 3. `/export` 和 `opencode export` 不是同一种导出

当前官方 docs 显示：

- `/export`：导出当前会话到 Markdown 并打开编辑器
- `opencode export`：导出 session data as JSON

这组关系不能写成 exact mapping。

### 4. `/thinking` 只控制显示，不控制真实推理能力

TUI 页当前明确写到：

- `/thinking` 只切 reasoning block visibility
- 不会启用或关闭模型的实际 reasoning capabilities
- 真正切 reasoning 相关变体，要用 `ctrl+t` cycle model variants

所以不要把 `/thinking` 写成模型路由开关。

### 5. OpenCode 当前 docs 没有列出内建 `/agents` 或 `/permissions`

这点很重要，因为其他 agent 常常有：

- `/agents`
- `/permissions`

但 OpenCode 这次官方 TUI 命令清单里没有这两个 built-in commands。

所以在 `practice_opencode/` 里应该明确：

- agent 的交互入口主要是 `@agent` mention 和 custom commands
- permission 的主入口主要是 config / agent config

不要硬补出并不存在的 slash command。

### 6. 主题命令的 docs 目前存在不一致

当前官方文档里：

- TUI 页列的是 `/themes`
- Themes 页写的是 `/theme`

在没有本机实测前，这次不把某一个名字写死成绝对正确。

更稳的写法是：

- OpenCode 确实有 TUI 里的 theme selector
- 但命令名在当前 docs 中存在 `/themes` vs `/theme` 不一致
- 真正执行前，应在你本机版本用 `/help` 再确认

## 对 `practice_opencode/` 的融合策略

### 融入现有章节

- `01`：`/new`、`/sessions`、`/resume`、`/continue`、`/quit`
- `02`：`/undo`、`/redo`
- `03`：没有内建 `/plan`，但要明确 `plan` agent 切换和 custom `/plan-*` commands
- `04`：`/init`
- `05`：`/editor`，以及 custom commands 对 `@file` / `!command` 的支持
- `06`：`/compact`、`/summarize`
- `07`：`/models`、`/thinking`
- `09`：`/export`、`/share`、`/unshare`、`/sessions`
- `10`：`@explore`、`@general`、custom `/review-*` commands
- `11`：`/connect` 和 `/models` 作为 provider/model onboarding

### 需要兜底的 interactive-only

本轮判断，OpenCode 里有一组命令很有价值，但没有稳定 external 等价物，也不适合硬塞进原 11 章：

- `/help`
- `/details`
- `/thinking` 的“显示层”侧面
- theme selector 的 TUI 操作

因此这次适合新增第 `12` 章，专门承接 interactive-only session ops。

## 本轮落地决策

1. 新增一份 command surface mapping reference
2. 补强现有 11 个 `commands/`
3. 回写现有 11 个 `practices/`
4. 新增第 `12` 章作为 OpenCode 的 interactive-only 兜底章

## 待确认或不写死的点

- 当前 docs 的 `/themes` vs `/theme` 不一致
- 某些命令在不同版本 TUI 里的 UI 细节
- custom command 覆盖 built-in command 之后的优先级边界细节
- 没有本机安装，无法确认本地 `/help` 菜单最终显示形态
