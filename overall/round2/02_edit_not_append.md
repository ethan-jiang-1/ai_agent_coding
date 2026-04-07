# 第二章（深化）：三次纠错法则——修改原提示词，而非追加对话

> 本章为 round1 同章的深化版，假设读者已熟悉 round1 内容。
> 标注：`A/B` — 本机 CLI 或官方文档核验；`I` — 工程推论

---

## 习惯

**Aider 的 /undo 命令和 Codex CLI 的 fork 命令，是"回退并重试"这个动作在工具层面的正式实现。理解这些机制，能让 Edit & Rerun 更有效率。**

---

## 正确的做法

### 深化：各工具的"回退并重试"机制对比

| 工具 | 回退操作 | 重试操作 |
|------|---------|---------|
| Claude Code | `git restore --source=HEAD --worktree <file>`（明确回退目标文件）| 重新发送优化后的提示词 |
| Aider | `/undo`（撤销上一次 AI 修改，包含 git commit）| 重新发送优化后的提示词 |
| Codex CLI（exec）| `git restore --source=HEAD ...` | 重新运行 `codex exec` |
| Codex CLI（交互式）| `git restore ...` 或 `codex fork --last` | 在分叉的新会话里重试 |
| Cursor | Checkpoint 回退，或 git restore | 编辑原提示词重新运行 |

**Aider /undo 的特殊价值**（`A/B`：Aider 官方文档）：

Aider 默认对每次 AI 修改都会自动 commit。`/undo` 不只是撤销文件修改，而是撤销那个 commit：

```bash
# AI 做了一次修改（自动 commit）
# /undo 之后：
# 1. 文件内容回到修改前
# 2. git 历史里那个 commit 被移除
# 3. 聊天历史里 AI 的那次回复也会移除

# 然后重新发送更完整的提示词
```

这比 `git restore` + 手动清理更干净，因为它同时清理了工作区状态和聊天历史。

**Codex CLI fork 作为"Edit & Rerun"的等价物**（`A/B + I`）：

```bash
# 交互式场景：方向错了，不想在当前会话继续追加
# 1. 先撤销文件修改
git restore --source=HEAD --worktree src/auth/login.ts

# 2. fork 到一个干净分支重试（保留历史，但新会话从当前状态出发）
codex fork --last "重新实现登录函数，必须处理 null 值和空字符串"

# fork 的好处：保留了历史上下文（包括之前的探索），
# 但新会话不会把错误的实现带入上下文
```

### 深化：约束条件结构化的工具兼容性

round1 提出了结构化提示词格式。补充各工具对这种格式的处理差异（`I`）：

```markdown
任务：写一个用户登录函数

必须满足：
- [ ] 处理 null/空字符串输入，返回 {error: 'invalid_credentials'}
- [ ] 密码验证失败不暴露具体原因
- [ ] 数据库连接失败时抛出 DatabaseError

禁止：
- 使用 console.log 调试输出
- 修改现有的 User 模型结构

参考：`src/auth/register.ts`（保持风格一致）
```

这种结构对所有工具都有效，但引用方式不同：
- Aider：`参考：` 后的文件引用需要对应的 `--read` 或 `/add`，否则 AI 不知道文件内容
- Claude Code / Codex CLI：AI 可以自主读取 `src/auth/register.ts`，不需要提前加载
- Cursor：可以用 `@src/auth/register.ts` 直接在提示词里引用

---

## 错误的可能

### 深化：Aider 的 /undo 只能撤销上一次 AI 修改

`/undo` 不能多次回退（`A/B`）。如果 AI 做了多次修改，每次 /undo 只撤销最近的一次。

如果需要回退到更早的状态：

```bash
# 查看 Aider 相关的 commit
git log --oneline | head -20

# 回退到特定 commit 之前的状态
git revert <commit-hash>
# 或者
git reset --hard <earlier-commit>  # 危险操作，确认后再用
```

### 深化：Codex CLI exec 不能在同一会话里"编辑提示词重试"

`codex exec` 是非交互式的，每次运行是独立的（`A/B`）。没有"编辑上一次提示词"这个概念。"Edit & Rerun"在 Codex CLI exec 里的正确实现是：

```bash
# 上一次（方向错了）
codex exec --ephemeral "写登录函数"

# 回滚文件
git restore --source=HEAD --worktree src/auth/login.ts

# 重新运行更完整的命令（这就是 exec 的 Edit & Rerun）
codex exec --ephemeral "写登录函数，必须处理 null 值，..."
```

---

## 背后的原理

### 深化：负向 few-shot 的工具层表现

round1 解释了负向 few-shot 的机制，round2 补充它在不同工具里的具体表现（`I`）：

**Aider 里负向 few-shot 的特殊情况**：

Aider 默认保存聊天历史（`.aider.chat.history.md`）。如果你 `AIDER_RESTORE_CHAT_HISTORY=1` 启动新会话，上次的失败历史会被加载进来——这是比 Claude Code session resume 更隐蔽的负向 few-shot 来源。

实践建议（`I`）：如果上次会话里有大量失败的尝试，启动新会话时不要恢复历史（或者先删除 `.aider.chat.history.md`）。

**Claude Code 的 fork-session 避免负向 few-shot**（`A/B + I`）：

`claude --resume <id> --fork-session` 创建的新会话，继承了原会话的历史（包括失败的尝试）。这对于"我想基于规划结果尝试不同实现"是好的，但对于"我想用干净状态重试"则不适合。

重试场景应该用**全新会话**，而不是 fork：

```bash
# 错误的重试（会继承失败历史）
claude --resume <id> --fork-session "重新实现"

# 正确的重试（干净上下文）
git restore --source=HEAD --worktree <target-files>
claude "重新实现登录函数，必须处理 null 值..."
```

### 深化：对话树扁平化的经济学

round1 说"追加纠正增加了 AI 需要解决的约束数量"。补充 token 成本的维度（`I`）：

一个包含 5 次追加纠正的对话，比一个包含单次完整提示词的对话：
- 消耗更多 token（每次追加都增加上下文长度）
- 需要更多轮次的 API 调用
- 每轮 API 调用都需要重新计算前面所有的历史（成本累积）

相比之下，一次完整的提示词通常只需要 1-2 轮 API 调用，成本更低，也更可预测。

---

## 延伸的习惯

### 深化：提示词版本管理和工具集成

round1 建议维护 `prompts/` 目录。补充如何与工具集成（`I`）：

```bash
# Aider：通过 --message 加载提示词文件
cat prompts/auth-login-v3.md | aider --message - src/auth/login.ts

# Claude Code / Codex CLI：通过管道
cat prompts/auth-login-v3.md | claude -p
cat prompts/auth-login-v3.md | codex exec -

# 更结构化的方式：.aider.conf.yml 里的默认消息
# 不在配置文件里，但可以用 shell alias 封装
alias aider-login='aider --message "$(cat prompts/auth-login.md)"'
```

---

## 个人 / 协作注意

**个人使用**：如果你发现自己在同一个 Aider 会话里追加了超过 3 次纠正，先 `/undo` 回到最近的干净状态，然后考虑是否应该 `/clear` 历史并重新开始。

**团队协作**：提示词模板库（`prompts/` 目录）是高价值的团队资产。特别是对于高频任务（代码审查、测试生成、格式化），团队共享一套经过迭代优化的提示词模板，能避免每个人从零开始摸索。

---

## 延伸调查方向

1. **追加纠正 vs 重新提示的输出质量对比**：目前无系统性数据。工程推论（`I`）：在任务复杂度高、约束条件多的场景，差距更明显；在简单的单点修改上，差距可能不显著。

2. **不同工具的 Edit & Rerun 实现效果**：Aider `/undo` + 重试 vs Claude Code git restore + 重试 vs Codex CLI fork，在实际场景里的效率差异，需要实测。

3. **提示词版本管理的最佳实践**：目前没有专门的工具被广泛采用。git + markdown 文件是最简单的方案，但缺少"运行特定版本提示词"的便捷工具层。

4. **"三次纠错"阈值的任务类型差异**：对于探索性任务（不知道正确答案是什么），三次可能太少；对于有明确规格的实现任务，三次可能刚好。目前是经验值（`I`），无研究支撑。
