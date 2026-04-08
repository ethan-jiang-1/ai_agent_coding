# 06 长上下文、重开与 Max Mode

## Cursor 没有这章的官方 `/compact`

所以这章的核心动作不是“压缩历史”，而是：

- 稳定规则层
- 及时外化任务状态
- 在正确时机新开 tab

## 最接近会话内重置的 quick command

Cursor 官方 `Commands` 页可确认：

- `/Reset Context`

它适合：

- 当前推理链已经被污染
- 你想在同一个交互面里先清一下上下文

但更稳的边界仍然是：

- 任务边界变了，直接新开 tab
- 阶段状态需要保留，就先落文件

## lint 自动迭代的会话开关

官方还给出了：

- `/Disable Iterate on Lints`

它适合：

- agent 在 lint 修复循环里浪费太多轮次
- 你想先停掉这类自动迭代，再手工判断

这也是 Cursor 的交互式控制层，但它不是“上下文压缩”能力。

## 正常上下文 vs Max Mode

Cursor 官方文档强调：

- 正常模式已经有较大的上下文窗口
- `Max Mode` 提供更大的上下文，但更慢也更贵

这意味着：

- 上下文不够时，可以考虑 Max Mode
- 但 Max Mode 不是“无限续旧 tab”的理由

## 何时直接重开 tab

- 历史已经明显污染判断
- 任务目标变了
- 你已经有计划文件或 handoff 文件
- 旧 tab 里混了多个失败方案

## 稳定前缀的实践重点

- 让 `.cursor/rules/*.mdc` 慢变
- 让 `AGENTS.md` 慢变
- 让任务状态落到计划文件和 handoff 文件
- 用新 tab 替代硬续巨长历史

## 这章最容易写错的地方

- 不要把 Max Mode 当默认。
- 不要把 rules / memories 当作“可以无限叠历史”的理由。
- 不要把 Cursor 写成有和 Claude `/compact` 对等的官方操作。
- 不要把 `/Reset Context` 写成“保留全部历史语义、同时自动压缩”的操作。
