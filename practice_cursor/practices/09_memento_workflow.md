# 09 跨会话状态传递：tab、memories、handoff 三层分工

## 核心习惯

Cursor 的跨会话状态传递至少分三层：chat history / past chats、Project Memories、handoff 文件。长任务不要只靠其中一层。

## 在 Cursor 里怎么做

- 短期继续同一任务，直接回原 tab 或通过历史找到旧对话。
- 需要复用旧对话片段时，用 `@Past Chats`。
- 项目级长期经验交给经你确认后写入的 Project Memories。
- 需要 handoff、留档或复盘时，直接把 chat 导出成 markdown，再整理成 handoff 文件。
- 当前任务状态、下一步和风险点交给 handoff 文件。

一个实用判断：

- 同一任务继续做：tab / history 层
- 项目通用经验：memories 层
- 当前阶段交接：handoff 层

## 为什么 handoff 文件仍然重要

因为：

- memories 不是完整对话历史
- 旧 tab 太长会继续污染上下文
- export chat 只是导出原记录，不自动替你整理当前阶段状态
- 别人接手时，明文 handoff 比隐藏在历史和 memories 里更可靠

## 不要这样做

- 不要把 memories 当任务快照。
- 不要把旧 tab 当跨人交接方案。
- 不要让下一步只依赖“Cursor 应该记得刚才做过什么”。

## 验收标准

- 你能说清现在靠的是 history、memories 还是 handoff。
- 当前任务至少有一个明确的 handoff 或计划文件。
- 跨天或跨人工作时，不靠单一机制硬撑。

## 对应支撑

见 `../commands/09_memories_history_handoff_commands.md`。
