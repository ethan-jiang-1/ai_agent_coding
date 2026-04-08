# 11 成本与认证方式

## 切换登录方式

```bash
codex login
codex logout
```

实践上先分清两种路径：

- 用 ChatGPT 账号登录
- 用 API key 或自动生成的 API key

这两种路径对应的成本边界不一样。

## 低成本的命令形态

```bash
codex exec --ephemeral -s read-only "先分析，不改文件"
codex exec --ephemeral -s read-only "生成计划" -o /tmp/plan.md
codex exec -s workspace-write "只按计划实现第一步"
```

这类工作流的优点是：

- 范围明确
- 输出可外化
- 成本相对可预测

## 把强模型留给难任务

```bash
codex exec -m <strong-model> "做跨模块分析"
codex exec "做常规实现"
codex --oss --local-provider ollama "处理简单日志或重命名"
```

## 成本控制的核心动作

- 先读少量关键文件，再决定是否进入执行
- 先生成计划，再给写权限
- 用 handoff 文件减少重复分析
- 长会话及时重开，不要无限续

## 这章最容易写错的地方

- 不要把最新价格数字硬编码进长期文档。
- 不要把所有任务都交给最强模型。
- 不要在没有看清范围时直接进入高权限执行。
