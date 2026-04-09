# global__environment_isolation_threshold

- `global_candidate_id`: `global__environment_isolation_threshold`
- `decision_run_id`: `2026-04-09_additional_practices_r1`
- `supersedes_run_id`: `none`
- `decision`: `Merge`
- `source_tool_candidate_ids`:
  - `claude_code__first_party_worktree_isolation`
  - `cursor__local_cloud_worktree_handoff_threshold`
- `why_merge`:
  - 这个候选不适合全工具统一，但对已支持的工具价值很高。
  - Claude 已有成熟第一方 worktree 叙事；Cursor 2026-04-02 后开始形成多环境 handoff 叙事。
  - 需要补进现有高级协调章节，避免继续沿用过时边界。
- `stability_judgment`:
  - 中。Claude 高，Cursor 仍早期。
- `cross_tool_judgment`:
  - 部分跨工具成立，应保留差异而不是强行对齐。
- `landing_hint`:
  - 优先 merge 到 Claude / Cursor 的 `10` / `13`，其他工具保持 no-op。

