# global__interactive_to_durable_handoff_threshold

- `global_candidate_id`: `global__interactive_to_durable_handoff_threshold`
- `decision_run_id`: `2026-04-09_additional_practices_r1`
- `supersedes_run_id`: `none`
- `decision`: `Merge`
- `source_tool_candidate_ids`:
  - `codex_cli__cloud_detach_threshold`
  - `claude_code__long_horizon_layering`
  - `cursor__long_horizon_cross_surface_layering`
  - `cursor__automations_workflow_control_plane`
  - `opencode__shared_backend_long_horizon_layering`
  - `opencode__external_runner_durable_automation_threshold`
- `why_merge`:
  - 四个工具都出现了“何时继续交互会话、何时切后台/共享状态/自动化”的新阈值问题。
  - 这是真正的行为变化，但各工具实现面差异很大。
  - 最适合并入现有 `09` / `13`，而不是抽成新章。
- `stability_judgment`:
  - 中高。事实层强，采用层在 Cursor / OpenCode 上仍偏官方主导。
- `cross_tool_judgment`:
  - 强跨工具成立，但需要保留 vendor-specific 分层。
- `landing_hint`:
  - 优先 merge 到 `09_memento_workflow` 和 `13_advanced_agentic_coordination`。

