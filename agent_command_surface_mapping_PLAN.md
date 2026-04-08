# Coding Agent Command Surface Mapping PLAN

## Purpose

这是一份执行计划，不是单纯的原则说明。

它用于指导这样一类工作：

- 先研究某个 coding agent 的外部命令面
- 再研究它的交互式命令面（slash commands / interactive commands / command palette / session-local actions）
- 然后把两者做映射，融合进目标 agent 目录下的 `commands/`
- 最后再把这些映射反映回 `practices/`

这件事不是 Codex CLI 独有，而是一条跨 agent 的整理流程。

---

## Core Goal

同一个 practice，不应该只在 shell 外部命令下成立，也应该尽量在交互式会话里成立。

所以未来整理任何 agent 的 `commands/` 时，默认都要同时覆盖两层命令面：

1. external command surface
2. in-session command surface

如果某个 agent 同时具备这两层入口，而我们只整理了外部命令，那这套 practice 就是不完整的。

---

## Output Contract

对某个具体 agent 执行这项工作时，默认目标目录结构是：

```text
<target_root>/
  reference/
  commands/
  practices/
```

说明：

- `reference/`：存放这次命令面研究得到的原始整理材料
- `commands/`：融合后的外部命令 + 交互式命令映射结果
- `practices/`：把交互式执行方式反映回习惯层

如果目标根目录下已经有 `reference/`，优先复用它。

如果没有，就创建一个。

---

## Execution Handshake

这份计划在真正执行前，也必须先完成一轮明确的握手，而不是直接开做。

### Step 0. Ask which coding agent

如果用户还没有明确指定目标 agent，先反问一句：

`这次要做哪个 coding agent？`

没有拿到 agent 名称之前，不进入实际研究和写文件阶段。

### Step 1. Restate the current run

拿到目标 agent 名称后，要先把本轮执行内容复述给用户，至少包括：

- 这次处理的是哪个 coding agent
- 对应的目标根目录是什么
- 是否已有 `reference/`，如果没有会不会创建
- 这次会如何处理 `commands/`
- 这次会如何处理 `practices/`
- 是否可能出现第 12 章兜底

### Step 2. Show the expected output shape

正式执行前，要把这轮工作的目标结构告诉用户，例如：

```text
<target_root>/
  reference/
    ...
  commands/
    ...
  practices/
    ...
```

### Step 3. Wait for confirmation

在用户确认“没问题”之前，不正式执行。

只有在用户确认后，才进入：

- 官方资料审计
- reference 落盘
- commands 融合
- practices 回写

### Step 4. After confirmation, continue automatically

用户确认之后，后续步骤默认连续执行。

除非出现：

- 真实阻塞
- 关键歧义
- 高风险分歧

否则不再重复打断用户，只做进度汇报。

---

## Reference Rule

研究交互式命令面时，第一步往往要到官方文档、官方手册或本机帮助输出中系统找资料。

这些资料不应该只停留在临时分析里，而应该沉淀到目标 agent 的 `reference/` 目录。

### Rule 1. Reuse if it exists

如果目标 agent 目录下已经有合适的 reference 文件：

- 优先读取和复用
- 不重复制造新的 reference

### Rule 2. Produce if it does not exist

如果目标 agent 目录下没有相关 reference 文件，就新建一个。

这个 reference 文件至少要包含：

- 官方术语
- 命令清单
- 关键行为边界
- 明显和外部命令面的对应关系
- 无法确认或待验证的部分

### Rule 3. Reference comes before fusion

不要跳过 reference 直接改 `commands/`。

正确顺序是：

1. 先研究
2. 先落 reference
3. 再融合进 `commands/`
4. 再回写 `practices/`

---

## Mapping Types

做映射时，统一使用下面四类判断，不要硬凑：

### Type A. Exact mapping

外部命令和会话内命令几乎等价，只是入口不同。

### Type B. Combined mapping

一个外部命令，需要多个交互式动作组合，才能达到近似效果。

### Type C. Approximate mapping

没有严格等价物，但有足够接近的交互式做法。

### Type D. No mapping

没有合理对应关系，或者只能存在于某一种命令面。

---

## Fallback Chapter 12 Rule

大多数情况下，应该优先把交互式命令面融合进原有的 11 个习惯，而不是轻易新增章节。

但如果某些交互式命令同时满足：

- 很重要
- 没有 external 命令对应
- 也很难自然并入原 11 个习惯

那就不要硬塞。

这时应该新增一个**特殊的第 12 章**，作为交互式命令面的兜底层。

### Chapter 12 is for

- interactive-only 的重要命令
- 没有 external 对应物的 slash commands
- 很难自然并入原 11 个习惯，但对真实工作流很有价值的会话内操作

### Chapter 12 is not for

- 其实能融入原 11 章、只是需要多写一句映射说明的内容
- 只是某个外部命令的会话内别名
- 只是组合对应，但没有必要独立成章的内容

---

## Standard Workflow

这项工作固定按下面几个步骤执行。

每一步都要先讲清楚：

- 目标
- 验收
- 具体执行

### Step 0. Run the execution handshake

先执行上面的 `Execution Handshake`。

也就是说，先拿到目标 agent 名称，先复述本轮计划，先让用户确认无误，再进入正式执行。

### Step 1. Audit the interactive command surface

目标：

找全目标 agent 的交互式命令面，并确认官方术语和边界。

验收：

- 已确认该 agent 对这类功能的官方叫法
- 已拿到尽可能完整的交互式命令清单
- 已区分哪些来自官方文档，哪些来自本机帮助或本地实现

具体执行：

- 先查官方网站或官方手册
- 再查本机帮助输出或本地安装包
- 如果内容很多，先整理进 `reference/`

### Step 2. Create or update reference/

目标：

把第一步找到的命令面资料沉淀成可复用 reference，而不是只停留在临时分析里。

验收：

- `reference/` 已存在
- 有一份可读的交互式命令面 reference
- 里面已经区分了已确认事实和待验证内容

具体执行：

- 如果目标目录已有 `reference/`，优先复用并补充
- 如果没有，则创建 `reference/`
- 如果已有合适 reference 文件，就更新它
- 如果没有，就新增一份新的 reference 文件

### Step 3. Map into commands/

目标：

把交互式命令面映射进现有 `commands/`，而不是单独做一份脱节的 slash command 清单。

验收：

- `commands/` 已同时覆盖 external 和 interactive 两层命令面
- 每个重要命令都明确标注了映射类型
- 能融入原章节的内容已经融入原章节

具体执行：

- 逐章检查现有 `commands/`
- 对每一章补充：
  - 外部命令
  - 交互式对应
  - 映射类型
  - 使用建议
- 不把映射清单写成脱离 practice 的孤岛

### Step 4. Add Chapter 12 only if needed

目标：

只在确实必要时，为 interactive-only 的命令面补一个第 12 章。

验收：

- 如果新增了第 12 章，它收的确实是无法自然并入原 11 章的内容
- 如果没有新增第 12 章，也能解释为什么现有章节已经足够承载

具体执行：

- 先尝试融入原 11 章
- 只有在映射类型长期是 `No mapping` 且内容重要时，才新增第 12 章

### Step 5. Reflect back into practices/

目标：

让 `practices/` 不再默认只服务外部命令，而是也说明交互式会话下怎么执行。

验收：

- 每个相关 practice 都说明了交互式入口下如何落地
- 如果交互式入口和外部命令有差异，差异已经写清楚
- `practices/` 和 `commands/` 之间保持一致

具体执行：

- 按习惯逐章回写
- 把“在交互态下怎么做”补进已有内容
- 如有必要，增加对第 12 章的引用

### Step 6. Final consistency review

目标：

确认整套内容结构统一、命令准确、reference 可追溯。

验收：

- `reference/`、`commands/`、`practices/` 三层已经打通
- 命令示例和映射类型没有自相矛盾
- 不存在本该在 reference 里的研究结果却只残留在临时分析中的情况

具体执行：

- 检查文件结构
- 检查内部引用
- 检查命令样例
- 检查第 12 章是否被滥用或遗漏

---

## Dialogue Rule For This Plan

执行这份计划时，要步步为营。

每个步骤开始时，优先用下面这套结构汇报：

```text
Step X: <step name>

目标：
<这一步要解决什么>

验收：
<怎样算这一步完成且正确>

具体执行：
<这一小步接下来做什么>
```

如果步骤较大，也可以在 `目标` 和 `验收` 之间加入 `思路`，但至少不能缺少：

- 目标
- 验收
- 具体执行

---

## Current Default

这份计划当前首先服务于 Codex CLI 的 command-surface mapping。

但今后切换到其他 agent 时，只需要替换：

- 官方资料来源
- 目标根目录
- 交互式命令面的真实形态

其余流程默认不变。
