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

另外几条会直接影响 practice 设计的事实：

- 如果 command 不指定 `agent`，默认继承当前 agent
- 如果 command 绑定的是 subagent，默认就会触发 subagent invocation
- `subtask: true` 可以强制 command 以 subtask 方式运行，即使它绑定的是 primary agent

这意味着 OpenCode 的 custom commands 不只是“提示词模板”，而是真正会影响上下文污染边界的交互层。

## 交互层不只 slash commands

官方 TUI / keybinds docs 当前还明确写到：

- 大多数命令也有 keybind
- 默认 leader key 是 `ctrl+x`
- 这层配置走 `tui.json` / `tui.jsonc`

所以 OpenCode 的 interactive command surface 更准确地说有三层：

1. built-in slash commands
2. custom slash commands
3. TUI keybind layer

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

### 3.1 `/undo` 对第 02 章的影响比“有个回退命令”更深

官方使用指南当前明确写到：

- `/undo` 后会把原来的用户消息重新显示出来
- 你可以从那里 tweak the prompt and try again
- 还可以连续多次 `/undo`

这意味着 OpenCode 对“修改原提示词，而不是继续追加”这条习惯给了正式的交互支撑。

### 4. `/thinking` 只控制显示，不控制真实推理能力

TUI 页当前明确写到：

- `/thinking` 只切 reasoning block visibility
- 不会启用或关闭模型的实际 reasoning capabilities
- 真正切 reasoning 相关变体，要用 `ctrl+t` cycle model variants

所以不要把 `/thinking` 写成模型路由开关。

### 4.1 `/new` 和 `/sessions` 对第 01 章的影响也不只是命令别名

因为这两个动作都可以在 TUI 内完成，所以：

- session boundary management 变成了会话内低摩擦动作
- 用户不该因为“已经在这个 TUI 里了”就继续污染同一线程

这会强化第 01 章，而不是只给它补一个 slash command 清单。

### 5. OpenCode 当前 docs 没有列出内建 `/agents` 或 `/permissions`

这点很重要，因为其他 agent 常常有：

- `/agents`
- `/permissions`

但 OpenCode 这次官方 TUI 命令清单里没有这两个 built-in commands。

所以在 `practice_opencode/` 里应该明确：

- agent 的交互入口主要是 `@agent` mention 和 custom commands
- permission 的主入口主要是 config / agent config

不要硬补出并不存在的 slash command。

### 5.1 `/init` 会影响第 04 章的规则主入口

官方 rules docs 当前明确写到：

- `/init` 会创建或更新 `AGENTS.md`
- 如果已有 `AGENTS.md`，会就地改进
- 规则优先级里，`AGENTS.md` 高于同层的 `CLAUDE.md`

所以对一个原本依赖 `CLAUDE.md` fallback 的项目来说，运行 `/init` 不是纯粹“写个说明文件”，而是会改变后续 session 的项目规则入口。

### 5.2 `AGENTS.md` 不会自动解析外部文件引用

官方 rules docs 当前明确写到：

- OpenCode 不会自动解析 `AGENTS.md` 中的外部文件引用
- 推荐用 `instructions` 来包含更多文档、规则和约束

这意味着：

- 稳定规则材料应该优先放 `instructions`
- `AGENTS.md` 更适合写高密度、慢变化的项目入口规则

这件事会同时影响第 04 章和第 05 章，而不只是规则层本身。

### 6. 主题命令的 docs 目前存在不一致

当前官方文档里：

- TUI 页列的是 `/themes`
- Themes 页写的是 `/theme`

在没有本机实测前，这次不把某一个名字写死成绝对正确。

更稳的写法是：

- OpenCode 确实有 TUI 里的 theme selector
- 但命令名在当前 docs 中存在 `/themes` vs `/theme` 不一致
- 真正执行前，应在你本机版本用 `/help` 再确认

### 6.1 `/export` vs `opencode export` 会影响第 09 章的 handoff 分工

因为：

- `/export` 更像人类可读的 Markdown handoff
- `opencode export` 更像可搬运、可重新导入的 session state

所以第 09 章不该只写成“都叫 export，选一个用”，而应该明确它们服务的是不同层级的状态传递。

### 6.2 keybind layer 会影响第 12 章的边界

由于 OpenCode 有 leader-key 为中心的 TUI 操作层，所以第 12 章不应只理解成“slash commands 补充章”。

更准确的理解是：

- slash commands + keybinds + TUI behavior config

共同组成了 OpenCode 的 interaction layer。

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
