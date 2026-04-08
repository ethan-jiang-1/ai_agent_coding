# 10 多智能体分流：探索、规划、执行要隔离

## 核心习惯

OpenCode 的多智能体价值不在于“多开几个 agent”，而在于让探索、规划、执行和审查使用不同的上下文与权限边界。

## 在 OpenCode 里怎么做

- `build` 负责主执行。
- `plan` 优先负责分析和建议，默认把 edits / bash 收成 `ask`。
- `explore` 负责快速代码库探索。
- `general` 负责较复杂的并行研究或子任务。
- 重复工作流可以抽成 custom command，并指定 agent / subtask。

这章在 OpenCode 里的关键不是 agent 数量，而是职责清楚：

- 谁读很多文件
- 谁产摘要
- 谁最终改代码

## 什么时候不用多智能体

- 任务范围太小
- 只有一个文件要改
- 需求还没定清
- 你自己都说不清每个 agent 负责什么

## 不要这样做

- 不要让多个 agent 同时改同一块代码。
- 不要用 `general` 去做纯探索而忽略更轻的 `explore`。
- 不要在没有职责边界时就把任务拆给多个 agent。

## 验收标准

- 每个 agent 只有一个清楚职责。
- 探索、规划、执行的上下文是隔离的。
- 如果用了 subtask / subagent，它返回的是压缩过的结果，不是把整段噪音灌回主线程。

## 对应支撑

见 `../commands/10_agents_and_parallel_commands.md`。
