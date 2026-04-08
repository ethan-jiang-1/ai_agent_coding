# 12 交互命令层：重复性会话动作优先做成 custom commands

## 核心习惯

当你的目标不是“给模型一次性补背景”，而是“反复触发同一种会话动作”时，优先考虑 Cursor 的 custom commands，而不是每次重打一大段提示词。

## 在 Cursor 里怎么做

- 团队共享、跟项目绑定的交互动作，放进 `.cursor/commands`。
- 你个人跨项目复用的交互动作，放进 `~/.cursor/commands`。
- 当前只是一次性任务，就直接写在当前 chat 里，不要过度抽象。
- 长期稳定约束仍然放回 `.cursor/rules/*.mdc` 或 `AGENTS.md`，不要混到 custom command 里。

更实用的区分是：

- 稳定约束：rules
- 重复动作：commands
- 单次任务：prompt

## 为什么这很重要

因为如果不分层，最后很容易变成：

- 规则里混进临时动作
- 提示词里反复粘贴同一套流程
- 项目级和个人级工作流搅在一起

custom commands 的价值，不是“命令越多越好”，而是让高频、稳定、交互式的动作有一个合适的承载层。

## 不要这样做

- 不要把只会用一次的提示词做成 custom command。
- 不要把仓库长期规则塞进 `.cursor/commands`。
- 不要把个人偏好直接提交进团队共享 commands，除非它真是团队约定。

## 验收标准

- 你能区分 rules、commands、prompt 三层。
- 高频重复动作已经有合适的 custom command 承载。
- 项目级和个人级命令没有混在一起。

## 对应支撑

见 `../commands/12_interactive_only_commands.md`。
