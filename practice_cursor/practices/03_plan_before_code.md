# 03 先规划，后执行：Ask 先读，Agent 再写

## 核心习惯

在 Cursor 里，复杂任务的起手应该是：

- Ask 先读
- 计划先过审
- Agent 或后台 agent 再执行

重点不是虚构一个固定名称的 “Plan Mode”，而是把 `Ask -> plan file -> approval -> execution` 写成稳定开局节奏。

## 在 Cursor 里怎么做

- 规划阶段，先切到 `Ask`。
- 多文件改动前，先让 Ask 产出明确计划。
- 计划经人工审查后，再交给 `Agent` 执行。
- 如果后续要交给 Background Agent、web/mobile agent 或 Automations，也先用 Ask 把计划写清再发起。
- 如果这个流程反复出现，可以在启用 beta 之后做一个 Plan / Debug / Refactor 类的 `Custom Mode`。
- 如果团队里某类规划入口反复出现，再考虑配一个 project/global custom command，例如把 `setup-new-feature` 这种起手模板固定下来。

最稳的节奏是：

1. Ask 只读分析
2. 计划落盘
3. 人工审查
4. Agent 执行
5. 验证结果

## 好计划至少要写清楚什么

- 受影响的文件
- 每个文件为什么要改
- 明确不改哪些文件
- 执行顺序
- 风险点和验证方式

## 不要这样做

- 不要把 Cursor 写成“内建固定名称的标准 Plan Mode”。
- 不要在 Agent 已经能写代码时，一边写一边想还自称是在规划。
- 不要让计划只留在对话里，不落到文件。
- 不要跳过 Ask，直接把大任务丢给 Background Agent 或 cloud surface。

## 验收标准

- 已经有一个可检查的计划文件。
- 计划和执行是两个可区分的阶段。
- 执行时明确引用了计划文件，而不是让 Agent 自己“记得刚才说过什么”。

## 对应支撑

见 `../commands/03_ask_and_custom_modes_commands.md`。
