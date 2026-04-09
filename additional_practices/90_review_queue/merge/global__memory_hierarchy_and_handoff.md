# global__memory_hierarchy_and_handoff

- `global_candidate_id`: `global__memory_hierarchy_and_handoff`
- `decision_run_id`: `2026-04-09_additional_practices_r1`
- `supersedes_run_id`: `none`
- `decision`: `Merge`
- `source_tool_candidate_ids`:
  - `claude_code__memory_hierarchy_and_handoff`
  - `cursor__memory_hierarchy_and_handoff`
  - `opencode__memory_hierarchy_and_handoff`
- `why_merge`:
  - 三个工具都已显示“会话状态、长期规则、分享/导出、handoff 文档”必须分层。
  - 与现有 `09` 高度重叠，但需要更明确写出层次分工。
  - 不需要新章，只需把旧叙述拉直。
- `stability_judgment`:
  - 中高。能力事实层强，采用层在 Cursor / OpenCode 上略弱于 Claude。
- `cross_tool_judgment`:
  - 跨工具成立。
- `landing_hint`:
  - 优先 merge 到 `09_memento_workflow`，并回补到 `04` / `12` 的相关边界说明。

