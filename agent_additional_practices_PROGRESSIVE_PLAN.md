# Coding Agent Additional Practices Progressive Plan

## Background And Current State

这份 plan 不是从零开始写的，它建立在前面已经完成的一轮大规模整理工作之上。

当前项目背景是：

- 最初的 practice 体系，核心出发点来自 `Claude Code` 这一侧较强的 agentic 使用方式
- 之后已经把一整套 practice / commands / reference 的整理方法，迁移并适配到了 4 个工具目录：
  - `/Users/bowhead/ai_agent_coding/practice_codex_cli`
  - `/Users/bowhead/ai_agent_coding/practice_claude_code`
  - `/Users/bowhead/ai_agent_coding/practice_cursor`
  - `/Users/bowhead/ai_agent_coding/practice_opencode`
- 这 4 个目录目前都已经有相对完整的：
  - `commands/`
  - `practices/`
  - `reference/`
- 在前面的整理里，还已经额外处理过一些近阶段才变重要的话题，例如：
  - 外部命令面与内建 slash / command surface 的映射
  - sub-agent / delegation / worktree / long horizon 这类高级 agentic 能力

也就是说，当前不是“还没整理 practice”，而是：

- 已经有一套可用的基线
- 已经对 4 个工具做过一轮结构化适配
- 已经掌握了不少这 4 个工具的官方能力、交互入口和使用差异

正因为已经跑过这一轮，才出现了新的问题：

- 原来从 `Claude Code` 出发形成的 practice，是否仍然遗漏了别的高价值做法
- 经过 4 个工具的横向调查之后，是否能看见一些更值得补充的新 practice
- `2026-01-01` 之后的新能力、新工作流和可信社区/KOL 做法，是否已经足以支持新的 practice
- 这些候选是否应该先进入 review queue，而不是直接写回 4 个正式工具目录

所以，这份 plan 的任务不是直接改那 4 个正式目录，而是先做一轮带证据链的 deep research。

这轮 deep research 的定位是：

- 先调查
- 先缓冲
- 先 review
- 最后才决定是否 apply 到正式工具目录

当前这轮 research 的专用缓冲根目录是：

- `/Users/bowhead/ai_agent_coding/additional_practices`

这意味着：

- 所有新增 research 材料先落到这个缓冲目录
- review 完成前，不把新的候选直接混进 4 个正式工具目录
- 只有当候选被判断值得 `promote` 或 `merge`，才进入后续 apply 阶段

再补一条很重要的上下文约束：

- 这份文件应该足够自解释
- 即使后续清理上下文、切换到更轻的 session，重新打开本文件时，也应能立刻知道：
  - 为什么要执行这个 plan
  - 当前基线是什么
  - 当前不是在做什么
  - 这轮执行完成后会产出什么
  - 这些产物会先放在哪里，等待人工 review

如果后续是一个轻 session 接手这份文件，默认理解应当是：

1. 4 个正式工具目录已经存在，不需要从零搭建
2. 当前任务是额外挖掘“是否还有缺失的高价值 practice”
3. 这次先做 research 和 review queue，不直接改正式 practice
4. 执行时必须按这份 progressive plan 的 phase / step / status 纪律推进

## Purpose

这是一份新的 progressive plan，用来系统判断：

- 现有 practice 集合是否还有明显缺口
- 原来从 `Claude Code` 出发形成的 practice，是否遗漏了更适合 `session-centric coding agent` 的新习惯
- 最近出现的更高级能力，是否已经足以支撑一些新的、高价值的 practice

这份文件的目标不是立刻新增章节，而是先建立一套可重复执行的“补充 practice 审计流程”。

这套流程既要审计内部已有材料，也要向外做带时间边界的 deep research：

- 只重点看本轮固定主窗口内的新做法
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

例外规则：

- 如果某个高价值行为早于 `2026-01-01` 就已存在，但当前基线里明显缺失，允许作为 `legacy gap` 进入候选池
- 这类 `legacy gap` 不能只因为“发布时间太早”就被排除
- 但必须额外说明：
  - 它为什么在前一轮没有被纳入
  - 它为什么在当前仍然值得补
  - 它是否在 `2026-01-01` 之后获得了新的采用信号、更清晰的官方支撑，或更强的跨工具可迁移意义
- 也就是说，主时间窗控制的是“新增证据优先级”，不是禁止发现旧遗漏

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

## Operational Definitions

为了减少不同执行者对模糊词的理解漂移，下面几个词在本计划里固定按此解释。

### `可信 KOL`

至少同时满足大部分以下条件：

- 持续产出该领域内容，而不是一次性转帖或热点搬运
- 能展示具体工作流证据，例如命令、脚本、截图、repo、session 片段或可复现过程
- 关键结论能回到工具事实、官方资料或可观察行为，不是纯观点输出
- 明显不是以营销推广为主，且没有大量未经核实的夸张 claim

### `高信号候选`

默认至少满足下面条件中的 2 条，才算高信号：

- 有 `Tier A` 新能力或正式入口直接支撑
- 有 2 个及以上独立来源重复指向同一行为变化
- 能明确描述一个稳定用户行为，而不是单次技巧或功能记忆点

### `强工具`

`强工具` 指的是：在 4 个工具里，对某个候选提供最强原生支撑、最清晰事实锚点、且最容易观察到行为变化的那个工具。

约束：

- `强工具` 必须在 candidate card 中被显式点名
- `强工具` 不等于“主观最喜欢的工具”
- 一个候选可以只有 1 个 `强工具`，但仍然对其他工具有迁移意义

### `legacy gap`

`legacy gap` 指的是：

- 这个行为或能力不是最近才出现
- 但当前基线明显漏掉了它
- 且本轮重新审计后，发现它仍然足以影响当前阶段的高质量使用方式

### `数量受控`

凡是文中出现“数量受控”“不泛滥”这类表述，默认都以本文件后面的 `Research Budget And Stop Rules` 为准，而不是靠临场感觉判断。

---

## Research Storage Rule

这次调查期间，所有需要沉淀的中间研究材料，统一放在：

- `/Users/bowhead/ai_agent_coding/additional_practices`

这是这次 deep research 的专用缓冲根目录。

作用边界：

- 这里放调研过程中的材料、候选判断、review queue 和最终建议稿
- 不直接改动 4 个正式工具目录中的 `commands/` 和 `practices/`
- 先在这里完成 research 和 review，再决定哪些内容值得 apply 到正式目录

### Directory Contract

根目录固定采用下面这套结构：

```text
/Users/bowhead/ai_agent_coding/additional_practices/
  INDEX.md
  STATUS_LOG.md
  01_global/
    baseline/
    candidate_pool/
    cross_tool/
  10_codex_cli/
    baseline/
    intake/
    validation/
    summary/
  20_claude_code/
    baseline/
    intake/
    validation/
    summary/
  30_cursor/
    baseline/
    intake/
    validation/
    summary/
  40_opencode/
    baseline/
    intake/
    validation/
    summary/
  90_review_queue/
    promote/
    merge/
    hold/
    reject/
    apply_matrix/
  99_final/
    final_recommendation.md
```

### Directory Roles

#### `INDEX.md`

作为总导航和当前状态面板：

- 当前 phase
- 当前 phase 状态（`not_started / in_progress / completed`）
- 当前进度
- 当前工具
- 当前步骤
- 当前步骤状态（`not_started / in_progress / completed`）
- 上一步刚完成什么
- 下一步要做什么
- 下一个强制检查点
- 当前 blockers / open questions
- 已完成产物
- 待 review 区域入口

#### `STATUS_LOG.md`

作为追加式进度日志：

- 每次进入新的大阶段时追加一条
- 每次完成一个大阶段时追加一条
- 每次完成一个关键检查点时追加一条
- 记录时间、当前 phase、当前工具、当前步骤、已完成产物、下一步动作
- 作用不是写长总结，而是防止长任务里失去位置感

#### `01_global/`

放全局层材料：

- `baseline/`
  - 当前 4 个工具已有 practice 的总基线
- `candidate_pool/`
  - 初始候选池和全局观察框架
- `cross_tool/`
  - 跨工具比较、去重、分型材料

#### `10_* / 20_* / 30_* / 40_*`

每个工具一个闭环目录，内部结构固定一致：

- `baseline/`
  - 当前工具现有 practice 覆盖情况
- `intake/`
  - 近期外部线索池，不直接等于结论
- `validation/`
  - 对高信号候选做事实核实和定向深挖
- `summary/`
  - 当前工具的收口摘要

#### `90_review_queue/`

这是给人工 review 的主入口，不需要先翻所有 research 原始文件。

- `promote/`
  - 建议新增为正式 practice 的候选
- `merge/`
  - 建议并入既有 practice 的候选
- `hold/`
  - 先不动，但值得记住的候选
- `reject/`
  - 已经明确不应升格为 practice，但需要留下记录，防止后续重复研究
- `apply_matrix/`
  - 候选与 4 个正式工具目录的映射关系
  - 说明哪些候选会影响哪些工具
  - 说明建议修改哪些现有 practice 或新增哪些 practice
  - 默认细到文件 / 章节级，而不只停留在目录级

#### `99_final/`

放整个 deep research 收口后的最终建议稿。

存放原则：

- 先存事实，再做结论
- 能复用已有 reference 时优先复用，不重复造文档
- 如果必须新增研究文件，优先使用可追溯命名
- 每条外部线索尽量记录：
  - 发布时间
  - 抓取或访问时间
  - 来源层级（`Tier A / B / C`）
  - 链接
  - 版本、release、tag、commit 或页面状态（如果有）
  - 搜索 query 或发现路径
  - 具体 claim 摘要
  - 关键信号
  - 它可能影响的 practice 候选

建议命名格式：

- `YYYY-MM-DD_<agent>_additional_practices_scan.md`
- `YYYY-MM-DD_cross_tool_gap_hypotheses.md`
- `YYYY-MM-DD_<topic>_validation.md`
- `YYYY-MM-DD_<agent>_exit_summary.md`
- `YYYY-MM-DD_apply_matrix.md`

---

## Fixed Output Contract

这份 plan 的直接产物不是 practice 文件本身，而是：

1. 一套有顺序、有验收标准的调研执行法
2. 一套判断“要不要新增 practice”的评估框架
3. 一批落在 `additional_practices/` 下的研究材料
4. 一个经过筛选的候选 practice 判断清单
5. 一份带证据来源分层的候选判断依据
6. 一套供人工 review 后再 apply 到 4 个正式工具目录的缓冲输出

等调研完成后，最终结论应至少能分成四类：

- `Promote`
  - 应进入正式 practice 体系的候选
- `Merge`
  - 不单独成章，但应该并入已有 practice 的候选
- `Hold`
  - 方向可能成立，但当前证据不足、时机不够成熟、或需要后续再观察的候选
- `Reject`
  - 已明确不应升格为 practice，保留记录只是为了避免重复判断

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

补充说明：

- `cross-tool before universal` 的意思是先比较，再决定能否通用
- 不是要求 4 个工具最后长成一样
- 工具之间可以互相启发，但不需要强行对齐成同一套表达
- 如果某个 practice 明显只适合一个 vendor 的设计哲学、入口设计或用户预期，保留为 agent-specific 是正常结果

5. `absence is valid`

如果某些想法只在一个工具里成立，或者尚未成熟，正确结论可以是“不补”。

6. `explore before converge`

在外部搜索阶段，先允许更宽的探索，不要过早把候选压缩掉；但进入收敛阶段后，必须回到证据和行为影响。

7. `recent evidence wins`

当旧做法和 `2026-01-01` 之后的新做法冲突时，优先核实新做法，因为 agent 能力和使用面正在快速变化。

补充说明：

- `recent evidence wins` 用来解决“新旧说法冲突”时的优先级，不是把历史遗漏一刀切排除
- 如果某个旧做法在本轮被确认是基线遗漏，仍可作为 `legacy gap` 进入候选池
- 但必须补充说明它为什么在当前阶段重新变得重要

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

重要澄清：

- `tool-first` 在这里不是一句偏好，而是长任务主线控制规则
- 一旦进入某个工具，就要把这个工具的：
  - 基线复核
  - 外部扫描
  - 候选提炼
  - 事实核实
  - 必要深挖
  - 工具内收口
  连续做完
- 当前工具未收口前，不切到下一个工具

---

## Long-Run Flow Skeleton

为了避免长任务执行时被中途发现的新线索带偏，这次执行采用固定骨架。

总流程固定为：

1. `Run Setup`
2. `Global Baseline And Candidate Frame`
3. `Tool Loop: Codex CLI`
4. `Tool Loop: Claude Code`
5. `Tool Loop: Cursor`
6. `Tool Loop: OpenCode`
7. `Cross-Tool Convergence`
8. `Promotion / Merge / Hold / Reject Decision`
9. `Landing Design`
10. `Final Conclusion`

其中真正允许发散探索的，只发生在：

- `Global Baseline And Candidate Frame` 的候选准备期
- 每个 `Tool Loop` 内部受控的外部扫描与定向深挖

其余阶段都以收敛为主。

这意味着：

- 中途发现新线索时，不是立刻改主线
- 而是先判断这个线索属于当前工具还是后续工具
- 属于当前工具就放进当前工具的待验证池
- 不属于当前工具就登记到后续工具的待处理列表

---

## Long-Run Control Rules

为了保证长任务执行时始终“心里有数”，执行中额外遵守下面这些控制规则。

### Rule 1. Single Active Tool

任意时刻只允许一个“当前工具”处于激活状态。

含义：

- 当前工具之外的资料可以顺手记线索
- 但不对其他工具展开完整研究
- 不写其他工具的判断结论

### Rule 2. Separate Intake From Judgment

把“发现线索”和“做结论”分开。

含义：

- 新线索先进入 intake
- 只有经过当前工具核实后，才能变成候选判断
- 避免把调研时的兴奋感直接写成 practice 结论

### Rule 3. Branch, Then Return

如果某个线索值得深挖，允许开分支，但必须回到主线。

含义：

- 深挖分支只服务于当前步骤
- 深挖结束后，必须显式回答：
  - 这个分支改变了什么判断
  - 没改变什么判断
- 如果答不出来，说明这个分支不够值

### Rule 4. Exit Gate Before Switching Tools

每个工具结束前，必须先通过工具级收口验收。

只有在下列内容都齐了之后，才进入下一个工具：

- 当前工具的近期外部线索已整理
- 当前工具的候选点已归纳
- 当前工具的事实核实已完成
- 当前工具需要深挖的点已处理或明确搁置
- 当前工具有一份简明的收口摘要

### Rule 5. Protect the Main Question

每一步都要反复回到这一个主问题：

`这个发现，是否足以支持一个新的用户行为习惯？`

如果答案还只是：

- “这是个新功能”
- “这是个方便入口”
- “这是个命令技巧”

那它还不是 practice。

---

## Research Budget And Stop Rules

为了避免 `Step 2B` 和 `Step 2E` 无上限膨胀，这次执行默认采用下面这些预算与停止规则。

### Rule A. External Scan Pass Budget

每个工具的外部扫描默认分成 3 段：

1. `Tier A pass`
   - 扫当前时间窗内最关键的官方 docs / changelog / release notes / engineering posts
2. `Tier B pass`
   - 补可信 KOL、工程博客、维护者复盘
3. `Supplemental pass`
   - 只对前两轮里暴露出的缺口补充一次定向搜索

默认不做无限轮搜索。

### Rule B. External Scan Stop Condition

`Step 2B` 满足下面条件中的大部分，就应停止继续扩搜：

- 当前工具的 `Tier A` 时间窗材料已经完成一轮完整扫描
- 已完成至少一轮 `Tier B` 补充扫描
- 最近新增的 5 条“有意义来源”没有再带来新的行为级候选，只是在重复例子
- 当前 intake 已足以进入候选提炼，而不是继续堆线索

如果还想继续搜，必须先写明：

- 还缺什么关键信息
- 继续搜索预期会改变哪一个判断

### Rule C. Candidate Count Budget

每个工具默认最多保留 `7` 个活跃候选进入 `Step 2C / 2D`。

超出的线索处理方式：

- 不直接丢弃
- 但先降级留在 intake 或 parking notes
- 只有当已有活跃候选被合并、降级或排除后，才补位进入活跃候选集合

### Rule D. Deep Dive Budget

每个工具默认最多允许 `3` 个深挖对象进入 `Step 2E`。

超过这个数量时，必须满足至少一条：

- 这个额外深挖很可能改变 `Promote / Merge / Hold / Reject` 判断
- 不做这个深挖，会让当前工具无法通过 exit gate

并且要在状态记录里写清楚为什么超预算。

### Rule E. Diminishing Return Stop Rule

如果新增搜索只是在增加案例密度，而没有带来下面这些变化，就应停止：

- 新的候选
- 对已有候选的决策级证据
- 对已有候选的强反例或边界条件

也就是说：

- 新例子很多，不等于还值得继续搜
- 只有会改变判断的新增信息，才值得继续消耗预算

### Rule F. Budget Override Discipline

允许在个别工具上超出默认预算，但必须同时满足：

- 已在 `INDEX.md` 或 `STATUS_LOG.md` 里留下 override 理由
- 说明超预算会改变哪个判断
- 说明为什么不能留到后面的 `Hold` 或下次审计再处理

---

## Working Memory Contract

长任务执行时，最容易出问题的是工作记忆被新线索冲散，所以需要固定一套最小工作记忆结构。

任意时刻脑中只保留 5 个当前对象：

1. `Current Phase`
2. `Current Tool`
3. `Current Step`
4. `Current Candidate Set`
5. `Current Open Questions`

其余信息一律外部化到研究文件，而不是靠临时记忆维持。

执行要求：

- 新线索进文件，不在脑中堆积
- 新问题进 `Current Open Questions`
- 当前工具做完，就把该工具的 open questions 清空或转移

这样可以减少长流程里“知道很多，但不知道现在该做什么”的情况。

补充要求：

- 当前工作记忆要同步外部化到 `INDEX.md`
- 不允许“脑中知道跑到哪了，但文件里没有状态”
- 如果执行中断，应该能只靠 `INDEX.md` 和 `STATUS_LOG.md` 快速恢复

---

## Progress Update Contract

因为这是长程 progressive plan，所以状态更新不是可选动作，而是执行纪律的一部分。

### Progress Position Rule

progress 不能只写成模糊的“做到一半了”或“差不多在这里”。

必须能明确回答这 6 个定位问题：

1. 现在处于哪个 `phase`
2. 这个 `phase` 是 `not_started / in_progress / completed` 中的哪一个
3. 当前激活的是哪个工具
4. 当前执行的是哪个步骤
5. 上一个已经完成的硬检查点是什么
6. 下一个强制检查点是什么

如果这 6 个问题不能被快速回答，就说明 progress 定义还不够硬。

### Status Update Goal

状态更新的目的不是汇报给用户看热闹，而是为了：

- 让主线持续可见
- 让自己知道现在到底跑到哪一步
- 让中断后的恢复成本变低
- 防止因为 deep research 的分支过多而失去位置感

### Two-Layer Status System

这次执行固定使用两层状态系统：

1. `INDEX.md`
   - 始终保持“当前态”
   - 只展示现在最重要的信息

2. `STATUS_LOG.md`
   - 作为时间顺序追加日志
   - 记录关键检查点的推进轨迹

### Minimum Status Fields

每次更新状态时，至少要写清：

- 时间
- 当前 phase
- 当前 phase 状态
- 当前工具
- 当前步骤
- 当前步骤状态
- 刚完成的内容
- 产出了哪些文件
- 下一步要做什么
- 下一个强制检查点
- 当前 blockers / open questions

### Fixed Major Phases

这次执行里，所谓“大阶段”固定指下面这些，不临时发明：

1. `Phase 0. Run Setup`
2. `Phase 1. Global Baseline And Candidate Frame`
3. `Phase 2. Tool Loop: Codex CLI`
4. `Phase 3. Tool Loop: Claude Code`
5. `Phase 4. Tool Loop: Cursor`
6. `Phase 5. Tool Loop: OpenCode`
7. `Phase 6. Cross-Tool Convergence`
8. `Phase 7. Promotion / Merge / Hold / Reject Decision`
9. `Phase 8. Landing Design`
10. `Phase 9. Final Conclusion`

补充说明：

- 4 个工具各自的 loop 都单独算一个大阶段
- 这样在长任务里，“跑到哪个工具了”不会被一句笼统的 `Step 2` 淹没
- `Step` 是执行结构，`Phase` 是进度定位结构，两者必须同时存在

### Phase-Step Mapping Rule

为了避免执行时把 `phase` 和 `step` 混掉，固定采用下面这套对应关系：

- `Phase 0. Run Setup`
  - 对应 `Step 0. 初始化 research workspace 与状态系统`
- `Phase 1. Global Baseline And Candidate Frame`
  - 对应 `Step 1. 建立全局基线与候选框架`
- `Phase 2. Tool Loop: Codex CLI`
  - 对应 `Step 2. 进入逐工具闭环` 中当前工具为 `Codex CLI`
- `Phase 3. Tool Loop: Claude Code`
  - 对应 `Step 2. 进入逐工具闭环` 中当前工具为 `Claude Code`
- `Phase 4. Tool Loop: Cursor`
  - 对应 `Step 2. 进入逐工具闭环` 中当前工具为 `Cursor`
- `Phase 5. Tool Loop: OpenCode`
  - 对应 `Step 2. 进入逐工具闭环` 中当前工具为 `OpenCode`
- `Phase 6. Cross-Tool Convergence`
  - 对应 `Step 3. 做跨工具收敛`
- `Phase 7. Promotion / Merge / Hold / Reject Decision`
  - 对应 `Step 4. 判断是新增、融合、搁置还是排除`
- `Phase 8. Landing Design`
  - 对应 `Step 5. 设计未来落章方式`
- `Phase 9. Final Conclusion`
  - 对应 `Step 6. 形成最终调研结论`

如果 `Phase` 和 `Step` 对不上，说明当前 progress 记录不合格。

### Phase Entry / Exit Rule

每个大阶段都必须有两次状态更新：

1. `phase entry`
   - 刚进入这个阶段时
2. `phase exit`
   - 这个阶段真正完成时

不能只写进入，不写退出。
也不能做完了才补一句“刚才那个阶段算结束”。

### Definition Of Phase Completion

一个大阶段只有同时满足下面三件事，才算 `completed`：

1. 该阶段要求的核心产物已经写出
2. 下一步动作已经明确
3. `INDEX.md` 和 `STATUS_LOG.md` 都已更新

如果缺任何一项，这个阶段仍然算 `in_progress`。

### Mandatory Checkpoints

下列时刻必须更新一次状态：

1. 进入 `Phase 0 Run Setup`
2. 完成 `Phase 0 Run Setup`
3. 进入 `Phase 1 Global Baseline And Candidate Frame`
4. 完成 `Phase 1 Global Baseline And Candidate Frame`
5. 每次进入一个新的工具 phase
6. 完成每个工具 phase，也就是完成对应工具的 `Step 2F Tool Exit Summary`
7. 进入 `Phase 6 Cross-Tool Convergence`
8. 完成 `Phase 6 Cross-Tool Convergence`
9. 进入 `Phase 7 Promotion / Merge / Hold / Reject Decision`
10. 完成 `Phase 7 Promotion / Merge / Hold / Reject Decision`
11. 进入 `Phase 8 Landing Design`
12. 完成 `Phase 8 Landing Design`
13. 进入 `Phase 9 Final Conclusion`
14. 完成 `Phase 9 Final Conclusion`

### Optional But Recommended Checkpoints

如果出现下面这些情况，也应该补一次状态：

- 某个工具的外部 intake 特别多
- 某个候选触发了定向深挖
- 某个判断被新证据明显改写
- 预计会有一段较长时间不更新

### Update Discipline

- 先更新状态，再切换 phase 或工具
- 每个大阶段都要有清晰的 entry 和 exit
- 先写“刚完成了什么”，再写“下一步做什么”
- 状态更新要短、准、可恢复，不写成长篇讨论
- 如果只更新了 `STATUS_LOG.md` 没更新 `INDEX.md`，视为状态未完成
- 如果只更新了 `INDEX.md` 没有留下关键轨迹，视为恢复能力不足

这样做的结果应该是：

- 任意时刻打开 `INDEX.md`，都知道现在在哪里
- 任意时刻回看 `STATUS_LOG.md`，都知道是怎么走到这里的
- 任意时刻都能回答“现在做到哪一个大阶段、这个大阶段是否已经完成”

### Recommended Status Template

为了避免状态写得太粗，可以默认按这个模板更新：

```text
Time:
Current Phase:
Phase Status:
Current Tool:
Current Step:
Step Status:
Last Completed Checkpoint:
Artifacts Produced:
Next Action:
Next Mandatory Checkpoint:
Blockers / Open Questions:
```

---

## Research Artifact Contract

这次 deep research 不只是收资料，还要让资料天然支撑主线推进。

所以研究文件至少要区分 4 类角色：

1. `baseline`
   - 当前已有 practice 到底覆盖了什么
2. `intake`
   - 新发现的线索池，不直接等于结论
3. `validation`
   - 对高信号候选做事实核实和深挖
4. `synthesis`
   - 当前工具或跨工具的收口判断

这样做的目的，是避免：

- intake 文件越写越多
- 但没有地方完成真正收敛

补充落位：

- `baseline` 主要放在 `01_global/baseline/` 和各工具目录的 `baseline/`
- `intake` 主要放在各工具目录的 `intake/`
- `validation` 主要放在各工具目录的 `validation/`
- `synthesis` 主要放在各工具目录的 `summary/`、`01_global/cross_tool/`、`90_review_queue/` 与 `99_final/`

补充说明：

- `summary/` 是目录名
- `synthesis` 是内容角色
- 两者不是两套并列概念，而是“收口判断”落在 `summary/` 这类目录里

### Evidence Record Minimum Fields

每条外部证据至少记录：

- `source_title`
- `published_at`
- `accessed_at`
- `tier`
- `url`
- `version_ref`
  - release / tag / commit / doc version / page state，能记则记
- `discovery_path`
  - 搜索 query、referral 路径或从哪个线索跳转而来
- `claim_summary`
- `why_it_matters`
- `candidate_links`
  - 这条证据支持、削弱或启发了哪个 `candidate_id`
- `reliability_notes`
  - 例如营销偏重、只有单一案例、版本不明等限制

### Candidate Card Minimum Fields

每个候选至少要有一张 candidate card，字段最少包括：

- `candidate_id`
  - 使用稳定 slug；后续改名时也不应变化
- `candidate_name`
- `candidate_aliases`
  - 近义名、旧名称或跨工具比较时出现的别名，能记则记
- `scope`
  - `cross-tool / agent-specific / legacy gap`
- `behavior_change`
  - 它到底改变了什么用户行为
- `why_not_just_feature_tip`
- `trigger_or_context`
  - 何时适用
- `supporting_tool_facts`
- `strong_tool`
- `counterevidence_or_limits`
- `current_decision`
  - `Promote / Merge / Hold / Reject`
- `affected_tools`
- `proposed_landing`
  - new-practice / merge-into-existing / reference-only / no-op

补充说明：

- `current_decision` 是决策层字段
- `proposed_landing` 是落位层字段
- 两者不应混写成同一个枚举
- 一个候选完全可以满足：
  - `current_decision = Hold`
  - `proposed_landing = no-op`

### Apply Matrix Minimum Granularity

`apply_matrix` 默认至少细到下面这些字段：

- `candidate_id`
- `candidate_name`
- `decision_status`
  - `Promote / Merge / Hold / Reject`
- `target_tool`
- `target_directory`
- `target_file`
- `target_section_or_practice`
- `landing_action`
  - `new-practice / merge-into-existing / reference-only / no-op`
- `confidence`
- `rationale`

补充说明：

- `decision_status` 记录候选判断
- `landing_action` 记录是否以及如何落到正式目录
- `Hold / Reject` 候选如果进入 `apply_matrix`，默认应写成 `landing_action = no-op`

---

## Tool Loop Contract

每个工具都按完全相同的闭环推进，避免长任务中主线漂移。

单工具闭环固定为：

1. `Tool Baseline`
2. `Tool External Scan`
3. `Tool Candidate Extraction`
4. `Tool Fact Validation`
5. `Tool Focused Deep Dive`
6. `Tool Exit Summary`

每个工具都只在这个闭环里推进，不跳步。

### Tool Exit Summary 必须回答的问题

当前工具收口时，至少要明确回答：

1. 这个工具最近有什么真正新的高价值使用方式
2. 这些新方式里，哪些只是功能熟练度，不是 practice
3. 哪些点可能值得进入正式 practice 候选池
4. 哪些点只适合并入已有章节
5. 哪些点证据还不够，先 hold
6. 哪些点已经被明确否掉，应 reject

如果这 6 个问题还答不清，就不应切到下一个工具

---

## Cross-Tool Comparison Rule

跨工具比较的目的，是发现：

- 哪些模式确实可以迁移
- 哪些模式只是看起来相似，实则依赖不同工具哲学
- 哪些模式应该保留差异，而不是被抹平

所以跨工具比较时，默认遵守下面几条：

1. `inspiration is allowed`

一个工具上的高质量做法，可以启发另一个工具的 practice 审计方向。

2. `identity is not required`

即使两个工具都在解决近似问题，它们的最佳 practice 也不必写成同一个东西。

3. `vendor-specific is valid`

如果某个做法明显贴合某家工具的产品设计、权限模型、上下文模型、交互入口或典型用户群体，那么它单独存在是合理的。

4. `forced uniformity is a bug`

为了章节整齐、编号对齐或表面一致，而把本来不同的 practice 硬写成一样，是错误做法。

比较时真正要回答的问题是：

- 这个 practice 是“本质通用”还是“表面相似”
- 它在不同工具里是同一个行为，还是只是同一问题的不同解法
- 如果不同，差异是不是恰恰构成了值得保留的 practice

---

## Autonomy Rule

这次工作默认是一个持续推进的 deep research run，不应频繁打扰用户。

执行规则：

- 默认连续推进，不逐步等待用户确认
- 只做阶段性进度提示，不停下来要反馈
- 只有遇到真正阻塞、目标目录冲突、事实无法判定、或高风险误写时，才主动打断用户

补充说明：

- 对用户的阶段性进度提示，应该和内部状态更新同步
- 也就是说，外部简短进度提示之前，先把 `INDEX.md` / `STATUS_LOG.md` 更新好
- 这样外部提示和内部状态不会脱节

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

再补一条边界：

- 可以新增“受控深挖分支”
- 不可以新增“平行主线”

也就是说，整个长任务始终只有一条主线在推进。

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

补充执行要求：

- 每一步开始前，先明确“这一步结束时我手里应该多出什么”
- 每一步结束后，先写出本步产物，再决定是否进入下一步
- 不允许在当前步产物未形成时，靠“感觉差不多了”进入下一步
- 每个大步骤或工具切换前，先完成一次状态更新，再进入下一段

---

## Progressive Steps

### Step 0. 初始化 research workspace 与状态系统

目标：

在真正研究开始前，把目录骨架、状态文件和初始 phase 定位先搭好，避免后面边跑边补基础设施。

思路：

既然 `Phase 0` 已经被定义成正式大阶段，它就必须有对应的执行步和产物，而不是只存在于抽象进度结构里。

验收：

- `additional_practices/` 的基础目录骨架已建立
- `INDEX.md` 和 `STATUS_LOG.md` 已初始化
- 当前 progress 已能明确显示处于 `Phase 0` 或即将进入 `Phase 1`

具体执行：

- 建立或复核 `additional_practices/` 根目录结构
- 初始化 `INDEX.md`
- 初始化 `STATUS_LOG.md`
- 记录 `Phase 0` 的 entry 状态
- 完成 setup 后，更新 `Phase 0` 的 exit 状态，并把下一步指向 `Step 1`

### Step 1. 建立全局基线与候选框架

目标：

先做一次全局定锚，明确现有 4 个工具目录里的 practice 主轴，以及本轮值得观察的候选方向。

思路：

这一步只做“搭主线”，不进入逐工具深挖。它的作用是让后面的单工具闭环有统一参照，不会一边跑一边改研究问题。

验收：

- 能列出当前 practice 的主轴
- 能看出哪些主题已经跨工具存在
- 能看出哪些主题目前只是局部提及，尚未形成稳定 practice
- 能形成一份初始候选观察框架，供后续每个工具重复套用

具体执行：

- 先更新状态，进入 `Phase 1`
- 逐个复核 4 个工具目录中的 `practices/`
- 做一份现有 practice 覆盖矩阵
- 产出一份初始候选观察框架
- 把矩阵或摘要存到 `additional_practices/01_global/`
- 更新 `INDEX.md` 与 `STATUS_LOG.md`

### Step 2. 进入逐工具闭环

目标：

按固定顺序对 4 个工具逐个完成闭环研究，保证主线始终只有一个当前工具。

思路：

真正的 deep research 主体都放在这里做，但不是无边界搜索，而是“当前工具内发散，当前工具内收口”。

验收：

- 4 个工具都各自完成一轮闭环
- 每个工具都有完整的 intake、validation、summary
- 不发生“某工具只扫了线索却没收口就切到下一个工具”的情况

具体执行：

- 严格按 `Codex CLI -> Claude Code -> Cursor -> OpenCode` 顺序推进
- 对每个工具都执行 Step 2A 到 Step 2F
- 对应输出分别落到：
  - `10_codex_cli/`
  - `20_claude_code/`
  - `30_cursor/`
  - `40_opencode/`
- 每次进入新工具前先更新状态，标记对应 tool phase 的 entry

### Step 2A. Tool Baseline

目标：

在当前工具内部确认：现有 practice 已经讲了什么，还没讲什么。

思路：

先看当前工具自己的实践体系，再看外部材料，能减少后续误判“其实已经写过”的情况。

验收：

- 当前工具已有 practice 覆盖面清楚
- 已知空白和薄弱点被列出

具体执行：

- 复核当前工具目录中的 `practices/`
- 记录与全局基线相比的特殊点和空白点

### Step 2B. Tool External Scan

目标：

在当前工具的固定主窗口内搜集新信号。

思路：

先广泛 intake，再保留来源和日期，不急于给判断。

验收：

- 当前工具形成一份带来源分层的 intake
- 能区分官方新增、KOL 方法、社区线索
- 当前工具的外部扫描满足停止条件，而不是无限延长

具体执行：

- 搜当前工具官方 docs / changelog / engineering posts
- 补可信 KOL、工程博客、论坛和讨论区
- 记录搜索 query、发现路径和访问时间
- 把线索沉淀到当前工具的 intake 材料
- 达到 `Research Budget And Stop Rules` 的停止条件后，结束外扫并进入候选提炼

### Step 2C. Tool Candidate Extraction

目标：

把当前工具的 intake 转成受控候选，而不是继续无限搜集。

思路：

这一步是当前工具内的第一次收敛，先筛掉纯功能点和纯命令点。

验收：

- 每个候选都能说清“它为什么是行为习惯，而不是功能点”
- 每个候选都能给出至少一个继续核实的理由
- 当前工具的候选数保持可控，不泛滥
- 每个候选都有最小 candidate card，而不是只剩口头标签

具体执行：

- 整理当前工具的 baseline 和 intake
- 产出当前工具的候选缺口清单
- 为每个活跃候选分配稳定 `candidate_id`
- 对每个候选加一句“为什么值得继续核实”
- 默认只保留最多 `7` 个活跃候选进入后续验证

### Step 2D. Tool Fact Validation

目标：

验证当前工具里的候选，是否真的有工具事实支撑。

思路：

practice 不是抽象空想，必须回到当前工具的正式能力、入口和边界。

验收：

- 当前工具的每个候选都能说明是 `支持 / 部分支持 / 不支持`
- 重要判断都有可追溯材料
- 不把“社区习惯”误写成“工具正式能力”
- 每个候选至少记录一条最强支撑证据和一条主要限制或反例

具体执行：

- 针对当前工具的候选逐项回查官方资料与高质量外部证据
- 形成当前工具的 validation 结果
- 在 candidate card 中写清 `strong_tool`、支持等级和主要反证

### Step 2E. Tool Focused Deep Dive

目标：

只对当前工具内最有价值但仍模糊的候选做定向深挖。

思路：

深挖是为了减少误判，不是为了延长搜索时间。

验收：

- 深挖对象数量受控，不泛滥
- 每个深挖对象都补齐了关键证据缺口
- 深挖后能明显提高当前工具候选判断的清晰度
- 默认不超过 `3` 个深挖对象，除非已记录超预算理由

具体执行：

- 选择当前工具中高信号但仍模糊的候选
- 追加官方资料、实践案例、跨来源佐证
- 把深挖结果继续存到当前工具目录的 `validation/`
- 如果深挖没有明显改变判断，要明确写出“没有改变什么”

### Step 2F. Tool Exit Summary

目标：

在离开当前工具前，把当前工具的研究结果收成一份可复用结论。

思路：

工具级收口是整个长任务能否稳定推进的关键。如果工具结束时没有摘要，后面跨工具收敛会非常混乱。

验收：

- 当前工具有一份简明 summary
- 已明确哪些点进入跨工具候选池
- 已明确哪些点只属于当前工具
- 已明确哪些点被搁置
- 已明确哪些点被 reject，防止后续重复研究

具体执行：

- 为当前工具写出简明收口摘要
- 把需要跨工具比较的点移入跨工具候选池
- 把仅限当前工具的点标明 agent-specific
- 把已明确排除的点标为 `reject`
- 把摘要存到当前工具目录的 `summary/`
- 更新 `INDEX.md` 与 `STATUS_LOG.md`

### Step 3. 做跨工具收敛

目标：

在四个工具都完成闭环后，再统一比较哪些候选是真正跨工具成立的。

思路：

跨工具收敛必须发生在每个工具都各自收口之后，而不是边做边混。
这里的“收敛”是比较和分型，不是强行统一。

验收：

- 能清楚区分：
  - 跨工具成立的候选
  - 只在单工具成立的候选
  - 看起来新但其实不可推广的候选
- 不会因为想追求整齐，就把 vendor-specific 的东西硬并掉

具体执行：

- 先更新状态，进入 `Phase 6`
- 汇总 4 个工具的 exit summaries
- 对跨工具近义候选做聚类，但保留稳定 `candidate_id` 与 alias 记录
- 做跨工具候选对照
- 去掉重复命名和近义重叠
- 明确保留那些“互相有启发，但不应写成一样”的差异
- 把跨工具比较结果存到 `01_global/cross_tool/`
- 更新 `INDEX.md` 与 `STATUS_LOG.md`

### Step 4. 判断是新增、融合、搁置还是排除

目标：

把候选点从“看起来有意思”变成结构化结论：新增 practice、并入旧 practice、暂时搁置，还是明确排除。

思路：

真正有价值的 practice，应该具备稳定性、跨场景复用性、可操作性和明显收益。

验收：

- 每个候选都有明确归类
- 归类理由能被复核，而不是主观喜好
- 不会因为某个点新鲜就强行升格

具体执行：

- 先更新状态，进入 `Phase 7`
- 为每个候选做 4 类判断：`Promote / Merge / Hold / Reject`
- 同时记录判断理由：
  - 是否真的改变行为
  - 是否足够稳定
  - 是否跨工具成立
  - 是否和现有章节高度重叠
  - 是“证据暂时不足”还是“已经明确不成立”
- 将归类结果分别落到：
  - `90_review_queue/promote/`
  - `90_review_queue/merge/`
  - `90_review_queue/hold/`
  - `90_review_queue/reject/`
- 更新 `INDEX.md` 与 `STATUS_LOG.md`

### Step 5. 设计未来落章方式

目标：

把候选判断翻译成落位方案，明确哪些要新建、哪些要并入、哪些只落到 `commands/` 或 `reference/`、哪些保持 `no-op`，而不是直接补文件。

思路：

有些内容更适合作为新章节，有些更适合只补进既有章节；结构先想清楚，后面执行才不会反复返工。

验收：

- 每个 `Promote` 候选都有建议落位
- 每个 `Merge` 候选都知道应并到哪些现有章节
- 每个 `Hold / Reject` 候选都已明确是否需要 `no-op` 记录
- 不出现明显重复章节或职责冲突
- `apply_matrix` 至少精确到文件 / 章节，而不是只到工具目录

具体执行：

- 先更新状态，进入 `Phase 8`
- 输出一份“候选落章设计”
- 为每个候选同时写清 `decision_status` 与 `landing_action`
- 说明它们更适合：
  - 成为跨工具通用 practice
  - 成为某一工具的 agent-specific practice
  - 只在 `commands/` 或 `reference/` 层表达
- 对 `Hold / Reject` 且本轮不应落文的候选，明确写成 `landing_action = no-op`
- 同时生成 `90_review_queue/apply_matrix/`，明确哪些候选建议 apply 到哪一个正式工具目录、哪个文件、哪个章节、采用哪种落位动作
- 更新 `INDEX.md` 与 `STATUS_LOG.md`

### Step 6. 形成最终调研结论

目标：

把整个调研压缩成一个可执行结论，告诉后续整理阶段应该补什么、不补什么、先补什么。

思路：

最后的结论不应是一堆分散笔记，而应是一份足够清晰的后续施工输入。

验收：

- 能明确回答“我们还有没有缺 practice”
- 能列出优先级最高的补充方向
- 能区分通用新增与工具特有新增

具体执行：

- 先更新状态，进入 `Phase 9`
- 汇总 `additional_practices/` 中的研究材料
- 生成一份最终结论稿
- 作为后续真正落文和改目录结构的输入
- 最终结论稿放到 `99_final/final_recommendation.md`
- 最后一次更新 `INDEX.md` 与 `STATUS_LOG.md`

---

## Decision Standard For A New Practice

一个候选想升级为真正的 practice，至少要同时满足大部分以下条件：

- 它改变的不是一句提示词，而是一种稳定工作方式
- 它能明显降低失败率、返工率、上下文浪费或协作混乱
- 它不是某个瞬时版本的小技巧
- 它能够被清晰讲明“何时用、如何用、何时不用”
- 它在至少一个强工具上被充分支撑，并且对其他工具有可迁移意义

补充说明：

- `强工具` 指的是对该候选拥有最强原生支撑和最清晰证据锚点的工具
- “对其他工具有可迁移意义”不等于“其他工具必须照抄”
- 一个候选完全可以满足：
  - 在 A 工具里升格为正式 practice
  - 在 B 工具里只作为 merge
  - 在 C / D 工具里暂时 hold

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
