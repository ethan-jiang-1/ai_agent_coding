# 09 跨会话状态传递：会话、memory、handoff 三层分工

## 核心习惯

Claude Code 的跨会话状态传递至少分三层：会话历史、auto-memory、handoff 文件。长任务不要只靠其中一层。

## 在 Claude Code 里怎么做

- 短期继续同一任务，用 `--continue` / `--resume`。
- 想保留历史但试另一方向，用 `--fork-session`。
- 项目级长期经验交给 auto-memory。
- 当前任务状态、下一步和风险点交给 handoff 文件。

一个实用判断：

- 同一任务继续做：会话层
- 项目通用经验：memory 层
- 当前阶段的交接：handoff 层

## 为什么 handoff 文件仍然重要

因为：

- auto-memory 不是完整对话历史
- 会话历史太长会继续污染上下文
- 别人接手时，明文 handoff 比隐藏在历史里更可靠

## 不要这样做

- 不要把 auto-memory 当任务快照。
- 不要把会话 resume 当成跨人交接方案。
- 不要让下一步只依赖“AI 应该记得刚才做过什么”。

## 验收标准

- 你能说清现在靠的是会话、memory 还是 handoff。
- 当前任务至少有一个明确的 handoff 或计划文件。
- 跨天或跨人工作时，不靠单一机制硬撑。

## 对应支撑

见 `../commands/09_memory_resume_handoff_commands.md`。
