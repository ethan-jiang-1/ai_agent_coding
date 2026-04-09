# 第三章（深化）：先规划，后执行——探索与执行强制分离

> 本章为 round1 同章的深化版，假设读者已熟悉 round1 内容。
> 标注：`A/B` — 本机 CLI 或官方文档核验；`I` — 工程推论

---

## 习惯

**规划阶段的工具级保障比提示词约束更可靠。Claude Code 的 `--permission-mode plan` 和 Codex CLI 的 `-s read-only` 提供了不同程度的硬性约束，而不只是靠提示词说"不要写代码"。**

---

## 正确的做法

### 深化：Claude Code 的 plan permission mode

`--permission-mode plan` 是 Claude Code 的正式参数（`A/B`：claude --help 核验）：

```bash
# 规划阶段：plan mode
claude --permission-mode plan \
  "分析 src/auth/ 的现有认证架构，给出 OAuth2 迁移的完整计划，
   包括需要修改的具体文件和每个文件的变更大纲"
```

plan mode 的官方说明（`A/B`）：Claude 在此模式下只能读取文件和进行分析，被禁止修改任何文件。

**plan mode + --print 的非交互式工作流**（`A/B + I`）：

```bash
# 生成规划文档（非交互式）
claude --permission-mode plan \
       --print \
       "分析认证架构并生成迁移计划" > .claude/plans/oauth2-plan.md

# 审查生成的计划文档
cat .claude/plans/oauth2-plan.md

# 确认后，切换到执行模式
claude --permission-mode default \
  "按 .claude/plans/oauth2-plan.md 的计划实现中间件层"
```

### 深化：Codex CLI 的只读规划模式

Codex CLI 没有"plan mode"这个概念，但 `-s read-only` 提供了类似的文件系统保障（`A/B`：codex --help 核验）：

```bash
# 规划阶段：只读 sandbox
codex exec --ephemeral -s read-only \
  "分析 src/auth/ 的现有实现，给出 OAuth2 迁移的完整计划，
   只输出计划，不修改任何文件"

# 规划输出写到文件（通过 -o 参数）
codex exec --ephemeral -s read-only \
  "生成 OAuth2 迁移计划" \
  -o .codex/plans/oauth2-plan.md

# 执行阶段：可写 sandbox
codex exec -s workspace-write \
  "读取 .codex/plans/oauth2-plan.md，按计划实现中间件层"
```

`-s read-only` 和 Claude Code `plan` mode 的区别（`I`）：
- `-s read-only` 是文件系统层面的约束：AI 在技术上无法写文件
- `plan` mode 是模型层面的约束：AI 被指示不修改文件，但如果模型"想"修改，工具层是否有硬性阻断需要验证（`A/B 待核验`）

### 深化：Aider 的只读规划工作流

Aider 没有专用的 plan mode，但可以通过"不 /add 任何可编辑文件"来实现只读规划（`A/B + I`）：

```bash
# 规划阶段：只读文件（--read），不添加可编辑文件
aider --read src/auth/login.ts \
      --read src/auth/middleware.ts \
      --read docs/auth-spec.md \
      --message "不要修改任何文件。分析这些文件，给出 OAuth2 迁移计划"

# 注意：--read 的文件 AI 无法修改（A/B：Aider 官方）
# 所以"只用 --read，不用 /add"就是 Aider 的只读规划工作流
```

这与 `--add`（可编辑）的本质区别：
- `--read`：AI 可以读取，但物理上无法修改（`A/B`）
- `/add`（或 `--file`）：AI 可以读取并修改

### 深化：计划文件的最优位置和格式

规划文件放在固定目录，方便后续引用（`I`）：

```
.claude/plans/          # Claude Code 的规划文件
.codex/plans/           # Codex CLI 的规划文件
docs/plans/             # 工具中立的位置（纳入版本控制）
```

计划文件的最小化结构（结合 round1 的验收标准，以下是精炼版）（`I`）：

```markdown
# 迁移计划：JWT → OAuth2

## 变更文件（按执行顺序）
1. `src/config/auth.config.ts` — 新增 OAuth2 配置项（无依赖，先改）
2. `src/auth/middleware.ts` — 修改 token 验证逻辑（依赖配置，第二改）
3. `src/auth/login.ts` — 添加 OAuth2 授权码流（依赖中间件，第三改）
4. `tests/auth/login.test.ts` — 更新测试（最后改）

## 不变的文件
- `src/models/User.ts`（产品决策：用户模型不变）

## 关键决策
- 选择 PKCE 流程（原因：合规要求）

## 风险点
- 现有 JWT token 在迁移期间会失效，需要处理过渡期
```

---

## 错误的可能

### 深化：Aider 的 /ask 命令是轻量版规划工具

round1 没有提到 Aider 的 `/ask` 命令。它提供了一种不同的规划-执行分离方式（`A/B`：Aider 官方文档）：

```bash
# /ask 模式：提问但不修改文件
/ask 如果我要把这个模块迁移到 OAuth2，有哪些需要注意的地方？

# /code 模式（默认）：提问并修改文件
/code 把这个模块迁移到 OAuth2
```

`/ask` 和 `--read` 规划工作流的区别（`I`）：
- `/ask`：在当前已 /add 的文件上下文里提问，不修改文件（临时的）
- `--read` 工作流：完全不 /add 可编辑文件，系统层面禁止修改（更严格）

对于"我想先问一个问题再决定是否要改代码"的场景，`/ask` 更轻量。

---

## 背后的原理

### 深化：工具级约束比提示词约束更可靠

round1 解释了"为什么要先规划"的原理，round2 补充"工具级约束比提示词约束更可靠"的原因（`I`）：

提示词约束（"不要写代码"）的可靠性取决于模型的指令遵循能力——有时候模型会"忍不住"开始写代码，特别是在复杂任务里。

工具级约束（`-s read-only`、`--permission-mode plan`）的可靠性来自工具层：
- Codex CLI 的 `read-only` sandbox：文件系统层面的约束，AI 生成的写文件命令会被 sandbox 拦截
- Aider 的 `--read`：工具层面的约束，--read 文件不在可编辑列表里，AI 发出的写指令会被拒绝
- Claude Code 的 `plan` mode：官方说明是禁止修改，但底层实现细节待核验

**实践建议**（`I`）：对重要的规划阶段，优先使用工具级约束，而不是只依赖提示词约束。

---

## 延伸的习惯

### 深化：计划文件的版本控制

计划文件应该纳入版本控制（`I`）：

```bash
# 规划阶段
claude --permission-mode plan --print "生成迁移计划" \
  > docs/plans/oauth2-migration.md

git add docs/plans/oauth2-migration.md
git commit -m "plan: OAuth2 迁移计划（待审查）"

# 团队审查计划（Code Review 的前置步骤）
# 修改后：
git commit -m "plan: OAuth2 迁移计划（已审查通过）"

# 执行阶段
claude "按 docs/plans/oauth2-migration.md 实现"
```

这样计划的演变是可追溯的，也让计划成为团队沟通的载体，而不只是 AI 的内部文档。

---

## 个人 / 协作注意

**个人使用**：在开始一个影响超过 3 个文件的修改之前，花 2-3 分钟用 plan mode 生成计划并审查，通常能节省后续 20-30 分钟的返工时间（`I`，经验值）。

**团队协作**：
- 把计划文件 PR 作为代码 PR 的前置步骤——先合并计划，再开始写代码
- 这让团队成员能在 AI 开始大量修改文件之前参与决策，成本最低

---

## 延伸调查方向

1. **规划阶段对输出质量的量化影响**：目前无可靠公开数据。工程推论（`I`）：规划阶段的价值在跨文件重构中最显著，在单文件修改中价值相对较低。

2. **不同工具规划模式的实现差异**：Claude Code `plan` mode 是否真的在工具层面阻止写文件（还是只靠提示词约束），需要实测验证（`A/B 待核验`）。Codex CLI `-s read-only` 的阻断是确定的（文件系统层面）。

3. **计划粒度与执行偏移率的关系**：计划越细越好吗？还是过细的计划会限制 AI 的合理判断？目前无研究数据。工程推论（`I`）：计划的最小必要粒度是"受影响的文件列表 + 每个文件的变更方向"，不需要细化到具体代码行。

4. **工具化规划的实际效果**：Codex CLI 只读 exec、Claude Code plan mode，在真实大型任务里的计划质量如何？有没有用户报告计划阶段成功预防了哪些方向性错误？目前无公开数据。
