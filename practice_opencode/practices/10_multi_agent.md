# 10 多智能体分流：探索、规划、执行要隔离

## 核心习惯

OpenCode 的多智能体价值不在于“多开几个 agent”，而在于让探索、规划、执行和审查使用不同的上下文与权限边界，并保持拆分有界。

## 在 OpenCode 里怎么做

- `build` 负责主执行。
- `plan` 优先负责分析和建议，默认把 edits / bash 收成 `ask`。
- `explore` 负责快速代码库探索。
- `general` 负责较复杂的并行研究或子任务。
- 重复工作流可以抽成 custom slash command，并指定 agent / subtask。
- 如果一个 custom command 不写 `agent`，它默认继承你当前 agent；如果你本来想让它走独立研究流，这会直接改变结果。
- 如果你要的是“不要污染主上下文”，就不要只写一个 command 名字；要考虑它是不是 subagent invocation，以及是否需要 `subtask: true`。
- 如果你要的是多客户端继续同一状态，或者更长程的协调，就不要还停在这章，要转到第 `13` 章。

这章在 OpenCode 里的关键不是 agent 数量，而是职责清楚：

- 谁读很多文件
- 谁产摘要
- 谁最终改代码

OpenCode 当前 local CLI/TUI 已经有内建 `/agents`，所以交互里的真实入口更接近：

- `/agents`
- `@explore`
- `@general`
- 你自己定义的 `/review-*`、`/plan-*` custom commands

更准确地说：

- `/agents` 适合看和切当前 agent
- `@explore`、`@general` 适合直接在 prompt 里调用 subagent
- custom commands 适合把固定研究流固化下来

这章更接近：

- 单会话里的角色拆分
- subtask 级上下文隔离
- 主线程收口

不是：

- 第一方 worktree
- detached background worker
- scheduler

## 什么时候不用多智能体

- 任务范围太小
- 只有一个文件要改
- 需求还没定清
- 你自己都说不清每个 agent 负责什么

## 不要这样做

- 不要让多个 agent 同时改同一块代码。
- 不要用 `general` 去做纯探索而忽略更轻的 `explore`。
- 不要以为 custom command 天然会走独立子任务。很多时候它只是沿用当前 agent。
- 不要在没有职责边界时就把任务拆给多个 agent。
- 不要把 `@explore`、`@general` 或 custom command 误写成后台常驻 worker。
- 不要把 subtask 当成 detached worker 的替代品。

## 验收标准

- 每个 agent 只有一个清楚职责。
- 探索、规划、执行的上下文是隔离的。
- 如果用了 subtask / subagent，它返回的是压缩过的结果，不是把整段噪音灌回主线程。
- 如果你需要的其实是 shared backend、多客户端接续或更长程协调，你知道应该切到第 `13` 章。

## 对应支撑

见 `../commands/10_agents_and_parallel_commands.md`。
