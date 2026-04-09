# 2026-04-09 OpenCode Additional Practices Scan

Run ID: `2026-04-09_additional_practices_r1`
Tool: `OpenCode`
Step: `Step 2B. Tool External Scan`
Pass: `Tier A + early Tier B/C`
Effective Window: `2026-01-01` to `2026-04-09`
Accessed: `2026-04-09`

## 本轮查询与发现路径

主要查询：

- `OpenCode 2026 changelog agents commands share skills server github`
- `OpenCode plan agent skills share attach workflow`
- `OpenCode GitHub Action schedule review issue triage`
- `OpenCode workflow skills plan-first`

发现路径：

1. 先从 OpenCode 官方 docs、官方 changelog、官方 GitHub integration docs 抓 capability facts
2. 再补 OpenCode 生态里的高信号开源 workflow，优先看明确写 plan-first、remote/local、share 或 workflow packaging 的项目
3. 回到现有 `practice_opencode/` 基线，判断哪些新事实已经足以改变行为叙述

## Source Ledger

| Date / State | Tier | Source | Why it matters |
| --- | --- | --- | --- |
| docs, updated 2026-04-07 | A | [OpenCode Commands](https://opencode.ai/docs/commands/) | built-in commands 与 custom commands 的事实锚点 |
| docs, updated 2026-04-05 | A | [OpenCode Agents](https://opencode.ai/docs/agents/) | `plan` / `build` / `general` / `explore` 与 permission override 的事实锚点 |
| docs, updated 2026-04-05 | A | [OpenCode Config](https://opencode.ai/docs/config/) | `default_agent`、share mode、custom commands、snapshot、permission 的当前配置面 |
| docs, updated 2026-04-06 | A | [OpenCode Web](https://opencode.ai/docs/web/) | web client 和 attach 的正式入口 |
| docs, updated 2026-04-06 | A | [OpenCode Server](https://opencode.ai/docs/server/) | shared backend、OpenAPI、multi-client、`/tui` endpoint 的事实锚点 |
| docs, updated 2026-04-07 | A | [OpenCode Share](https://opencode.ai/docs/share/) | share / unshare、公开链接、retention、安全边界 |
| docs, updated 2026-04-05 | A | [OpenCode Rules](https://opencode.ai/docs/rules/) | `/init`、`AGENTS.md`、`instructions`、remote URL、lazy loading 边界 |
| docs, updated 2026-02 | A | [OpenCode Agent Skills](https://opencode.ai/docs/skills) | first-class skills、on-demand loading、`.claude/skills` / `.agents/skills` compatibility |
| docs, accessed 2026-04-09 | A | [OpenCode GitHub Integration](https://dev.opencode.ai/docs/github/) | issue / PR / schedule / workflow_dispatch 上的 durable automation surface |
| 2026-04-04 | A | [OpenCode Changelog](https://opencode.ai/changelog) | review modes、remote server switching、agent & skill ordering、tool discovery 等近期变化 |
| accessed 2026-04-09 | B | [OpenWork](https://github.com/different-ai/openwork) | 高星生态项目，明确把 OpenCode 用成 local/remote host + sessions + permissions + templates 工作流 |
| latest release 2026-01-30 | B | [OpenAgentsControl](https://github.com/darrenhinde/OpenAgentsControl) | 明确围绕 OpenCode 构建 plan-first approval-based workflow 的开源框架 |

## Tier A 关键事实

### 1. `plan` agent 已经是 OpenCode 的正式开局层，不只是一个模式名称

当前官方 agents docs 明确：

- `build` 是默认 primary agent
- `plan` 是受限 primary agent
- `plan` 默认把 file edits 和 bash 都设为 `ask`
- `default_agent` 可以直接设成 `plan`

Implication:

- `plan_first_kickoff` 在 OpenCode 上不是弱信号
- 它甚至比 Cursor 更显式，因为它是第一方 primary agent

### 2. OpenCode 的 command surface 已经是多层控制面

当前官方 docs 已明确至少三层：

- TUI built-ins
- built-in commands，例如 `/init`、`/review`
- project / global custom commands

加上：

- keybind layer
- MCP prompt commands

Implication:

- `command_surface_control_plane` 在 OpenCode 上不是“命令很多”这么简单
- 更像一套会话控制层

### 3. `serve` / `web` / `attach` 的 shared backend 是 OpenCode 的长程主轴

当前官方 server / web docs 明确：

- `opencode` 运行时会同时启动 TUI 和 server
- `opencode serve` 启动 standalone headless server
- `opencode web` 提供浏览器界面
- `opencode attach` 可接到已有 backend
- 同一 server 暴露 OpenAPI 3.1 spec，并支持 `/tui` endpoint

Implication:

- OpenCode 的 long horizon 主轴不是 detached worker
- 而是 multi-client shared state

### 4. OpenCode 当前已有官方 durable automation surface，但主要在外部 runner 上

官方 GitHub integration docs 明确：

- `/opencode` / `/oc` 可在 issue / PR 评论里触发
- 支持 `issue_comment`、`pull_request_review_comment`、`issues`、`pull_request`
- 还支持 `schedule` 与 `workflow_dispatch`
- scheduled workflows 需要显式 `prompt`
- 输出会到 logs 和 PRs

Implication:

- OpenCode 没有原生 scheduler，并不等于完全没有 durable automation 路径
- 更准确的说法是：
  - native long horizon = shared backend
  - durable automation = external GitHub Actions runner

### 5. share、export、import、handoff 的安全边界被写得更清楚了

当前官方 share docs 明确：

- `manual` / `auto` / `disabled`
- `/share` 生成公开链接
- `/unshare` 删除公开访问与相关数据
- shared conversations 会持续可访问，直到显式 unshare
- 企业可禁用、限制到 SSO、或 self-host

Implication:

- `memory_hierarchy_and_handoff` 在 OpenCode 上不只是 UX 习惯
- 还带明确的安全与 retention 边界

### 6. skills 在 OpenCode 上已经是第一方、按需加载的上下文层

当前官方 skills docs 明确：

- skills 通过原生 `skill` tool 按需加载
- 搜索 `.opencode/skills`、`.claude/skills`、`.agents/skills`
- project 和 global 路径都支持
- skills 是 reusable instructions，而不是一次性 prompt

再加上 changelog 的：

- `Agent & Skill Ordering`

Implication:

- `skills_as_lazy_context_layer` 是真实候选
- 这不是 Codex 独有叙事

## Tier A 时间窗内官方行为信号

在当前窗口内，官方资料共同强化了几条主线：

- Apr 2026 docs 更新把 `plan` agent、skills、commands、server、share 都写成更成熟的正式能力
- Mar/Apr changelog 强调：
  - git-backed review modes
  - remote server switching
  - agent & skill ordering
  - tool discovery
- 官方 GitHub docs 把 OpenCode 接进：
  - comment-driven fixes
  - PR review
  - scheduled tasks
  - issue triage

这说明：

- OpenCode 当前不该只被写成一个本地 TUI
- 它已经同时具备：
  - plan-first primary agent
  - multi-layer command surface
  - shared backend multi-client continuation
  - external runner durable automation

## Tier B/C 采用层信号

### 1. OpenWork 把 OpenCode 明确当成 local/remote workflow runtime

OpenWork README 当前明确强调：

- powered by OpenCode
- local-first, cloud-ready
- host mode / client mode
- sessions
- permissions
- templates
- local or remote server

Implication:

- shared backend / local-remote continuation 不是纯文档幻想
- 已经有高信号生态项目围绕这条线产品化

### 2. OpenAgentsControl 明确把 OpenCode 用在 plan-first approval-based workflow

当前 repo README 直接把：

- plan-first development workflows
- approval-based execution
- automatic testing / review / validation

写成核心卖点。

Implication:

- `plan_first_kickoff` 在 OpenCode 上已经有明确生态采用信号

### 3. 官方 GitHub integration 本身也在推动“外部 durable automation”

虽然这是官方 surface，不是第三方复盘，但它已经把：

- scheduled review
- issue triage
- PR review
- comment-triggered implementation

写成正式 workflow。

Implication:

- `durable automation threshold` 在 OpenCode 上不能只按“native scheduler 缺失”来写

## 当前最强候选方向

1. `plan_first_kickoff`
2. `command_surface_control_plane`
3. `shared_backend_long_horizon_layering`
4. `external_runner_durable_automation_threshold`
5. `memory_hierarchy_and_handoff`
6. `bounded_subtask_orchestration`
7. `skills_as_lazy_context_layer`

## 当前更像 merge 的方向

- `03` 先规划后执行
- `04` rules / instructions / skills layering
- `09` share / export / handoff
- `10` 多智能体分流
- `12` 交互命令层
- `13` 高级 agentic 协调

## 当前判断

OpenCode 当前也更像：

- 强化既有章节
- 纠正“只有 shared backend、没有 durable automation”的过窄叙述
- 把 plan-first、command-layer 和 skills 提升成更明确的行为层讨论

而不是明显需要一个全新正式章节。

下一步可进入 `Step 2C. Tool Candidate Extraction`。
