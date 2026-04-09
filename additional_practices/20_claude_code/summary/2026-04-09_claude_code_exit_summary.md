# 2026-04-09 Claude Code Exit Summary

Run ID: `2026-04-09_additional_practices_r1`
Tool: `Claude Code`
Phase: `Phase 3. Tool Loop: Claude Code`

## 工具内结论

Claude Code 当前也不是“缺章节”，而是：

- 在 `2026-01-01` 到 `2026-04-09` 之间，官方持续强化了：
  - plan-oriented kickoff
  - built-in command control plane
  - first-party worktree isolation
  - session / loop / durable scheduling layering
  - rules / memory / handoff 的分层组织
  - subagents 与更重型 orchestration 的边界

这些变化确实带来了行为级信号，但当前更像：

- 强化既有章节
- 把部分已经存在于 `commands/` 与 `reference/` 的事实，抬升成更明确的 practice 叙述
- 尤其强化 `01`、`03`、`09`、`10`、`12`、`13`

而不是明确要求新增一个全新正式 practice 章节。

## 活跃候选

本工具最终保留 `6` 个活跃候选：

1. `claude_code__plan_first_kickoff`
2. `claude_code__command_surface_control_plane`
3. `claude_code__first_party_worktree_isolation`
4. `claude_code__long_horizon_layering`
5. `claude_code__agent_teams_threshold`
6. `claude_code__memory_hierarchy_and_handoff`

## 当前最强的跨工具候选

- `claude_code__plan_first_kickoff`
  - 进入跨工具候选池
- `claude_code__command_surface_control_plane`
  - 进入跨工具候选池
- `claude_code__first_party_worktree_isolation`
  - 进入跨工具候选池
- `claude_code__long_horizon_layering`
  - 进入跨工具候选池
- `claude_code__memory_hierarchy_and_handoff`
  - 进入跨工具候选池，但优先级略低于前四项

## 当前更像 Claude-specific 强化的点

- `claude_code__agent_teams_threshold`
  - 当前更像 `13` 高级 agentic 协调章节里的 Claude-specific 边界
  - 保留，但不强行假设其他工具会等价成立

## 当前明确不单独升格的方向

- 把单个 built-in command 本身当成正式 practice
  - 不成立；重点是 control-plane fluency，不是命令清单
- 把 agent teams 写成默认并行入口
  - 不成立；正确顺序仍是 subagents / worktree 优先
- 把 `--resume`、`/loop`、`/schedule`、GitHub Actions 全都混成同一种“继续跑”
  - 不成立；正确问题是 long-horizon 分层

## Tool Exit Questions

### 1. 当前工具最值得进入跨工具比较的是什么

- 先计划、再批注、后执行的开局契约
- built-in command 作为控制平面
- first-party worktree isolation
- session continuation / loop / durable automation 的分层
- memory、rules、handoff 的分层上下文打包

### 2. 当前工具只在本工具特别强的是什么

- `--worktree` 作为第一方 CLI 能力
- agent teams 的 experimental but real heavy-orchestration surface

### 3. 当前工具有哪些点证据还不够

- durable scheduling 的团队级公开复盘仍少
- agent teams 的 adoption 仍弱于 plan-first 和 worktree
- command-surface fluency 的跨工具可迁移强度，还要等 Cursor / OpenCode 对比

### 4. 当前工具有哪些点应先 hold 或谨慎处理

- `claude_code__agent_teams_threshold`
  - 保留但谨慎，先作为 Claude-specific 边界说明
- `claude_code__memory_hierarchy_and_handoff`
  - 保留，但更像 merge 强化，不预设一定 promote

### 5. 当前工具有哪些点明确 reject

- 把 built-in command 数量直接等同于 practice 数量
- 把 teams-first 写成推荐默认工作法
- 把所有长任务都推向最重的 durable automation

## 下一步

- 进入 `Phase 4. Tool Loop: Cursor`
- 先做 `Step 2A. Tool Baseline`
- 对 Cursor 重点对比：
  - Ask / Agent / Custom Modes 的开局控制
  - command surface 与 custom commands 的真实控制面
  - tabs / branch / Background Agents 的隔离方式
  - history / memories / handoff 的分层
