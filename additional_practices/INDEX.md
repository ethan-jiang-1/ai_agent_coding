# Additional Practices Research Index

## Run Snapshot

Time: 2026-04-09 11:46:46 CST
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

Current Phase: Phase 5. Tool Loop: OpenCode
Phase Status: in_progress
Current Tool: OpenCode
Current Step: Step 2A. Tool Baseline
Step Status: completed
Last Completed Checkpoint: Phase 4 completed and OpenCode Step 2A completed

Next Action:

- 执行 `Step 2B. Tool External Scan`
- 先做 OpenCode 的 `Tier A` 官方时间窗扫描，再补必要的 practitioner / community 信号
- 以 `40_opencode/baseline/2026-04-09_opencode_baseline_audit.md` 里标出的优先方向作为外部搜索入口

Next Mandatory Checkpoint:

- 完成 OpenCode 的 `Step 2F. Tool Exit Summary`

Blockers / Open Questions:

- OpenCode 的官方时间窗扫描还未开始。
- `plan` agent 的开局纪律，到底有多强 adoption，还需要外部信号确认。
- `serve` / `web` / `attach` 的 shared-backend long horizon，是否真的改变了长程协作习惯，需要优先核实。

## Working Memory

Current Candidate Set:

- `Codex CLI` 已保留 `6` 个活跃候选，并完成工具级 exit summary
- `Claude Code` 已保留 `6` 个活跃候选，并完成工具级 exit summary
- `Cursor` 已保留 `6` 个活跃候选，并完成工具级 exit summary
- 当前候选观察框架已固定在 `01_global/candidate_pool/2026-04-09_initial_candidate_observation_frame.md`
- `OpenCode` 刚完成 baseline，尚未分配任何 `tool_candidate_id`

Current Open Questions:

- `plan` agent 是否已经在 2026 窗口里变成更强的开局契约，而不只是功能点。
- OpenCode 的 TUI built-ins、built-in commands、custom commands，到底有多强的行为层意义。
- shared backend、share/export/import、handoff 这些层次，外部 workflow 是否已经把边界说清。
- bounded subtask orchestration 是否足以成为强 guardrail 候选，而不是纯 capability facts。

## Artifacts Produced

- `01_global/baseline/2026-04-09_global_baseline_matrix.md`
- `01_global/baseline/2026-04-09_canonical_landing_index.md`
- `01_global/candidate_pool/2026-04-09_initial_candidate_observation_frame.md`
- `10_codex_cli/baseline/2026-04-09_codex_cli_baseline_audit.md`
- `10_codex_cli/intake/2026-04-09_codex_cli_additional_practices_scan.md`
- `10_codex_cli/validation/` candidate cards and deep dives
- `10_codex_cli/summary/2026-04-09_codex_cli_exit_summary.md`
- `20_claude_code/baseline/2026-04-09_claude_code_baseline_audit.md`
- `20_claude_code/intake/2026-04-09_claude_code_additional_practices_scan.md`
- `20_claude_code/validation/` candidate cards and deep dives
- `20_claude_code/summary/2026-04-09_claude_code_exit_summary.md`
- `30_cursor/baseline/2026-04-09_cursor_baseline_audit.md`
- `30_cursor/intake/2026-04-09_cursor_additional_practices_scan.md`
- `30_cursor/validation/` candidate cards and deep dives
- `30_cursor/summary/2026-04-09_cursor_exit_summary.md`
- `40_opencode/baseline/2026-04-09_opencode_baseline_audit.md`
- `99_final/final_recommendation.md`

## Review Queue Status

- `90_review_queue/promote/`: empty
- `90_review_queue/merge/`: empty
- `90_review_queue/hold/`: empty
- `90_review_queue/reject/`: empty
- `90_review_queue/apply_matrix/`: empty
