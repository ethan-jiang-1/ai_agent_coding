# 2026-04-09 claude_code__long_horizon_layering deep dive

Run ID: `2026-04-09_additional_practices_r1`
Tool Candidate: `claude_code__long_horizon_layering`

## 为什么深挖这个点

这个候选的主要风险不是能力不存在，而是层次被写塌。

- `--continue` / `--resume`
- `/loop`
- `/schedule`
- GitHub Actions / Cloud / Desktop durable automation

这些入口都和“长任务”有关，但它们不是一回事。

## 深挖后的判断

当前应把 Claude Code 的 long horizon 明确写成三层：

1. session continuation
   - `--continue`、`--resume`、`--fork-session`
   - 适合保持同一条本地会话脉络
2. session-scoped loop
   - `/loop`
   - 适合短周期轮询、当前会话内的自动重复动作
3. durable automation
   - `/schedule`
   - GitHub Actions with `claude_args`
   - Cloud / Desktop 的持续化入口
   - 适合跨时段、跨入口、需要 timeout / concurrency / ownership 的任务

当前 adoption signal 最强的是：

- long-horizon complex delivery 被官方 webinar 反复强调
- practitioner workflow 已经清楚区分本地推进、文档化 handoff、验证与部署阶段

相对更弱的是：

- 团队级 durable scheduling 的公开复盘仍少

## 这次深挖没有改变什么

- 没有把它升级成新章节
- 没有把 durable scheduling 写成所有复杂任务的默认终点

## 这次深挖改变了什么

- 把这个候选从“长任务相关功能很多”收紧成：
  - 一个有明确阈值分层的 merge 候选
  - 可优先并入 `09` / `13`
  - 能直接用于后续与 Codex Cloud、Cursor Background Agents、OpenCode share/backend 做跨工具比较
