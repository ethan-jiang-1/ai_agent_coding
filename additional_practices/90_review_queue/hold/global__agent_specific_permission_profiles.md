# global__agent_specific_permission_profiles

- `global_candidate_id`: `global__agent_specific_permission_profiles`
- `decision_run_id`: `2026-04-09_additional_practices_r1`
- `supersedes_run_id`: `none`
- `decision`: `Hold`
- `source_tool_candidate_ids`:
  - `codex_cli__approval_profiles_and_remembered_grants`
- `why_hold`:
  - 这在 Codex 上是明确能力，也确实影响工作流。
  - 但当前没有足够跨工具等价物，不适合过早并入 canonical 叙述。
  - 先保留为 Codex-specific 候选，避免强行泛化。
- `stability_judgment`:
  - 中高，但主要局限于单工具。
- `cross_tool_judgment`:
  - 当前不具备跨工具成立条件。
- `revisit_hint`:
  - 如果后续其他工具出现 remembered approvals / approval profiles 等强等价物，再重判是否 merge。
