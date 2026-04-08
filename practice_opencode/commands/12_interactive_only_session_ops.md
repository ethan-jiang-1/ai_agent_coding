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
- `<leader>h`：tips/help
- `<leader>t`：theme list

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

官方 docs 当前说明：

- `/help` 打开 help dialog
- `ctrl+x h` 或 `/help` 也被用作 command palette 入口

适合：

- 你忘了当前版本到底有哪些 built-in commands
- 你想先确认 slash 名称，再决定是不是要发自然语言提示

## `/details`

切换 tool execution details。

适合：

- 你想看清当前到底调了哪些工具
- 当前工具链比较复杂，需要先做观察

这类动作的重点不是“让模型产出内容”，而是“让你看清当前会话在做什么”。

## `/thinking`

它只控制 thinking/reasoning blocks 的显示，不控制模型是否真的进入 reasoning 模式。

适合：

- 你想看 reasoning blocks
- 或者反过来，想把它们隐藏掉，减少界面噪音

不要把它写成“切换推理模型”的命令。

## 主题选择命令的边界

这次官方 docs 里有一个值得明确记下的边界：

- TUI 页写的是 `/themes`
- Themes 页写的是 `/theme`

在没有本机实测前，不把某一个名字写死成绝对正确。

更稳的做法是：

- 真正执行时先用 `/help` 查你当前版本
- 或直接在 `tui.json` 里设 `theme`

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
- 不要在 docs 已经不一致时，把 `/themes` 或 `/theme` 写成绝对事实。
- 不要把“看状态、调显示、查命令”这类动作混进主提示词。
