# 13 高级 Agentic 协调：先分 subagents、worktree、agent teams、schedule

## 核心习惯

Claude Code 的高级 agentic 能力不是一个开关，而是四层：

- subagents
- worktree
- agent teams
- scheduled automation

先选最轻、最窄、最可控的一层，而不是一上来就把所有自动化能力全开。

更稳的升级顺序通常是：

- 先 subagents
- 再 worktree
- 再看是否真的需要 agent teams
- 最后才进入 durable scheduling

## 在 Claude Code 里怎么做

- 只是角色拆分，用 subagents。
- 写入面需要隔离，用 `--worktree` 或 subagent `isolation: worktree`。
- worker 之间需要彼此直接通信，或需要 lead 显式编排多个 teammate，用 agent teams。
- 只是当前 session 内短期轮询，用 `/loop`。
- 需要跨重启、跨离线、跨 CI 持续运行，用 Cloud / Desktop / GitHub Actions 这类 durable scheduling。

一个简单判断顺序：

1. 这是不是单会话里就能完成的角色拆分
2. 这是不是文件冲突问题
3. 这是不是需要独立 session 之间直接通信
4. 这是不是要跨当前会话继续存在

## Claude Code 的特殊边界

- `--worktree` 是正式 CLI 能力
- subagents 和 worktree 都是稳定主路径
- agent teams 是官方文档能力，但仍是 experimental
- `/loop` 是 session-scoped，不是 durable scheduling
- GitHub Actions 文档里的 `--max-turns` 不能直接等同于本机 help 已稳定暴露
- agent teams 的真正门槛是 worker 之间要彼此直接通信，而不是“任务比较大”

## 不要这样做

- 不要把 subagents 和 agent teams 写成同一种并行。
- 不要把 `/loop` 当成长期计划任务系统。
- 不要忽略 agent teams 的更高 token 成本。
- 不要在没有 timeout、turns、recurrence 护栏时直接放长任务自己跑。
- 不要在 subagents 或 worktree 已够用时，过度升级成 agent teams。

## 验收标准

- 你能清楚说出这次用的是哪一层协调能力。
- 如果有并行写入，工作区边界是清楚的。
- 如果是长程自动化，持久性边界是清楚的。
- 如果用了 agent teams，你知道它是 experimental，而且职责和成本都可解释。

## 对应支撑

见 `../commands/13_advanced_agentic_coordination_commands.md`。
