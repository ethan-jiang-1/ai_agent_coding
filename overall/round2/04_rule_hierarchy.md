# 第四章（深化）：规则文件的分层设计——轻全局，精按需

> 本章为 round1 同章的深化版，假设读者已熟悉 round1 内容。
> 标注：`A/B` — 本机 CLI 或官方文档核验；`I` — 工程推论

---

## 习惯

**规则文件体系的目标是：每条规则只在需要它的场景下出现在上下文里，不在不需要它的场景里消耗注意力。不同工具实现这个目标的机制不同，需要分别配置。**

---

## 正确的做法

### 深化：Codex CLI 的规则载体是 AGENTS.md，不是 codex.md

round1 里把 Codex CLI 的规则文件写成 `codex.md`，这是错误的（`A/B`：OpenAI Codex 官方文档）。

Codex CLI 使用的是 `AGENTS.md`，与 OpenAI 的 agent 文档体系一致：
- 项目根目录的 `AGENTS.md` — 项目级规则
- 子目录的 `AGENTS.md` — 目录级规则（更深层目录覆盖上层）
- `--no-project-doc`：禁止加载项目文档（`A/B`：codex --help 未直接列出，但 README 提及）

**已修正的规则文件对照表**（`A/B`）：

| 工具 | 全局层 | 路径层 | 专项工作流层 |
|------|--------|--------|------------|
| Claude Code | `CLAUDE.md` | `.claude/rules/*.md`（YAML frontmatter） | subagents（`.claude/agents/`）|
| Cursor | `AGENTS.md` 或 always-apply 的 `.cursor/rules/*.mdc` | `.cursor/rules/*.mdc` 的 `globs` 字段 | Custom Modes / Background Agents |
| Aider | 通过 `--read` 或 `.aider.conf.yml` 显式加载的约定文件 | `--read` 按需加载特定文件 | `/ask` / `architect` 模式 / 专用 prompt 文件 |
| Codex CLI | `AGENTS.md` | 更深层目录的 `AGENTS.md` | Skills / subagents |
| Gemini CLI | `GEMINI.md` | 子目录的 `GEMINI.md`（JIT 加载）| 无内置 |

### 深化：Aider 没有"自动感知的约定文件"

Aider 的 `CONVENTIONS.md` **不会被自动加载**（`A/B`：官方文档）。必须通过以下方式之一显式加载：

```bash
# 方式 1：启动参数
aider --read CONVENTIONS.md

# 方式 2：交互式命令
/read CONVENTIONS.md

# 方式 3：配置文件（.aider.conf.yml）
read:
  - CONVENTIONS.md
```

这意味着 Aider 的"全局规则"需要在配置文件里预设，而不像 Claude Code 的 CLAUDE.md 那样自动加载。实践上，`.aider.conf.yml` 是 Aider 的"规则自动加载"机制。

### 深化：Claude Code 的规则层级加载顺序

Claude Code 的规则加载有明确的优先级（`A/B`：官方文档）：

```
全局策略（企业级）
  /etc/claude-code/CLAUDE.md（Linux）
    ↓
用户级偏好
  ~/.claude/CLAUDE.md
    ↓
项目级规则
  <project-root>/CLAUDE.md
    ↓
目录级规则
  .claude/rules/*.md（YAML frontmatter 指定路径匹配）
    ↓
个人本地覆盖
  CLAUDE.local.md（.gitignore 掉，不提交）
```

`.claude/rules/*.md` 的路径匹配使用 YAML frontmatter（`A/B`）：
```yaml
---
paths:
  - "src/api/**/*.ts"
---
## API 路由规范
...
```

只有当 AI 访问 `src/api/` 下的 TypeScript 文件时，这条规则才会被加载。

### 深化：Cursor 的规则体系现状

Cursor 的规则体系经历了演变，round1 里的说法需要更新（`A/B`：Cursor 官方文档研究）：

- `.cursorrules`：仍然支持，但是 **legacy** 格式
- `.cursor/rules/*.mdc`：当前推荐格式，支持 `globs`、`alwaysApply`、description 等字段
- `AGENTS.md`：Cursor 官方支持，与其他工具共享

**alwaysApply vs Glob 触发的实际行为**（`A/B`）：

```yaml
# alwaysApply: true → 每次对话都加载，无论当前文件是什么
---
alwaysApply: true
---
## 核心约束（总是有效的规则放这里）

# globs 触发 → 只在访问匹配文件时加载
---
globs:
  - "src/components/**/*.tsx"
alwaysApply: false
---
## React 组件规范（只在处理 React 组件时有效）
```

**Token 成本的差异**（`I`）：
- `alwaysApply: true` 的规则：每次对话都消耗（固定开销）
- `globs` 触发的规则：只在处理特定文件时消耗（按需开销）
- 对于和具体文件类型紧密绑定的规范，优先用 globs 而不是 alwaysApply

---

## 错误的可能

### 新增错误：以为 Codex CLI 读取 codex.md 或 CLAUDE.md

Codex CLI 读取的是 `AGENTS.md`（`A/B`）。如果你在项目里只有 `CLAUDE.md`，Codex CLI 不会自动读取它。

对于同时使用 Claude Code 和 Codex CLI 的项目，需要：
- `CLAUDE.md`（Claude Code 读取）
- `AGENTS.md`（Codex CLI 读取，也被 Cursor 支持）
- 或者维护一份统一源，再同步到两个文件

### 深化：规则文件"越精简越好"和"精简的边界在哪里"

round1 说全局规则限制在 800 Token / 60 行以内。这个数字是合理的下限指导，但"60 行"本身不是边界，**不相关内容的比例**才是判断标准（`I`）：

- 如果一个规则在 80% 的任务里都是相关的 → 适合放全局层
- 如果一个规则只在处理特定文件时才相关 → 应该放路径层
- 如果一个规则只在执行特定工作流时才相关 → 应该放专项工作流层

---

## 背后的原理

### 深化：注意力稀释的工具级表现

round1 解释了注意力稀释的原理。round2 补充工具层面的表现（`I`）：

当 `alwaysApply: true` 的规则文件里包含了大量和当前任务无关的内容时，观察到的现象通常是：
- AI 在简单任务上过度工程化（试图满足不相关的规范）
- AI 生成的代码同时包含多个框架的风格（规范文件里有多种框架规范）
- AI 在回答问题时引用了无关的约束条件

这些都是"注意力被不相关内容分散"的具体表现，不只是抽象的"Token 浪费"。

### 深化：Aider --cache-prompts 和规则文件的配合

当使用 `--cache-prompts` 时，通过 `--read` 加载的文件会被**缓存**（`A/B`：Aider 官方文档）。这意味着：

```bash
aider --cache-prompts \
      --read CONVENTIONS.md \
      --read docs/architecture.md \
      src/auth/login.ts
```

在这个配置里：
- `CONVENTIONS.md` 和 `docs/architecture.md` 在第一次加载后被缓存
- 后续多轮对话中，这两个文件不需要重新计算
- 节省的是**重复加载相同只读文件**的计算成本

但如果这两个文件内容变化了，缓存会失效，需要重新计算。所以"让只读参考文件保持稳定"对 Aider 的缓存效益也很重要（`I`）。

---

## 延伸的习惯

### 深化：跨工具统一规则的实践方案

对于同时使用 Claude Code + Cursor + Codex CLI 的团队，维护三份独立的规则文件成本很高。一种实践方案（`I`）：

```
项目结构：
.ai-rules/
├── core.md          # 核心约束（所有工具共享的内容）
├── typescript.md    # TypeScript 规范
└── testing.md       # 测试规范

# CLAUDE.md（Claude Code）
<!-- 引用自 .ai-rules/ -->
@.ai-rules/core.md
@.ai-rules/typescript.md

# AGENTS.md（Codex CLI + Cursor）
# 内容与 CLAUDE.md 同步，或直接包含相同内容
```

注意：`@` 语法是否在 CLAUDE.md 里有效，需要验证（`A/B` 待核验）。如果不支持，则需要维护两份内容相同的文件，或使用 rulesync 这类工具自动同步。

---

## 个人 / 协作注意

**个人使用**：对 Codex CLI 用户，检查项目里是否有 `AGENTS.md`；如果只有 `CLAUDE.md`，Codex CLI 不会读取它，需要补一个 `AGENTS.md`（可以内容相同）。

**团队协作**：
- 把 `CLAUDE.md` 和 `AGENTS.md` 都纳入版本控制
- 个人偏好（编辑器设置等）放 `CLAUDE.local.md` 并加入 `.gitignore`
- 对 `.cursor/rules/*.mdc` 里的 `alwaysApply: true` 规则做定期审查，确保这些规则真的需要在所有任务里出现

---

## 延伸调查方向

1. **规则文件大小与输出质量的量化关系**：目前无可靠公开数据。工程推论（`I`）：关键变量不是 Token 数量本身，而是"规则文件里和当前任务无关的内容比例"。

2. **alwaysApply 的实际 Token 成本**：可以通过实测量化——比较同一任务在有/无 alwaysApply 规则下的 API 调用 Token 消耗差异。目前无公开数据。

3. **单一规则源同步到多工具的实际效果**：工具对规则文件的解析方式不同（Claude Code 支持 YAML frontmatter 路径匹配，Cursor .mdc 有自己的 frontmatter 格式，AGENTS.md 格式相对简单）。统一源同步时是否会有语义损失，需要测试。

4. **Skill/subagent 机制的跨工具兼容性**：Claude Code 的 subagents 和 Codex CLI 的 Skills 在概念上类似，但实现机制不同。能否用同一份 SKILL.md 在两个工具里复用，目前没有公开验证。
