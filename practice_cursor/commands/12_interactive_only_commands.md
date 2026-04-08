# 12 交互式专用 commands

这一章承接那些很重要、但没有稳定 external 对应，或者很难自然并入前 11 章的 Cursor 交互式命令面。

## 先分清三层

对 Cursor 来说，最容易混的是这三层：

- 稳定规则层：`AGENTS.md`、`.cursor/rules/*.mdc`
- 交互命令层：`/` quick commands、project commands、global commands
- 单次任务层：当前 chat prompt

不要把它们混成一层。

## project commands

官方 `Commands` 页明确支持：

```text
.cursor/commands
```

这类命令更适合放：

- 团队共享的重复性 chat workflow
- 项目级 code review 检查单
- 固定 handoff 模板
- 固定排查动作

它的定位不是规则文件，而是“可复用会话动作”。

## global commands

官方同样支持：

```text
~/.cursor/commands
```

更适合放：

- 你个人常用的工作流
- 不适合提交到仓库的个人偏好
- 跨项目复用的交互动作

## 什么时候该做成 custom command

适合做成 custom command 的，通常是：

- 每周都要重复好几次
- 结构稳定
- 更像“触发一个会话动作”而不是“声明长期规则”

例如：

- 生成 handoff 初稿
- 发起 review checklist
- 让聊天按固定模板先收集信息

## 什么时候不要做成 custom command

不适合的通常是：

- 长期必须遵守的仓库规则
- 一次性任务背景
- 只会用一次的大段提示词

这三类分别更适合放到：

- `.cursor/rules/*.mdc` / `AGENTS.md`
- handoff / plan 文件
- 当前 chat

## 发现与使用方式

更稳的发现路径是：

- 在 chat 里输入 `/`
- 看当前可用 quick commands 和 custom commands

所以这章的本质不是“多记命令名称”，而是知道：

- 哪些东西该进规则层
- 哪些东西该进 custom command 层
- 哪些东西仍然只是当前任务提示词

## 这章最容易写错的地方

- 不要把 project commands 当成 rules 的替代品。
- 不要把个人偏好全塞进仓库级 `.cursor/commands`。
- 不要为了“好像有个命令”就把一次性任务也做成 custom command。
