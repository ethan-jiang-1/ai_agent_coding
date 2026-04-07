# 第四章：规则文件的分层设计——轻全局，精按需

## 习惯

**全局规则文件保持极度精简（≤800 Token / 60 行），复杂规则按路径和触发条件分层存放，专项工作流封装成 Skill 按需调用。**

---

## 正确的做法

### 三层规则架构

| 层级 | 载体 | 加载时机 | 适合存放的内容 |
|------|------|---------|--------------|
| 全局层 | CLAUDE.md / .cursorrules | 每次会话自动加载 | Git 流程、技术栈基线、禁止事项（≤800 Token） |
| 路径层 | .cursor/rules/*.mdc / .claude/rules/*.md | 访问特定文件时触发 | 框架特定规范、目录级约束 |
| 技能层 | SKILL.md / Agent Skills | 明确调用时加载 | 复杂工作流、专项任务模板 |

### 全局层：只放"宪法级"约束

```markdown
# CLAUDE.md（好的示例，控制在 60 行以内）

## 技术栈
- Node.js 20 + TypeScript 5.x
- 测试框架：Vitest（不是 Jest）
- 包管理器：pnpm（不是 npm/yarn）

## 提交规范
- 格式：feat/fix/chore: 描述
- 提交前必须运行：pnpm test

## 禁止事项
- 不要创建新文件，除非明确被要求
- 不要修改 src/legacy/ 目录下的任何文件
- 不要引入新的第三方依赖，除非明确被要求

## 参考
- API 规范：@docs/api-spec.md
- 架构决策：@docs/adr/
```

**核心原则**：全局规则只放"不变的宏观共识"。如果一条规则只在特定场景下有意义，它就不应该出现在全局层。

### 路径层：按文件类型精准触发

**Cursor（.mdc 文件）**
```yaml
# .cursor/rules/react-components.mdc
---
description: React 组件开发规范
globs:
  - "src/components/**/*.tsx"
  - "src/pages/**/*.tsx"
alwaysApply: false
---

## React 组件规范
- 使用函数式组件，不使用 class 组件
- Props 必须定义 TypeScript 接口
- 使用 React.memo 优化高频渲染组件
- 样式使用 CSS Modules，文件名格式：ComponentName.module.css
```

```yaml
# .cursor/rules/database.mdc
---
description: 数据库操作规范
globs:
  - "src/db/**/*.ts"
  - "src/models/**/*.ts"
alwaysApply: false
---

## 数据库规范
- 所有查询必须使用参数化查询，禁止字符串拼接
- 事务操作必须有明确的回滚逻辑
- 查询结果必须做空值检查
```

**效果**：当 AI 在编辑 React 组件时，数据库规范不会出现在上下文里；反之亦然。

**Claude Code（.claude/rules/ 目录）**
```yaml
# .claude/rules/api-routes.md
---
paths:
  - "src/api/**/*.ts"
---

## API 路由规范
- 所有路由必须有 Zod schema 验证
- 错误响应格式：{ error: string, code: string }
- 认证中间件：使用 requireAuth() 而不是直接检查 token
```

### 技能层：复杂工作流的按需调用

**SKILL.md 结构（跨工具通用）**
```markdown
# SKILL: security-audit

## 何时使用（When）
当被要求进行安全审查，或者修改了认证、权限、数据验证相关代码时

## 如何执行（How）
1. 检查所有用户输入是否经过验证和转义
2. 检查 SQL 查询是否使用参数化
3. 检查认证 token 的生命周期管理
4. 检查敏感数据是否被意外记录到日志
5. 输出安全报告，格式：[HIGH/MED/LOW] 描述 → 建议

## 产出（What）
一份结构化的安全问题清单，按严重程度排序
```

**Cursor Agent Skills** 和 **Trae Skills** 遵循类似的三段式结构。

### 各工具的规则文件对应关系

| 工具 | 全局层 | 路径层 | 技能层 |
|------|--------|--------|--------|
| Claude Code | CLAUDE.md | .claude/rules/*.md（YAML frontmatter） | SKILL.md |
| Cursor | .cursorrules 或 .cursor/rules/*.mdc | .mdc 的 globs 字段 | Agent Skills |
| Aider | CONVENTIONS.md（通过 --read 加载） | 无内置支持，通过 --read 手动加载 | 无内置支持 |
| Codex CLI | codex.md（项目根目录，启动时自动加载） | 无内置路径匹配，通过提示词指定范围 | 无内置支持 |
| Gemini CLI | GEMINI.md | JIT 目录扫描（自动加载子目录的 GEMINI.md） | 无内置支持 |
| Cline | .clinerules | 无内置支持 | Memory Bank 结构 |
| Windsurf | 工作区规则 | 目录级规则 | 无内置 Skill 概念 |

---

## 错误的可能

### 错误一：规则膨胀（Rule Bloat）

最常见的错误。用户把公司开发手册、所有可能的边缘情况处理指南、完整的框架文档全部塞进全局规则文件。

**真实后果**：一个包含 11,000 Token 的 .cursorrules 文件，即使用户只是让 AI "修改按钮颜色"，系统也会先加载这 11,000 个 Token。这不只是浪费钱，更会导致"注意力稀释"——AI 在处理简单的 CSS 问题时，脑子里还装着数据库迁移规范和 API 安全审计要求，注意力被无关内容分散。

有用户报告，精简 .cursorrules 文件后（从 8000 Token 降至 600 Token），AI 的输出质量反而提升了，因为模型能更专注于当前任务。

### 错误二：alwaysApply: true 滥用

在 Cursor 的 .mdc 文件中，将 `alwaysApply: true` 设置为默认值，导致所有规则在所有情况下都被加载。

**真实后果**：开发者在写 Python 脚本时，被强制注入 JSDoc 注释规范和 React Hooks 依赖注入标准。不相关的规则不只是浪费 Token，还会让 AI 产生混乱，试图在 Python 代码里应用 JavaScript 规范。

### 错误三：在规则文件里大段复制代码示例

```markdown
# 错误的做法（规则文件里内联大量代码）
## 标准的 API 路由写法
\`\`\`typescript
import { Router } from 'express';
import { z } from 'zod';
// ... 50 行代码示例
\`\`\`

# 正确的做法（引用而不是内联）
## API 路由规范
参考标准实现：@src/api/users.ts（第 1-30 行）
```

代码示例会迅速占满 Token 预算，而且当代码库迭代后，规则文件里的示例可能已经过时，但没人记得更新它。

### 错误四：忽视跨工具碎片化问题

一个团队同时使用 Cursor（前端）和 Claude Code（后端），各自维护 .cursorrules 和 CLAUDE.md，内容不同步。

**真实后果**：前端 AI 和后端 AI 遵循不同的命名规范，生成的接口类型定义风格不一致，导致集成时需要大量手工调整。

---

## 背后的原理

### 注意力稀释（Attention Dilution）

Transformer 模型的注意力机制有固定的计算预算。当上下文里有 10,000 个 Token 时，每个 Token 能分配到的注意力权重比上下文只有 1,000 个 Token 时低得多。

**类比**：想象你在解一道数学题，但同时桌上摆着 20 本无关的书，你的注意力会被这些书分散。把无关的书移走，你的解题速度和准确率都会提升。

全局规则文件的每一行，都在"分散" AI 处理当前任务的注意力。只有当一条规则和当前任务直接相关时，它才应该出现在上下文里。

### Token 的固定成本

全局规则文件在**每一次**会话启动时都会被加载。假设一个团队每天使用 AI 工具 100 次，一个 5000 Token 的规则文件，每天产生 500,000 个无谓的 Input Token 消耗——其中大部分在大多数任务里完全用不上。

相比之下，一个 500 Token 的精简全局文件 + 按需触发的路径规则，在保持相同约束效果的同时，可以将规则文件相关的 Token 消耗降低 80-90%。

### 提示词缓存的协同效应

精简的、稳定的全局规则文件，更容易触发提示词缓存（KV Cache）。当规则文件内容不频繁变化时，每次会话启动时的规则文件加载可以直接命中缓存，将 Input Token 成本降至 10%（Anthropic）或接近免费（Gemini）。反之，一个内容频繁变动的大型规则文件，每次修改都会导致缓存失效，所有 Token 重新全量计费。

（KV Cache 的详细机制见第六章）

---

## 延伸的习惯

### 负面约束优先于正面建议

```markdown
# 效率低的规则（正面建议）
"应该使用函数式组件，推荐使用 React Hooks，
建议使用 TypeScript 定义 Props..."

# 效率高的规则（负面约束）
"禁止使用 class-based 组件"
"禁止 Props 不定义 TypeScript 类型"
```

负面约束用更少的 Token 达到更强的约束效果。"禁止"比"建议"更清晰，AI 不需要推断"建议"的强度。

### 引用锚点替代代码内联

```markdown
## 数据库连接池规范
- 参考标准实现：@src/db/pool.ts
- 禁止在路由处理器中直接创建新连接
```

AI 能自主读取 @src/db/pool.ts，不需要把内容内联到规则文件里。锚点引用既保持规则文件轻量，又确保 AI 看到的是最新的代码。

### 跨工具同步：rulesync / vibe-rules

对于同时使用多个 AI 工具的团队，可以使用 rulesync 这类 CLI 工具统一管理规则：

```bash
# 在 .ai-rules/ 目录维护单一数据源
.ai-rules/
├── core.md          # 核心约束（所有工具共享）
├── typescript.md    # TypeScript 规范
└── testing.md       # 测试规范

# rulesync 自动同步到各工具的格式
rulesync sync
# → CLAUDE.md
# → .cursorrules
# → .clinerules
```

---

## 个人 / 协作注意

**个人使用**：定期审查规则文件，删除已经过时或不再需要的规则。规则文件不是"越多越好"，每一行都应该有明确存在的理由。

**团队协作**：
- 规则文件纳入版本控制，修改需要 Code Review
- 个人偏好（如编辑器格式化设置）放 CLAUDE.local.md 并加入 .gitignore，不要污染团队共享的规则文件
- 建立规则文件的审查周期（如每个 sprint 结束时检查一次）

---

## 延伸调查方向

1. **规则文件大小与输出质量的量化关系**：有没有实验数据，测量 1000 Token vs 5000 Token vs 10000 Token 的规则文件对 AI 输出质量和准确率的影响？注意力稀释的临界点在哪里？

2. **alwaysApply 的实际 Token 成本**：在 Cursor 中，一个 2000 Token 的 .mdc 文件设置 alwaysApply: true vs 按 Glob 触发，一个月下来的 Token 消耗差异是多少？有没有用户做过实测？

3. **rulesync 的实际采用情况**：在多工具团队中，rulesync 这类统一管理工具的实际效果如何？有没有规则在不同工具格式转换时出现语义损失的情况？

4. **Skill 机制的跨工具兼容性**：SKILL.md 的概念能否在 Cursor、Claude Code、Aider 之间真正复用？各工具对 Skill 触发条件的解析方式有哪些差异？
