# 2026-04-09 opencode__bounded_subtask_orchestration deep dive

Run ID: `2026-04-09_additional_practices_r1`
Tool Candidate: `opencode__bounded_subtask_orchestration`

## 为什么深挖这个点

OpenCode 容易被误读成：

- 既然有 `@general`、`@explore`、`subtask: true`
- 那就等于已经有 detached parallel worker 体系

但官方 docs 其实没有这么说。

## 深挖后的判断

当前更准确的判断是：

1. subtask / subagent 是正式能力
   - 适合把研究、review、只读探索从主线程切出去
2. 但它仍是 bounded orchestration
   - 结果需要回到主线程整合
   - 它不等于 background job
   - 也不等于 filesystem-isolated worker
3. 因此更稳的默认写法是：
   - 浅层拆分
   - 角色清晰
   - 主线程收口

## 这次深挖没有改变什么

- 没有把 OpenCode 降成“只有单 agent”
- 没有把 subtask 写成 detached worker

## 这次深挖改变了什么

- 把这个候选从一般性的“多 agent 存在”收紧成：
  - 一个有边界的 orchestration guardrail
  - 能与 Codex shallow delegation、Claude teams threshold 做更准确比较
