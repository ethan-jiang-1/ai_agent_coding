# 第十章（深化）：多智能体分流——Writer / Reviewer 双路并发

> 本章为 round1 同章的深化版，假设读者已熟悉 round1 内容。
> 标注：`A/B` — 本机 CLI 或官方文档核验；`I` — 工程推论

---

## 习惯

**多智能体工作流的核心是上下文隔离，不是"同时运行更多任务"。Claude Code 的 subagents 和 Codex CLI 的 exec 串行模式，提供了两种不同程度的隔离。**

---

## 正确的做法

### 深化：Claude Code Subagents 的官方机制

round1 对 subagents 的描述是准确的，但缺少具体的配置细节。官方确认的机制（`A/B`：官方文档研究 + claude --help 核验）：

**定义方式**：

```bash
# 通过 CLI 参数定义临时 subagent
claude --agents '{"reviewer": {"description": "Reviews code", "prompt": "You are a code reviewer"}}'

# 通过文件定义持久化 subagent
# 项目级：.claude/agents/<agent-name>.md
# 用户级：~/.claude/agents/<agent-name>.md
```

**关键特性**（`A/B`）：
- 每个 subagent 拥有**独立的 Context Window**，不共享主会话上下文
- 可以为不同 subagent 配置不同的**工具权限集**
- subagent 的输出作为结构化结果返回给主会话

**实际工作流**（`I`）：

```bash
# 主会话派遣 subagent 做代码库探索
claude "用 subagent 探索 src/auth/ 目录，理解现有认证架构，
       只把架构摘要（不超过 500 字）返回给我"

# 效果：subagent 在独立上下文中读取数十个文件
# 主会话只接收 500 字摘要，上下文增量极小
```

**worktree + subagents 的组合**（`A/B`：claude --help 核验）：

```bash
# 在 worktree 里运行主会话，同时派遣 subagent 审查 diff
git worktree add .worktrees/feature-x -b feature-x
claude -w feature-x  # 在 worktree 里工作

# 实现完成后，用独立的干净会话审查 diff
git diff main > /tmp/feature.diff
claude --permission-mode plan  # 只读模式的审查者
< /tmp/feature.diff "审查这个 diff，列出所有问题"
```

### 深化：Codex CLI exec 串行多任务模式

Codex CLI 不像 Claude Code 那样有明确的"subagents"概念，但通过 exec 命令的串行组合，可以实现类似的上下文隔离效果（`A/B + I`）：

**方式一：独立 exec 串行（推荐的保守做法）**

```bash
# 三个独立任务，每个都有完全隔离的上下文
# 并行执行（不同终端或后台）
codex exec --ephemeral -s read-only \
  "分析 src/auth/ 的现有实现，生成架构摘要" \
  -o /tmp/auth-summary.md

codex exec --ephemeral -s read-only \
  "分析 tests/auth/ 的现有测试，找出缺失覆盖" \
  -o /tmp/test-gaps.md

codex exec --ephemeral -s read-only \
  "检查 src/config/ 里与 auth 相关的配置项" \
  -o /tmp/config-analysis.md
```

`--o` / `--output-last-message`：将 agent 最后一条消息写入文件（`A/B`：codex exec --help）

**方式二：exec resume 串行（适合同一逻辑任务的分步执行）**

```bash
# 第一步：规划
codex exec -s read-only "分析 src/auth/ 并给出 OAuth2 迁移计划"
# 记住 session ID

# 第二步：基于第一步的规划执行（resume 同一会话）
codex exec resume --last -s workspace-write "按刚才的计划实现中间件层"

# 第三步：验证
codex exec resume --last -s read-only "运行测试，报告结果"
```

### 深化：Claude Code Worktree 的完整用法

```bash
# 方式一：--worktree 参数（A/B：claude --help 核验）
claude -w feature-x  # 创建命名 worktree 并进入

# 方式二：手动 git worktree + 独立 claude 实例
git worktree add .worktrees/oauth2 -b feature/oauth2
cd .worktrees/oauth2
claude  # 在 worktree 目录里启动，上下文与主目录完全隔离

# 方式三：--tmux 集成（A/B：claude --help 核验）
claude -w feature-x --tmux  # 在 worktree 里创建 tmux 会话
# iTerm2 环境下使用原生面板
```

**worktree 和 auto-memory 的关系**（`A/B`：官方文档）：
同一 Git 仓库内的不同 worktree **共享** auto-memory（`~/.claude/projects/<project>/memory/`）。这意味着在 worktree 里工作积累的项目经验，对主工作树同样有效。

---

## 错误的可能

### 新增错误：把 Codex CLI exec 并行当成"多 subagent 协同"

并行运行多个 `codex exec` 是独立的无协调进程，它们之间没有通信机制（`A/B + I`）。如果多个 exec 都在修改同一个文件，会产生冲突。

正确的并行用法：
- 只读分析任务（`-s read-only`）可以安全并行，因为不会写文件
- 写入任务必须确保范围不重叠

```bash
# 安全的并行（都是只读）
codex exec --ephemeral -s read-only "分析模块 A" &
codex exec --ephemeral -s read-only "分析模块 B" &
wait

# 危险的并行（都写同一区域）
# 不要这样做：
codex exec "修改 src/auth/middleware.ts" &
codex exec "修改 src/auth/middleware.ts" &  # 冲突！
```

### 深化：fork 在多智能体工作流中的用途

round1 提到了 fork，但没有说清楚它在多智能体场景里的具体用途。

`codex fork` 适合的场景（`I`）：
- "探索性审查"：从编写者的会话 fork 出一个审查者分支，审查者能看到所有历史但不影响编写者
- "方向试验"：基于同一个规划状态，fork 出两个执行会话，尝试不同的实现方案

---

## 背后的原理

### 深化：上下文隔离的两种实现方式

round1 说"审查者需要干净上下文"，但没有解释为什么不同的实现方式（新 session vs fork vs subagent）在隔离效果上有差异（`I`）：

**完全干净的上下文**（新 session 或 subagent）：
- 不带任何历史负担
- 最适合"从第一次看到这段代码的角度审查"
- 缺点：需要在新上下文里重新提供足够的任务背景

**携带历史的分叉**（fork）：
- 继承原会话的完整历史
- 适合"我想在现有基础上尝试不同方向"
- 缺点：审查者如果 fork 了编写者的会话，会带有编写者的"思维惯性"

**结论**（`I`）：真正用于"无认知偏差审查"的角色，应该用**全新 session 或 subagent**，而不是 fork。fork 更适合"分叉执行路径"，不适合"独立审查"。

### 深化：子智能体的上下文经济学（有数据支撑的部分）

官方确认：subagent 有独立的 context window（`A/B`）。这意味着：

- 主会话派遣 subagent 探索 50 个文件
- subagent 的 context window 承载了这 50 个文件的内容
- subagent 结束后，主会话只收到摘要，不继承 subagent 的全部上下文

这是上下文隔离的**工具级保障**，不只是靠提示词说"把结果总结给我"。

---

## 延伸的习惯

### 深化：利用 --output-last-message 串联多个 exec

Codex CLI 的 `--output-last-message <file>`（`A/B`：codex exec --help）允许把 agent 的最后一条消息写入文件，这使得多个 exec 任务可以通过文件系统串联：

```bash
# 第一步：分析，输出摘要到文件
codex exec --ephemeral -s read-only \
  "分析 src/auth/ 的现有架构" \
  -o /tmp/auth-analysis.md

# 第二步：基于摘要规划，输出计划到文件
codex exec --ephemeral -s read-only \
  "读取 /tmp/auth-analysis.md，制定 OAuth2 迁移计划" \
  -o /tmp/migration-plan.md

# 第三步：基于计划执行
codex exec --ephemeral -s workspace-write \
  "读取 /tmp/migration-plan.md，按计划实现中间件层"
```

这种模式把"信息传递"外化到文件系统，避免了在单一会话里积累过多历史。

---

## 个人 / 协作注意

**个人使用**：
- 对于 Claude Code，优先用 subagents 做探索任务（保护主会话上下文）
- 对于 Codex CLI，--output-last-message 是串联多个 exec 的关键工具，养成习惯

**团队协作**：
- 团队成员各自在独立 worktree 里工作时，auto-memory 是共享的（`A/B`）——这意味着一个人的探索结果会影响所有人的 AI 行为。约定不要在 auto-memory 里记录"临时绕过某约束"的经验。
- 对于需要跨人员传递的任务，handoff 文件比 fork session 更可靠（handoff 文件是明文，可以检查和修改；fork session 的历史对其他人来说是不透明的）

---

## 延伸调查方向

1. **双路审查 vs 单路自我审查的 Bug 发现率**：目前无可靠公开数据，待实测验证。可以设计对照实验：相同代码变更，让同一 AI 会话自我审查 vs 用独立 subagent 审查，统计发现的问题数量和严重性。

2. **Claude Code subagents 的实际 token 节省**：主会话派遣 subagent 探索 vs 主会话直接探索，token 消耗差异是多少？目前无公开数据。理论上差异应该很大（subagent 的 context 不并入主会话），但实际摘要的信息损失率未知。

3. **Git Worktree 在 AI 工作流中的采用率**：没有公开统计数据。工程推论（`I`）：worktree 的使用门槛是 git 熟练度，对不熟悉 git worktree 的开发者来说，`claude -w` 参数是更低门槛的入口。

4. **并行任务数量与质量的关系**：Codex exec 并行运行时，sandbox 隔离能否完全防止冲突？在 workspace-write 模式下，不同 exec 进程同时写文件是否有原子性保证？目前无公开文档说明。
