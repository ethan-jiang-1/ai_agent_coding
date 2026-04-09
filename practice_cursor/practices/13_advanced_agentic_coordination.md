# 13 高级 Agentic 协调：先分 IDE 后台、跨入口编排和 API 编排

## 核心习惯

Cursor 的高级 agentic 能力主轴不是本地 subagent，而是远程 branch worker 的多入口编排。

所以先分三层：

- IDE 内 Background Agent
- web / mobile / Slack / GitHub / Linear 这些跨入口 agent surface
- Background Agents API

## 在 Cursor 里怎么做

- 只是本地轻量分流，用 tabs。
- 只是把实现任务丢到后台跑，用 IDE 内 Background Agent。
- 需要跨设备接续、分享链接或 PR 交接，用 web / mobile。
- 需要从团队工作流里触发，用 Slack / GitHub / Linear。
- 需要批量化和程序化控制，用 API。

一个简单判断顺序：

1. 这是不是只需要 IDE 内部的异步执行
2. 这是不是需要从别的入口触发同一种后台 worker
3. 这是不是要规模化并发

## Cursor 的特殊边界

- Cursor 的原生隔离重点是 isolated remote machine + separate branch
- 不是本地 worktree orchestration
- Background Agent 会 auto-run terminal commands，风险和前台不同
- web / mobile / Slack / GitHub / Linear / API 都会把“长程”放大成真正的流程编排问题

## 不要这样做

- 不要把 Background Agent 当成本地后台 tab。
- 不要把 separate branch 误写成 local worktree。
- 不要把跨入口编排误当成“只是换个按钮点同一件事”。
- 不要在没算清 usage、并发和 follow-up 前就把后台规模拉大。

## 验收标准

- 你能清楚说出当前任务属于 IDE 后台、跨入口编排，还是 API 编排。
- 如果用了后台 agent，branch、repo、follow-up 入口是清楚的。
- 如果用了 API 或多入口 follow-up，你知道成本会被放大。
- 你没有把本地 rules / commands / prompt 和远程 orchestration 混成一层。

## 对应支撑

见 `../commands/13_advanced_agentic_coordination_commands.md`。
