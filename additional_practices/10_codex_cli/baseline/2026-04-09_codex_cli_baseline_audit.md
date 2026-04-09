# 2026-04-09 Codex CLI Baseline Audit

Run ID: `2026-04-09_additional_practices_r1`
Phase: `Phase 2. Tool Loop: Codex CLI`
Step: `Step 2A. Tool Baseline`

## 目标

先确认 `practice_codex_cli/` 里已经讲了什么、还没讲什么，以及哪些内容已经落在 `practices/`、`commands/`、`reference/`，避免后续把已存在表达误判为新增缺口。

## 本次复核范围

- `practice_codex_cli/README.md`
- `practice_codex_cli/practices/`
- `practice_codex_cli/commands/`
- `practice_codex_cli/reference/`

## 当前工具的既有覆盖骨架

| Layer | 数量 | 结论 |
| --- | ---: | --- |
| `README.md` | 1 | 已明确这套材料的阅读顺序、当前现实边界和参考入口 |
| `practices/` | 13 | 已覆盖原始 `11` 个习惯，并补入 `12` 交互式专属能力、`13` 高级 agentic 协调 |
| `commands/` | 13 | 已把主要外部命令、交互命令、参数边界和常见误写点落文 |
| `reference/` | 2 | 已对 slash commands 和 advanced agentic features 做 grounding |

## 已在 practice 层明确表达的内容

| 主题 | 既有锚点 | Step 2A 判断 |
| --- | --- | --- |
| 会话形态选择 | `ba.codex_cli.practice.session_lifecycle` | 已明确区分 `exec --ephemeral`、持久化 `exec`、交互式 `codex` |
| 读写分离与计划落盘 | `ba.codex_cli.practice.plan_before_code` | 已是明确 practice，不是空白 |
| 渐进式信息投喂与文件外化 | `ba.codex_cli.practice.progressive_feed` | 已明确强调“指针 + 文件外化”，不是仅靠 prompt 追加 |
| 权限设计 | `ba.codex_cli.practice.tool_permission` | 已把权限写成 workflow 问题，而不是单一开关 |
| 跨会话状态传递 | `ba.codex_cli.practice.memento_workflow` | 已明确 `resume / fork / handoff / cloud` 的分层 |
| 多智能体分流 | `ba.codex_cli.practice.multi_agent` | 已明确职责隔离、并行写入边界、subagent 适用条件 |
| 交互式命令增强层 | `ba.codex_cli.practice.interactive_only` | 已承接 interactive-only slash commands 的兜底层 |
| 高级 agentic 协调 | `ba.codex_cli.practice.advanced_agentic_coordination` | 已把角色隔离、文件隔离、运行时隔离做出三分法 |

## 已在 commands / reference 层明确表达的内容

| 主题 | 既有锚点 | Step 2A 判断 |
| --- | --- | --- |
| `exec` / `resume` / `fork` 的准确命令形态 | `ba.codex_cli.command.session_commands` | 已明确命令语法和常见误写 |
| 规划与执行的命令链 | `ba.codex_cli.command.plan_and_execute_commands` | 已明确只读规划、计划落盘、按计划执行 |
| `AGENTS.md` 发现顺序与规则验证 | `ba.codex_cli.command.agents_and_rules_commands` | 已明确规则载体和发现顺序 |
| sandbox / approval / `--full-auto` | `ba.codex_cli.command.sandbox_and_approval_commands` | 已明确交互式与 `exec` 权限面的差异 |
| `resume` / `fork` / handoff 模板 | `ba.codex_cli.command.resume_and_handoff_commands` | 已明确续跑、分叉和 handoff 文件模板 |
| built-in subagents、自定义 agents、并行配置 | `ba.codex_cli.command.subagents_and_parallel_commands` | 已明确 built-in roles、配置项和边界 |
| interactive-only slash commands | `ba.codex_cli.command.interactive_only_commands` | 已明确一组没有合理 external mapping 的会话内增强能力 |
| cloud / worktree / `/ps` / detached tasks | `ba.codex_cli.command.advanced_agentic_coordination_commands` | 已明确 `resume`、`fork`、`cloud`、外部 `git worktree` 不是一回事 |
| slash surface grounding | `ba.codex_cli.reference.slash_commands_reference` | 已明确 built-in slash commands 列表与 mapping 类型 |
| subagents / worktree / cloud grounding | `ba.codex_cli.reference.advanced_agentic_features_reference` | 已明确稳定能力、开发中能力、CLI 与 App 的边界 |

## 当前工具相对全局基线的特殊点

### 1. Session 形态分得很清楚

Codex CLI 的 session 不是抽象概念，而是明确分为：

- 交互式 `codex`
- 非交互式 `codex exec`
- `codex exec resume`
- 交互式 `codex resume`
- 交互式 `codex fork`
- detached `codex cloud`

这使得 `01`、`03`、`09`、`13` 之间的边界比其他工具更可操作，也更容易形成新的行为级 practice。

### 2. 权限和沙箱已经非常 workflow 化

当前基线不只讲“有权限控制”，而是已经讲：

- 规划阶段先只读
- 实施阶段再工作区写入
- 自动化阶段再考虑 `--full-auto`
- 不把交互式 approval 语义硬搬到 `codex exec`

这意味着后续需要审计的不是“是否有权限功能”，而是 `2026` 窗口里是否出现了新的稳定权限策略。

### 3. 多智能体与高级协调已经显式进入正式章节

Codex CLI 当前基线已经不是“顺带提一下 subagents”，而是：

- 第 `10` 章处理可控拆分
- 第 `13` 章处理角色隔离、文件隔离、运行时隔离
- `reference/` 已专门核实 `multi_agent`、`enable_fanout`、worktree、cloud

这意味着外部研究更可能改变的是“这些章节是否该强化或细分”，而不是从零补课。

## 当前工具的薄弱点与待验证区

下面这些不是结论，只是进入 `Step 2B` 前的 baseline 薄弱点：

### `session_kickoff_discipline`

- 当前相关表达分散在 `01`、`03`、`05`、`08`
- 已经有“先选会话形态”和“先规划后执行”
- 但还没有一个特别明确的“session 开局 checklist / contract”叙述
- 需要在时间窗外部材料里看，是否已形成更明确的新习惯

### `command_surface_fluency`

- 当前已经有第 `12` 章和 slash surface reference
- 但现有表达仍偏“收口与分类”
- 还需要判断外部实践里，外部命令、slash commands、cloud、交互态是否已经形成更强的组合工作流

### `workspace_worktree_isolation`

- 当前已经明确：CLI 自身没有第一方 worktree 子命令，真正隔离靠外部 `git worktree`
- 这已经是事实边界
- 但还需要判断在 2026 窗口里，这是否已经被广泛采用为稳定使用习惯

### `long_horizon_control`

- 当前已明确 `resume / fork` 与 `codex cloud` 的分层
- 但还缺少更完整的“何时切到 detached cloud task、何时只留在本地续跑”的行为判断证据
- 这很可能是外部时间窗里需要重点验证的点

### `delegation_subagent_orchestration`

- 当前已明确 built-in agents、自定义 agents、保守并行配置和 fan-out 边界
- 但需要进一步验证：这些能力在 2026 窗口里是否已经带来新的稳定 adoption pattern

## Step 2A 结论

- `practice_codex_cli/` 当前已经是成熟基线，不存在“大块未覆盖”的问题。
- Codex CLI 下一步外部搜索不该再问“有没有这些能力”，而该问：
  - 哪些能力在 `2026-01-01` 到 `2026-04-09` 之间出现了新的行为级升级
  - 哪些原本只是 reference/commands 的东西，开始值得上升到更明确的 practice
  - 哪些看起来很新，但最终只适合 merge 到现有第 `12` / `13` 章或既有章节

## 建议进入 Step 2B 时优先验证的方向

1. `session_kickoff_discipline`
2. `command_surface_fluency`
3. `delegation_subagent_orchestration`
4. `workspace_worktree_isolation`
5. `long_horizon_control`
6. `recovery_replay_resume_hygiene`

当前仍未分配任何 `tool_candidate_id`。这一步只负责确认 baseline，不提前进入候选卡片。
