# Cursor Practice Set

这个目录把 `overall/round2_detailed` 里的 11 个习惯，整理成了适配 Cursor 的一套实践材料。

- `practices/`：按习惯组织的主阅读内容
- `commands/`：支撑这些习惯的 Cursor 工作模式、规则载体、上下文入口和后台 agent 工作流

这套内容保留了 round2 的骨架，但收紧到当前 Cursor 的真实能力边界，重点包括：

- Chat tabs / history，而不是硬套 CLI 式 session 语义
- `Ask` 做只读探索，`Agent` / `Manual` / `Custom Modes` 做执行
- `.cursor/rules/*.mdc`、`AGENTS.md`、Project Memories、`.cursorrules` legacy
- checkpoints、Background Agents、model picker、Max Mode、usage-based pricing

建议阅读顺序：

1. 先读 `practices/01_session_lifecycle.md`
2. 再读 `practices/03_plan_before_code.md`
3. 接着看 `practices/04_rule_hierarchy.md`
4. 然后看 `practices/08_tool_permission.md`
5. 需要落地具体操作时，再去对应的 `commands/` 文件
