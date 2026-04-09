# 2026-04-09 Cursor Exit Summary

Run ID: `2026-04-09_additional_practices_r1`
Tool: `Cursor`
Phase: `Phase 4. Tool Loop: Cursor`

## 工具内结论

Cursor 当前也不是“缺章节”，而是：

- 在 `2026-01-01` 到 `2026-04-09` 之间，官方把产品主叙事继续推向了：
  - Ask / Agent / Custom Modes 的计划控制
  - quick commands / custom commands 的可扩展控制层
  - Background Agents 与 cloud agents 的长程执行
  - automations 的 schedule / event-driven workflow
  - Cursor 3 Agents Window 下的 local / worktree / cloud / remote SSH 多环境 handoff
  - history / memories / background chat / handoff 的分层

这些变化确实带来了新的行为级信号，但当前更像：

- 强化既有章节
- 修正部分已经开始过时的旧基线边界
- 把原本偏 editor 内的叙述，升级为多环境、多入口、长程工作流叙述

而不是明确要求新增一个全新正式 practice 章节。

## 活跃候选

本工具最终保留 `6` 个活跃候选：

1. `cursor__plan_first_kickoff`
2. `cursor__command_surface_control_plane`
3. `cursor__local_cloud_worktree_handoff_threshold`
4. `cursor__long_horizon_cross_surface_layering`
5. `cursor__memory_hierarchy_and_handoff`
6. `cursor__automations_workflow_control_plane`

## 当前最强的跨工具候选

- `cursor__plan_first_kickoff`
  - 进入跨工具候选池
- `cursor__command_surface_control_plane`
  - 进入跨工具候选池
- `cursor__local_cloud_worktree_handoff_threshold`
  - 进入跨工具候选池，但当前仍属早期能力边界
- `cursor__long_horizon_cross_surface_layering`
  - 进入跨工具候选池
- `cursor__memory_hierarchy_and_handoff`
  - 进入跨工具候选池

## 当前更像 Cursor-specific 强化的点

- `cursor__automations_workflow_control_plane`
  - 当前更像 Cursor 在 `13` 章上的放大器
  - 先保留，不预设其他工具会以同样形态成立

## 当前明确不单独升格的方向

- 把 quick commands 或 custom commands 的具体名称本身当成正式 practice
  - 不成立；重点是控制面分层，不是命令清单
- 把 Cursor 3 的 worktree / cloud surface 写成所有任务默认入口
  - 不成立；正确问题是环境切层阈值
- 把 automations 写成所有团队默认都该开的 always-on agent
  - 不成立；正确问题是 durable automation 的适用场景

## Tool Exit Questions

### 1. 当前工具最值得进入跨工具比较的是什么

- Ask / plan approval / execution 的开局契约
- commands + modes 的柔性控制面
- local / worktree / cloud / remote SSH 的环境切层
- IDE、web/mobile、chatops、API 的长程分层
- history、memories、background chat、handoff 的分层

### 2. 当前工具只在本工具特别强的是什么

- Cursor 3 Agents Window 下的多环境 agent workspace
- automations 的 schedule / event-driven control plane

### 3. 当前工具有哪些点证据还不够

- worktree / remote SSH 的外部 workflow 复盘仍少
- automations 的 adoption 仍偏官方示例
- commands / modes 的行为变化强度，还要和 Claude / Codex / OpenCode 对比

### 4. 当前工具有哪些点应先 hold 或谨慎处理

- `cursor__local_cloud_worktree_handoff_threshold`
  - 保留但谨慎，先视为新能力带来的边界校正
- `cursor__automations_workflow_control_plane`
  - 保留，但倾向并回 `long_horizon_cross_surface_layering`

### 5. 当前工具有哪些点明确 reject

- 把 Cursor command 名称表直接等同于 practice 列表
- 把新的多环境 surface 写成 worktree-first 默认工作法
- 把 automations 视为“反正能自动化就应该全开”

## 下一步

- 进入 `Phase 5. Tool Loop: OpenCode`
- 先做 `Step 2A. Tool Baseline`
- 对 OpenCode 重点对比：
  - session kickoff 与计划控制
  - slash / command / share / backend 的控制面
  - 本地与共享后端的长程阈值
  - memory / handoff / context packaging
