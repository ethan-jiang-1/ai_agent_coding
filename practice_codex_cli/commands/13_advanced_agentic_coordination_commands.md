# 13 高级 Agentic 协调命令

这一章承接三个会一起出现的新问题：

- subagents 怎么用
- 什么时候需要 worktree 级隔离
- 什么时候该用 detached long-running，而不是死守本地会话

## 先分清三种“隔离”

- 角色隔离：`/agent`、custom agents、多个 `codex exec`
- 文件系统隔离：外部 Git worktree
- 运行时隔离：`resume` / `fork` / `codex cloud`

这三者不是一回事。

## 1. 角色隔离：subagents

```text
/agent
/review
```

当前官方可确认：

- built-in agents：`default`、`worker`、`explorer`
- custom agents：`~/.codex/agents` 或 `.codex/agents`
- 当前本机 `multi_agent` 是 `stable true`
- 当前本机 `enable_fanout` 仍是 `under development false`

保守配置：

```toml
[agents]
enabled = true
max_threads = 6
max_depth = 1
job_max_runtime_seconds = 120
```

适合：

- 一个主代理做整合
- 一个 `explorer` 读多文件找线索
- 一个 `worker` 在明确边界里改代码

## 2. 文件系统隔离：CLI 当前靠外部 Git worktree

当前 Codex 官方 worktree 文档落在 `Codex App`，不是 `Codex CLI` 顶层命令面。

如果你在 CLI 里真的需要隔离写入，做法是：

```bash
git worktree add ../repo-auth -b codex-auth
codex -C ../repo-auth "只处理认证模块"
```

或者：

```bash
git worktree add ../repo-review -b codex-review
codex exec -C ../repo-review -s read-only "审查当前分支改动"
```

这层要明确：

- `git worktree` 是外部 Git 能力
- `codex -C <dir>` 是让 Codex 进入那个隔离目录
- 不要把这写成 Codex CLI 自带 `worktree` 子命令

## 3. 长程与 detached：`resume` / `fork` 不是 `cloud`

### 继续本地会话

```bash
codex resume --last
codex fork --last "保留历史，但走另一条实现路径"
```

适合：

- 人还在场
- 当前工作树不变
- 只是继续或分叉

### 提交 detached cloud task

```bash
codex cloud list --env <env-id>
codex cloud exec --env <env-id> --branch <branch> "执行这项任务"
codex cloud exec --env <env-id> --branch <branch> --attempts 3 "对同一任务做 best-of-3"
codex cloud status <task-id>
codex cloud diff <task-id>
codex cloud apply <task-id>
```

适合：

- 任务更像异步长程执行
- 你要先放着跑，再回来取结果
- 你希望先看 diff，再决定是否 apply

## 4. 交互式运行中的观察面

```text
/ps
/statusline
```

这层更像：

- 会话内观察正在运行的背景终端
- 看当前运行态，而不是发起新的 detached cloud task

不要把它和 `codex cloud status` 混写。

## 5. 一张快速决策表

- 同一任务继续跑：`codex resume`
- 同一历史走另一条路：`codex fork`
- 一个主代理带几个只读/定界角色：`/agent`
- 多个写入面需要彻底隔离：外部 `git worktree` + `codex -C`
- 需要 detached long-running：`codex cloud`

## 这章最容易写错的地方

- 不要把 subagent 写成 worktree 替代品。
- 不要把 `Codex App worktrees` 误写成 `Codex CLI` 子命令。
- 不要把 `resume`、`fork`、`cloud` 当成同一类会话延续。
- 不要在没有 branch、env、handoff 的情况下直接丢给 detached cloud task。
