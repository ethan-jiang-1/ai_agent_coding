# Coding Agent Advanced Agentic Features Progressive Plan

## Purpose

这是一份跨工具的 progressive plan，不是某一次执行的临时笔记。

它用来指导一类新的整理工作：

- 把 `sub agent / multi-agent / task delegation`
- `worktree / workspace isolation / parallel coordination`
- `long horizon / long-running / loop boundary / autonomous runtime`

这三条新兴能力线，系统整理进已经处理过的 4 个 coding agent：

- `Claude Code`
- `Codex CLI`
- `Cursor`
- `OpenCode`

这件事和原来的基础 practice 不同。

它不是补一个普通命令，也不是修一两个章节，而是评估这些高级 agentic 能力如何改变：

- 上下文处理方式
- 多任务拆分方式
- 并行运行方式
- 长时运行与风险控制方式
- `commands/` 与 `practices/` 的章节结构

---

## Topic Scope

这次整理的主题边界固定包括三组内容：

### 1. Sub Agent / Multi-Agent

包括但不限于：

- 内建 subagents
- 自定义 agents
- delegation / task routing / orchestration
- 主 agent 与子 agent 的上下文隔离
- interactive 入口、CLI 入口、slash command 入口、UI 入口

### 2. Worktree / Workspace Isolation

包括但不限于：

- 内建 worktree 能力
- Git worktree 配合方式
- 多 agent 并行时的文件系统隔离策略
- 同仓库、同工作树、多工作树之间的协作边界
- 和 sub agent / parallel execution 的耦合关系

### 3. Long Horizon / Long-Running

包括但不限于：

- loop / max-iterations / max-turns / max-duration
- autonomous continuation 的边界
- background tasks / detached tasks / long-running jobs
- 长任务的恢复、截断、压缩、handoff
- 成本、权限、风险护栏

补充说明：

- `worktree` 在本专题里视为 `sub agent / multi-agent` 的强相关配套能力
- `long horizon` 也视为高级 agentic coordination 的一部分
- 但落文时，仍然要分别确认各工具是否真的有这些正式能力

---

## Fixed Target Agents

这份 plan 当前固定服务这 4 个目录：

- `/Users/bowhead/ai_agent_coding/practice_claude_code`
- `/Users/bowhead/ai_agent_coding/practice_codex_cli`
- `/Users/bowhead/ai_agent_coding/practice_cursor`
- `/Users/bowhead/ai_agent_coding/practice_opencode`

后续如果要扩到别的工具，可以复用此 plan，但应先补充目标根目录清单。

---

## Core Principle

这次整理不是先想“我要补一章什么”，而是先确认：

1. 这个工具是否真的有这类能力
2. 它的正式入口是什么
3. 它会不会实质改变已有 practice 的判断方式
4. 它应该融进原章节，还是作为最后一个新增补充章

也就是说：

- 先研究能力边界
- 再研究命令面与交互面
- 再决定 practice 结构

不要反过来。

---

## Output Contract

每个工具在执行这项工作时，默认输出到它自己的目录里，而不是先做一份混合总稿。

每个工具的最小输出包括：

```text
<target_root>/
  reference/
    <feature-specific-reference>.md
  commands/
    ...
  practices/
    ...
```

说明：

- `reference/`：该工具关于这三类能力的官方事实整理
- `commands/`：该工具的 CLI / slash command / UI / config / workflow 支撑文档
- `practices/`：这些能力对习惯层造成的影响

补充约定：

- 如果现有章节足够承载，就优先融合
- 如果主题已经明显超出原有章节吸收能力，就新增最后一个补充章
- 章节编号不写死

当前基准章节数如果是 `N`，新增补充章就是 `N+1`。

对当前这 4 个工具来说，现阶段大概率是：

- 已有 12 章
- 新增专题章大概率会是第 `13` 章

但这不是预设结论，执行时仍然要看真实材料。

---

## Execution Handshake

这份 plan 在真正执行前，仍然要求先做一次握手。

### Step 0. Clarify execution scope

如果用户还没有明确说明是：

- 一次处理全部 4 个工具
- 还是先做其中一个工具

就先确认范围。

推荐反问句：

`这次是四个工具一起推进，还是先指定一个起点工具？`

### Step 1. Restate the current run

在用户给出执行范围后，先复述：

- 本轮处理哪些工具
- 每个工具对应的目标根目录
- 本轮是否会新增 feature-specific reference
- 本轮是否预计会影响现有章节
- 本轮是否可能新增最后一个补充章

### Step 2. Show expected output shape

至少展示：

```text
<target_root>/
  reference/
    ...
  commands/
    ...
  practices/
    ...
```

如果已经能判断到可能新增 `13_*`，也可以提前说明：

```text
<target_root>/
  commands/
    13_<advanced_agentic_topic>.md
  practices/
    13_<advanced_agentic_topic>.md
```

### Step 3. Wait for confirmation

用户确认前，不进入实际研究和写文件阶段。

### Step 4. After confirmation, continue automatically

确认后，除非出现：

- 真实阻塞
- 关键歧义
- 高风险错配

否则默认连续执行到结束。

---

## Reference Rule

这次整理的第一步必须是信息收集，而且信息必须先落到各工具自己的 `reference/`。

### Rule 1. Tool-specific references first

不要先做跨工具混合总结，再回头拆。

正确顺序是：

1. 先收某个工具的事实
2. 先落到该工具自己的 `reference/`
3. 再分析它对 `commands/` 和 `practices/` 的影响

### Rule 2. Reference files should be feature-specific

建议命名采用这一类格式：

- `YYYY-MM-DD_<agent>_advanced_agentic_features.md`
- 或
- `YYYY-MM-DD_<agent>_subagent_worktree_long_horizon.md`

文件至少要包含：

- 官方术语
- 正式入口
- 能力边界
- 明显的 UI / slash / CLI / config 对应
- 不确定或待验证点

### Rule 3. One tool at a time

即使最终会覆盖 4 个工具，信息收集阶段也要一工具一工具地落盘。

不要在 reference 阶段把 4 个工具混在一起写。

---

## Progressive Workflow

下面是这项工作的标准 progressive workflow。

每一步都必须先讲清：

- 目标
- 验收
- 具体执行

### Step 1. Define the feature frame

目标：

先把这次专题里的 3 个子主题边界定清楚，避免后面把普通 commands、普通 session 管理、普通 planning 混进来。

验收：

- 已明确 `sub agent`
- 已明确 `worktree`
- 已明确 `long horizon`
- 已明确哪些内容不在这次专题范围内

具体执行：

- 先用本文档里的 `Topic Scope`
- 必要时补一个当前轮次的专题范围说明
- 不直接进工具细节

### Step 2. Collect official evidence for one tool

目标：

针对某一个工具，先找全这三类能力的官方资料。

验收：

- 已拿到该工具关于 sub agent 的官方资料
- 已拿到该工具关于 worktree / workspace isolation 的官方资料
- 已拿到该工具关于 long horizon / loops / long-running 的官方资料
- 已区分官方 docs、help 输出、本机验证与推断

具体执行：

- 先查官方文档
- 再查本机帮助或本地实现
- 再查该工具已有的 `reference/`
- 只在当前工具的上下文里整理，不跨到别的工具

### Step 3. Write the tool-specific reference

目标：

把刚收集到的能力边界和命令面，沉淀到该工具自己的 `reference/`。

验收：

- 该工具目录下有一份 feature-specific reference
- 这份 reference 足够支撑后续写 `commands/` 和 `practices/`
- 文件里已明确写出不确定点

具体执行：

- 新建或更新该工具的 feature-specific reference
- 明确写出：
  - capability taxonomy
  - CLI surface
  - slash / command palette / keybind surface
  - config surface
  - worktree / long-running / delegation 边界

### Step 4. Build the impact map for that tool

目标：

判断这三类高级能力会影响该工具现有哪些 practice。

验收：

- 已列出受影响章节
- 已区分“轻微补充”与“结构性改写”
- 已判断是否需要新增最后补充章

具体执行：

至少检查这些现有主题：

- `01_session_lifecycle`
- `03_plan_before_code`
- `04_rule_hierarchy`
- `05_progressive_feed`
- `08_tool_permission`
- `09_memento_workflow`
- `10_multi_agent`
- `11_billing_tips`
- 当前该工具的最后一个 interactive-only 补充章

并明确判断：

- sub agent 是否只影响 `10`
- worktree 是否还会反过来影响 `01`、`09`
- long horizon 是否会反过来影响 `08`、`11`

### Step 5. Decide placement: integrate or append

目标：

决定新内容是：

- 融进旧章节
- 还是在最后新增专题章

验收：

- 已明确每个影响点的落位
- 没有把本该统一收束的内容碎裂到多个章节里
- 也没有把本该融入原章节的内容强行新开一章

具体执行：

- 对每个工具单独判断
- 默认优先：
  - 在 `10_multi_agent` 里吸收 sub agent 主线
  - 在 `09` 里吸收 worktree / handoff 相关影响
  - 在 `08` / `11` 里吸收 long horizon 的权限与成本边界
- 如果三条线共同构成一个明显的新专题，再新增 `N+1`

### Step 6. Update commands/

目标：

把这三类能力的真实入口补到该工具的 `commands/`。

验收：

- 已明确 CLI 入口
- 已明确 slash command / UI / keybind / toggle 入口
- 已明确 config / file-level 入口

具体执行：

至少补这些维度：

- sub agent 如何启动、切换、限制
- worktree 如何建立、进入、隔离、配合多 agent
- long horizon 如何配置边界、观察运行、限制 loop 或 autonomous continuation

### Step 7. Update practices/

目标：

把这些高级能力对习惯层的影响补回 `practices/`。

验收：

- practice 里写的是“判断方式变化”
- 不是只把命令清单重新抄一遍
- 新写法能说明为什么旧习惯因高级能力而发生变化

具体执行：

优先写这些类型的影响：

- 上下文污染是否因 sub agent 得到隔离
- 并行执行是否因 worktree 变成可控
- 长任务是否因 long horizon 设置而改变计划/权限/成本策略
- 旧的 handoff / restart / reset 判断是否需要更新

### Step 8. Repeat tool by tool

目标：

按同一套流程把 4 个工具逐个推进，而不是边做边混。

验收：

- 每个工具都有自己的 reference
- 每个工具都完成 commands / practices 更新
- 没有把一个工具的能力嫁接到另一个工具

具体执行：

推荐顺序：

1. Claude Code
2. Codex CLI
3. Cursor
4. OpenCode

这样更容易对齐已有材料密度，但顺序可按用户指定调整。

### Step 9. Cross-tool synthesis review

目标：

在 4 个工具都做完之后，做一次跨工具总复核。

验收：

- 术语没有互相污染
- 同名概念在不同工具里的真实含义已区分
- `sub agent`、`worktree`、`long horizon` 三条线都没有被写成同一种东西

具体执行：

- 横向对比 4 套 reference
- 横向对比 4 套新增或修改的章节
- 清理“借别家语言硬套”的写法

---

## Placement Rule For This Topic

这次专题默认不强行改写基础 practice 骨架。

优先级应当是：

1. 先补强已有相关章节
2. 再判断是否需要最后一个新增专题章

当前更可能受影响的章节有：

- `10_multi_agent`
- `09_memento_workflow`
- `08_tool_permission`
- `11_billing_tips`
- 当前最后一个 interactive-only / session ops 补充章

如果需要新增章节，建议主题名称围绕：

- `advanced_agentic_coordination`
- `subagents_worktree_long_horizon`
- `parallel_and_long_running_workflows`

但是否真的新增，要等每个工具的 reference 先收集完再定。

---

## Deliverable Standard

一次完整执行结束时，应满足：

1. 4 个工具各自都有 feature-specific reference
2. 每个工具的 `commands/` 都写清高级能力的真实入口
3. 每个工具的 `practices/` 都写清高级能力如何改变旧习惯
4. 没有把一个工具的工作流硬嫁接到另一个工具
5. worktree、sub agent、long horizon 三条线都有清楚边界
6. 新内容落位合理，不是把所有新概念都硬堆在一个旧章节里

---

## Current Understanding Lock

这份 plan 先锁定当前共识：

- 这次工作是跨 4 个工具的
- 每个工具都要先做信息收集
- 收集结果先落到该工具自己的 `reference/`
- 然后才判断对 `practice` 的影响
- 最终内容大概率要落在最后面的某个合理位置
- `sub agent`、`worktree`、`long horizon` 被视为同一条高级专题线里的三个子主题

后续如果执行范围、工具集合或主题边界发生变化，应先更新这份 plan，再开始执行。
