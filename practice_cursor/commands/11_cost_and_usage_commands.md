# 11 成本、usage 与额度观察

## Cursor 里先分清三种消耗

- 前台 Ask / Agent 的日常使用
- Max Mode 带来的更高成本
- Background Agents 的额外消耗

## Ask 是更便宜的起点

如果方向还没看清，先 Ask，通常比直接进 Agent 或后台长跑更省。

## Max Mode 的成本含义

官方文档明确把 Max Mode 写成：

- 更大上下文
- 更慢
- 更贵

所以它应该是按需开启，而不是默认常驻。

## Background Agents 的成本含义

官方文档提到：

- Background Agents 使用 usage-based pricing
- 首次使用时会要求你设置 spending limit

这意味着它不适合拿来做“先试试看”的默认入口。

## usage 观察

官方文档支持：

- usage dashboard
- token / request breakdown
- spend threshold notifications

所以这章最实用的动作不是猜，而是定期看 dashboard。

## 这章为什么几乎没有 slash 映射

这次重新深挖官方 editor 文档后，没有确认到成本 / usage 相关的 built-in quick command。

因此这章的主要入口仍然是：

- usage dashboard
- spending limit
- 模式和模型的前置克制

如果团队总是重复做同一种预算提醒动作，可以自己做 custom command，但那属于自定义 workflow，不是当前官方内建 quick command。

## 这章最容易写错的地方

- 不要把 Background Agents 当成“看不见就不算成本”。
- 不要不看 usage dashboard。
- 不要在方向还没确认时就直接开 Max Mode 或后台长跑。
