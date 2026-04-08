# 09 跨会话状态传递：session、export、share、handoff 分层用

## 核心习惯

在 OpenCode 里，跨会话状态传递至少有四层：当前 session、可导出的 session 数据、公开 share link、你自己写的 handoff 文件。不要把它们混成一种东西。

## 在 OpenCode 里怎么做

- 短期继续同一任务，用 session 层。
- 跨天、跨机或要备份状态，用 export / import。
- 需要和别人共享会话时，才考虑 share。
- 当前阶段的真正交接信息，仍然写 handoff 文件。

一个实用判断：

- 还是你自己继续做：session
- 要搬到另一台环境或留底：export / import
- 要给别人看这段会话：share
- 要让人高质量接手：handoff

## 为什么 handoff 仍然重要

因为：

- session ID 更适合同一使用者继续
- share link 是公开链接，不是私有交接通道
- export 是数据搬运，不等于别人能快速理解当前阶段

## 不要这样做

- 不要把 share link 当成默认交接方式。
- 不要把公开分享当成内部安全协作。
- 不要让“下一步做什么”只留在会话历史里。

## 验收标准

- 你能说清现在依赖的是 session、export、share 还是 handoff。
- 当前任务至少有一份清楚的 handoff 或计划文件。
- 跨人或跨天工作时，不靠单一机制硬撑。

## 对应支撑

见 `../commands/09_sessions_share_handoff_commands.md`。
