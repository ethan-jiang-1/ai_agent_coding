# Additional Practices Research Index

## Run Snapshot

Time: 2026-04-09 12:09:08 CST
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

Current Phase: Phase 9. Final Conclusion
Phase Status: completed
Current Tool: none
Current Step: Step 6. 形成最终调研结论
Step Status: completed
Last Completed Checkpoint: Phase 9 completed

Next Action:

- 当前 research run 已完成
- 下一步如果进入施工阶段，应按 `90_review_queue/apply_matrix/2026-04-09_apply_matrix.md` 的顺序，把 `Merge` 候选 apply 回 4 个正式工具目录
- `Hold` 候选本轮保持 `no-op`

Next Mandatory Checkpoint:

- none

Blockers / Open Questions:

- none for this run

## Working Memory

Current Candidate Set:

- `Codex CLI` 已保留 `6` 个活跃候选，并完成工具级 exit summary
- `Claude Code` 已保留 `6` 个活跃候选，并完成工具级 exit summary
- `Cursor` 已保留 `6` 个活跃候选，并完成工具级 exit summary
- `OpenCode` 已保留 `7` 个活跃候选，并完成工具级 exit summary
- 候选已收敛为 `8` 个 `global_candidate_id`
- 当前候选观察框架与收敛结果已落到 `01_global/cross_tool/2026-04-09_cross_tool_candidate_convergence.md`

Current Open Questions:

- 本轮已无开放问题；后续若 rerun，重点应复核 `Hold` 候选是否获得更强 adoption signal。

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
- `40_opencode/intake/2026-04-09_opencode_additional_practices_scan.md`
- `40_opencode/validation/` candidate cards and deep dives
- `40_opencode/summary/2026-04-09_opencode_exit_summary.md`
- `01_global/cross_tool/2026-04-09_cross_tool_candidate_convergence.md`
- `90_review_queue/merge/*.md`
- `90_review_queue/hold/*.md`
- `90_review_queue/apply_matrix/2026-04-09_apply_matrix.md`
- `99_final/final_recommendation.md`

## Review Queue Status

- `90_review_queue/promote/`: empty
- `90_review_queue/merge/`: 6 decision memos
- `90_review_queue/hold/`: 2 decision memos
- `90_review_queue/reject/`: empty
- `90_review_queue/apply_matrix/`: 1 matrix file
