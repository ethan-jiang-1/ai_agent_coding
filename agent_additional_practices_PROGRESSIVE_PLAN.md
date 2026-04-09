# Coding Agent Additional Practices Progressive Plan

## Purpose

这是一份新的 progressive plan，用来系统判断：

- 现有 practice 集合是否还有明显缺口
- 原来从 `Claude Code` 出发形成的 practice，是否遗漏了更适合 `session-centric coding agent` 的新习惯
- 最近出现的更高级能力，是否已经足以支撑一些新的、高价值的 practice

这份文件的目标不是立刻新增章节，而是先建立一套可重复执行的“补充 practice 审计流程”。

这套流程既要审计内部已有材料，也要向外做带时间边界的 deep research：

- 只重点看最近 3 个月内的新做法
- 对当前这轮执行，时间窗固定为 `2026-01-01` 到执行当天
- 允许从可信社区和可信 KOL 处试探性搜索
- 但最终判断仍然要回到工具事实和可复核证据

也就是说，这一轮先回答：

1. 还有没有缺的 practice
2. 哪些缺口只是局部技巧，不值得升格为 practice
3. 哪些缺口已经跨工具成立，值得进入正式 practice 体系
4. 哪些缺口只在个别工具成立，应保留为 agent-specific practice

---

## Scope

这次审计的对象，不再局限于最初那 11 个习惯和后续零散补充，而是扩大到：

- 4 个已经处理过的 coding agent 的完整使用面
- 它们在 `session / conversation / run / agent loop` 维度上的真实工作方式
- 它们在 `commands / slash commands / TUI / GUI / config / workflow` 这些入口上的配合方式
- 它们最近新增的高级能力是否改变了“高质量使用习惯”的定义

当前固定纳入审计的 4 个工具：

- `Codex CLI`
- `Claude Code`
- `Cursor`
- `OpenCode`

---

## Fixed Inputs

当前研究的基础输入包括：

- 既有通用原始材料：
  - `/Users/bowhead/ai_agent_coding/overall/round2_detailed`
  - `/Users/bowhead/ai_agent_coding/overall/reference_r1`
  - `/Users/bowhead/ai_agent_coding/overall/reference_r2`
- 已经整理过的 4 个工具目录：
  - `/Users/bowhead/ai_agent_coding/practice_codex_cli`
  - `/Users/bowhead/ai_agent_coding/practice_claude_code`
  - `/Users/bowhead/ai_agent_coding/practice_cursor`
  - `/Users/bowhead/ai_agent_coding/practice_opencode`
- 各工具后续需要继续补充的官方资料、说明文档、release notes、手册页面
- `2026-01-01` 之后发布的外部材料：
  - 官方 changelog / docs / engineering blog / release notes
  - 可信工程团队的 workflow 复盘
  - 可信 KOL 的实践总结
  - 高信号社区讨论、论坛帖、GitHub issues / discussions

使用原则：

- 原来的 practice 体系先视为“当前基线”，不是默认正确无误的终局
- 新调研到的能力，先验证是否真的改变用户行为，再决定是否升格为 practice
- 不为了追求章节数量而硬造 practice

---

## External Research Window

这次计划明确允许也要求做外部搜索，但必须受时间窗约束。

默认时间窗：

- 主时间窗：`2026-01-01` 到当前执行日期
- 对当前这轮计划，当前执行日期是 `2026-04-09`
- 所以当前有效主窗口是：`2026-01-01` 到 `2026-04-09`

执行原则：

- 只把这个窗口内的材料当成“新增 practice 候选”的主要证据
- 更早的材料只能作为背景对照，不能单独决定新增 practice
- 如果某个做法在 `2026-01-01` 之后被反复提到、且与工具新能力同步出现，优先纳入深挖

---

## Source Trust Ladder

外部搜索时，不同来源的权重要明确分层。

### Tier A

最高优先级，作为事实锚点：

- 官方文档
- 官方 changelog
- 官方 engineering blog
- 官方 announcement / release notes

### Tier B

高价值实践信号，作为 workflow 候选来源：

- 有持续产出、能展示具体工作流的可信 KOL
- 知名工程团队、公司工程博客、项目维护者的实战总结
- 附带具体 repo、截图、脚本、命令或 session 证据的公开写作

### Tier C

只作为线索发现源，不单独决定结论：

- 社区论坛
- Reddit
- Discord / Slack 转帖
- 零散社交媒体线程

使用规则：

- `Promote` 候选，至少应有 `Tier A` 对工具能力的支撑
- 如果主要灵感来自 `Tier B / Tier C`，必须再回到官方资料或重复独立信号做核实
- `Tier C` 的作用是发现新方向，不是直接定案

---

## Research Storage Rule

这次调查期间，所有需要沉淀的中间研究材料，统一放在：

- `/Users/bowhead/ai_agent_coding/overall/additional_practices`

存放原则：

- 先存事实，再做结论
- 能复用已有 reference 时优先复用，不重复造文档
- 如果必须新增研究文件，优先使用可追溯命名
- 每条外部线索尽量记录：
  - 发布时间
  - 来源层级（`Tier A / B / C`）
  - 链接
  - 关键信号
  - 它可能影响的 practice 候选

建议命名格式：

- `YYYY-MM-DD_<agent>_additional_practices_scan.md`
- `YYYY-MM-DD_cross_tool_gap_hypotheses.md`
- `YYYY-MM-DD_<topic>_validation.md`

---

## Fixed Output Contract

这份 plan 的直接产物不是 practice 文件本身，而是：

1. 一套有顺序、有验收标准的调研执行法
2. 一套判断“要不要新增 practice”的评估框架
3. 一批落在 `overall/additional_practices` 下的研究材料
4. 一个经过筛选的候选新增 practice 清单
5. 一份带证据来源分层的候选判断依据

等调研完成后，最终结论应至少能分成三类：

- `Promote`
  - 应进入正式 practice 体系的候选
- `Merge`
  - 不单独成章，但应该并入已有 practice 的候选
- `Reject / Hold`
  - 目前还不值得形成 practice，或者证据不足的候选

---

## Core Principle

这次工作不是“再想一些技巧”，而是“重新审计哪些用户习惯真正能放大 agent 能力”。

执行时遵守 7 条原则：

1. `practice must change behavior`

一个候选点如果只是知识点、功能说明、命令备忘录，不足以成为 practice。

2. `session-first`

优先找那些真正改变 session 使用方式、上下文组织方式、任务拆分方式、长期协作方式的行为习惯。

3. `tool facts first`

先核实工具真实能力，再讨论 practice，不先拍脑袋设章节。

4. `cross-tool before universal`

先确认一个候选是不是在多个工具上都成立，再决定是否提升为更通用的 practice。

5. `absence is valid`

如果某些想法只在一个工具里成立，或者尚未成熟，正确结论可以是“不补”。

6. `explore before converge`

在外部搜索阶段，先允许更宽的探索，不要过早把候选压缩掉；但进入收敛阶段后，必须回到证据和行为影响。

7. `recent evidence wins`

当旧做法和 `2026-01-01` 之后的新做法冲突时，优先核实新做法，因为 agent 能力和使用面正在快速变化。

---

## Candidate Gap Areas

下面这些只是候选研究方向，不是已经确认要新增的 practice：

- `session kickoff discipline`
  - 如何开局、如何给边界、如何让 agent 在一个 session 里形成稳定工作模式
- `context packaging`
  - 如何把背景、限制、目标、参考信息打包给 agent，而不是碎片化追加
- `delegation / sub-agent orchestration`
  - 什么时候该拆任务，什么时候不该拆，如何避免并行引入混乱
- `workspace / worktree isolation`
  - 并行 agent 或长任务运行时，如何管理工作树和文件冲突
- `long-horizon control`
  - 长时间运行、自动续跑、循环上限、检查点、恢复点等是否形成新习惯
- `command-surface fluency`
  - 外部命令、内建 slash command、UI 入口之间的协同是否值得形成稳定 practice
- `permission strategy`
  - 权限不是单纯护栏，而是工作流的一部分时，是否会形成稳定习惯
- `model / provider routing`
  - 模型切换、 provider 选择、不同强度 agent 的任务分配是否足够重要
- `artifact externalization`
  - 把计划、提示词、handoff、检查清单沉淀成文件，而不是只存在聊天中
- `recovery / replay / resume hygiene`
  - 失败后如何恢复、分叉、回放、继续，而不是重开一个模糊 session

这部分的作用只是保证后续调研不会漏掉明显新方向。

---

## Execution Order

这次调研默认采用 `tool-first`，不是 `topic-first`。

推荐顺序：

1. `Codex CLI`
2. `Claude Code`
3. `Cursor`
4. `OpenCode`

在每个工具内部，一次性做完：

- 既有 practice 基线审计
- 候选缺口扫描
- 官方能力核实
- 对已有 practice 的影响判断

不要用这种顺序：

1. 先把 4 个工具的某一个话题全扫完
2. 再回头扫下一个话题

因为那样更容易把不同工具的近似概念混成同一种 practice。

补充说明：

- 外部搜索也遵守 `tool-first`
- 也就是说，做某个工具时，优先把这个工具的官方资料、KOL 做法、社区线索一起扫完
- 再进入下一个工具，而不是全网先扫一轮再回头分发

---

## Autonomy Rule

这次工作默认是一个持续推进的 deep research run，不应频繁打扰用户。

执行规则：

- 默认连续推进，不逐步等待用户确认
- 只做阶段性进度提示，不停下来要反馈
- 只有遇到真正阻塞、目标目录冲突、事实无法判定、或高风险误写时，才主动打断用户

换句话说：

- 这份 plan 的执行应该尽量自驱
- 调研过程中允许发现新方向
- 但不能因为发现新方向就失去步骤纪律

---

## Plan Evolution Rule

这份计划允许随着调研推进而变得更清晰，但变化必须是“可追溯收敛”，不能是随意漂移。

允许改变的部分：

- 候选缺口列表
- 某一步内部的深挖分支
- 哪些候选应优先验证

不应改变的部分：

- `tool-first` 总顺序
- `目标 / 思路 / 验收 / 具体执行` 的步骤纪律
- “先探索，再核实，再收敛”的基本流

如果调研中发现某个候选明显值得深挖，正确做法是：

1. 先在当前步骤记录为什么值得深挖
2. 新增一个受控深挖小分支
3. 深挖结束后再回到主流程收敛

而不是直接跳过当前步骤。

---

## Step-by-Step Execution Contract

这份计划强调“步步为营”，所以后续真正执行时，每一步都固定包含：

1. `目标`
2. `思路`
3. `验收`
4. `具体执行`

含义约定如下：

### 目标

明确这一步只解决什么问题，不提前越界做下一步结论。

### 思路

说明这一步为什么这么做，用什么方法缩小不确定性。

### 验收

说明怎样判断这一步的结果是可靠、可继续推进的。

### 具体执行

列出会读哪些材料、会产出什么文件、会形成什么中间结论。

---

## Progressive Steps

### Step 1. 建立当前基线

目标：

先把现有 4 个工具目录里的 practice 体系当作“当前基线”，明确它们已经覆盖了什么，没有覆盖什么。

思路：

如果不先看清当前基线，后面很容易把“已经存在但表达不够好”的内容误判成“缺失 practice”。

验收：

- 能列出当前 practice 的主轴
- 能看出哪些主题已经跨工具存在
- 能看出哪些主题目前只是局部提及，尚未形成稳定 practice

具体执行：

- 逐个复核 4 个工具目录中的 `practices/`
- 做一份现有 practice 覆盖矩阵
- 把矩阵或摘要存到 `overall/additional_practices`

### Step 2. 做外部近三个月探索扫描

目标：

在 `2026-01-01` 到 `2026-04-09` 的时间窗内，扫描官方、可信 KOL 和高信号社区，找出最近才明显浮现的使用方法和行为模式。

思路：

这一步先发散，不急于判断哪些一定要升级为 practice；重点是发现“新而且反复出现”的信号。

验收：

- 至少形成一批带来源和日期的外部线索
- 能区分哪些线索来自官方，哪些来自社区或 KOL
- 能初步看出哪些模式是 `2026-01-01` 之后的新变化，而不是旧话重提

具体执行：

- 按工具顺序做外部扫描
- 优先搜官方 docs / changelog / engineering posts
- 再补可信 KOL、工程博客、社区论坛
- 把外部线索沉淀到 `overall/additional_practices`

### Step 3. 提出候选缺口假设

目标：

基于内部基线和外部扫描，提出一组“可能缺失的 practice 假设”。

思路：

这一步开始从发散进入初步整理，只保留那些明显会改变用户行为、且在近期有真实信号支撑的方向。

验收：

- 每个候选都能说清“它为什么是行为习惯，而不是功能点”
- 每个候选都能给出至少一个继续核实的理由
- 候选清单不混入纯命令说明和纯 UI 功能记忆

具体执行：

- 整理 Step 1 和 Step 2 的材料
- 产出一份候选缺口清单
- 对每个候选加一句“为什么值得继续核实”

### Step 4. 逐工具做事实核实

目标：

验证这些候选缺口，是否在每个工具上都有真实能力支撑，还是只是某个工具特有现象。

思路：

practice 不是抽象空想，必须有工具事实支撑；即使是通用习惯，也要看各工具的入口差异。

验收：

- 对每个候选点，都能说明在每个工具里是 `支持 / 部分支持 / 不支持`
- 重要判断都有可追溯材料
- 不把“社区习惯”误写成“工具正式能力”

具体执行：

- 按 `Codex CLI -> Claude Code -> Cursor -> OpenCode` 的顺序推进
- 每个工具需要时都补充新的参考材料到 `overall/additional_practices`
- 每个工具形成一份“候选缺口可用性扫描”

### Step 5. 对高信号候选做定向深挖

目标：

当某些候选在表层扫描后表现出明显价值，但证据还不够扎实时，对它们做第二轮 focused deep research。

思路：

这一步不是把所有候选都重查一遍，而是只深挖那些最可能成为新增 practice、同时最容易误判的点。

验收：

- 深挖对象数量受控，不泛滥
- 每个深挖对象都补齐了关键证据缺口
- 深挖后能明显提高候选判断的清晰度

具体执行：

- 选择高信号但仍模糊的候选
- 追加官方资料、实践案例、跨来源佐证
- 把深挖结果继续存到 `overall/additional_practices`

### Step 6. 判断是新增、融合还是搁置

目标：

把候选点从“看起来有意思”变成结构化结论：新增 practice、并入旧 practice，还是暂时搁置。

思路：

真正有价值的 practice，应该具备稳定性、跨场景复用性、可操作性和明显收益。

验收：

- 每个候选都有明确归类
- 归类理由能被复核，而不是主观喜好
- 不会因为某个点新鲜就强行升格

具体执行：

- 为每个候选做 3 类判断：`Promote / Merge / Hold`
- 同时记录判断理由：
  - 是否真的改变行为
  - 是否足够稳定
  - 是否跨工具成立
  - 是否和现有章节高度重叠

### Step 7. 设计未来落章方式

目标：

如果确实有新增 practice 候选成立，要先设计它们如何落到现有体系，而不是直接补文件。

思路：

有些内容更适合作为新章节，有些更适合只补进既有章节；结构先想清楚，后面执行才不会反复返工。

验收：

- 每个 `Promote` 候选都有建议落位
- 每个 `Merge` 候选都知道应并到哪些现有章节
- 不出现明显重复章节或职责冲突

具体执行：

- 输出一份“候选落章设计”
- 说明它们更适合：
  - 成为跨工具通用 practice
  - 成为某一工具的 agent-specific practice
  - 只在 `commands/` 或 `reference/` 层表达

### Step 8. 形成最终调研结论

目标：

把整个调研压缩成一个可执行结论，告诉后续整理阶段应该补什么、不补什么、先补什么。

思路：

最后的结论不应是一堆分散笔记，而应是一份足够清晰的后续施工输入。

验收：

- 能明确回答“我们还有没有缺 practice”
- 能列出优先级最高的补充方向
- 能区分通用新增与工具特有新增

具体执行：

- 汇总 `overall/additional_practices` 中的研究材料
- 生成一份最终结论稿
- 作为后续真正落文和改目录结构的输入

---

## Decision Standard For A New Practice

一个候选想升级为真正的 practice，至少要同时满足大部分以下条件：

- 它改变的不是一句提示词，而是一种稳定工作方式
- 它能明显降低失败率、返工率、上下文浪费或协作混乱
- 它不是某个瞬时版本的小技巧
- 它能够被清晰讲明“何时用、如何用、何时不用”
- 它在至少一个强工具上被充分支撑，并且对其他工具有可迁移意义

如果不满足这些条件，更适合：

- 落到 `commands/`
- 落到 `reference/`
- 或只作为局部补充，不单独成 practice

---

## Final Reminder

这份 progressive plan 的重点，不是“多写几个章节”，而是避免漏掉那些真正改变用户行为的新 practice。

正确结果可以是：

- 发现确实还有几项高价值新 practice
- 发现大部分新增能力只需要并入旧章节
- 甚至发现当前体系已经足够完整，只需微调表达

只要判断是基于充分审计得出的，这三种结果都成立。
