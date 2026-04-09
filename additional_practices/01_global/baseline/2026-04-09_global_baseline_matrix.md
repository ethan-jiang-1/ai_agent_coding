# 2026-04-09 Global Baseline Matrix

Run ID: `2026-04-09_additional_practices_r1`
Effective Window: `2026-01-01` to `2026-04-09`

## 目的

在进入逐工具 deep research 前，先把当前仓库里已经存在的 4 个正式工具目录锚定为“当前基线”，避免后续把已存在表达误判为新增缺口。

## Baseline Snapshot

| Tool | README | practices | commands | reference | 当前骨架 | 主要现实差异 |
| --- | --- | ---: | ---: | ---: | --- | --- |
| Codex CLI | yes | 13 | 13 | 2 | `11` 个原始习惯 + `12` 交互式专属能力 + `13` 高级 agentic 协调 | `codex` 交互式、`codex exec`、`resume`、`AGENTS.md`、sandbox/approval、subagents、外部 worktree |
| Claude Code | yes | 13 | 13 | 2 | `11` 个原始习惯 + `12` 会话内 slash 操作 + `13` 高级 agentic 协调 | slash command 面、`CLAUDE.md` / `.claude/rules`、permission modes、auto-memory、subagents、worktree、agent teams、schedule |
| Cursor | yes | 13 | 13 | 2 | `11` 个原始习惯 + `12` custom/interactive command layer + `13` 高级 agentic 协调 | tabs/history、`Ask` / `Agent` / `Custom Modes`、rules/memories、quick commands、checkpoints、Background Agents、Max Mode |
| OpenCode | yes | 13 | 13 | 3 | `11` 个原始习惯 + `12` TUI 会话内操作 + `13` 高级 agentic 协调 | TUI slash commands、`plan/build` primary agents、subagents、`share/export/import`、`instructions`、共享 backend、多客户端同状态 |

## Shared Baseline Axes

当前 4 个正式工具目录不是从零开始，而是都已经稳定落到下面这组轴线上：

| Axis | 初始来源 | 当前 4 工具现状 | Step 1 判断 |
| --- | --- | --- | --- |
| `01` 会话生命周期 | `initial_practices/round2_detailed/01_session_lifecycle.md` | 4 个工具都已落在 `practices/` 和 `commands/` | 稳定基线，不是空白 |
| `02` 纠错重试 | `initial_practices/round2_detailed/02_edit_not_append.md` | 4 个工具都已覆盖 | 稳定基线，不是空白 |
| `03` 先规划后执行 | `initial_practices/round2_detailed/03_plan_before_code.md` | 4 个工具都已覆盖 | 稳定基线，不是空白 |
| `04` 规则分层 | `initial_practices/round2_detailed/04_rule_hierarchy.md` | 4 个工具都已覆盖，规则载体不同 | 稳定基线，但工具实现差异很大 |
| `05` 渐进式信息投喂 | `initial_practices/round2_detailed/05_progressive_feed.md` | 4 个工具都已覆盖 | 稳定基线，后续主要观察是否出现新的打包方式 |
| `06` 稳定前缀 / 历史压缩 | `initial_practices/round2_detailed/06_kv_cache.md` | 4 个工具都已覆盖，但实现差异显著 | 稳定基线，后续主要观察工具新入口是否改变用法 |
| `07` 模型路由 | `initial_practices/round2_detailed/07_model_routing.md` | 4 个工具都已覆盖 | 稳定基线，可能出现 provider / mode 维度的新行为 |
| `08` 工具权限克制 | `initial_practices/round2_detailed/08_tool_permission.md` | 4 个工具都已覆盖 | 稳定基线，后续主要看权限是否进一步变成 workflow 设计问题 |
| `09` 跨会话状态传递 | `initial_practices/round2_detailed/09_memento_workflow.md` | 4 个工具都已覆盖 | 稳定基线，后续重点看 share/resume/export/memory 的新分工 |
| `10` 多智能体分流 | `initial_practices/round2_detailed/10_multi_agent.md` | 4 个工具都已覆盖 | 稳定基线，但可能被 2026 窗口的新 agentic 能力改写 |
| `11` 成本与额度 | `initial_practices/round2_detailed/11_billing_tips.md` | 4 个工具都已覆盖 | 稳定基线，后续重点看更细的 route/cost control |
| `12` 交互命令层 / 会话内操作层 | 后续扩展章节 | 4 个工具都已新增，但命名和功能边界不同 | 高优先审计区，可能只适合 agent-specific 落文 |
| `13` 高级 agentic 协调 | 后续扩展章节 | 4 个工具都已新增 | 高优先审计区，最可能承接 2026 窗口的新行为变化 |

## 既有内容落位分工

当前正式目录的职责分工已经相对清晰：

- `README.md`
  - 负责入口说明、阅读顺序、该工具的现实边界摘要
- `practices/`
  - 负责稳定行为习惯层
- `commands/`
  - 负责命令面、交互入口、配置片段、操作细节和典型误写点
- `reference/`
  - 负责当前事实边界、命令面映射和高级能力 grounding

这意味着本轮新增 research 默认先回答下面两个问题，而不是直接加章节：

1. 这是行为习惯，还是只是命令面补充
2. 它应该进入 `practice`、`merge`、`reference-only`，还是 `no-op`

## 各工具当前最显著的结构特征

### Codex CLI

- 会话形态明确分为交互式 `codex`、非交互式 `codex exec`、以及 `resume`
- 规则层集中在 `AGENTS.md` 发现链与 `CODEX_HOME` 稳定前缀
- 第 `12` 章和第 `13` 章明显围绕 interactive-only commands、subagents、外部 worktree 与长程运行边界展开

### Claude Code

- 已显式把 built-in slash command surface 融进总体结构
- `permission mode`、`allowedTools`、`.claude/rules`、auto-memory、subagents/worktree 共同影响行为层
- 第 `12`、`13` 章已经开始承接 interactive-only、agent teams、schedule 等更高级能力

### Cursor

- 基线不是 CLI session，而是 tabs/history + IDE mode 切换
- `Ask` / `Agent` / `Custom Modes`、`@` 引用、memories、checkpoints、Background Agents 共同构成行为面
- 第 `12` 章明显偏向“什么时候值得把高频操作做成 command layer”，这和其他 3 个工具并不完全同构

### OpenCode

- 当前结构已经把 TUI built-ins、`plan/build`、subagents、share/export/import、shared backend 等事实纳入
- 第 `12` 章与第 `13` 章更偏向会话内控制和多客户端同状态协作
- `worktree` 与 `scheduler` 在当前基线中反而更多以“缺失边界”方式存在

## Step 1 结论

- 当前基线已经成熟，不存在“4 个工具目录还没整理”的问题。
- 这轮 research 更可能发现的是：
  - 某些行为是否值得从局部补充升级为正式 practice
  - 某些新能力是否只需要 merge 进第 `12` / `13` 章或既有章节
  - 某些看似新鲜的点其实应该只落到 `commands/` 或 `reference/`
- 下一步不应直接跨话题搜，而应严格按 `tool-first` 进入 `Codex CLI` 工具闭环。
