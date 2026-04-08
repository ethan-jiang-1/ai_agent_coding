# Coding Agent Command Surface Mapping

## Purpose

这份文档记录一个通用原则：

当我们为某个 coding agent 整理 `practices/` 和 `commands/` 时，不应该只覆盖“在 shell 里直接运行外部命令”的做法，也应该同时检查这个 agent 在**交互式会话**里是否提供：

- slash commands
- interactive commands
- 内置菜单命令
- 会话内快捷操作

如果存在，就要尽量把这两套入口做成可对照的映射，而不是让同一个 practice 只能在其中一种入口下成立。

---

## Why This Matters

这件事的目的，不是单纯补一份命令清单，而是统一整套 practice 的执行入口。

更具体地说，有五个目标：

### 1. Unify the mental model

同一个习惯，不应该因为用户现在：

- 还在 shell 外部
- 已经进入交互式 TUI
- 正在当前会话中继续操作

就变成三套不同的工作流。

理想状态是：

- 外部命令怎么做
- 进入交互后怎么做
- 两者是否等价
- 如果不等价，差异在哪里

都能被明确说明。

### 2. Make practices executable in real use

`practices/` 讲原则，`commands/` 讲操作。

如果 `commands/` 只覆盖外部 CLI 命令，而不覆盖交互式入口，那么用户在真实工作中很容易卡在：

`我已经在会话里了，现在这个习惯应该怎么执行？`

所以命令层必须覆盖真实使用状态，而不是只覆盖一种启动方式。

### 3. Reduce mode-switching cost

很多 coding agent 的真实使用过程都会在以下几种状态之间切换：

- shell 里直接起命令
- 进入交互式会话
- 在会话中继续完成剩余动作

如果没有映射关系，用户就会频繁：

- 退出当前会话
- 回到 shell
- 再重新起命令

这会增加摩擦，也会让 practice 看起来“不顺手”。

### 4. Keep one practice set, not two

我们要维护的是**一套习惯体系**，不是：

- 一套 shell-only 习惯
- 一套 interactive-only 习惯

同一个习惯应该能被不同入口共同支撑，只是操作表面不同。

### 5. Prepare for cross-agent reuse

这件事不是 Codex CLI 独有。

以后在 Cursor、Claude Code、OpenCode 或其他 agent 里，也可能存在：

- slash commands
- interactive menus
- command palette
- session-local actions

所以这是一条跨 agent 的整理原则。

---

## Core Principle

以后整理任何 agent 的 `commands/` 时，都要把“命令入口”看成两层：

### Layer 1. External command surface

也就是用户在 shell 里直接运行的命令，例如：

- `codex exec ...`
- `claude --print ...`
- `cursor-agent ...`

### Layer 2. In-session command surface

也就是用户已经进入 agent 会话之后，可以在当前会话里调用的命令入口，例如：

- slash commands
- interactive commands
- command palette actions
- session-local built-ins

整理时，不能只写 Layer 1，而忽略 Layer 2。

---

## Mapping Types

在做映射时，不要强行假设所有外部命令都有会话内等价物。应该显式标注映射类型。

### Type A. Exact mapping

外部命令和会话内命令几乎等价，只是入口不同。

例子形态：

- 外部命令设置模型
- 会话内 `/model` 也设置模型

这种情况可以写成“完全对应”。

### Type B. Combined mapping

单个外部命令，在交互态里需要两个或多个动作组合才能达到相近效果。

例子形态：

- 外部命令一次完成“切模型 + 切权限 + 执行”
- 交互态需要先 `/model`，再 `/status` 或其他命令确认，再发送任务

这种情况应该写成“组合对应”，而不是假装一一等价。

### Type C. Approximate mapping

交互态里没有完全等价的命令，但有一个足够接近的操作方式。

这种情况要写清楚：

- 它解决的是相近问题
- 但不是严格同一个动作

### Type D. No mapping

有些外部能力就是没有会话内对应物，或者反过来，会话内某些能力没有 shell 级入口。

这时不要硬凑。

应该明确标注：

- 无对应
- 只能在某一命令面上完成

---

## What To Add Into commands/

以后对每个 agent 的 `commands/` 目录，应该逐步增加这样一层信息：

### For each practice-relevant command

至少回答四个问题：

1. 外部命令是什么
2. 会话内命令是什么
3. 它们是 Exact / Combined / Approximate / None 的哪一种
4. 用户在什么状态下优先用哪一个

### Recommended presentation

最实用的写法不是单纯罗列所有 slash commands，而是按 practice 组织，例如：

```text
Practice: 先规划，后执行

外部命令：
- ...

交互式对应：
- ...

映射类型：
- Combined mapping

说明：
- ...
```

也就是说，映射应该服务 practice，而不是反过来让 practice 迁就命令清单。

---

## Fallback Chapter 12 Rule

大多数情况下，应该优先把交互式命令面融合进原有的 11 个习惯，而不是轻易新增章节。

但有一种情况需要例外处理：

- 某个 slash command / interactive command 很重要
- 它没有合适的 external 命令对应
- 它也很难自然并入现有 11 个习惯

这时不要硬塞进某一章，也不要为了对齐结构而假装它属于某个已有习惯。

应该新增一个**特殊的第 12 章**，作为交互式命令面的兜底容器。

### Chapter 12 is for

第 12 章只收下面这些内容：

- interactive-only 的重要命令
- 没有 external CLI 对应物的 slash commands
- 很难自然并入原 11 个习惯，但对真实工作流很有价值的会话内操作
- 明显属于“命令面补充能力”，而不是“新习惯”的内容

### Chapter 12 is not for

第 12 章不是用来偷懒的，也不是把本来应该融进原 11 章的内容一股脑扔进去。

以下情况不应该直接开 12：

- 其实能并入某个已有习惯，只是需要多写一句映射说明
- 其实只是一个已有外部命令的交互式别名
- 其实只是组合对应，没有必要独立成章

### The decision order

以后处理交互式命令面时，顺序固定如下：

1. 先查官方文档，找全 interactive command surface
2. 优先映射进现有 `commands/` 的 11 章
3. 再把对应的交互式执行方式补进 `practices/` 的 11 章
4. 只有在确实无法自然归档时，才新增第 12 章

### The nature of Chapter 12

第 12 章的定位是：

- interactive-surface-only supplement
- unmapped command bucket
- 结构兜底层

它不是新的核心习惯，而是对命令面现实差异的诚实承认。

---

## Output Standard For Future Agent Docs

未来给任何 agent 做整理时，`commands/` 最好都同时覆盖：

1. shell / CLI 入口
2. interactive / slash / palette 入口
3. 两者之间的映射关系
4. 映射是否完整
5. 哪些动作只能在某一种入口下完成

这样 `practices/` 才能真正做到：

- 不依赖某一种具体操作入口
- 在真实工作流里可执行
- 对不同使用偏好的用户都成立

---

## Practical Rule

以后只要某个 agent 同时具备：

- 外部命令入口
- 交互式命令入口

那么在整理该 agent 的 `commands/` 时，就默认要做 command-surface mapping。

除非该 agent 的交互式入口非常弱、几乎不承载任何真实工作流，否则不能只写外部 CLI 命令。

---

## Current Implication

这条原则已经直接影响当前的 Codex CLI 整理工作：

- 现有 `commands/` 不能只写 `codex exec`、`codex resume`、`codex fork`
- 还要检查 Codex CLI 的 slash commands 是否能对上这些 practice
- 如果能对上，就补上映射
- 如果只能部分对上，就明确标注是组合对应或近似对应
- 如果有重要的 interactive-only 命令无法自然并入原 11 章，就新增一个第 12 章兜底

这也是后续整理其他 agent 时的默认做法。
