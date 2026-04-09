# 13 高级 Agentic 协调命令

Cursor 到这一步要分开五层：

- 前台 tabs / Ask / Agent
- IDE 内 Background Agents
- local / worktree / cloud / remote SSH 的多环境 handoff
- web / mobile / Slack / GitHub / Linear 这些跨入口 agent surface
- Background Agents API / Automations

它们都能“让 agent 跑起来”，但控制面、持久性和协作方式并不一样。

## 1. IDE 内 Background Agent

官方当前明确支持两条 IDE 入口：

- Background Agent sidebar
- `Ctrl+E` 触发 background agent mode

提交后你可以：

- 看 status
- 发 follow-up
- 进入机器接管

这适合：

- 你还在 IDE 主工作流里
- 只是想把执行放到后台
- 后续主要还是自己接回来处理

## 2. 多环境 handoff：Cursor 3 Agents Window

Cursor 2026 新界面已经把这些执行面放进同一个 agent workspace：

- local
- worktrees
- cloud
- remote SSH

这适合：

- 需要在不同执行环境之间切换
- 本地与云端来回接力
- 多 workspace 并行观察和接管

但要注意：

- 这不等于所有任务都该默认切到 worktree 或 cloud

## 3. 跨设备 handoff：Web & Mobile

官方当前明确写了：

- 可在 `cursor.com/agents` 启动 agent
- 完成后可 `Open in Cursor`
- 可分享链接给团队协作
- 可直接在 web 界面创建、审查、合并 PR

这适合：

- 你不在桌面 IDE 前
- 需要从手机或浏览器发起或跟进
- 需要“先远程跑，后回 IDE 接手”

## 4. 外部工作流入口：Slack / GitHub / Linear

### Slack

官方当前明确写了：

- `@Cursor [prompt]` 启动 Background Agent
- advanced options 支持 `branch`、`model`、`repo`
- 线程里可 add follow-up
- 完成后可收 notifications 和 PR links

### GitHub

官方当前明确写了：

- 在 issue / PR 里 `@cursor [prompt]` 可触发 agent
- GitHub app 负责 clone repo、create working branches、push changes、create PR、report checks

### Linear

官方当前明确写了：

- 可直接把 issue 委派给 Cursor
- 也可在评论里 `@Cursor`

这三层的本质是：

- 从团队工作流系统里直接派 remote branch worker

## 5. API 与 Automations

官方 Background Agents API 当前明确写了：

- 可 programmatically create and manage agents
- 可向运行中的 agent 发送 follow-up prompts
- 支持最多 256 个 active agents / API key

Automations 则更适合：

- schedule
- event trigger
- recurring review / triage

这说明：

- API 更适合程序化控制
- Automations 更适合 durable workflow

## 6. Cursor-native 的切层重点

所以如果你要选协调层，先这样判断：

- 只需要轻量上下文隔离：tabs
- 需要远程异步实现：Background Agent
- 需要换执行环境：Agents Window 的 local/worktree/cloud/remote SSH
- 需要从别的入口触发同一类 worker：web/mobile/Slack/GitHub/Linear
- 需要批量编排：API
- 需要 schedule / event-driven：Automations

## 7. 一张快速决策表

- 本地短平快探索：Ask / tab
- 本地受控执行：Agent
- IDE 内异步实现：Background Agent
- 多环境切层：Agents Window
- 跨设备接续：Web / Mobile
- 团队聊天触发：Slack
- 仓库事件触发：GitHub / Linear
- 大规模程序化调度：API
- 定时 / 事件驱动长期流程：Automations

## 这章最容易写错的地方

- 不要把 Background Agent 写成本地后台线程。
- 不要把多环境 handoff 写成“以后默认都该切 worktree / cloud”。
- 不要只写 IDE，而漏掉 web / mobile / Slack / GitHub / Linear / API / Automations 这些真正的长程入口。
- 不要低估 API、Automations、follow-up 带来的成本放大。
