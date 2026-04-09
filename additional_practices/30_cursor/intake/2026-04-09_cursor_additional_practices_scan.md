# 2026-04-09 Cursor Additional Practices Scan

Run ID: `2026-04-09_additional_practices_r1`
Tool: `Cursor`
Step: `Step 2B. Tool External Scan`
Pass: `Tier A + early Tier C`
Effective Window: `2026-01-01` to `2026-04-09`
Accessed: `2026-04-09`

## 本轮查询与发现路径

主要查询：

- `Cursor 2026 changelog background agents automations memories`
- `Cursor custom modes commands history memories docs`
- `Cursor 3 agents window worktrees cloud local handoff`
- `Cursor long-running agents automations workflow 2026`

发现路径：

1. 先用 Cursor 官方 docs、官方 changelog、官方 blog 抓 capability facts
2. 再用官方 forum announcement / release discussion 补 adoption 早期信号
3. 对照现有 `practice_cursor/` 基线，重点看哪些官方新增已经足以改变行为叙述

## Source Ledger

| Date / State | Tier | Source | Why it matters |
| --- | --- | --- | --- |
| docs, accessed 2026-04-09 | A | [Cursor Background Agents](https://docs.cursor.com/en/background-agents) | remote agent、isolated machine、follow-up、take-over 的事实锚点 |
| docs, accessed 2026-04-09 | A | [Cursor Modes](https://docs.cursor.com/chat/custom-modes) | Ask / Agent / Custom Modes 的正式分工 |
| docs, accessed 2026-04-09 | A | [Cursor Commands](https://docs.cursor.com/en/agent/chat/commands) | quick commands / project commands / global commands 的事实锚点 |
| docs, accessed 2026-04-09 | A | [Cursor History](https://docs.cursor.com/agent/chat/history) | tabs、history、`@Past Chats`、background chat 分层 |
| docs, accessed 2026-04-09 | A | [Cursor Memories](https://docs.cursor.com/fr/context/memories) | per-project memories、approval gate、sidecar extraction |
| 2026-02-12 | A | [Long-running Agents in Research Preview](https://cursor.com/changelog/02-12-26/) | plan-first long-horizon 的官方 capability signal |
| 2026-02-12 | A | [Expanding our long-running agents research preview](https://cursor.com/blog/long-running-agents) | 官方把 larger tasks / days-long delegation 写成产品主叙事 |
| 2026-02-24 | A | [Cursor agents can now control their own computers](https://cursor.com/blog/agent-computer-use) | cloud agent artifacts、remote desktop、multi-entry availability |
| 2026-02-26 | A | [The third era of AI software development](https://cursor.com/blog/third-era) | Cursor 官方对 adoption shift 的总叙事与内部使用信号 |
| 2026-03-05 | A | [Automations](https://cursor.com/changelog/03-05-26/) | schedules / event triggers / memory-across-runs 的正式能力事实 |
| 2026-03-25 | A | [Self-hosted Cloud Agents](https://cursor.com/changelog/03-25-26/) | cloud agent capability 扩展到自有网络的事实锚点 |
| 2026-04-02 | A | [Meet the new Cursor](https://cursor.com/blog/cursor-3) | Agents Window、本地与 cloud handoff、多环境并行的核心变化 |
| 2026-04-02 | A | [New Cursor Interface](https://cursor.com/en-US/changelog) | Cursor 3 / Agents Window / worktrees / remote SSH 的 changelog 事实锚点 |
| 2026-02-12 | C | [Forum: Introducing long-running agents](https://forum.cursor.com/t/introducing-long-running-agents/151696) | 早期用户讨论 plan approval 与 multi-hour work |
| 2026-03-05 | C | [Forum: Introducing Cursor Automations](https://forum.cursor.com/t/introducing-cursor-automations/153733) | 早期用户围绕 always-on agents、memory、workflow template 的反馈 |
| 2026-04-02 | C | [Forum: Cursor 3: Agents Window](https://forum.cursor.com/t/cursor-3-agents-window/156509) | 用户实际尝试 multi-workspace / local-cloud handoff 的 rollout signal |

## Tier A 关键事实

### 1. Cursor 在当前窗口里已经从 IDE agent 扩展到 cloud agent orchestration

官方 Q1/Q2 2026 连续推出并强化了：

- long-running agents
- cloud agents with computer use and artifacts
- automations
- self-hosted cloud agents
- Cursor 3 Agents Window

Implication:

- Cursor 当前不该只被写成“IDE 里有 Ask / Agent 的编辑器”
- `long_horizon_control` 与 `advanced_agentic_coordination` 在 Cursor 上已经明显变重

### 2. Background Agents 仍是正式 remote worker，而不是普通后台 tab

当前官方 docs 仍明确：

- asynchronous remote agents
- isolated ubuntu-based machine
- internet access
- install packages
- status / follow-up / take over

Implication:

- 前台 Agent 与远程 Background Agent 之间有真实阈值
- `remote_branch_isolation` 在 Cursor 上是正式能力，不只是 workflow 想象

### 3. Cursor 3 改变了“本地 vs cloud vs worktree”叙述

`2026-04-02` 的 Cursor 3 / Agents Window 官方 changelog 与 blog 已明确：

- run many agents in parallel
- across repos and environments
- locally
- in worktrees
- in the cloud
- on remote SSH
- local <-> cloud handoff

Implication:

- 现有 `practice_cursor/` 里“没有官方 editor worktree 命令面”的旧基线，需要在后续 validation 里重新界定
- 新的重点不再只是 “Background Agent = separate branch”
- 而是“如何在多环境 agent surface 之间切换和收口”

### 4. Ask / Agent / Custom Modes 与 Commands 构成的是柔性控制面

官方 docs 当前明确：

- Ask = read-only exploration
- Agent = autonomous execution
- Custom = user-defined tool combinations and instructions
- Commands = markdown-based project/global commands

Implication:

- `plan_first_kickoff` 与 `command_surface_control_plane` 在 Cursor 上可能成立
- 但其形态和 Claude Code 的 built-in control plane 不同，更像：
  - mode selection
  - reusable commands
  - lightweight workflow templating

### 5. history、memories、export、background chat 已经分成多层

官方 docs 当前明确：

- history 在本地 SQLite
- `@Past Chats` 可引用旧对话
- Background Agent chats 不在 regular history
- export chat to markdown
- Memories 是 per-project、需 approval 的自动生成记忆

Implication:

- `memory_hierarchy_and_handoff` 在 Cursor 上已经不是弱信号
- 尤其是 background / foreground chat 不同存储层，进一步强化了 handoff 文件的必要性

### 6. Automations 把 Cursor 推向真正的 workflow control plane

官方 `2026-03-05` changelog 明确：

- automations run on schedules
- or are triggered by Slack / Linear / GitHub / PagerDuty / webhooks
- spin up a cloud sandbox
- use configured MCPs and models
- access a memory tool across runs

Implication:

- Cursor 的 long horizon 已经不只是“过会儿回来看看后台任务”
- 它开始进入 always-on agent / event-driven workflow 叙事

## Tier A 时间窗内官方行为信号

官方 blog / changelog 在当前窗口共同强调了几条主线：

- Feb 12: long-running agents plan first and finish larger tasks
- Feb 24: cloud agents gain computer use、artifacts、multi-entry access
- Feb 26: agent usage in Cursor grew rapidly；官方直接说内部有相当比例 PR 来自 cloud VMs
- Mar 05: automations 把 agent 变成 schedule / event-driven service
- Mar 25: self-hosted cloud agents 把同一套能力推进到更强安全边界
- Apr 02: Cursor 3 把 local / cloud / worktree / remote SSH 收到同一个 Agents Window

这说明：

- Cursor 官方在 `2026 Q1` 里，主叙事已经从“IDE 助手”继续推向：
  - multi-environment agents
  - long-horizon delegation
  - workflow automation
  - human review + handoff between surfaces

## Tier C 早期采用层信号

### 1. long-running agents 的用户讨论已经默认接受“先计划再跑”

官方 forum announcement 明确把：

- planning before execution
- multi-agent checking against the plan
- hours or days of autonomous work

写成主要卖点。

Implication:

- 即使 Cursor editor 本身没有固定命名的 Plan Mode，plan-first 叙事在 cloud long-running surface 上已经更强

### 2. Automations 的早期讨论已经把 Cursor 放进持续 workflow

官方 forum announcement 里的示例直接围绕：

- every push security review
- overnight bug triage
- weekly digest

展开。

Implication:

- Cursor 的长程工作流不再只靠“人手动 follow-up”
- schedule / event-driven agent 现在是正式候选方向

### 3. Cursor 3 的用户讨论显示本地-cloud handoff 已开始进入真实操作层

官方 forum discussion 里，用户已经在尝试：

- Agents Window
- Open Agents Window
- multi-workspace view
- local/cloud 切换

同时也出现 rollout friction。

Implication:

- 这是真实行为变化，而不是只存在于宣传页
- 但仍处在新界面刚上线的早期稳定化阶段

## 当前最强候选方向

1. `plan_first_kickoff`
2. `command_surface_control_plane`
3. `local_cloud_worktree_handoff_threshold`
4. `long_horizon_cross_surface_layering`
5. `memory_hierarchy_and_handoff`
6. `automations_as_workflow_control_plane`

## 当前更像 merge 的方向

- `03` 先规划后执行
- `09` memory / handoff / export / replay
- `10` 前台与后台分流
- `12` interactive command layer
- `13` 高级 agentic 协调

## 当前判断

Cursor 当前也更像：

- 强化既有章节
- 纠正旧基线里已经开始过时的能力边界
- 把原本偏 editor 内的叙述，升级到 local / cloud / worktree / automation 的多环境框架

而不是明显需要全新章节。

当前一个特别需要在 `Step 2C / 2D` 明确处理的点是：

- `2026-04-02` 的 Cursor 3 已经引入官方 worktree / multi-environment 叙事
- 这会直接影响原有 `practice_cursor/` 对“Cursor 不具备第一方 worktree agent surface”的判断

下一步可进入 `Step 2C. Tool Candidate Extraction`。
