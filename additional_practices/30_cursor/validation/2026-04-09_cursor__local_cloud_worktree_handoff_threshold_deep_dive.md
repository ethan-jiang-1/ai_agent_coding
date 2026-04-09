# 2026-04-09 cursor__local_cloud_worktree_handoff_threshold deep dive

Run ID: `2026-04-09_additional_practices_r1`
Tool Candidate: `cursor__local_cloud_worktree_handoff_threshold`

## 为什么深挖这个点

这个候选直接卡在旧基线和新官方事实之间。

- 旧的 `practice_cursor/` 基线把 Cursor 主要写成 remote branch / cloud worker 叙事
- 但 `2026-04-02` 的 Cursor 3 已经把 local、cloud、worktree、remote SSH 纳入同一个 Agents Window

如果这里不深挖，后面很容易出现两种错误：

- 继续沿用“Cursor 没有官方 worktree agent surface”的旧说法
- 或者反过来，把新能力过度写成“以后默认都该切 worktree / cloud”

## 深挖后的判断

当前更准确的判断是：

1. Cursor 的多环境 handoff 已经是官方产品主叙事
   - Cursor 3 明确强调 unified workspace、multi-workspace、local <-> cloud handoff
2. 但它还不是一个“稳定到足以彻底改写旧章节”的成熟默认法
   - 对外 adoption 仍明显早于 Claude Code `--worktree` 这种成熟 CLI surface
   - 公开 workflow 复盘主要仍来自官方和新界面 rollout 讨论
3. 因此这条候选当前更适合作为：
   - `partial support`
   - 对 `10` / `13` 章的校正与升级
   - 后续跨工具比较中的重要环境切层候选

## 这次深挖没有改变什么

- 没有把 Cursor 改写成 worktree-first 工具
- 没有把 cloud handoff 写成所有任务都该走的默认路径

## 这次深挖改变了什么

- 明确旧基线已经部分过时
- 把这个候选从“也许只是新 UI”收紧成：
  - 有正式事实层支撑
  - adoption 仍早期
  - 值得保留进入后续跨工具收敛
