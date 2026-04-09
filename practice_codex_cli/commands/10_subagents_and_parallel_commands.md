# 10 Subagents 与并行拆分

## 内置 subagents

当前 Codex 自带这些角色：

- `default`
- `worker`
- `explorer`

适合：

- `explorer`：只读探索、读多文件、产出摘要
- `worker`：按明确边界执行实现

## 控制并行深度

```toml
[agents]
enabled = true
max_threads = 6
max_depth = 1
job_max_runtime_seconds = 120
```

建议先从 `max_depth = 1` 开始，避免代理继续派代理，把上下文和成本放大。

## 项目级自定义 agent

```text
.codex/agents/reviewer.toml
```

示例：

```toml
name = "reviewer"
description = "Reviews diffs and lists concrete risks."
model = "<model>"
sandbox_mode = "read-only"

developer_instructions = """
Focus on bugs, regressions, and missing tests.
Do not propose broad refactors unless they block correctness.
"""
```

## 提示词里的分工方式

```text
先让 explorer 阅读 src/auth/ 和 tests/auth/，输出不超过 300 字的结构摘要；
再让 worker 只修改 src/auth/middleware.ts；
最后返回改动说明和剩余风险。
```

## 交互式对应

```text
/agent
/review
```

映射关系：

- `/agent` ↔ Exact / Combined mapping
  是交互态里进入 subagents 能力的核心入口
- `/review` ↔ Exact mapping
  与外部 `codex review` 高度对应

这章里，交互态比 shell 外更适合临场拆角色、派探索代理和发起审查。

## 当前要明确写出的边界

- `multi_agent` 当前本机是 `stable true`
- `enable_fanout` 当前本机仍是 `under development false`
- child agent 会继承 parent agent 的审批与沙箱边界

所以这里适合写“可控拆分”，不适合写“默认激进 fan-out”。

## 保守做法：不用 subagent，也能做隔离

```bash
codex exec --ephemeral -s read-only "分析模块 A" -o /tmp/a.md
codex exec --ephemeral -s read-only "分析模块 B" -o /tmp/b.md
cat /tmp/a.md /tmp/b.md | codex exec -s workspace-write "整合分析并实现最小改动"
```

## 这章最容易写错的地方

- 不要并行让两个会写文件的任务改同一块代码。
- 不要不给职责边界就让多个代理一起动手。
- 不要把 `fork` 当成真正无偏见的独立审查者。
- 不要把 subagent 误写成 worktree 隔离；真正的文件系统隔离见 `13_advanced_agentic_coordination_commands.md`。
