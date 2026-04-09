# 2026-04-09 Claude Code Additional Practices Scan

Run ID: `2026-04-09_additional_practices_r1`
Tool: `Claude Code`
Step: `Step 2B. Tool External Scan`
Pass: `Tier A + early Tier B`
Effective Window: `2026-01-01` to `2026-04-09`
Accessed: `2026-04-09`

## 本轮查询与发现路径

主要查询：

- `Claude Code 2026 release notes`
- `Claude Code worktree agent teams schedule docs`
- `Claude Code workflow 2026 blog`

发现路径：

1. 先从 Anthropic 官方 docs、官方 webinar、官方 changelog 汇总 capability facts
2. 再补高信号 practitioner workflow，优先看能展示具体 session / plan / worktree / skills / handoff 节奏的写作

## Source Ledger

| Date / State | Tier | Source | Why it matters |
| --- | --- | --- | --- |
| docs, accessed 2026-04-09 | A | [Claude Code built-in commands](https://docs.anthropic.com/en/docs/claude-code/slash-commands) | 当前 built-in command surface 的事实锚点 |
| docs, accessed 2026-04-09 | A | [Claude Code subagents](https://docs.anthropic.com/en/docs/claude-code/sub-agents) | subagents、独立 context window、团队共享 workflow |
| docs, accessed 2026-04-09 | A | [Identity and Access Management](https://docs.anthropic.com/en/docs/claude-code/team) | permission modes、组织级 fine-grained permission、working directories |
| docs, accessed 2026-04-09 | A | [Claude Code GitHub Actions](https://docs.anthropic.com/en/docs/claude-code/github-actions) | durable automation、`@claude`、`claude_args`、`--max-turns` surface |
| docs, accessed 2026-04-09 | A | [Claude Code overview](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview) | 当前产品定位、CI / automation、Unix philosophy |
| changelog, accessed 2026-04-09 | A | [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/raw/refs/heads/main/CHANGELOG.md) | 当前窗口内持续新增的 capability facts |
| 2026-01-27 | A | [Claude Code for Service Delivery webinar](https://www.anthropic.com/webinars/claude-code-service-delivery) | 官方直接强调 long-horizon、复杂迁移、best delivery teams |
| 2026-03-24 | A | [Claude Code Advanced Patterns webinar](https://www.anthropic.com/webinars/claude-code-advanced-patterns) | 官方把 subagents、MCP、真实代码库扩展作为专门主题 |
| 2026-03-31 | A | [Claude Code for Semiconductor Teams webinar](https://www.anthropic.com/webinars/claude-code-for-semiconductor-teams) | 官方强调 project-level task execution 和工程环境集成 |
| 2026-04-07 | A | [What We Shipped webinar](https://www.anthropic.com/webinars/what-we-shipped-feature-updates-tips-and-live-q-a-with-the-claude-code-team) | 官方承认近期 feature cadence 和 day-to-day workflow tips 是主要 adoption 主题 |
| 2026-02-10 | B | [Boris Tane: How I Use Claude Code](https://boristane.com/blog/how-i-use-claude-code/) | 资深工程负责人长期实战，总结出严格 research -> plan -> annotate -> implement 流程 |
| 2026-01-05 | B | [Rungie: Claude Code Part 3 - The Workflow](https://rungie.com/blog/claude-code-workflow/) | months of daily use，总结 plan-first、parallel sessions、git worktrees、skills |
| 2026-02-xx | B | [Joe Hu: My Claude Code Workflow](https://cc.hubeiqiao.com/) | 非传统工程背景下的真实构建流程，强调 plan mode、context hygiene、multi-session |

## Tier A 关键事实

### 1. Claude Code 的 built-in command surface 已经是显式控制平面

当前官方 built-in command 文档明确列出：

- `/agents`
- `/permissions`
- `/plan`
- `/resume`
- `/rewind`
- `/status`
- `/tasks`
- `/ultraplan`
- `/mcp`
- `/doctor`

Implication:

- `command_surface_fluency` 在 Claude Code 上不是边缘补充，而是正式行为面
- 但命令数量多，不等于每个命令都值得升格为 practice

### 2. `--worktree` 是第一方 CLI 能力，不是外部 Git 补丁

当前官方文档和现有基线都明确：

- `--worktree` / `-w` 是正式 CLI flag
- subagent frontmatter 可写 `isolation: worktree`
- worktree hook 和 cleanup 都是第一方 surface 的一部分

Implication:

- `workspace_worktree_isolation` 在 Claude Code 的能力事实层非常强
- 这比 Codex CLI 更可能推动实践升级

### 3. long horizon 明显分成 session-scoped 与 durable 两层

官方材料已经分出：

- `--continue` / `--resume` / `--fork-session`
- `/loop`
- Cloud / Desktop / GitHub Actions durable scheduling

GitHub Actions 文档还明确：

- `claude_args` 可透传
- `--max-turns` 应配合 timeout 与 concurrency controls 使用

Implication:

- `long_horizon_control` 在 Claude Code 上不是单点 feature，而是多 surface 阈值判断

### 4. agent teams 已经是官方文档能力，但仍需严格对待 experimental 边界

当前官方文档和现有基线都明确：

- agent teams 是 experimental
- teammates 是独立 sessions
- 可彼此通信
- 成本显著更高

Implication:

- `agent_teams_threshold` 是真实候选
- 但很可能只能 merge 到 `13`，不应被写成默认推荐主路径

### 5. permission / policy / hooks 已经明显 workflow 化

当前官方 team / IAM 文档明确：

- `default`
- `acceptEdits`
- `plan`
- `bypassPermissions`

当前 changelog 还显示：

- `PermissionRequest` hook
- permission rules、managed settings、hooks 输出和 settings 刷新持续被强化

Implication:

- Claude Code 的权限问题同样更像 workflow 设计，而不是单一参数

## Tier A 时间窗内官方行为信号

官方 webinar 与 changelog 在当前窗口共同强调了几条主线：

- Jan 27: long-horizon、复杂迁移、delivery teams
- Mar 24: subagents、MCP、scaling to real codebases
- Mar 31: project-level task execution、工程环境集成
- Apr 7: recent shipped features + the workflows the team actually uses

这说明：

- Anthropic 自己在 2026 Q1 里，把 Claude Code 的主叙事从“会写代码的 CLI”继续推向了：
  - project-level execution
  - multi-agent coordination
  - durable automation
  - control-plane fluency

## Tier B 采用层信号

### 1. 高信号用户已经把“先计划、再批注、后执行”写成硬纪律

Boris Tane 的 2026-02-10 实战文非常明确：

- 先 deep-read
- 研究结果必须落盘
- 计划必须单独落盘
- 计划会经历 1-6 轮人工批注
- 只有在“don’t implement yet”解除后才进入实现

Rungie 的 2026-01-05 workflow 文同样强调：

- `defaultMode: "plan"` 是默认起点
- skip plan mode 会浪费时间和上下文

Joe Hu 也把：

- raw text -> plan mode -> ask clarifying questions -> document review

写成固定流程。

Implication:

- `session_kickoff_discipline` 在 Claude Code 上有明确重复 adoption signal

### 2. parallel sessions + worktree isolation 已经被真实用户当成主要手段

Rungie 明确说自己通常并行开 2-3 个 Claude Code sessions，并把这解释为对抗 context pollution 的主要方法。

同一篇还直接建议：

- 想获得更强能力时，用 git worktrees

Implication:

- `workspace_worktree_isolation` 不只是官方 feature
- 已经在高信号工作流里进入主路径

### 3. skills / agents / project personality 已经开始按项目类型分化

Rungie 把 3 个项目配成 3 种不同 personality：

- 配置项目
- 强约束 TypeScript 项目
- skill-heavy 内容工作流项目

他同时区分：

- skills 更轻
- agents 更重
- orchestrator 仍要负责收口

Implication:

- `context_packaging`、`skills_over_agents_bloat`、`agent_teams_threshold` 都有早期 adoption signal
- 但 agent teams 自身的 adoption 仍弱于 plan-first / parallel sessions

### 4. context hygiene 不是抽象原则，而是具体纪律

Joe Hu 明确写：

- 最后 20% context 是毒药
- 把任务拆小
- 一次只实现一个任务
- 记录状态
- 手动 compact
- 多 session 并行

Implication:

- `context_packaging` 和 `recovery_replay_resume_hygiene` 在 Claude Code 上都不是弱信号

## 当前最强候选方向

1. `session_kickoff_discipline`
2. `command_surface_fluency`
3. `workspace_worktree_isolation`
4. `long_horizon_control`
5. `agent_teams_threshold`
6. `context_packaging`

## 当前更像 merge 的方向

- `01` 会话生命周期
- `03` 先规划后执行
- `08` 权限策略
- `09` memory / resume / handoff
- `10` 多智能体分流
- `12` 交互式操作层
- `13` 高级 agentic 协调

## 当前判断

Claude Code 当前同样更像：

- 强化既有章节
- 把某些已有 command/reference 表达抬升为更明确的 practice 叙述

而不是明显需要全新章节。

下一步可进入 `Step 2C. Tool Candidate Extraction`。
