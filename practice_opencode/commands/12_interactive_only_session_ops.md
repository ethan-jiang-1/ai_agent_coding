# 12 交互式专用 session ops

这一章收那些很有价值、但没有稳定 external 对应，或者很难自然并入原 11 章的 OpenCode 会话内操作。

还要补一个边界：

- OpenCode 的交互操作层不只 slash commands
- 官方 docs 当前明确写到，大多数命令也有 leader-key keybind
- 默认 leader key 是 `ctrl+x`
- 这层配置走 `tui.json`，不是 `opencode.json`

## keybind layer

官方 keybinds docs 当前给出的例子包括：

- `<leader>n`：new session
- `<leader>l`：session list
- `<leader>x`：session export
- `<leader>m`：model list
- `<leader>a`：agent list
- `<leader>t`：theme list
- `<leader>s`：status
- `ctrl+p`：command palette / command list
- `f2`：recent model cycle
- `ctrl+t`：variant cycle

还有一些交互动作目前更明显地出现在 keybind config 层，例如：

- `status_view`
- `session_timeline`
- `session_rename`
- `session_fork`
- `session_share`
- `session_unshare`

这说明 OpenCode 的会话内操作层不只是“打 `/`”，还包括一整层可配置的键盘流。

## `/help`

最直接的命令发现入口。

当前 local CLI/TUI 注册点更稳的说法是：

- `/help`：打开 help dialog
- `ctrl+p`：打开 command palette / command list

适合：

- 你忘了当前版本到底有哪些 slash commands 或动作
- 你想先确认 slash 名称，再决定是不是要发自然语言提示

## tool details：当前更稳的入口是 command palette

当前 local CLI/TUI route 确实注册了 “Show tool details / Hide tool details”。

但这次本地源码里没有看到稳定的 slash 名称，并且：

- `tool_details` 默认 keybind 是 `none`
- 所以更稳入口是 `ctrl+p` command palette
- 或者你自己在 `tui.json` 里配一个 keybind

适合：

- 你想看清当前到底调了哪些工具
- 当前工具链比较复杂，需要先做观察

## `/thinking`

它只控制 thinking/reasoning blocks 的显示，不控制模型是否真的进入 reasoning 模式。

适合：

- 你想看 reasoning blocks
- 或者反过来，想把它们隐藏掉，减少界面噪音

不要把它写成“切换推理模型”的命令。

当前本地 CLI/TUI 还确认了 alias：

- `/toggle-thinking`

## `/status`

当前 local CLI/TUI 明确注册了 `/status`。

适合：

- 看当前系统状态
- 先确认 provider / model / TUI 状态，再决定要不要继续深挖

## `/themes`

这次本地 CLI/TUI 注册点里，主题命令名已经能稳定写成：

- `/themes`
- 默认 keybind：`<leader>t`

所以这版不再需要把 `/themes` / `/theme` 写成纯不确定状态。

## `/editor`

这次本地 CLI/TUI 也能确认：

- `/editor` 是真实注册过的 built-in
- 默认 keybind：`editor_open`
- 默认 leader 组合是 `<leader>e`

它适合：

- 先在外部编辑器里写长 prompt
- 再把整理过的内容送回当前会话

## `tui.json` vs `opencode.json`

官方 docs 当前明确写到：

- `tui.json` / `tui.jsonc` 负责 TUI behavior
- 这和 `opencode.json` 的 server/runtime behavior 是分开的

所以：

- slash/keybind/theme/scroll/mouse 这一层，先看 `tui.json`
- model/agent/permission/instructions 这一层，先看 `opencode.json`

## 为什么这些动作值得单独成章

因为它们的共同点是：

- 服务的是会话控制、观察或 TUI 自身
- 不是在要求 OpenCode “完成一个编码任务”

如果把这些动作都写成自然语言提示，会带来两个问题：

- 主任务上下文被无谓污染
- 会话控制动作和真正的实现意图混在一起

## 这章最容易写错的地方

- 不要把 `/thinking` 当成模型切换。
- 不要把 command palette `ctrl+p`、`/help` 和某一个 slash 名字混成同一层。
- 不要把 docs 提到过的 `/details` 直接写成本机这版 CLI/TUI 已确认的稳定 slash。
- 不要把“看状态、调显示、查命令”这类动作混进主提示词。
