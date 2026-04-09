# 2026-04-09 opencode__external_runner_durable_automation_threshold deep dive

Run ID: `2026-04-09_additional_practices_r1`
Tool Candidate: `opencode__external_runner_durable_automation_threshold`

## 为什么深挖这个点

这个候选最容易被写成两个极端：

- “OpenCode 没 scheduler，所以没有 durable automation”
- “GitHub integration 已经证明 OpenCode 本身就是 scheduler 工具”

这两种都不准确。

## 深挖后的判断

当前更准确的分层是：

1. native long horizon
   - `serve`
   - `web`
   - `attach`
   - `run --attach`
   - 核心是 shared state continuation
2. external durable automation
   - GitHub Actions runner
   - issue / PR / comment trigger
   - `schedule`
   - `workflow_dispatch`
   - 核心是 outside-in workflow automation

所以这个候选真正成立的不是：

- OpenCode 内建了 scheduler

而是：

- 一旦任务进入定时 review、issue triage、PR automation，正确切层是进入外部 runner，而不是继续把 shared backend 当成后台作业系统

## 这次深挖没有改变什么

- 没有把 OpenCode 改写成 native background-job 工具
- 没有把 GitHub integration 写成所有团队都该用的默认路径

## 这次深挖改变了什么

- 把“absence of scheduler”收紧成：
  - native durable automation 缺失
  - external durable automation 正式存在
  - 值得并入跨工具 long-horizon 阈值比较
