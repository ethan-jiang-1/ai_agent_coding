# 12 会话内操作层：控制 TUI 时，用 slash commands 和 keybinds，不要污染主提示词

## 核心习惯

当你的目标不是“让 OpenCode 完成一个编码任务”，而是：

- 查看当前有哪些命令
- 观察工具执行细节
- 切换 reasoning blocks 的显示
- 调整 TUI 自己的状态

优先用 OpenCode 的 slash commands，而不是自然语言提示词。

还要补一个关键理解：

- OpenCode 的交互操作层不只 slash commands
- 官方 docs 当前明确写到，大多数命令也有 leader-key keybind
- 这层配置走 `tui.json`，不是 `opencode.json`

## 在 OpenCode 里怎么做

- 想查当前命令面，先 `/help`。
- 想看工具执行细节，先 `/details`。
- 想切 reasoning blocks 可见性，用 `/thinking`。
- 想选主题时，先在当前版本用 `/help` 核对命令名，或直接改 `tui.json`。
- 如果你更习惯键盘流，先记住默认 leader key 是 `ctrl+x`，很多会话控制动作都可以不经过 `/...` 输入。
- 如果问题已经变成“怎么跨客户端接同一 backend”或“怎么做更长程协调”，不要再停在这章，要转到第 `09` 章或第 `13` 章。

这条习惯的本质是：

- 产出型请求，走正常提示词
- 会话控制型请求，优先走 slash commands

## 为什么这很重要

因为很多旁路动作一旦混进主提示词，会带来两个问题：

- 主任务上下文被稀释
- 会话控制动作和实现意图混在一起，后续更难恢复

## 不要这样做

- 不要每次都用自然语言问“现在有哪些命令可用”。
- 不要把“打开细节”“切显示”“查主题”都写成普通任务提示词。
- 不要把 `/thinking` 误解成切换模型或切换推理强度。
- 不要把 `tui.json` 的交互层配置和 `opencode.json` 的 runtime/config 层混在一起。
- 不要把 `serve` / `attach` 这类 backend 协调问题误归到 interactive-only 小操作。

## 验收标准

- 你能分清什么时候该发提示词，什么时候该用 slash command。
- 旁路问题不会明显污染主任务上下文。
- 你知道 OpenCode 有一层真正的 interactive-only session ops。
- 你知道这层主要处理 TUI 控制，不处理后台编排。

## 对应支撑

见 `../commands/12_interactive_only_session_ops.md`。
