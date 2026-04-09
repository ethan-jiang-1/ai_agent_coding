# Claude Code 官方能力核验

来源：本机 `claude --help`（Claude Code 2.1.92）+ Anthropic 官方文档（经 agent 研究）
核验日期：2026-04-07

---

## 1. 会话管理参数（已本机确认）

| 参数 | 说明 |
|------|------|
| `-c` / `--continue` | 继续当前目录最近的会话 |
| `-r` / `--resume [value]` | 按 session ID 恢复会话，或打开选择器 |
| `--fork-session` | 与 `--resume` 或 `--continue` 配合，创建新 session ID 而不复用原来的（即分叉） |
| `--no-session-persistence` | 禁用会话持久化（仅 `--print` 模式有效） |
| `-n` / `--name <name>` | 为当前会话设置显示名称（显示在 /resume 和终端标题） |
| `--from-pr [value]` | 恢复与 PR 关联的会话 |

**resume vs fork 的写法**：
```bash
claude --resume <session-id>              # 继续原会话
claude --resume <session-id> --fork-session  # 从历史会话分叉为新会话
claude --continue                         # 继续最近会话
```

---

## 2. Permission Mode（--permission-mode）

可选值（已本机确认）：
- `acceptEdits` — 接受所有文件编辑
- `auto` — 自动决策
- `bypassPermissions` — 绕过所有权限检查
- `default` — 默认模式
- `dontAsk` — 不询问
- `plan` — **只读规划模式**：只能读取和分析，禁止修改任何文件

```bash
claude --permission-mode plan "分析 src/auth/ 并给出重构计划"
```

---

## 3. 工具控制参数（已本机确认）

| 参数 | 说明 |
|------|------|
| `--allowedTools <tools>` | 逗号或空格分隔的工具白名单（如 `"Bash(git:*) Edit"`）|
| `--disallowedTools <tools>` | 工具黑名单 |
| `--tools <tools>` | 指定可用工具集；`""` 禁用所有工具，`"default"` 使用所有工具 |

```bash
claude --allowedTools "Read Edit" "分析并修改 src/auth/"
claude --tools "" "只用你的知识回答，不调用任何工具"
```

---

## 4. 预算控制（已本机确认）

```bash
claude --print --max-budget-usd 2 --model sonnet "总结日志"
```

- `--max-budget-usd <amount>`：设置本次会话的 API 消费上限（美元）
- **仅在 `--print` 模式下有效**
- 达到上限后，Claude Code 停止执行并退出

---

## 5. 多智能体 / Subagents（官方文档研究结果）

已确认的事实：
- `--agents <json>`：定义自定义 agent（CLI 参数，已在 `claude --help` 确认）
- subagent 存储路径：项目级 `.claude/agents/`，用户级 `~/.claude/agents/`
- 每个 subagent 拥有独立的 context window
- 可以为不同 subagent 配置不同的工具权限集
- `claude agents` 是管理 subagent 的专用子命令

---

## 6. Worktree 支持（已本机确认）

```bash
claude -w              # 创建新 worktree
claude -w feature-x    # 创建命名为 feature-x 的 worktree
claude --tmux          # 在 worktree 里创建 tmux 会话（需要 --worktree）
```

---

## 7. Auto-Memory（官方文档研究结果）

已确认的事实：
- 存储路径：`~/.claude/projects/<project_name>/memory/`，入口文件 `MEMORY.md`
- 触发：Claude 在交互中识别出值得持久化的知识/偏好时自动更新
- 读取：每次 session 启动时自动加载前 **200 行** 或前 **25KB**
- 作用域：同一 Git 仓库内的不同 worktree 共享同一项目的 auto-memory

**注意**：`--bare` 模式会跳过 auto-memory。

---

## 8. /compact 行为（官方文档研究结果）

已确认的事实：
- 压缩后，Claude Code 会**从磁盘重新读取** `CLAUDE.md` 等规则文件并重新注入
- 即规则层在压缩后依然完整，不会因为 /compact 而丢失

---

## 9. 非交互式模式（--print）

```bash
claude -p "分析这段代码" < input.txt
cat error.log | claude -p "找出根因"
```

- `-p` / `--print`：打印响应后退出
- `--output-format`：`text`（默认）、`json`、`stream-json`
- `--max-budget-usd`：仅在此模式有效

---

## 10. MCP 配置（已本机确认）

```bash
claude --mcp-config sentry.json "分析生产错误"
claude --strict-mcp-config  # 只使用 --mcp-config 指定的 MCP，忽略其他配置
```

---

## 11. 不确认的内容

- `--permission-mode plan` 的底层实现（是否真的禁止所有写操作，还是只是提示）— 官方文档说"禁止修改任何文件"，具体实现未核验
- subagent 是否单独绑定模型的配置方式 — 帮助文本未详细说明
