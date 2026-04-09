# 2026-04-09 claude_code__agent_teams_threshold deep dive

Run ID: `2026-04-09_additional_practices_r1`
Tool Candidate: `claude_code__agent_teams_threshold`

## 为什么深挖这个点

这个候选最容易被写偏。

- 官方能力事实层已经成立
- 但 adoption 层远弱于 plan-first、worktree 和 memory layering
- 如果不单独深挖，很容易把“Claude Code 已经有 agent teams”误写成“普通并行任务就该上 teams”

## 深挖后的判断

当前更准确的判断是：

1. agent teams 是真实能力，不是社区幻觉
   - 官方 docs 已明确 teammates、shared task list、独立 sessions 和 direct communication
2. 但它仍是高门槛能力
   - 官方仍把它放在 experimental 语境
   - 成本明显更高
   - orchestration 负担更重
3. 当前默认路径仍应是：
   - 先 subagents
   - 再 worktree isolation
   - 只有在 workers 需要彼此直接通信、共享任务板、由 lead 显式编排时才上 teams

## 这次深挖没有改变什么

- 没有把 agent teams 降成 `reject`
- 没有把它升级成跨工具 canonical 候选
- 没有把它写成 Claude Code 的默认并行工作法

## 这次深挖改变了什么

- 把这个候选从“文档里有、采用层不明”收紧成：
  - `partially_supported`
  - `agent-specific`
  - 更像 `13` 章里的边界守则，而不是独立新增 practice
