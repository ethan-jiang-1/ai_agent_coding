# 08 沙箱与权限命令

## 交互式命令的权限面

```bash
codex -s read-only -a untrusted "先读代码，别改文件"
codex -s workspace-write -a on-request "实现这个改动，必要时再请求批准"
codex --search "查一下这个依赖的最新文档"
```

适用范围：

- `codex`
- `codex resume`
- `codex fork`

这些入口当前都暴露了：

- `-s` / `--sandbox`
- `-a` / `--ask-for-approval`
- `--search`

## 交互式对应

```text
/approval-mode
/approvals
/status
```

映射关系：

- `/approval-mode` ↔ Exact / Combined mapping
  与 `-a` / `--ask-for-approval` 最接近
- `/approvals` ↔ `/approval-mode` 的隐藏别名
- `/status` ↔ Combined mapping
  用来查看当前模型、沙箱、审批等会话状态

## 非交互式 exec 的权限面

```bash
codex exec -s read-only --ephemeral "只读分析"
codex exec -s workspace-write "修改工作区"
codex exec --full-auto "低摩擦自动执行"
```

当前本机 CLI 的 `codex exec --help` 暴露的是：

- `-s` / `--sandbox`
- `--full-auto`
- `--dangerously-bypass-approvals-and-sandbox`

它没有单独暴露 `-a` 或 `--search`。

当前官方 CLI 文档还说明：`codex exec` 默认就在 `read-only` sandbox 里运行。真正要改文件时，最好显式写出 `-s workspace-write`，不要依赖默认行为。

## 权限使用建议

- 规划、审计、定位问题：`read-only`
- 实施改动：`workspace-write`
- 低摩擦自动执行：`--full-auto`
- 危险绕过：只在外部已沙箱环境里使用

## 这章最容易写错的地方

- 不要把交互式的 `-a` 用法直接写到 `codex exec` 上。
- 不要默认用危险绕过模式。
- 不要把“工作区可写”当成“没有风险”。
- 不要忘了 `/approvals` 只是 `/approval-mode` 的隐藏别名。
