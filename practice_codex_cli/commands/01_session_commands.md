# 01 会话与恢复命令

## 一次性任务

```bash
codex exec --ephemeral "总结 src/auth/ 的当前结构"
```

- 适合：分析一次就结束、不需要恢复历史的任务
- 重点：`--ephemeral` 只表示不落盘，不表示本轮里没有上下文

## 分步非交互任务

```bash
codex exec "第一步：分析 src/auth/"
codex exec -s workspace-write resume --last "第二步：按刚才的分析实现中间件"
codex exec -s read-only resume --last "第三步：运行测试并总结结果"
```

关键点：

- `resume` 是 `exec` 的子命令
- 如果要给 resumed exec 指定沙箱，参数要放在 `resume` 前面
- `codex exec resume --last -s read-only ...` 这种写法当前会报错

## 交互式任务

```bash
codex
codex resume --last
codex resume <session-id>
codex fork --last
```

- `codex resume`：继续原会话
- `codex fork`：从历史分叉出新会话

## 把 exec 历史切回交互式

```bash
codex resume --include-non-interactive --last
```

适合：前面用 `exec` 跑了一段，现在想进入交互式 TUI 接着看。

## 这章最容易写错的地方

- 不要把交互式 `resume` / `fork` 和 `exec resume` 混写。
- 不要把 `fork` 当成默认的 exec 延续方式。
- 不要把 `--ephemeral` 理解成“本轮没有历史”。
