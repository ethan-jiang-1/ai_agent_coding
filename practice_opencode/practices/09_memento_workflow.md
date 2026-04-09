# 09 跨会话状态传递：session、export、share、handoff 分层用

## 核心习惯

在 OpenCode 里，跨会话状态传递至少有五层：当前 session、共享 backend 状态、可导出的 session 数据、公开 share link、你自己写的 handoff 文件。不要把它们混成一种东西。

## 在 OpenCode 里怎么做

- 短期继续同一任务，用 session 层。
- 同一状态要在 terminal 和 web 之间来回切，用 `serve` / `web` / `attach` 这一层。
- 跨天、跨机或要备份状态，用 export / import。
- 需要和别人共享会话时，才考虑 `/share`，结束时记得 `/unshare`。
- 如果任务是在 GitHub runner 里自动跑出来的，就把它当成外部 automation 产物，而不是 shared backend 的自然续跑。
- 当前阶段的真正交接信息，仍然写 handoff 文件。
- 在 TUI 里要切历史会话，用 `/sessions`；要导出当前对话摘要或记录，用 `/export`。
- 如果是给人接手，优先 `/export` 这种 Markdown 交接材料；如果是搬运会话状态或做机器可导入的备份，优先 `opencode export` / `import`。

一个实用判断：

- 还是你自己继续做：session
- 还是同一套 backend 状态，但换客户端：serve / web / attach
- 还是外部 runner 的自动化结果：workflow / PR / logs
- 要搬到另一台环境或留底：export / import
- 要给别人看这段会话：share
- 要让人高质量接手：handoff

## 为什么 handoff 仍然重要

因为：

- session ID 更适合同一使用者继续
- shared backend 更适合同一状态续做，不等于别人一看就能接手
- 外部 automation 结果更不等于可读 handoff
- share link 是公开链接，不是私有交接通道
- JSON export 是数据搬运，不等于别人能快速理解当前阶段
- Markdown export 更适合人看，但不等于完整 session state

## 不要这样做

- 不要把 share link 当成默认交接方式。
- 不要把公开分享当成内部安全协作。
- 不要把 `serve` / `attach` 写成 scheduler 或 detached worker。
- 不要让“下一步做什么”只留在会话历史里。

## 验收标准

- 你能说清现在依赖的是 session、export、share 还是 handoff。
- 你能分清现在依赖的是 shared backend，还是导出/分享/交接。
- 当前任务至少有一份清楚的 handoff 或计划文件。
- 跨人或跨天工作时，不靠单一机制硬撑。

## 对应支撑

见 `../commands/09_sessions_share_handoff_commands.md`。
