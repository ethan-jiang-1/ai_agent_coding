# 11 成本与批处理命令

## 非交互式输出

```bash
claude --print "总结这段代码"
claude -p "总结这段代码"
```

## 预算护栏

```bash
claude --print \
       --max-budget-usd 2 \
       --model sonnet \
       "总结日志，只输出根因"
```

`--max-budget-usd` 当前只对 `--print` 有效。

## 成本更低的规划工作流

```bash
claude --permission-mode plan \
       --print \
       "分析 src/auth/ 并给出 OAuth2 迁移计划" \
  > /tmp/plan.md

claude "读取 /tmp/plan.md，按计划实现第一阶段"
```

## 结构化输出

```bash
claude --print \
       --output-format json \
       "输出 JSON 结果"
```

## 非持久化批处理

```bash
claude --print --no-session-persistence "做一次性分析"
```

## GitHub Actions 的长程预算面

官方 GitHub Actions 文档当前明确支持通过 `claude_args` 透传 CLI 参数：

```yaml
- uses: anthropics/claude-code-action@v1
  with:
    prompt: "Review this PR for security and test coverage issues"
    claude_args: "--max-turns 5 --model sonnet"
```

还要再加两层护栏：

- GitHub workflow 自己的 timeout
- GitHub concurrency controls

注意：

- 官方 Actions 文档当前明确写了 `--max-turns`
- 但本机 `claude --help` 2.1.96 没有列出这个参数
- 所以这里应把它理解为 automation / Actions surface，而不是盲目当成常规本地 help 参数

## 高级并行能力的成本现实

- subagents 会额外吃 context 和 tokens
- agent teams 的 token 成本显著高于单一 session
- scheduled automation 会把调用次数变成持续成本，而不是一次性成本

## 会话内对应 slash commands

- `Type D` `/cost`：看当前对话成本。
- `Type D` `/usage`：看 usage 数据。
- `Type D` `/stats`：看统计信息。

这些命令适合会话内观察，不替代 `--max-budget-usd` 这种启动前预算护栏。

## 这章最容易写错的地方

- 不要把 `--max-budget-usd` 用在交互式会话里。
- 不要把昂贵执行放在方向确认之前。
- 不要在 CI 里省略预算护栏。
- 不要把 agent teams 和 schedule 产生的重复调用成本忘掉。
