# Cursor Practice Set

这个目录把 `overall/round2_detailed` 里的 11 个习惯，整理成了适配 Cursor 的一套实践材料。

- `practices/`：按习惯组织的主阅读内容
- `commands/`：支撑这些习惯的 Cursor 工作模式、规则载体、上下文入口、editor 内 `/` commands、自定义 commands 和后台 agent 工作流
- `reference/`：这轮整理用到的官方术语、命令面边界和映射参考

这套内容保留了 round2 的骨架，但收紧到当前 Cursor 的真实能力边界，重点包括：

- Chat tabs / history，而不是硬套 CLI 式 session 语义
- `Ask` 做只读探索，`Agent` / `Manual` / `Custom Modes` 做执行
- `.cursor/rules/*.mdc`、`AGENTS.md`、Project Memories、`.cursorrules` legacy
- editor 内 `Commands`、`/` quick commands、`.cursor/commands`、`~/.cursor/commands`
- checkpoints、Background Agents、model picker、Max Mode、usage-based pricing
- 追加第 `13` 章，把 `Background Agent orchestration / web-mobile / Slack / API / long horizon` 放到同一层处理

建议阅读顺序：

1. 先读 `practices/01_session_lifecycle.md`
2. 再读 `practices/03_plan_before_code.md`
3. 接着看 `practices/04_rule_hierarchy.md`
4. 然后看 `practices/08_tool_permission.md`
5. 如果任务进入后台、跨设备或异步编排，再读 `practices/13_advanced_agentic_coordination.md`
6. 如果要补交互式命令层，再看 `practices/12_interactive_command_layer.md`
7. 需要落地具体操作时，再去对应的 `commands/` 文件

如果要先看这轮 grounding 的事实底座，先读：

- `reference/2026-04-09_cursor_command_surface_mapping.md`
- `reference/2026-04-09_cursor_advanced_agentic_features_reference.md`
