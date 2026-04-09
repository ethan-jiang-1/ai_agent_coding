# 2026-04-09 OpenCode Exit Summary

Run ID: `2026-04-09_additional_practices_r1`
Tool: `OpenCode`
Phase: `Phase 5. Tool Loop: OpenCode`

## 工具内结论

OpenCode 当前也不是“缺章节”，而是：

- 在 `2026-01-01` 到 `2026-04-09` 之间，官方把产品主叙事继续推向了：
  - `plan` primary agent 驱动的开局纪律
  - TUI built-ins + built-in commands + custom commands 的多层控制面
  - `serve` / `web` / `attach` 的 shared-backend long horizon
  - GitHub runner 上的外部 durable automation
  - share / export / import / handoff 的更清晰边界
  - skills 作为按需加载的上下文层

这些变化确实带来了新的行为级信号，但当前更像：

- 强化既有章节
- 修正“只有本地 TUI”和“没有 durable automation”这类过窄叙述
- 把 `03`、`04`、`09`、`10`、`12`、`13` 的实践表达拉到更准确的边界

而不是明确要求新增一个全新正式 practice 章节。

## 活跃候选

本工具最终保留 `7` 个活跃候选：

1. `opencode__plan_first_kickoff`
2. `opencode__command_surface_control_plane`
3. `opencode__shared_backend_long_horizon_layering`
4. `opencode__external_runner_durable_automation_threshold`
5. `opencode__memory_hierarchy_and_handoff`
6. `opencode__bounded_subtask_orchestration`
7. `opencode__skills_as_lazy_context_layer`

## 当前最强的跨工具候选

- `opencode__plan_first_kickoff`
  - 进入跨工具候选池
- `opencode__command_surface_control_plane`
  - 进入跨工具候选池
- `opencode__shared_backend_long_horizon_layering`
  - 进入跨工具候选池
- `opencode__external_runner_durable_automation_threshold`
  - 进入跨工具候选池，但更像 long-horizon 子层
- `opencode__memory_hierarchy_and_handoff`
  - 进入跨工具候选池
- `opencode__bounded_subtask_orchestration`
  - 进入跨工具候选池

## 当前更像 OpenCode-specific 强化的点

- `opencode__skills_as_lazy_context_layer`
  - 当前更像 `04` 章上的 OpenCode/Codex/Claude 共同增强方向
  - 保留，但不预设一定会在所有工具上同强度成立

## 当前明确不单独升格的方向

- 把 `serve` / `attach` 写成 native scheduler 或 detached worker
  - 不成立；正确问题是 shared backend continuation
- 把 GitHub integration 写成 OpenCode 原生调度器
  - 不成立；正确问题是 external durable automation threshold
- 把 skill layer 写成“以后别写 AGENTS.md 了”
  - 不成立；正确问题是静态合同与按需加载知识的分工

## Tool Exit Questions

### 1. 当前工具最值得进入跨工具比较的是什么

- `plan` primary agent 驱动的开局契约
- 多层 command surface 的控制面
- shared backend 与 external runner 的长程分层
- session / share / export / handoff 的状态分层
- bounded subtask orchestration

### 2. 当前工具只在本工具特别强的是什么

- `serve` / `web` / `attach` 的同状态 shared-backend 架构
- 官方 GitHub integration 下的 issue / PR / schedule durable automation

### 3. 当前工具有哪些点证据还不够

- skills 的公开团队级 adoption 仍有限
- shared backend 的团队工作流复盘少于 Cursor cloud agents
- external durable automation 当前仍多由官方 docs/examples 主导

### 4. 当前工具有哪些点应先 hold 或谨慎处理

- `opencode__external_runner_durable_automation_threshold`
  - 保留，但更倾向并入 long-horizon 候选
- `opencode__skills_as_lazy_context_layer`
  - 保留，但先视为 merge 强化而不是直接 promote

### 5. 当前工具有哪些点明确 reject

- 把 multi-client shared state 等同于 background job 系统
- 把 absence of native scheduler / worktree 硬补成存在的功能
- 把 skills layer 误写成静态规则层的替代品

## 下一步

- 进入 `Step 3. Cross-Tool Convergence`
- 对四个工具的候选做聚类、分配 `global_candidate_id`
- 明确：
  - 哪些是跨工具 canonical 候选
  - 哪些应保留 agent-specific
  - 哪些应在 review queue 里先 `merge` / `hold` / `reject`
