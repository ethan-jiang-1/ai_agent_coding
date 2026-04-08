# Coding Agent Practice Migration Plan

## Purpose

把 `overall/round2_detailed` 中质量最高、结构最稳定的内容，按不同 coding agent 的使用环境做定向迁移。

这不是重写一套新材料，而是做一套可重复执行的适配流程：

- 固定输入来源基本一致
- 输出目录结构保持一致
- 只有目标根目录和 agent-specific 内容需要调整

---

## Fixed Inputs

主来源：

- `/Users/bowhead/ai_agent_coding/overall/round2_detailed`

辅助参考：

- `/Users/bowhead/ai_agent_coding/overall/reference_r1`
- `/Users/bowhead/ai_agent_coding/overall/reference_r2`

使用原则：

- 以 `round2_detailed` 为主，不轻易推翻其结构
- `reference_r1` / `reference_r2` 主要用于补充背景、核对工具事实、避免错配
- 如 `round2_detailed` 已足够表达，就不额外扩写

---

## Fixed Output Contract

每个目标 coding agent 都遵守同一套输出结构，只是根目录不同。

目标根目录示例：

- Codex CLI: `/Users/bowhead/ai_agent_coding/practice_codex_cli`
- 后续可替换为 Cursor、Claude Code、OpenCode 等对应目录

输出结构保持一致：

- `commands/`
- `practices/`

语义约定：

- `practices/` 是 11 个习惯的主要承载位置
- `commands/` 放支撑 practice 的原始操作方式、命令示例、工具特有行为说明
- 如果目标目录已有现成结构，以现有真实目录名为准，不为概念一致性强行改名

---

## Execution Handshake

这个流程会反复执行，所以每次正式开始前都走同一套交互步骤。

### Trigger

当用户说：

- “开始做”
- “跑这个流程”
- “跑这个脚本”
- “继续下一个 agent”

都视为准备启动一次新的迁移任务。

### Handshake Steps

#### Step 0. Ask which coding agent

如果用户还没有明确指定目标 agent，先反问一句：

`这次要做哪个 coding agent？`

不要直接开始迁移。

#### Step 1. Restate the current run

当用户说出目标 agent 后，要先把本轮执行信息明确说出来：

- 这次处理的是哪个 coding agent
- 对应的目标根目录是什么
- 本轮会如何从 `overall/round2_detailed` 迁移到该目录
- 哪些内容会进 `practices/`
- 哪些内容会进 `commands/`

#### Step 2. Show the target output shape

在正式执行前，要把最终输出目录结构也告诉用户。

最小展示模板：

```text
<target_root>/
  commands/
  practices/
```

如果已经能确定更具体的落点，也可以提前展示更细结构，例如：

```text
<target_root>/
  commands/
    ...
  practices/
    01_session_lifecycle.md
    02_edit_not_append.md
    03_plan_before_code.md
    04_rule_hierarchy.md
    05_progressive_feed.md
    06_kv_cache.md
    07_model_routing.md
    08_tool_permission.md
    09_memento_workflow.md
    10_multi_agent.md
    11_billing_tips.md
```

#### Step 3. Wait for user confirmation

在用户确认“没问题”之前，不进入实际迁移和写文件阶段。

只有在用户确认后，才开始执行：

- 审核 source 和 target
- 建立章节映射
- 写入或更新目标目录文件
- 做一致性检查

#### Step 4. Shortcut rule

如果用户在同一条消息里已经同时给出了：

- 目标 agent
- 允许继续执行

仍然应该先复述本轮计划和输出目录结构，然后等待用户明确确认；不要因为用户预先说了“继续”就跳过确认步骤。

---

## Step-by-Step Dialogue Contract

这个项目要求“步步为营”，所以在执行流程里，对用户的每一步汇报都使用固定顺序，不跳步、不抢跑。

### Dialogue Order For Every Step

每一步对话都按这个顺序说：

1. 目标
2. 思路
3. 验收
4. 本步骤的具体执行内容

### What Each Part Means

#### 1. 目标

说明这一步到底要解决什么问题，边界是什么，不把下一步的事情混进来。

#### 2. 思路

说明这一步准备怎么做、为什么这样做。

这里的“思路”是高层方法和取舍，不展开冗长的内部推理。

#### 3. 验收

说明这一步完成时，如何判断它是对的，至少要有可观察的验收标准。

常见验收方式包括：

- 目录结构是否对齐
- 章节映射是否完整
- 文件职责是否清楚
- 工具事实是否和参考材料一致
- 实际写入结果是否符合预期

#### 4. 本步骤的具体执行内容

说明这一小步接下来会读哪些文件、核对哪些点、修改哪些文件、输出到哪里。

如果这一步不涉及写文件，也要明确说明只会做只读审计，不会越界进入编辑。

### Default Message Template

默认按下面这个模板和用户沟通：

```text
Step X: <step name>

目标：
<这一步要解决什么>

思路：
<这一步采用的高层做法>

验收：
<怎样算这一步完成且正确>

具体执行：
<这一小步接下来具体会做什么>
```

### Dialogue Rules

1. 不跨步

当前步骤没讲清楚前，不直接跳到下一步。

2. 先说明，再执行

无论是读文件、建映射、还是改文件，都先说当前步骤的四段内容，再动手。

3. 验收失败就留在当前步修正

如果本步骤的检查没有通过，就继续留在当前步骤修正，不假装进入下一步。

4. 写文件步骤要更明确

凡是要新增、修改、重组文件时，`具体执行` 里必须点明会动哪些路径。

5. 可以简短，但顺序不能丢

如果步骤很小，可以写得短；但“目标 / 思路 / 验收 / 具体执行”四段顺序不省略。

---

## What Changes Per Agent

每次切换 agent，只改这些内容：

1. 目标根目录
2. 命令语法
3. 会话模型
4. 权限模型
5. 规则文件载体
6. 多智能体或并行能力的真实边界
7. 成本控制方式

其余内容尽量保持稳定：

- 11 个习惯本身的组织方式
- 每个习惯的核心问题意识
- 章节间的顺序与映射关系
- “先有原则，再有工具适配”的写法

---

## Migration Principles

1. 不大改结构

以 `round2_detailed` 的章节结构为骨架，做必要的 agent 适配，不做无意义重写。

2. 先抽象，再落地

先判断某段内容是“跨工具通用原则”还是“某工具专属操作”，再决定放法。

3. 优先保留高质量表达

如果原文已经准确、清楚、可迁移，优先复用，只替换和目标 agent 冲突的部分。

4. 不强行映射不存在的能力

如果某 agent 没有 plan mode、resume、subagent、repo map 等能力，不要为了对齐格式而硬套。

5. practice 是主体，commands 是支撑

不要把核心习惯写散到命令说明里。命令材料是为了支持 practice 的理解和执行。

6. 事实核对高于写作完整

遇到工具能力边界不确定时，先核对再落文，不用想当然补齐。

---

## Standard Workflow

除非用户明确要求压缩沟通，以下每一步都按上面的 `Step-by-Step Dialogue Contract` 来汇报和执行。

### Step 0. Run the execution handshake

先执行上面的 `Execution Handshake`，确认本轮的目标 agent、目标根目录和输出结构，再进入实际迁移。

### Step 1. Audit source and target

先看三件事：

- `round2_detailed` 的 11 章有哪些可直接迁移
- 目标 agent 根目录当前已有多少内容
- 目标目录里的 `commands/` 和 `practices/` 分工是否已经存在

### Step 2. Build a chapter-to-output mapping

建立 `round2_detailed` 11 章到目标输出的映射，不直接开始写。

当前标准章节映射：

- `01_session_lifecycle.md`
- `02_edit_not_append.md`
- `03_plan_before_code.md`
- `04_rule_hierarchy.md`
- `05_progressive_feed.md`
- `06_kv_cache.md`
- `07_model_routing.md`
- `08_tool_permission.md`
- `09_memento_workflow.md`
- `10_multi_agent.md`
- `11_billing_tips.md`

### Step 3. Separate common content from agent-specific content

对每章都做一次拆分：

- 哪些是跨工具都成立的习惯
- 哪些必须改成目标 agent 的语境
- 哪些在目标 agent 中根本不成立，应该删掉或换写法

### Step 4. Decide commands vs practices placement

放置原则：

- 习惯解释、判断标准、工作流建议，进 `practices/`
- 命令形式、CLI 参数、调用方式、权限开关、恢复会话方式，进 `commands/`

如果一段内容同时包含原则和命令：

- 在 `practices/` 保留结论和流程
- 在 `commands/` 提供具体命令支撑

### Step 5. Write or update the target-agent files

写入时遵守：

- 尽量沿用已验证的高质量段落
- 例子替换成目标 agent 的真实命令和行为
- 不为了“看起来完整”而引入未经核实的内容

### Step 6. Cross-check with references

用 `reference_r1` / `reference_r2` 检查高风险部分：

- 命令是否真实存在
- 权限模型是否写对
- 会话能力是否写对
- 规则文件路径或载体是否写对
- 多 agent / resume / fork / plan 类能力是否被误写

### Step 7. Final consistency review

最终检查四件事：

- 11 个习惯是否齐
- `practices/` 与 `commands/` 是否职责清楚
- 语气和结构是否统一
- 是否出现“通用原则”和“工具事实”混写导致的误导

---

## Deliverable Standard

一次迁移工作完成时，应满足：

1. 目标 agent 根目录下的结构清晰可用
2. `practices/` 明确承载 11 个习惯
3. `commands/` 只承担支撑性材料，不喧宾夺主
4. 所有 agent-specific 例子都符合该工具真实能力
5. 没有把其他工具的能力错误嫁接到当前 agent 上
6. 主要内容仍然继承 `round2_detailed` 的质量和框架

---

## Current Run: Codex CLI

这一节是本轮迁移的可变记录，不属于固定流程。后续切换到别的 agent 时，应优先更新这一节，再开始执行。

当前目标 agent：

- `Codex CLI`

当前目标根目录：

- `/Users/bowhead/ai_agent_coding/practice_codex_cli`

当前已确认结构：

- `/Users/bowhead/ai_agent_coding/practice_codex_cli/commands`
- `/Users/bowhead/ai_agent_coding/practice_codex_cli/practices`

当前执行策略：

1. 先以 `round2_detailed` 为骨架，整理出适合 Codex CLI 的 11 个习惯内容
2. 再把 Codex CLI 特有的命令、权限、会话管理、规则载体等信息落到 `commands/`
3. 不大改目录，不重造概念，只做必要的工具适配

---

## Reuse Rule For Future Agents

未来切换到 Cursor、Claude Code、OpenCode 或其他 agent 时，沿用本文档流程即可。

每次只需要替换：

- `Current Run` 里的目标 agent 名称
- 目标根目录路径
- 该 agent 的事实能力边界
- 对应的命令和工作流示例

其余流程默认不变。
