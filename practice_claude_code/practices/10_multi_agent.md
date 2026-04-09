# 10 多智能体分流：探索、实现、审查要隔离

## 核心习惯

Claude Code 的多智能体工作流重点不是“多开几个代理”，而是让探索、实现和审查拥有彼此隔离的上下文。

## 在 Claude Code 里怎么做

- 用 subagents 做读多文件、产摘要的探索任务。
- 用主会话做最后的整合和决策。
- 用 worktree 做隔离实施或独立审查。
- 如果已经在交互里，要看或切 agents 入口，用 `/agents`。
- 如果 worker 之间需要彼此直接通信，或者你要显式编排一个 lead 加多个 teammate，那已经不是这章的普通 subagents，而是第 `13` 章的 agent teams。

Claude Code 在这章的优势是：

- subagents 有独立 context window
- 可以配置不同工具权限
- worktree 能把文件系统工作区也隔开
- worktree 还是官方一等 CLI 能力，不只是手工 Git 技巧

## 什么时候不用多智能体

- 任务范围太小
- 只有一个文件要改
- 需求还没定清
- 你自己都说不清每个代理该干什么

## 不要这样做

- 不要让多个代理都写同一块代码。
- 不要把 `--fork-session` 当成无偏见审查者。
- 不要不给职责边界就开多个 subagent。
- 不要在只是普通 subagent 场景时，过度升级成更贵的 agent teams。

## 验收标准

- 每个代理只有一个清楚职责。
- 探索、实现、审查三种角色边界清楚。
- 如果用了 worktree，不同工作区的目的也是清楚的。

## 对应支撑

见 `../commands/10_subagents_and_worktree_commands.md`。
