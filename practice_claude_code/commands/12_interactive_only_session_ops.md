# 12 交互式专用 slash commands

这一章收那些很有价值、但没有稳定 external command 对应，或者很难自然并入前 11 章的会话内操作。

## 先分清三类 `/` 入口

在 Claude Code 里，输入 `/` 后看到的东西不一定都是 built-in commands。

还可能有：

- bundled skills
- MCP prompts
- 插件或平台入口

所以先别把“slash 菜单里出现了”直接理解成“它一定有 CLI 参数等价物”。

## 高价值 interactive-only 命令

### `/help`

最直接的命令发现入口。你忘了当前会话有哪些 slash commands，先看它，不要把“有哪些命令”混进实现提示词。

### `/status`

适合先看当前会话状态、环境状态，再决定下一步，而不是先问模型一大段自然语言。

### `/btw [question]`

适合插入一个旁路问题，不把它并入主任务线程。

适合：

- 临时问一个概念
- 临时确认一个术语
- 不希望主任务上下文被旁支问题污染

### `/security-review`

适合在一轮实现后，单独拉起一次安全视角审查，而不是把“顺手再做安全审计”混在实现提示词里。

### `/tasks`

适合查看后台任务状态。它更像会话操作层，不属于外部命令面的常规主路径。

### 为什么 `/loop` 和 `/schedule` 不放这里

它们已经不只是“会话内操作”，而是会引入真正的调度、后台运行和 durable automation。

所以这次把它们放去第 `13` 章：

- `/loop`：session-scoped scheduling
- `/schedule`：durable scheduling surface

### `/ultraplan [description]`

适合拉起更重型的交互式规划流。它和 `/plan` 有亲缘关系，但没有稳定的一键 CLI 等价物，所以更适合留在补充章里理解。

### `/doctor`

虽然外部也有 `claude doctor`，但在交互会话里它依然是高价值操作，因为它服务的是“环境诊断”，不是主任务提示词。

## 次级辅助命令

- `/copy [N]`：复制最近消息
- `/config`：查看或调整配置入口
- `/bug`：反馈问题

## 这章最容易写错的地方

- 不要把所有 session 操作都塞进自然语言提示词。
- 不要把 interactive-only 命令强行说成有外部 CLI 完全等价物。
- 不要把 slash 菜单里的 skills、MCP prompts 和 built-in commands 混为一谈。
