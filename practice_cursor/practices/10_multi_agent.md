# 10 多智能体分流：tabs 和 Background Agents 都是隔离手段

## 核心习惯

Cursor 的多智能体工作流重点不是“多开几个东西”，而是让探索、实现和审查拥有彼此隔离的上下文和运行环境。

## 在 Cursor 里怎么做

- 用前台不同 tabs 做轻量角色分离。
- 用 `Background Agents` 做远程异步的独立执行任务。
- 用主 tab 做最终整合和决策。
- 审查和实现最好不要在同一条历史上混着走。
- 如果团队里前后台交接动作已经很稳定，再考虑用 custom commands 固定 PR 整理或 comments 返工。

Cursor 在这章的优势是：

- 背景 agent 有独立远程环境
- 可以在独立 branch 上工作
- 前台和后台的权限模型不同

## 什么时候不用多智能体

- 任务范围太小
- 只有一个文件要改
- 需求还没定清
- 你自己都说不清每个 agent 该干什么

## 不要这样做

- 不要让两个 agent 同时改同一组文件。
- 不要把 Background Agents 当成本地多开聊天窗。
- 不要不给职责边界就开多个执行通道。

## 验收标准

- 每个 tab 或 background agent 只有一个清楚职责。
- 探索、实现、审查三种角色边界清楚。
- 如果用了 background agent，不同环境和 branch 的目的也是清楚的。

## 对应支撑

见 `../commands/10_background_agents_commands.md`。
