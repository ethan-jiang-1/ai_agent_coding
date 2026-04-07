# 第九章（深化）：跨会话状态传递——Memento 工作流

> 本章为 round1 同章的深化版，假设读者已熟悉 round1 内容。
> 标注：`A/B` — 本机 CLI 或官方文档核验；`I` — 工程推论

---

## 习惯

**不同工具的"新会话默认行为"不同：Codex CLI 有 resume/fork，Claude Code 有 auto-memory 和 --continue。理解这些机制的边界，才能选择正确的状态传递方式。**

---

## 正确的做法

### 深化：各工具的会话持久化机制对比

round1 把所有工具都当作"无状态、每次重头来"，但实际上各工具的默认行为有重要差异：

**Claude Code**（`A/B`：claude --help 本机核验 + 官方文档研究）

```bash
# 三种会话延续方式
claude --continue              # 继续当前目录最近的会话
claude --resume <session-id>   # 按 session ID 恢复特定会话
claude --resume                # 打开选择器，选择要恢复的会话

# 从历史会话分叉（保留历史，创建新分支）
claude --resume <session-id> --fork-session
```

Auto-memory 机制（`A/B`：官方文档核验）：
- 存储路径：`~/.claude/projects/<project_name>/memory/MEMORY.md`
- 触发：Claude 在交互中识别出值得持久化的知识/偏好时**自动更新**
- 读取：每次 session 启动时**自动加载前 200 行或前 25KB**
- 作用域：同一 Git 仓库内的不同 worktree **共享**同一项目的 auto-memory

**Codex CLI**（`A/B`：本机 CLI 核验）

```bash
# 恢复历史交互式会话（不是 exec 的会话）
codex resume --last            # 恢复最近一次交互式会话
codex resume <session-id>      # 按 ID 恢复特定会话

# 从历史会话分叉（原会话不变，创建独立分支）
codex fork --last
codex fork <session-id>

# exec 的会话管理
codex exec --ephemeral "..."   # 临时会话，不保存到磁盘，不可 resume
codex exec "..."               # 可 resume（不加 --ephemeral 时默认持久化）
```

**resume vs fork 的本质区别**（`A/B`：CLI 帮助文本）：
- `resume`：继续同一会话，对话历史追加
- `fork`：基于历史会话创建**新的独立会话**，原会话不受影响

这意味着：
- 需要在同一上下文里继续工作 → `resume`
- 想基于当前成果尝试不同方向 → `fork`（等价于"从这里分叉，试试另一种方案"）

**Aider**（`A/B`：官方文档核验）

Aider 通过以下方式保留状态：
- `.aider.chat.history.md`：自动保存完整对话历史
- `AIDER_RESTORE_CHAT_HISTORY`：环境变量，启用后下次启动时自动恢复上次对话历史
- `.aider.conf.yml` 中 `restore-chat-history: true`：配置文件方式

Aider 还有聊天历史 token 软上限（超过时自动开始摘要），防止历史无限增长。

### 深化：Claude Code fork-session 作为"Memento 工作流"的工具版

round1 的 Memento 工作流主要依赖手工写 handoff 文件。实际上 Claude Code 的 `--fork-session` 提供了一种更轻量的方式（`A/B`）：

```bash
# 场景：当前会话已经完成了规划，想基于规划进行两种不同的实现尝试

# 记录当前 session ID
# （可以从 claude --name 设置的名称，或者 /resume 的列表里找到）

# 分叉出实现方案 A（原会话不变）
claude --resume <session-id> --fork-session "按方案 A 实现登录模块"

# 分叉出实现方案 B（再次基于同一个原会话分叉）
claude --resume <session-id> --fork-session "按方案 B 实现登录模块，使用 JWT"

# 两个分叉完全独立，可以对比结果后选择最优方案
```

这和 git branch 的逻辑很像：从同一个"提交点"出发，走不同的路。

### 深化：Codex CLI 的 exec --ephemeral 和 resume 如何配合

round1 里把 Codex CLI 的 handoff 文件模式和 resume 写成两种独立方案。实际上，它们是同一个工具的不同场景（`I + A/B`）：

```
任务类型 → 正确的 Codex CLI 工作流

一次性批处理（不需要恢复）：
  codex exec --ephemeral -s workspace-write "完成任务 A"
  # 会话不保存，完成即销毁

可分步的复杂任务（需要分批执行）：
  codex exec "第一步：分析架构"
  # 会话持久化，生成 session ID

  # 第一步完成后，第二步 resume 同一会话
  codex resume --last "第二步：按分析结果实现中间件"

同一任务的探索（想试不同方向）：
  codex fork --last "尝试方案 A：使用 JWT"
  # 或
  codex fork <session-id> "尝试方案 B：使用 Session"
```

---

## 错误的可能

### 新增错误：把 exec --ephemeral 和"没有会话历史"画等号

`--ephemeral` 控制的是**会话文件是否写入磁盘**，不是会话内是否有历史（`A/B`：codex exec --help）。

在一个 `--ephemeral` 的 exec 会话内，模型仍然有完整的对话历史（直到 exec 命令结束）。`--ephemeral` 只是让这个历史在命令结束后**不保存到磁盘**，因此不可 resume。

常见误解：认为每次 `exec --ephemeral` 的每轮对话都是独立的。实际上，在同一个 exec 命令内（哪怕有多轮工具调用），历史是连续的。

### 新增错误：auto-memory 会自动记住"对话内容"

Claude Code 的 auto-memory 记录的是**可持久化的知识和偏好**（如构建命令、调试洞察、代码风格偏好），不是完整的对话历史（`A/B`：官方文档）。

完整的对话历史通过 session 文件保存（用 `--resume` 恢复），auto-memory 是更高层的"项目经验积累"。

---

## 背后的原理

### 深化：三种状态存储的层次

理解了各工具的实际机制后，可以把跨会话状态传递归纳为三个层次（`I`）：

```
层次 1：对话历史（完整回放）
  → Claude Code: session 文件（--resume 恢复）
  → Codex CLI: session 文件（resume 恢复，--ephemeral 时不保存）
  → Aider: .aider.chat.history.md（AIDER_RESTORE_CHAT_HISTORY 恢复）
  特点：完整，但 token 成本高；长历史会有 context rot

层次 2：工具自动积累的项目经验
  → Claude Code: auto-memory（MEMORY.md，自动读写）
  → 其他工具：无等价机制
  特点：自动、轻量，但内容由模型决定，不透明

层次 3：人工编写的状态快照
  → 适用于所有工具
  → handoff.md / session-handoff.md / memory-bank/
  特点：人工控制，高密度，透明，但需要主动维护
```

**选择逻辑**（`I`）：
- 短期任务（< 1 小时，上下文还干净）→ 直接 resume
- 长期任务（上下文已污染，需要"脱毒"）→ 层次 3（手工快照 + 新会话）
- 跨天工作 → 层次 2 + 层次 3 配合（auto-memory 记积累，快照记当前状态）

### 深化：fork 为什么比"开新会话重新描述"更高效

从一个历史会话 fork，新会话的起点是**原会话的完整状态**，包括所有探索历史、已确认的事实和当前决策。相比"新会话 + 人工重新描述"，fork 保留了那些"说不清楚但已经确立了"的隐性上下文（`I`）。

代价是：如果原会话已经有 context rot（混乱的历史），fork 会继承这个问题。所以 fork 的最佳时机是：原会话状态还好，只是需要尝试不同的执行方向。

---

## 延伸的习惯

### 深化：利用 --name 为会话建立可读的索引

```bash
# 给当前会话命名，方便后续在选择器里找到（A/B）
claude --name "oauth2-migration-planning"

# 后续可以在 --resume 的选择器里通过名称识别
claude --resume  # 打开选择器，显示名称列表
```

与 Codex CLI 的 session 命名（`A/B`：codex resume --help 中的"线程名"概念）类似：
```bash
# Codex resume 可以按 UUID 或线程名恢复
codex resume <session-id>   # UUID
codex resume my-task-name   # 线程名（如果设置了）
```

### 深化：auto-memory 和 handoff 文件的分工

两者不互相替代，而是分工（`I`）：

- **auto-memory**：积累**项目级的通用经验**（这个项目用 pnpm 不用 npm；测试命令是 make test）
- **handoff 文件**：传递**当前任务的即时状态**（做到了哪里，下一步是什么，有什么坑）

auto-memory 是跨时间积累的，handoff 文件是一次性的。两者结合，才能覆盖"项目经验"和"当前任务进度"两种不同维度的记忆需求。

---

## 个人 / 协作注意

**个人使用**：
- 对于 Claude Code，把 `--name` 用起来，让会话列表有意义而不是一堆 UUID
- 对于 Codex CLI，养成对复杂任务不加 `--ephemeral` 的习惯，保留 resume 能力；对一次性任务加 `--ephemeral`，保持磁盘整洁

**团队协作**：
- Claude Code 的 auto-memory 在 worktree 间是共享的，一个人的会话积累的经验对团队所有 worktree 有效（`A/B`：官方文档）
- 这意味着一个人在调试中发现的特殊情况会自动成为团队共享的"项目经验"——这是优势，但也要小心：如果某人临时绕过了一个重要约束，这个"经验"可能被错误地记录

---

## 延伸调查方向

1. **Claude Code auto-memory 的实际记录质量**：目前无可靠公开数据。关键问题是：模型如何判断"值得持久化"？记录的内容是否真的有用？有没有用户测试过 auto-memory 在项目中的实际表现？

2. **快照 vs resume 的实际效果对比**：目前无系统性数据。工程推论（`I`）：对于超过 2 小时的会话，手工快照 + 新会话的输出质量通常优于 resume 同一个污染的长会话。但"2 小时"只是经验值。

3. **共享 Memory Bank 的团队采用情况**：Cline Memory Bank 在多人团队中的实际效果 — 目前无可靠公开数据，待实测验证。

4. **/compact 的压缩质量**：官方说明是"压缩后从磁盘重新注入规则层"（`A/B`），但压缩后的会话历史究竟保留了多少有效信息，目前无公开数据。
