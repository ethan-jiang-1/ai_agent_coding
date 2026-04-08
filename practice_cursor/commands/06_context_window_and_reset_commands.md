# 06 长上下文、重开与 Max Mode

## Cursor 没有这章的官方 `/compact`

所以这章的核心动作不是“压缩历史”，而是：

- 稳定规则层
- 及时外化任务状态
- 在正确时机新开 tab

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
