# Additional Practices Research Index

## Run Snapshot

Time: 2026-04-09 10:59:29 CST
Run ID: 2026-04-09_additional_practices_r1
Supersedes Run ID: none
Effective Window: 2026-01-01 to 2026-04-09

Main Question:

- 现有 4 个工具目录的 practice 体系，是否仍遗漏了会显著改变 session 使用方式、上下文组织方式、任务拆分方式或长期协作方式的高价值 practice。

Fixed Non-Goals:

- 不直接改动 `practice_codex_cli/`、`practice_claude_code/`、`practice_cursor/`、`practice_opencode/` 的正式 `practices/` 与 `commands/`。
- 不把单条社区经验、单次演示或纯功能点直接升格为 practice。
- 不混用未标注的研究时间窗；当前 run 只以 `2026-01-01` 到 `2026-04-09` 为主窗口。

Non-Drift Constraints:

- `tool-first` 顺序固定为 `Codex CLI -> Claude Code -> Cursor -> OpenCode`。
- `session-first`，优先审计真正改变工作方式的行为习惯，而不是命令备忘录。
- `tool facts first`，先核实产品事实，再讨论是否形成 practice。
- 本轮新增研究材料只落到 `additional_practices/`，先 research，再 review，再决定是否 apply。

## Progress

Current Phase: Phase 2. Tool Loop: Codex CLI
Phase Status: in_progress
Current Tool: Codex CLI
Current Step: Step 2A. Tool Baseline
Step Status: completed
Last Completed Checkpoint: Phase 2 entry and Codex CLI Step 2A completed

Next Action:

- 执行 `Step 2B. Tool External Scan`
- 先做 Codex CLI 的 `Tier A` 官方时间窗扫描，再补 `Tier B` 和必要的 supplemental pass
- 以 `10_codex_cli/baseline/2026-04-09_codex_cli_baseline_audit.md` 里标出的优先方向作为外部搜索入口

Next Mandatory Checkpoint:

- 完成 Codex CLI 的 `Step 2F. Tool Exit Summary`

Blockers / Open Questions:

- 还未开始 `2026-01-01` 到 `2026-04-09` 的 Codex CLI 官方时间窗扫描，需优先核实该窗口内的产品事实变化。
- `session_kickoff_discipline`、`command_surface_fluency`、`long_horizon_control` 在 Codex CLI 内已经有本地基线，但是否形成新增候选仍需外部 adoption 信号。
- Codex CLI 当前只是完成 baseline 审计，还没有任何 `tool_candidate_id` 或 candidate card。

## Working Memory

Current Candidate Set:

- 当前只有全局假设桶，Codex CLI 仍未分配任何 `tool_candidate_id`
- 当前候选观察框架已固定在 `01_global/candidate_pool/2026-04-09_initial_candidate_observation_frame.md`

Current Open Questions:

- Codex CLI 的时间窗内新增官方能力，是否真的改写了 `12` / `13` 章周边的行为层。
- 哪些 Codex CLI 线索最后只适合 merge 到既有锚点，而不应成为新 practice。
- 哪些 Codex CLI 现象只是命令面或 reference 层变丰富，而不是行为模式发生变化。

## Artifacts Produced

- `01_global/baseline/2026-04-09_global_baseline_matrix.md`
- `01_global/baseline/2026-04-09_canonical_landing_index.md`
- `01_global/candidate_pool/2026-04-09_initial_candidate_observation_frame.md`
- `10_codex_cli/baseline/2026-04-09_codex_cli_baseline_audit.md`
- `99_final/final_recommendation.md`

## Review Queue Status

- `90_review_queue/promote/`: empty
- `90_review_queue/merge/`: empty
- `90_review_queue/hold/`: empty
- `90_review_queue/reject/`: empty
- `90_review_queue/apply_matrix/`: empty
