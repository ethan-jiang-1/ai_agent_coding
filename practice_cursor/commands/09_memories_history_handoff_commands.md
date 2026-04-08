# 09 Memories、history 与 handoff

## history

Cursor 有聊天历史和旧对话检索能力。

适合：

- 找回旧任务
- 引用旧结论
- 用 `@Past Chats` 补上下文

补充理解：

- 不是所有重要交互入口都有 slash 前缀
- history 检索、`@Past Chats` 和 export chat 也属于这章的真实 command surface

## Project Memories

Cursor 官方支持 Project Memories。

更稳的理解是：

- memories 负责项目级长期经验
- 新记忆不是无条件落库，通常需要你确认
- 不是完整聊天记录
- 也不是当前任务快照

## 导出聊天

Cursor 官方支持把聊天导出为 markdown。

这很适合：

- handoff
- 复盘
- 留档

如果你准备跨天、跨人或跨阶段继续，这通常比“只相信旧 tab 还在”稳得多。

## 本地历史存储

官方文档提到 chat 历史保存在本地 SQLite 数据库。

实践含义：

- 旧对话是可找回的
- 但你不应该把当前任务推进完全寄托在“反正历史里有”

## handoff 文件建议

对跨天、跨人或跨阶段任务，仍然建议另写 handoff 文件，而不是只靠 history 或 memories。

## 这章最容易写错的地方

- 不要把 memories 当成完整历史。
- 不要把旧 tab 当成跨人交接方案。
- 不要因为可以 export markdown，就不再把关键状态整理成 handoff 文件。
- 不要让下一步只依赖“Cursor 自己会记得”。
