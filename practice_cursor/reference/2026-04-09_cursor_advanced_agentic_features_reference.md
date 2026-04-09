# Cursor Advanced Agentic Features Reference

核验日期：2026-04-09

## 来源

- Cursor Docs: `Background Agents`
- Cursor Docs: `Background Agents API`
- Cursor Docs: `Web & Mobile`
- Cursor Docs: `Slack`
- Cursor Docs: `GitHub`
- Cursor Docs: `Linear`
- 已有本地参考：`overall/reference_r1/2026-04-07_cursor_official.md`

说明：

- `docs.cursor.com` 在当前环境里直接抓正文经常命中 Vercel Security Checkpoint
- 本次主要依赖 Cursor 官方搜索摘要与官方页面可见片段
- 适合确认能力边界，不适合写死所有 UI 按钮细节

## 1. 本次只确认三条能力线

- `background agent / remote agent orchestration`
- `environment handoff / workspace isolation / branch isolation`
- `long horizon / async / cross-surface follow-up`

原则：

- 有官方证据的，写成正式能力
- 新旧叙事冲突时，以更新的官方环境模型为准
- 不把不同执行环境混写成同一种 worker

## 2. Cursor 的“多 agent”主叙事是 Background Agents

官方 `Background Agents` 页面当前明确写了：

- Background Agents 是 asynchronous remote agents
- 运行在 isolated ubuntu-based machine
- 有 internet access
- 可以 install packages
- 会自动运行 terminal commands
- 通过 GitHub clone repo
- 在 separate branch 上工作，并把改动 push 回 repo
- 可以 view status、send follow-ups、take over anytime

这说明对 Cursor 来说，最接近 subagent / autonomous worker 的正式能力，不是本地聊天多开，而是：

- 远程 agent 环境
- 独立 branch
- 可异步跟进

## 3. Cursor 3 当前已形成多环境 handoff 叙事

对 Background Agent 本身，本次仍能稳定确认的是：

- isolated remote machine
- separate branch
- remote repo clone

但在 `2026-04-02` 之后的 Cursor 3 Agents Window 叙事里，官方已经把这些环境放进同一个 agent workspace：

- local
- worktrees
- cloud
- remote SSH

因此更准确的写法应该是：

- Background Agent 本身是 remote branch worker
- Cursor 3 整体已经不适合再写成“没有 worktree / 多环境 story”
- 但这也不等于所有任务都该默认切到 worktree 或 cloud

如果真的需要更细的 filesystem 级并行隔离，仍要看当前入口到底落在 Cursor 的哪一层环境里，不要把 remote branch、worktree、cloud、remote SSH 混成一回事。

## 4. Background Agents 的权限与安全边界，比前台更强也更危险

官方当前明确写了：

- foreground agent 需要用户审批每条 terminal command
- background agent 会 auto-run terminal commands
- 这带来 prompt injection / exfiltration 风险
- 运行在 isolated VMs
- Privacy Mode 可用
- 如果启动时关闭 Privacy Mode，中途再打开，当前 run 仍会保持 disabled 直到完成

因此：

- Background Agent 不是“前台 Agent 换个位置跑”
- 它的权限模型和风险级别要单独写

## 5. 成本与模型边界

官方当前还能确认：

- Background Agents 用 usage-based pricing
- 首次使用前要设 spending limit
- 只有 Max Mode-compatible models 可用于 Background Agents
- Web & Mobile agent 使用同样的 pricing model

因此：

- 后台 agent 不是“放后台就不算成本”
- 它通常比前台试探式 Ask/Agent 更像真正的消耗入口

## 6. Long horizon：Cursor 的关键不只是后台跑，还包括跨入口延续

官方当前能确认至少四层长程能力：

### A. 编辑器内 Background Agent

- 从 native sidebar 查看、搜索、启动
- `Ctrl+E` 可触发 background agent mode
- run 完后可回 IDE 接管

### B. Web & Mobile

官方当前明确写了：

- 可从 `cursor.com/agents` 在 web / mobile 启动 agent
- 完成后可 `Open in Cursor`
- 可分享链接给团队协作
- 可直接从 web 界面创建、审查、合并 PR

### C. Slack / GitHub / Linear

官方当前明确写了：

- Slack：`@Cursor [prompt]` 启动 Background Agent
- Slack：支持 advanced options，如 `branch`、`model`、`repo`
- Slack：可 add follow-up，收 completion notifications 和 PR links
- GitHub：在 issue / PR 里 `@cursor [prompt]` 触发 agent
- Linear：可委派 issue 给 Cursor 或在评论里 `@Cursor`

### D. Background Agents API

官方当前明确写了：

- 可 programmatically create and manage background agents
- 可给运行中的 agents 添加 follow-up prompts
- usage-based pricing
- 支持最多 `256` 个 active agents per API key

这说明 Cursor 的 long horizon 主轴是：

- 同一个 remote agent 可持续 follow-up
- 并且可从 IDE、web、mobile、Slack、GitHub、Linear、API 多入口编排

## 7. 对 practice 写作的直接影响

### 对多智能体

- 这不是本地 subagent 叙事
- 更准确的写法是：tabs 做轻量本地分流，Background Agents 做远程异步分流
- 真正高级的编排层，应写到新增第 `13` 章

### 对 worktree / environment handoff

- 要明确写“Background Agent 本身不等于 local worktree”
- 也要明确写“Cursor 3 已经有 local / worktree / cloud / remote SSH 的多环境 handoff 叙事”
- 不要把 remote branch isolation 误说成本地 worktree
- 不要反过来把新环境模型抹掉成“只有 separate branch”

### 对 long horizon

- 不能只写 IDE sidebar
- 还要写 web / mobile handoff
- 还要写 Slack / GitHub / Linear 触发与 follow-up
- 还要写 API 批量编排能力

## 8. 当前最容易写错的地方

- 不要把 Background Agents 写成本地后台线程
- 不要把 separate branch 误写成 local worktree
- 不要忽略 background agent auto-run terminal 的额外风险
- 不要把 Web / Mobile / Slack / GitHub / Linear / API 这些入口漏掉，只写 IDE
