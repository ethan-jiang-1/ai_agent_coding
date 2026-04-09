# 2026-04-09 Initial Candidate Observation Frame

Run ID: `2026-04-09_additional_practices_r1`

## 目的

这份文件不是候选结论，而是后续每个工具在 `Step 2B -> Step 2E` 中复用的观察框架，用来保证：

- 候选关注的是行为变化，不是功能点堆积
- 候选能回到既有基线锚点，而不是脱离当前体系另起炉灶
- 候选在进入 `tool_candidate_id` 之前，就先有统一的观察口径

## 使用规则

- 先看当前工具 baseline，再用这份框架观察外部信号
- 任何候选都先回答“它改变了什么行为”，再回答“它依赖什么功能”
- 如果一个点只是命令面扩展或 reference 更新，不默认升格为 practice
- 每个工具的活跃候选预算默认最多 `7` 个
- 真正进入 `tool_candidate_id` 的候选，必须能指向至少一个既有 `baseline_anchor_id`

## 候选记录最小模板

后续每个工具在 `validation/` 下创建 candidate card 时，至少回答：

- 候选名称
- 为什么它是行为习惯，而不是功能点
- 当前影响的是哪一层：
  - `new-practice`
  - `merge-into-existing`
  - `reference-only`
  - `no-op`
- 主要覆盖哪个既有锚点或哪些锚点
- 它改变的是：
  - session 使用方式
  - 上下文组织方式
  - 任务拆分方式
  - 长期协作方式
- 最强支撑证据需要来自哪里：
  - `Tier A`
  - `Tier B`
  - `Tier C only`
- 强工具可能是谁
- 当前最大的反例或边界是什么

## 当前全局假设桶

下面这些是当前 run 的全局假设桶，不等于已经成立的新 practice：

| Hypothesis | 观察重点 | 当前可能 merge 的既有锚点 | Step 1 初判 |
| --- | --- | --- | --- |
| `session_kickoff_discipline` | 会话开局时如何给目标、边界、成功标准、执行姿态 | `01`、`03`、`05` | 高优先观察，当前基线里有相关内容，但可能还不够显式 |
| `context_packaging` | 背景、限制、参考资料如何成包提供，而不是碎片化追加 | `05`、`04`、`09` | 高优先观察，可能是 merge，不一定需要新章 |
| `delegation_subagent_orchestration` | 什么时候拆任务，如何分角色，如何防止并行混乱 | `10`、`13` | 高优先观察，最可能受新能力影响 |
| `workspace_worktree_isolation` | 并行 agent 和长任务运行时如何隔离文件系统与编辑面 | `10`、`13` | 高优先观察，但大概率强 vendor-specific |
| `long_horizon_control` | 长时间运行、自动续跑、检查点、恢复点、预算上限 | `09`、`11`、`13` | 高优先观察，可能只在部分工具成立 |
| `command_surface_fluency` | 外部命令、slash commands、UI 入口之间的协同是否形成稳定习惯 | `12`、相关 `commands/`、相关 `reference/` | 高优先观察，可能跨工具但表达层差异大 |
| `permission_strategy_upgrade` | 权限是否从护栏问题升级为 workflow 设计问题 | `08`、`13` | 中高优先，可能主要是 merge |
| `model_provider_routing_upgrade` | 模型、mode、provider、agent 级分工是否比旧基线更重要 | `07`、`11`、`13` | 中高优先，可能主要是 merge |
| `artifact_externalization_upgrade` | 计划、handoff、检查清单、审计文件是否形成更稳定的外化习惯 | `05`、`09` | 中优先，当前基线已有明显覆盖 |
| `recovery_replay_resume_hygiene` | 出错后如何恢复、回放、续跑、分叉，而不是重开模糊 session | `01`、`02`、`09` | 中高优先，当前基线已覆盖但可能被新能力改写 |

## Step 1 基于现有基线的初始判断

### 更像新增审计热点的区域

- 第 `12` 章附近的 command-surface fluency
- 第 `13` 章附近的高级 agentic 协调
- `workspace / worktree isolation`
- `long-horizon control`

### 更像 merge 目标的区域

- `session_kickoff_discipline`
- `context_packaging`
- `permission_strategy_upgrade`
- `model_provider_routing_upgrade`
- `artifact_externalization_upgrade`
- `recovery_replay_resume_hygiene`

这只是当前基于本地基线的预判，后续仍必须由工具事实和时间窗外部证据决定。

## 进入 Codex CLI 时的首批观察重点

进入 `Phase 2. Tool Loop: Codex CLI` 时，优先观察：

1. `codex` 交互式、`codex exec`、`resume` 的组合，是否已经形成新的 kickoff / recovery 行为。
2. interactive slash commands 与非交互链式执行的配合，是否值得升级为更明确的 command-surface practice。
3. subagents、自定义 agents、外部 git worktree 的组合，是否已经足以形成新的 delegation / isolation 习惯。
4. 长任务、detached run、resume、handoff 的边界，是否构成 `long_horizon_control` 的新增证据。

## 暂时不做的事

- 现在不分配任何 `tool_candidate_id`
- 现在不写 `Promote / Merge / Hold / Reject`
- 现在不把全局假设桶误当成正式候选列表
