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

本机核验边界：

- 当前环境里没有全局 `opencode` 命令
- 但本机存在 App bundle 自带 CLI：
  - `/Applications/OpenCode.app/Contents/MacOS/opencode-cli`
- 已核验：
  - `opencode-cli --version` = `1.4.0`
  - `opencode-cli --help` 可正常列出顶层命令面
  - `opencode-cli run --help`
  - `opencode-cli session --help`
  - `opencode-cli agent --help`
  - `opencode-cli serve --help`

这意味着这份 mapping reference 现在是：

- `官方 docs grounded`
- 并补了 `本机 app-bundled CLI help grounded`
- 但还不是 `本机 TUI 交互逐项实测 grounded`

本地源码补核入口：

- `packages/opencode/src/cli/cmd/tui/app.tsx`
- `packages/opencode/src/cli/cmd/tui/routes/session/index.tsx`
- `packages/opencode/src/cli/cmd/tui/component/prompt/index.tsx`
- `packages/opencode/src/cli/cmd/tui/component/prompt/autocomplete.tsx`
- `packages/opencode/src/command/index.ts`
- `packages/opencode/src/config/config.ts`

所以这份 mapping reference 这次实际采用的是三层 grounding：

- `官方 docs grounded`
- `本机 app-bundled CLI help grounded`
- `本地 CLI/TUI 注册点 source grounded`

## 官方术语

OpenCode 当前更稳的交互命令面术语至少有三层：

1. `slash commands`
2. `built-in commands`
3. `custom commands`

TUI 页的写法是：

- 在 TUI 里输入 `/` 后跟 command name 来执行动作

Commands 页的写法是：

- custom commands 也是在 TUI 里通过 `/my-command` 触发
- 它们是 built-in commands 之外的另一层可扩展命令面

再结合当前本地源码，更准确的分层是：

1. `TUI built-ins`
2. `built-in command layer`，例如默认的 `init` / `review`
3. `project/global custom commands`
4. `MCP prompt commands`
5. 一部分 `skills`

另外，TUI 文档还明确提到：

- `/help` 会打开 help dialog
- 当前本地源码里，真正稳定的 command palette 入口是 `ctrl+p`

所以对 OpenCode 来说，真正的 interactive command surface 不只是 built-in slash commands，还包括：

- `.opencode/commands/`
- `~/.config/opencode/commands/`
- `command` config 里定义的 custom commands

## 映射类型

- Type A：exact mapping
- Type B：combined mapping
- Type C：approximate mapping
- Type D：no stable external mapping

## 当前 local CLI/TUI 能确认的 slash built-ins

结合 `packages/opencode/src/cli/cmd/tui/` 当前注册点，本机这版 CLI/TUI 至少明确注册了：

- `/sessions`，aliases：`/resume`、`/continue`
- `/new`，alias：`/clear`
- `/models`
- `/agents`
- `/mcps`
- `/variants`
- `/connect`
- `/status`
- `/themes`
- `/help`
- `/exit`，aliases：`/quit`、`/q`
- `/share`
- `/rename`
- `/timeline`
- `/fork`
- `/compact`，alias：`/summarize`
- `/unshare`
- `/undo`
- `/redo`
- `/timestamps`，alias：`/toggle-timestamps`
- `/thinking`，alias：`/toggle-thinking`
- `/copy`
- `/export`
- `/editor`

其中一部分是 app 级 / system 级命令：

- `/sessions`
- `/new`
- `/models`
- `/agents`
- `/mcps`
- `/variants`
- `/connect`
- `/status`
- `/themes`
- `/help`
- `/exit`

另一部分是 session 级命令：

- `/share`
- `/rename`
- `/timeline`
- `/fork`
- `/compact`
- `/unshare`
- `/undo`
- `/redo`
- `/timestamps`
- `/thinking`
- `/copy`
- `/export`

还有 prompt 输入层命令：

- `/editor`

一个重要校正：

- 当前本地 CLI/TUI 源码里，确实注册了 “Show/Hide tool details” 命令
- 但没有看到稳定的 slash 名称
- 默认 keybind `tool_details` 也是 `none`

所以 `/details` 这次不能再写成“本机已确认的稳定 slash command”。

更稳的说法是：

- docs 曾提到 `/details`
- 但当前本地 CLI/TUI 注册点里，更可靠入口是 command palette `ctrl+p`
- 或者你自己在 `tui.json` 里给 `tool_details` 绑 keybind

## 当前 local CLI/TUI 能确认的 built-in command layer

除了上面那批 TUI built-ins，本地 `packages/opencode/src/command/index.ts` 还明确内置了两条默认命令：

- `/init`
- `/review`

已确认边界：

- `/init`：默认命令名 `init`，描述为 `create/update AGENTS.md`
- `/review`：默认命令名 `review`，描述为 `review changes [commit|branch|pr], defaults to uncommitted`
- `/review` 默认 `subtask: true`

所以 `/init` 和 `/review` 是正式的一等命令面，但它们更接近 built-in command layer，不是 `help/show status/themes` 这一组低层 TUI built-ins。

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

再结合本地源码，还有两个值得补上的边界：

- slash autocomplete 会把 server-side command list 也并进来
- 其中 `source === "mcp"` 的命令会以 slash 形式出现
- 当前 CLI/TUI autocomplete 会跳过 `source === "skill"` 的条目

所以“slash command surface”这次更准确的理解是：

- 不是只有 `.opencode/commands/*`
- 还会混入默认命令和一部分 MCP prompt commands

这意味着 OpenCode 的 custom commands 不只是“提示词模板”，而是真正会影响上下文污染边界的交互层。

## 交互层不只 slash commands

官方 TUI / keybinds docs 当前还明确写到：

- 大多数命令也有 keybind
- 默认 leader key 是 `ctrl+x`
- 这层配置走 `tui.json` / `tui.jsonc`

所以 OpenCode 的 interactive command surface 更准确地说至少有四层：

1. TUI built-in slash commands
2. built-in command layer，例如 `/init`、`/review`
3. project/global custom slash commands，以及一部分 MCP prompt commands
4. TUI keybind / command palette layer

## 映射总表

| Interactive command | 主要用途 | 外部命令 / 配置对应 | 映射类型 | 建议落位 |
|---|---|---|---|---|
| `/connect` | 在 TUI 里添加 provider 凭证 | `opencode providers login`（顶层别名 `auth`）+ provider config | B | `commands/11` |
| `/compact` `/summarize` | 压缩当前会话 | 无稳定外部等价物 | D | `commands/06` |
| `Show/Hide tool details` | 切换 tool execution details | 当前本地 CLI/TUI 未确认稳定 slash；更稳入口是 `ctrl+p` command palette 或自定义 keybind | D | `commands/12` |
| `/editor` | 打开外部编辑器写长提示词 | 手工用 `$EDITOR` 写 prompt 再送入 `run` | C | `commands/05` |
| `/exit` `/quit` `/q` | 退出当前 TUI | 结束当前 `opencode` 进程 | C | `commands/01` |
| `/export` | 导出当前会话到 Markdown | `opencode export [sessionID]` 只导 JSON | C | `commands/09` |
| `/help` | 打开 help dialog | `ctrl+p` 更接近 command palette；无单一外部等价物 | D | `commands/12` |
| `/init` | 生成或改进 `AGENTS.md` | 手工编辑 `AGENTS.md` | C | `commands/04` |
| `/models` | 在 TUI 中查看并选择模型 | `opencode models` + `-m/--model` + config | B | `commands/07` |
| `/agents` | 在 TUI 中查看并切换 agent | `opencode agent list` + `--agent` + `Tab`/`Shift+Tab` | B | `commands/10` |
| `/mcps` | 查看和切换 MCP 状态 | `opencode mcp list` / `mcp add` / `mcp auth` | B | `commands/08`、`commands/12` |
| `/variants` | 查看可用 model variants | `variant_cycle` keybind + model/provider-specific variant support | B | `commands/07` |
| `/status` | 查看系统状态 | 无单一外部等价物 | D | `commands/12` |
| `/themes` | 在 TUI 中切换主题 | `tui.json` theme config | B | `commands/12` |
| `/new` `/clear` | 新开当前 TUI 会话 | 重新启动 `opencode` 或不用 `--continue` | C | `commands/01` |
| `/redo` | 恢复刚刚被 `/undo` 的消息与改动 | 无稳定外部等价物 | D | `commands/02` |
| `/sessions` `/resume` `/continue` | 列会话并切换 | `opencode session list` + `--continue` / `--session` | B | `commands/01`、`commands/09` |
| `/share` | 分享当前 TUI 会话 | `opencode run --share` 只覆盖 run 场景 | C | `commands/09` |
| `/rename` | 重命名当前 session | 无直接外部等价物 | D | `commands/09`、`commands/12` |
| `/timeline` | 跳到特定消息 | 无直接外部等价物 | D | `commands/09`、`commands/12` |
| `/fork` | 从消息处分叉 | `--fork` 只覆盖启动/继续场景，不等于 timeline fork | C | `commands/01`、`commands/09` |
| `/copy` | 复制当前 session transcript | 无直接外部等价物 | D | `commands/05`、`commands/12` |
| `/thinking` | 只切换 reasoning block 可见性 | 无稳定外部等价物 | D | `commands/07`、`commands/12` |
| `/timestamps` | 切换时间戳显示 | 无直接外部等价物 | D | `commands/12` |
| `/undo` | 撤销最近消息和相关改动 | `git restore` 只能近似回退文件 | C | `commands/02` |
| `/unshare` | 取消当前会话分享 | 无直接外部命令对应 | D | `commands/09` |
| `/review` | 评审未提交改动、branch 或 PR | 无单一外部等价物；更接近 built-in review workflow | C | `commands/10`、`commands/12` |

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

### 5. `/agents` 与 `/permissions` 要分开看

这点很重要，因为本轮复核后，二者结论已经分叉：

- 当前 local CLI/TUI 源码已经注册了 `/agents`
- 但仍然没有看到一个第一方 built-in `/permissions`

也就是说：

- `/agents` 现在不该再写成“不存在”
- `/permissions` 仍然更应写成 config / agent config 主路径

所以在 `practice_opencode/` 里应该明确：

- agent 的交互入口包括：
  - `/agents`
  - `@agent` mention
  - custom commands
- permission 的主入口主要是 config / agent config

不要把这两件事混成同一个结论。

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

### 6. 主题命令的 docs 确实有过不一致，但本地 CLI/TUI 已能压实

当前官方文档里一度有：

- TUI 页列的是 `/themes`
- Themes 页写的是 `/theme`

但这次本地 CLI/TUI 注册点已经明确给出：

- 当前 local CLI/TUI 使用的是 `/themes`
- 默认 keybind 是 `<leader>t`

所以这轮落文不再需要把 `/themes` / `/theme` 保持成完全不确定状态。

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
- `07`：`/models`、`/variants`、`/thinking`
- `09`：`/export`、`/share`、`/unshare`、`/sessions`
- `10`：`/agents`、`@explore`、`@general`、custom `/review-*` commands
- `11`：`/connect` 和 `/models` 作为 provider/model onboarding

### 需要兜底的 interactive-only

本轮判断，OpenCode 里有一组命令很有价值，但没有稳定 external 等价物，也不适合硬塞进原 11 章：

- `/help`
- `/status`
- `/editor`
- command palette 下的 tool details
- `/thinking` 的“显示层”侧面
- theme selector 的 TUI 操作

因此这次适合新增第 `12` 章，专门承接 interactive-only session ops。

## 本轮落地决策

1. 新增一份 command surface mapping reference
2. 补强现有 11 个 `commands/`
3. 回写现有 11 个 `practices/`
4. 新增第 `12` 章作为 OpenCode 的 interactive-only 兜底章

## 待确认或不写死的点

- 某些命令在不同版本 TUI 里的 UI 细节
- custom command 覆盖 built-in command 之后的优先级边界细节
- 本轮没有逐项驱动真实 TUI 交互，所以 `/help` 菜单最终显示形态仍未逐项截图确认
