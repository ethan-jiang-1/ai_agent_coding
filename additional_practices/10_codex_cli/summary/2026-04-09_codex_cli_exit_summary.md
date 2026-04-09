# 2026-04-09 Codex CLI Exit Summary

Run ID: `2026-04-09_additional_practices_r1`
Tool: `Codex CLI`
Phase: `Phase 2. Tool Loop: Codex CLI`

## 工具内结论

Codex CLI 当前不是“缺高级能力”，而是：

- 在 `2026-01-01` 到 `2026-04-09` 之间，官方 release 密集推进了：
  - plan mode default
  - steer mode stable default
  - cloud tasks CLI 化
  - subagent orchestration 强化
  - permission profiles / remembered approvals
  - skills / memory / debug-config 等新控制面

这些变化确实带来了新的行为级信号，但当前更像：

- 强化旧章节
- 提升第 `12` / `13` 章附近内容的实践密度
- 把部分原来更像 command/reference 的内容推高到更明确的 practice 叙述

而不是明确要求新增一个全新正式 practice 章节。

## 活跃候选

本工具最终保留 `6` 个活跃候选：

1. `codex_cli__session_kickoff_contract`
2. `codex_cli__command_surface_fluency`
3. `codex_cli__cloud_detach_threshold`
4. `codex_cli__shallow_delegation_with_guardrails`
5. `codex_cli__skills_over_agents_bloat`
6. `codex_cli__approval_profiles_and_remembered_grants`

## 当前最强的跨工具候选

- `codex_cli__session_kickoff_contract`
  - 进入跨工具候选池
- `codex_cli__command_surface_fluency`
  - 进入跨工具候选池
- `codex_cli__cloud_detach_threshold`
  - 进入跨工具候选池
- `codex_cli__shallow_delegation_with_guardrails`
  - 进入跨工具候选池
- `codex_cli__skills_over_agents_bloat`
  - 保留进入跨工具候选池，但优先级低于前四项

## 当前更像 Codex-specific 强化的点

- `codex_cli__approval_profiles_and_remembered_grants`
  - 当前更像 `08` 权限章节的 Codex-specific 强化
  - 先保留，不强行假设其他工具会等价成立

## 当前明确不单独升格的方向

- `/copy`、`/clear`、`/statusline`、`/apps` 这类单点 convenience 命令
  - 重要，但单独看更像 command-surface 补充，不是 practice
- `spawn_agents_on_csv`
  - 当前仍更像实验性能力，不应直接写成默认最佳实践
- “所有长任务都该切到 cloud”
  - 不成立；正确问题是阈值判断，不是单一偏好

## Tool Exit Questions

### 1. 当前工具最值得进入跨工具比较的是什么

- 开局契约
- 命令面流利度
- 本地 loop 与 detached loop 的阈值
- 浅层 delegation
- 薄 AGENTS / 重 skills 的上下文打包方式

### 2. 当前工具只在本工具特别强的是什么

- remembered approvals / permission profiles
- `codex cloud` 与 `codex apply` 的闭环

### 3. 当前工具有哪些点证据还不够

- skills 真正的大规模 adoption 仍不够强
- 复杂 subagent 图的公开成功工作流仍少
- cloud task 的团队级长期复盘案例仍有限

### 4. 当前工具有哪些点应先 hold 或谨慎处理

- `skills_over_agents_bloat`
  - 保留但谨慎，不预设它会直接 promote
- `approval_profiles_and_remembered_grants`
  - 先保留为 agent-specific 候选

### 5. 当前工具有哪些点明确 reject

- 把单个 convenience slash command 本身当成正式 practice
- 把 experimental fan-out 视为当前默认稳定工作法

## 下一步

- 进入 `Phase 3. Tool Loop: Claude Code`
- 先做 `Step 2A. Tool Baseline`
- 对 Claude Code 重点对比：
  - session kickoff
  - slash command control plane
  - permission/workflow coupling
  - subagents/worktree/schedule
