# 10 Tabs 与 Background Agents

## Background Agents 的本质

Cursor 的 Background Agents 不是本地多开一个聊天窗口，而是远程异步 agent 环境。

官方文档能确认的关键点包括：

- 运行在隔离的 Ubuntu 环境
- 有 internet access
- 通过 GitHub 克隆仓库
- 在独立 branch 上工作
- 可以 install packages
- 可以 view status、send follow-ups、take over anytime

## 适合的任务

- 长时间实现任务
- 大范围代码探索
- 异步处理 issue / PR
- 和前台主 tab 分离的审查或实验

## 前台 tabs 的作用

前台多个 tabs 更适合：

- 轻量角色隔离
- 对比不同方案
- 保留主线与旁线分离

## 隔离重点先看执行环境，再看 branch

对 Background Agent 本身，当前 Cursor 官方材料稳定能确认的是：

- remote isolated machine
- separate branch
- repo clone / push back

但 Cursor 3 当前更完整的 agent workspace 叙事，已经包含：

- local
- worktrees
- cloud
- remote SSH

所以更稳的写法是：

- Background Agent 本身仍更接近 remote branch worker
- 如果你要切本地 / worktree / cloud / remote SSH，那是 Agents Window 的多环境 handoff 层
- 不要把 separate branch 误写成 local worktree
- 也不要反过来写成 Cursor 现在完全没有 worktree / 多环境叙事

## 更稳的组合

- Ask / 主 tab：做方向确认
- Agent：做前台受控执行
- Background Agent：做异步独立执行
- Agents Window：做 local / worktree / cloud / remote SSH 的环境切层

如果你的当前 Cursor 构建里仍然有 `Manual` 或其他窄编辑模式，再把它当成前台补充通道。

## 工作前的边界

- 明确仓库或 branch
- 明确任务边界
- 明确输出是 diff、PR，还是摘要
- 明确是留在 Background Agent，还是要切到 worktree / cloud / remote SSH
- 明确后续在哪里 follow-up：IDE、web/mobile、Slack，还是 API

## 前后台交接可以按需用 custom commands 固化

这次补查官方 `Commands` 页后，能确认至少两类很适合补进这一章的示例命令：

- `create-pr`
- `address-github-pr-comments`

它们不等于 Background Agents 的专用 slash command，但很适合承接：

- 后台 agent 已经完成一轮实现，前台准备整理成 PR
- PR 上已经出现反馈，前台需要围绕 comments 再做一轮返工

所以这一章更稳的写法不是“有个 `/background-agent` 命令”，而是：

- 背景 agent 负责异步执行
- 如果流程真的稳定复用，再用 custom commands 承接后续 PR / review / 返工动作

## 这章最容易写错的地方

- 不要把 Background Agents 写成本地后台线程。
- 不要让两个执行通道同时改同一组文件。
- 不要因为它在后台跑，就忽略安全和成本边界。
- 不要把 separate branch 误写成 local worktree。
- 不要把 Cursor 3 的多环境 handoff 抹掉成“只有 remote branch”。
