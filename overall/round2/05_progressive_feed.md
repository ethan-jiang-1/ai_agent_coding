# 第五章（深化）：渐进式信息投喂——用指针代替粘贴

> 本章为 round1 同章的深化版，假设读者已熟悉 round1 内容。
> 标注：`A/B` — 本机 CLI 或官方文档核验；`I` — 工程推论

---

## 习惯

**Aider 的 repo map 是"工具自己解决信息检索"的最成熟实现——它用图排名算法自动决定给模型看哪些文件的摘要。理解它的工作机制，能帮你更好地配置信息投喂策略。**

---

## 正确的做法

### 深化：Aider repo map 是如何工作的

round1 提到了 Aider 的 repo map，但没有解释具体机制。官方说明（`A/B`：Aider 官方文档）：

**repo map 是什么**：整个 git 仓库的简明映射，包含最重要的类、函数及其类型和调用签名。

**工作机制**：
1. 把整个代码库解析为依赖图（每个文件是节点，依赖关系是边）
2. 使用**图排名算法**（类似 PageRank）计算每个符号的重要性
3. 根据当前聊天中**实际 /add 的文件**，动态调整哪些部分的 repo map 最相关
4. 把最相关的部分（在 `--map-tokens` 预算内）注入到上下文

**大小控制**（`A/B`）：
- 默认：1024 tokens（`--map-tokens 1024`）
- 对大型仓库，可以适当增大（如 `--map-tokens 4096`）
- 如果没有 /add 任何文件，Aider 可能显著扩大 repo map 以帮助模型理解整个仓库

**对"信息投喂"的实际意义**（`I`）：

repo map 意味着你不需要手动告诉 AI"相关的文件还有 X 和 Y"——AI 通过 repo map 已经知道你 /add 的文件和哪些其他文件有依赖关系。这让 Aider 的信息投喂比其他工具更"自动化"：

```bash
# 你只需要 /add 要修改的文件
aider src/auth/login.ts

# Aider 会自动在 repo map 里包含 login.ts 的依赖关系
# 比如 User 模型、认证中间件等相关文件的函数签名
# 你不需要手动 /add 这些依赖文件
```

相比之下，Claude Code 和 Codex CLI 需要你在提示词里明确说明"还有哪些相关文件"，或者让 AI 自己用工具去探索。

### 深化：各工具"引用文件"的机制对比

| 工具 | 引用文件的方式 | 底层机制 |
|------|--------------|---------|
| Aider | `/add`（可编辑）、`--read`（只读）| 直接注入文件内容；repo map 自动补充相关文件的摘要 |
| Claude Code | 在提示词里指定路径（AI 自主读取）| AI 使用 Read 工具，按需读取文件 |
| Codex CLI | 在提示词里指定路径（AI 自主读取）| AI 使用工具，在 sandbox 限制内读取文件 |
| Cursor | `@file`（引用文件内容）| RAG 精准截取相关部分，不一定注入全文 |

**实践区别**（`I`）：
- Aider：你控制"哪些文件是可编辑范围"，repo map 自动处理"还有什么相关"
- Claude Code / Codex CLI：你在提示词里说"先读 X 文件和 Y 文件"，AI 按指示读取
- Cursor：你用 @file 指定，Cursor 的 RAG 决定注入多少内容

### 深化：管道输入在各工具中的实际支持

**Claude Code**（`A/B`：claude --help 核验）：

```bash
# stdin 作为补充输入
cat error.log | claude "分析根因"

# --print 模式下，stdin 是完整的 prompt
cat prompt.txt | claude --print

# stream-json 模式（高级）
cat stream-input.json | claude --print --input-format stream-json
```

**Codex CLI**（`A/B`：codex exec --help 核验）：

```bash
# `-` 作为 PROMPT 参数：从 stdin 读取指令
echo "分析这段日志" | codex exec -

# 如果同时提供了 PROMPT 和 stdin，stdin 作为 <stdin> 块追加
cat error.log | codex exec "分析根因"
# 等价于：提示词 = "分析根因" + stdin 内容块
```

**Aider**（`A/B`：官方文档）：

```bash
make test 2>&1 | aider "修复这些测试失败"
```

---

## 错误的可能

### 深化：Aider 里"不 /add 就不会修改"是一个保障

`--read` 的文件 AI 无法修改（`A/B`：Aider 官方）。这不只是约定，而是工具层面的保障。

实践意义：当你不确定 AI 会不会修改某个文件时，用 `--read` 而不是 `/add`——这比在提示词里说"不要修改这个文件"更可靠。

```bash
# 不确定是否要修改 schema 文件时
aider --read src/models/user.schema.ts \  # 只读参考
      src/auth/login.ts                   # 可编辑目标
```

### 深化：repo map 的大小设置不是越大越好

虽然 repo map 帮助 AI 了解代码库结构，但 repo map 本身消耗 context 空间（`A/B + I`）：

- 默认 1024 tokens 的 repo map 对小型项目通常足够
- 对大型项目（数百个文件），可以适当增大（4096 tokens）
- 但 repo map 占用了用于实际代码内容的空间，需要权衡

如果 `--map-tokens` 设得太大：
- repo map 包含过多不相关文件的摘要
- 注意力被分散到不相关的代码上
- 实际修改的准确率可能反而下降

经验参考（`I`）：对于 10-50 个文件的项目，1024-2048 tokens 通常够用；对于 50+ 文件的项目，2048-4096 tokens 可能更合适。

---

## 背后的原理

### 深化：repo map 的图排名算法

round1 说 Aider "使用 AST 图谱"，这是不准确的。官方说法（`A/B`）是：

- 把代码库解析为依赖图（不是 AST，是依赖关系图）
- 使用图排名算法（官方说类似 PageRank，但没有明确说就是 PageRank）
- 依赖关系是边：函数调用、import、类继承等

**PageRank 类比**（`I`）：PageRank 把"被链接次数多的页面"认为更重要；repo map 的图排名把"被更多文件引用的函数/类"认为更重要。当前 /add 的文件的直接依赖，会得到更高的权重。

这意味着：repo map 里展示的摘要，是"在理解当前要修改的文件时，最可能需要了解的其他代码"——而不是随机选择的。

### 深化：渐进式披露的三层结构

round1 提出了三轮渐进的节奏，round2 补充每一层对应的工具机制（`I`）：

```
第一轮：任务描述 + 核心约束
  → Claude Code: 直接在提示词里说
  → Aider: /add 目标文件 + 在提示词里说约束
  → Codex CLI: 在 prompt 里说

第二轮：按需补具体文件或文档
  → Claude Code: AI 用 Read 工具自主读取，或在提示词里指定路径
  → Aider: /read 追加只读参考文件
  → Codex CLI: stdin 追加（通过管道）

第三轮：补测试、日志或 diff
  → 所有工具: 管道传入（最完整的方式）
```

---

## 延伸的习惯

### 深化：Aider 里 --map-tokens 和 --read 的配合

对于有大量参考文件需要了解但不需要修改的任务（比如理解一个陌生代码库）：

```bash
# 让 repo map 负责结构理解，--read 补充关键文档
aider \
  --map-tokens 4096 \          # 更大的 repo map（理解整个仓库）
  --read docs/architecture.md \ # 手动补充文档
  --read CONVENTIONS.md \        # 约定文件
  --message "只分析，不修改任何文件。给我这个认证模块的工作原理"
```

### 深化：Codex CLI 的 --output-last-message 用于信息传递

当需要把一个 exec 的输出作为下一个 exec 的输入时（`A/B`）：

```bash
# 第一步：分析，输出到文件
codex exec --ephemeral -s read-only \
  "分析 src/auth/ 的核心接口，只输出接口摘要" \
  -o /tmp/auth-interfaces.md

# 第二步：基于摘要文件生成代码
cat /tmp/auth-interfaces.md | codex exec -s workspace-write \
  "基于这些接口定义，实现 OAuth2 集成的中间件"
```

这种方式把信息传递显式化，而不是依赖 AI 在单一会话里记住大量上下文。

---

## 个人 / 协作注意

**个人使用**：对于 Aider 用户，把 `--map-tokens` 的默认值调整到适合你项目规模的大小，然后固化在 `.aider.conf.yml` 里，是一次性的配置收益。

**团队协作**：
- 共享 MCP 配置文件（`.claude/mcp.json` 或等价文件）纳入版本控制，确保团队成员都使用相同的 MCP 接入点
- 对于频繁使用的第三方 API，统一配置 MCP 入口，而不是让每人临时搜索

---

## 延伸调查方向

1. **引用 vs 粘贴的 Token 效率差异**：对于 Aider，`/add` 注入全文 vs Cursor `@file` 使用 RAG 截取，Token 消耗差异有多大？在什么情况下 RAG 截取的信息损失是可接受的？目前无公开对比数据。

2. **llms.txt 的实际覆盖率**：主流框架（React, Next.js, FastAPI 等）提供 llms.txt 的比例，以及质量评估。目前是快速演变的领域，没有系统性统计。

3. **MCP 在 API 集成场景的收益**：用官方 OpenAPI MCP vs Web Search，在首次运行成功率上的差异。这需要对照实验，目前无公开数据。

4. **日志完整性对诊断质量的影响**：管道传入（完整日志）vs 手工复制（可能截断）对 AI 诊断准确率的影响，理论上差异应该显著，但目前无系统性数据。
