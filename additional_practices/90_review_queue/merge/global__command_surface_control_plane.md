# global__command_surface_control_plane

- `global_candidate_id`: `global__command_surface_control_plane`
- `decision_run_id`: `2026-04-09_additional_practices_r1`
- `supersedes_run_id`: `none`
- `decision`: `Merge`
- `source_tool_candidate_ids`:
  - `codex_cli__command_surface_fluency`
  - `claude_code__command_surface_control_plane`
  - `cursor__command_surface_control_plane`
  - `opencode__command_surface_control_plane`
- `why_merge`:
  - 四个工具都显示 command surface 已经是正式控制层。
  - 差异主要在命令面的形态，而不是候选本身是否成立。
  - 更适合补强 `12` 和相关 `commands/` / `reference/`，不值得新开章节。
- `stability_judgment`:
  - 高。官方事实层完整，且跨工具一致。
- `cross_tool_judgment`:
  - 强跨工具成立。
- `landing_hint`:
  - 优先 merge 到 `12_interactive_command_layer` 及各工具 command-surface reference。

