# 13 高级 Agentic 协调命令

OpenCode 到这一步要分开五层：

- primary agent
- subagent / subtask
- shared backend server
- external runner automation
- share / export / import

这五层都和“任务如何继续”有关，但不是同一种能力。

## 1. 最轻的协调：primary + subagent

会话内入口：

```text
@explore
@general
```

配置层入口：

```bash
opencode --agent plan
opencode run --agent build "..."
opencode agent create
```

适合：

- 让研究任务在独立上下文完成
- 把结果压缩返回主线程
- 不需要多客户端共享同一状态

## 2. 强制某个 command 走 subtask

```md
---
description: Review recent changes
agent: plan
subtask: true
---
Review the current diff and report correctness, risk, and missing tests.
```

这适合：

- 固定重复性工作流
- 避免把 review 噪音灌回主上下文

## 3. 多客户端同状态：serve / web / attach

```bash
opencode serve
opencode web --port 4096
opencode attach http://localhost:4096
opencode run --attach http://localhost:4096 "继续已有 backend 的工作"
```

官方当前明确写了：

- TUI 本身就是 server client
- `serve` 可起 standalone backend
- `web` 可在浏览器里继续同一状态
- `attach` 可让 terminal 接回已有 backend

这适合：

- 同一会话跨 terminal / web 切换
- 避免 MCP cold boot
- 让不同客户端共享同一 sessions / state

## 4. 外部 durable automation

如果任务已经变成：

- scheduled review
- issue triage
- PR automation
- comment-triggered implementation

那更稳的入口不是继续依赖 `serve` / `attach`，而是进入外部 runner，例如 GitHub Actions。

这层的本质是：

- OpenCode 作为 runtime
- 外部 workflow system 提供 durable trigger

不是：

- OpenCode native scheduler

## 5. Share / export / import

```bash
opencode export <session-id>
opencode import session.json
opencode import https://opncd.ai/s/abc123
```

会话内：

```text
/share
/unshare
/export
```

这适合：

- 公开分享
- 跨机器搬运状态
- 人类 handoff

但这不是：

- filesystem 隔离
- detached background worker
- scheduler

## 6. 当前要明确写出的 absence

在这次 reviewed docs 范围里，没有稳定证据显示 OpenCode 当前有：

- 第一方 worktree 命令面
- 独立 background agent
- native scheduler / cron / long-running autonomous jobs

所以如果你真的需要这些：

- worktree：用外部 Git 技术
- detached background job：不要硬说 OpenCode 原生已有
- durable automation：转到外部 runner surface，不要硬塞进 shared backend

## 7. 一张快速决策表

- 只是主线程里切换分析/执行：primary agent
- 需要独立上下文研究：subagent / subtask
- 需要多个客户端共享同一 backend：serve / web / attach
- 需要定时或事件驱动的长期流程：external runner
- 需要跨机器搬运或公开分享：share / export / import

## 这章最容易写错的地方

- 不要把 `@explore` / `@general` 写成 detached worker。
- 不要把 `serve` / `web` / `attach` 写成 scheduler。
- 不要把 GitHub runner 上的 durable automation 回写成 OpenCode 原生 scheduler。
- 不要把 absence of worktree / background jobs 补成存在的功能。
- 不要把 share/export/import 混成同一种状态传递。
