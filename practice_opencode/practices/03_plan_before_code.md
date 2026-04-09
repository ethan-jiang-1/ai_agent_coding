# 03 先规划，后执行：用 plan agent 把读和写分开

## 核心习惯

在 OpenCode 里，复杂任务最好把 `plan` agent 当成正式开局层，而不是让默认可执行的 `build` agent “顺便先分析一下”。

重点不是切了一个 agent 名字，而是先让 `plan` 产出可审查计划，再决定是否进入 `build`、subtask 或外部 runner。

## 在 OpenCode 里怎么做

- 规划阶段，用 `plan` agent。
- 计划尽量落盘，不要只留在会话历史里。
- 审查计划后，再切回 `build` agent 实施。
- 如果后续要进入 `subtask: true`、`@general` 或 GitHub runner，也先让 `plan` 产出可复核输入。
- 多文件改动时，把“计划”与“执行”拆成两个清楚阶段。
- 如果你已经在 TUI 里，OpenCode 当前更现实的做法是切到 `plan` agent，或者定义一个绑定到 `plan` 的 custom `/plan-*` command；不要假设它有内建 `/plan`。
- 如果你真去做 custom `/plan-*` command，别省略 `agent: plan`。OpenCode 官方 docs 当前明确说，command 如果不指定 agent，就默认继承你当前 agent。

最稳的节奏是：

1. `plan` 做分析，默认把 edits / bash 收成 `ask`
2. 计划落盘
3. 人工审查
4. `build` 实施
5. 验证结果

## 好计划至少要写清楚什么

- 受影响文件
- 每个文件为什么要改
- 明确不改哪些地方
- 执行顺序
- 风险和验证方式

## 不要这样做

- 不要让 `build` agent 一边想一边改，再回头补计划。
- 不要把超过 3 个文件的改动当成“边写边想”的轻任务。
- 不要让计划只存在于会话里，下一阶段还靠 AI 自己“记得刚才说过什么”。
- 不要把“已经切到 `plan` agent”误当成“计划已经被审查过”。

## 验收标准

- 已经有一个可检查的计划文件。
- 计划和执行是两个可区分的阶段。
- 执行提示词明确引用了那份计划，而不是依赖模糊会话记忆。

## 对应支撑

见 `../commands/03_plan_and_execute_commands.md`。
