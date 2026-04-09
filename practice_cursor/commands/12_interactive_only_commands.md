# 12 交互控制层与 custom commands

这一章不是只谈“值得复用时的 custom commands”，而是把 Cursor 的交互控制层拆清楚。

## 先分清四层

对 Cursor 来说，最容易混的是这四层：

- 稳定规则层：`AGENTS.md`、`.cursor/rules/*.mdc`
- 交互命令层：`/` quick commands、project commands、global commands
- 单次任务层：当前 chat prompt
- 执行模式层：Ask / Agent / 其他 modes

不要把它们混成一层。

更稳的理解是：

- rules 负责长期约束
- quick commands 负责当前会话控制
- commands 负责可复用 workflow
- current chat 承接一次性任务
- modes 决定当前这轮对话怎么执行

## quick commands：当前会话控制面

如果动作的目标是控制当前 chat，而不是生成一段新内容，优先先看 quick commands。

典型例子包括：

- `/summarize`
- `/Reset Context`
- `/Add Open Files to Context`

这类动作更像：

- 调整上下文
- 重置或压缩当前线程
- 改变这轮对话拿到什么材料

不要每次都把这些控制意图重写成 prompt。

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

补充边界：

- commands 当前仍是 beta
- 语法后续可能变化

## global commands

官方同样支持：

```text
~/.cursor/commands
```

更适合放：

- 你个人常用的工作流
- 不适合提交到仓库的个人偏好
- 跨项目复用的交互动作

## 什么时候值得做成 custom command

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

这一层和 Background Agents 的跨入口编排不是一回事。

如果你已经进入：

- web / mobile handoff
- Slack follow-up
- GitHub / Linear 触发
- API 级批量编排

那已经属于第 `13` 章的高级 agentic 协调，不属于这一章的 custom command 复用层。

所以这章的本质不是“多记命令名称”，而是知道：

- 哪些控制动作该留在 quick commands
- 哪些稳定 workflow 值得做成 commands
- 哪些东西该进规则层
- 哪些东西仍然只是当前任务提示词

## 这章最容易写错的地方

- 不要把 quick commands 和 custom commands 混成一层。
- 不要把 project commands 当成 rules 的替代品。
- 不要把个人偏好全塞进仓库级 `.cursor/commands`。
- 不要为了“好像有个命令”就把一次性任务也做成 custom command。
