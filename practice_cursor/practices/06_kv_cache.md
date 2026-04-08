# 06 稳定前缀：让规则稳定，让长对话可管理

## 核心习惯

对 Cursor 来说，最值得稳定的前缀仍然是规则层；长对话管理则要同时依赖自动 summarization、必要时的手动 `/summarize`、以及在确实偏航时重开 tab。

## 在 Cursor 里怎么做

- 让 `.cursor/rules/*.mdc` 和 `AGENTS.md` 慢变。
- 长任务不要死撑一个 tab，及时把阶段结果外化到计划文件或 handoff 文件。
- 长对话先利用 Cursor 的自动 summarization；真的需要时，再手动用 `/summarize`。
- 当前 tab 只是被杂音污染、但任务边界没变时，再考虑 `/Reset Context`。
- 当前 tab 已经变成历史包袱时，直接开新 tab。
- agent 如果在 lint 自动迭代里空转，可以用 `/Disable Iterate on Lints` 先停下来。
- 需要更大上下文时，用 Max Mode，但不要把它当“可以无限叠历史”的借口。

这章在 Cursor 里的重点不是 `/compact`，而是：

- 规则层稳定
- 长对话 summarization
- 任务状态外化
- tab 及时重开

## 不要这样做

- 不要把“有 rules 和 memories”理解成“长 tab 一定没问题”。
- 不要频繁重写 `.cursor/rules` 去承载短期任务。
- 不要把 `/summarize` 或自动 summarization 理解成“永远不用重开 tab”。
- 不要把 `/Reset Context` 理解成 Cursor 官方版 `/compact`。
- 不要把 Max Mode 当成所有上下文问题的默认解法。

## 验收标准

- 规则文件明显慢于任务文件变化。
- 当前任务状态已经外化到文件，而不是都挤在 tab 历史里。
- 你能分清现在该靠 summarization、`/Reset Context`，还是直接新开 tab。

## 对应支撑

见 `../commands/06_context_window_and_reset_commands.md`。
