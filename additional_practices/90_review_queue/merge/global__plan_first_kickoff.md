# global__plan_first_kickoff

- `global_candidate_id`: `global__plan_first_kickoff`
- `decision_run_id`: `2026-04-09_additional_practices_r1`
- `supersedes_run_id`: `none`
- `decision`: `Merge`
- `source_tool_candidate_ids`:
  - `codex_cli__session_kickoff_contract`
  - `claude_code__plan_first_kickoff`
  - `cursor__plan_first_kickoff`
  - `opencode__plan_first_kickoff`
- `why_merge`:
  - 四个工具都支持“先计划、再审查、后执行”的起步纪律。
  - 这是行为变化，不是单一 feature tip。
  - 但它与现有 `01` / `03` 高度重叠，不需要新章节。
- `stability_judgment`:
  - 高。四个工具都有正式能力支点，adoption signal 也最完整。
- `cross_tool_judgment`:
  - 强跨工具成立。
- `landing_hint`:
  - 优先 merge 到 `01_session_lifecycle` 与 `03_plan_before_code` 的开局段落。

