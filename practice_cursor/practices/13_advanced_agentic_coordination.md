# 13 高级 Agentic 协调：先分前台、后台、多环境 handoff 和跨入口编排

## 核心习惯

Cursor 的高级 agentic 能力主轴不是本地 subagent，而是远程 agent 与多环境 workspace 的编排。

所以先分四层：

- 前台 tabs / Ask / Agent
- IDE 内 Background Agent
- local / worktree / cloud / remote SSH 的多环境 handoff
- web / mobile / Slack / GitHub / Linear 这些跨入口 agent surface
- API / Automations

## 在 Cursor 里怎么做

- 只是本地轻量分流，用 tabs / Ask / Agent。
- 只是把实现任务丢到后台跑，用 IDE 内 Background Agent。
- 需要在本地、worktree、cloud 或 remote SSH 之间切执行环境，用 Agents Window 做多环境 handoff。
- 需要跨设备接续、分享链接或 PR 交接，用 web / mobile。
- 需要从团队工作流里触发，用 Slack / GitHub / Linear。
- 需要定时或事件驱动，就转到 Automations；需要程序化控制，再用 API。

一个简单判断顺序：

1. 这是不是前台 tabs / Ask / Agent 就够了
2. 这是不是需要 IDE 内部的异步执行
3. 这是不是需要切到另一种执行环境
4. 这是不是需要从别的入口触发或做 durable automation

## Cursor 的特殊边界

- Cursor 2026 的正式叙事已经包含 local / worktree / cloud / remote SSH
- 但这不等于所有任务都该切出 IDE 或切到 worktree
- Background Agent 会 auto-run terminal commands，风险和前台不同
- web / mobile / Slack / GitHub / Linear / API / Automations 都会把“长程”放大成真正的流程编排问题

## 不要这样做

- 不要把 Background Agent 当成本地后台 tab。
- 不要把 Cursor 3 的多环境能力误写成“以后默认都该 worktree / cloud”。
- 不要把跨入口编排误当成“只是换个按钮点同一件事”。
- 不要在没算清 usage、并发、follow-up 和执行环境边界前就把后台规模拉大。

## 验收标准

- 你能清楚说出当前任务属于前台、IDE 后台、多环境 handoff、跨入口编排，还是 API / automation。
- 如果用了后台 agent，branch、repo、follow-up 入口是清楚的。
- 如果用了 API、Automations 或多入口 follow-up，你知道成本会被放大。
- 你没有把本地 rules / commands / prompt 和远程 orchestration 混成一层。

## 对应支撑

见 `../commands/13_advanced_agentic_coordination_commands.md`。
