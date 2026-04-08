# 06 压缩历史与稳定前缀

## `/compact`

Claude Code 的 `/compact` 在实践上是这章最重要的操作。

它的关键行为是：

- 压缩对话历史
- 从磁盘重新读取 `CLAUDE.md` 等规则文件并重新注入

这意味着：

- 规则层不会因为 `/compact` 丢失
- 历史层会被压缩成摘要

## 稳定前缀的实践重点

- 让 `CLAUDE.md` 慢变
- 让 `.claude/rules/` 慢变
- 让任务状态落到计划文件和 handoff 文件
- 历史过长时，及时 `/compact`

## `--bare` 与这章的关系

```bash
claude --bare
```

适合：你要最小上下文，不想自动加载规则和 memory 时。

## 另一条关键 slash command：`/context`

- `Type D` `/context`：查看当前上下文窗口和占用情况
- 适合在决定“继续聊、`/compact`，还是直接重开”之前先看一眼

## 这章最容易写错的地方

- 不要把 `/compact` 当成计划文件的替代品。
- 不要频繁重写 `CLAUDE.md` 去承载短期任务。
- 不要让会话无限增长然后指望质量不变。
