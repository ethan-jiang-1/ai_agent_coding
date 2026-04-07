# 第五章：渐进式信息投喂——@ 引用替代全文粘贴

## 习惯

**外部材料用动态指针挂载，而不是复制粘贴整篇文档。只在 AI 真正需要某段信息的时刻，才注入最低限度的数据。**

---

## 正确的做法

### 核心原则：指针优于内容

把信息投喂给 AI，有两种方式：
- **内容投喂**：把文档全文复制粘贴到对话框
- **指针投喂**：告诉 AI 去哪里找信息

在大多数情况下，指针投喂是更好的选择。原因很简单：AI 有能力自己读取文件，你不需要帮它搬运。

### @ 引用：各工具的具体操作

**Cursor**
```
# 引用本地文件
"参考 @src/auth/login.ts 的实现方式，为 register.ts 写类似的逻辑"

# 引用文档
"根据 @docs/api-spec.md 中的 /users 接口定义，生成对应的客户端调用代码"

# 引用网页（实时抓取）
"参考 @https://docs.stripe.com/api/charges 实现支付接口"

# 引用 Git Diff
"审查 @git diff 中的变更，检查是否有安全问题"
```

**Claude Code（CLI）**
```bash
# 通过管道传入错误日志（而不是手动复制）
cat error.log | claude "分析这个错误，找出根本原因"

# 引用特定文件（AI 会自主读取）
claude "参考 src/db/pool.ts 的连接池实现，在 src/db/cache.ts 里实现类似的 Redis 连接池"

# 引用多个相关文件
claude "理解 src/auth/ 目录下的现有实现，然后..."
```

**Continue.dev（VS Code 插件）**
```
# 精确引用（@ 指令）
@File src/api/users.ts        # 引用整个文件
@Code UserService.findById    # 引用特定函数
@Git Diff                     # 引用当前分支的变更
@Terminal                     # 引用上一条终端命令及其输出
@Docs                         # 引用项目文档
```

**Aider（CLI）**
```bash
# --read 只读引用（不允许 AI 修改这些文件）
aider --read docs/api-spec.md --read CONVENTIONS.md

# /add 允许修改的文件（只加需要改的，不加整个目录）
/add src/auth/login.ts

# 管道传入错误
make test 2>&1 | aider "修复这些测试失败"
```

**Codex CLI**
```bash
# 管道传入（与 Claude Code 相同的方式，效果最佳）
cat error.log | codex "分析根本原因"
make test 2>&1 | codex "修复这些测试失败"

# 引用特定文件（在提示词中明确指定，AI 会在沙箱内读取）
codex "参考 src/db/pool.ts 的连接池实现，
       在 src/db/cache.ts 里实现类似的 Redis 连接池"

# 注意：Codex CLI 不支持 @ 语法，
# 通过提示词描述 + 沙箱内自主读取来代替
```

### 外部文档的正确处理方式

**优先找 llms.txt，而不是 HTML 文档页**

越来越多的框架和服务提供专门为 AI 优化的文档格式：

```bash
# 传统方式（错误）：复制 HTML 文档页的内容
# 问题：包含导航栏、广告、代码高亮标签等大量 HTML 噪音

# 现代方式（正确）：找 llms.txt
curl https://docs.anthropic.com/llms.txt
curl https://fastapi.tiangolo.com/llms.txt
curl https://docs.pydantic.dev/llms-full.txt
```

llms.txt 是专门为机器阅读设计的 Markdown 格式，剥离了所有视觉噪音，Token 密度远高于 HTML 页面。

**通过 MCP 接入 API 规范**

对于需要频繁集成的第三方服务（如 Stripe、Twilio、AWS），可以通过 MCP 服务器接入官方的 OpenAPI/Swagger 规范：

```json
// .claude/mcp.json 配置示例
{
  "mcpServers": {
    "stripe-api": {
      "command": "npx",
      "args": ["@anthropic/mcp-server-openapi", "--spec", "https://raw.githubusercontent.com/stripe/openapi/master/openapi/spec3.json"]
    }
  }
}
```

这样做的好处：AI 查询的是机器可解析的端点契约，而不是可能过时的网页教程。

### 渐进式披露的时机控制

不是所有信息都需要在第一轮就提供：

```
第一轮：提供任务描述 + 核心约束
  ↓ AI 开始工作，遇到不确定的地方
第二轮：按需提供具体文件或文档片段
  ↓ AI 完成主要逻辑，需要验证
第三轮：提供测试用例或错误日志
```

**原则**：在 AI 确实需要某段信息之前，不要主动提供。过早提供大量信息只会稀释注意力。

---

## 错误的可能

### 错误一：暴力粘贴整篇 API 文档

```
# 错误的做法
用户：[粘贴了 200 行 Stripe API 文档]
"根据上面的文档，帮我实现支付接口"

# 问题
1. 200 行文档里，真正需要的可能只有 20 行
2. HTML 转换后的文本包含大量格式噪音
3. 文档版本可能已过时（Stripe 的 API 经常更新）
4. 瞬间消耗大量 Token，却未必提升输出质量
```

**真实后果**：AI 集成第三方 API 时，如果通过 Web Search 自行搜索或用户粘贴了过时文档，可能会生成调用不存在接口的代码。这种错误很难发现，因为代码语法上是正确的，只有在运行时才会报错。

有用户报告，通过 MCP 接入官方 OpenAPI 规范后，API 集成代码在第一次运行时的成功率从约 60% 提升到接近 100%。

### 错误二：一次性挂载过多无关文件

```
# 错误的做法（在 Cursor 中）
@src/auth/login.ts @src/auth/register.ts @src/auth/middleware.ts 
@src/models/User.ts @src/models/Session.ts @src/config/database.ts
@src/utils/crypto.ts @src/utils/email.ts
"帮我修改密码重置功能"

# 正确的做法
@src/auth/reset-password.ts @src/models/User.ts
"帮我修改密码重置功能，确保 token 有 15 分钟的过期时间"
```

挂载越多无关文件，AI 的"注意力"越分散。如果你知道问题在哪里，精确挂载相关文件，而不是"以防万一"地挂载所有可能相关的文件。

### 错误三：依赖 AI 通过 Web Search 自行获取文档

```
# 危险的做法（不设限的 Web Search）
"搜索一下 Stripe 的最新 API，然后帮我实现支付"

# 实际发生的事情
1. AI 搜索，可能抓到 SEO 排名高的教程博客
2. 博客基于 Stripe API v2，实际已经是 v3
3. AI 生成了基于过时接口的代码
4. 代码运行失败，用户还不知道原因是文档版本
```

**真实案例**：在 API 集成场景中，"特定领域知识失效（Domain-Specific Knowledge Failures）"是 AI 生成错误代码最常见的原因之一。AI 训练数据的截止日期往往落后于 API 的实际版本，加上 Web Search 可能抓到过时内容，双重风险叠加。

### 错误四：把错误日志手动复制粘贴（而不是管道传入）

```bash
# 错误的做法
# 用户手动复制了终端里的错误日志，粘贴到对话框
# 问题：复制可能不完整，格式可能乱掉，耗时费力

# 正确的做法
make test 2>&1 | claude "修复这些测试失败"
# 或者
cat error.log | claude "分析根本原因"
```

管道传入不只是方便，更重要的是**完整性**——手动复制经常会截断重要的堆栈信息。

---

## 背后的原理

### 注意力的稀缺性

Transformer 模型在处理输入时，每个 Token 都会和其他所有 Token 计算注意力权重。当上下文里有 50,000 个 Token 时，AI 需要在这 50,000 个 Token 之间分配注意力。

**关键问题**：上下文里的无关内容不是"中性"的，它们会主动"消耗"注意力权重，降低 AI 对真正相关内容的关注度。

**类比**：你在嘈杂的咖啡馆里试图听清楚一段对话。背景噪音越大，你需要付出的认知努力越多，听清楚的效率越低。把 200 行文档里不相关的 180 行都放进上下文，相当于把背景噪音调大。

### 信息的时效性问题

AI 的训练数据有截止日期（通常滞后 6-12 个月）。对于快速迭代的 API 和框架：

- Stripe 每年会弃用旧 API 并推出新版本
- React 在 v18 和 v19 之间引入了重大的并发模式变化
- AWS SDK 的接口定期重构

llms.txt 和 MCP OpenAPI 规范直接指向官方最新文档，消除了训练数据滞后的风险。

### 管道的精确性

手动复制的错误日志通常只包含用户认为"重要"的部分，而忽略了可能更关键的堆栈尾部信息。管道传入保证了信息的完整性，同时避免了人工判断"哪部分重要"的认知偏差。

---

## 延伸的习惯

### 建立项目文档索引

在 CLAUDE.md 里维护一个"知识图谱入口"：

```markdown
## 文档索引
- API 规范：@docs/api-spec.md
- 数据库 Schema：@docs/schema.md
- 架构决策记录：@docs/adr/
- 第三方服务集成：@docs/integrations/
```

AI 在需要特定信息时，可以通过这个索引找到对应文档，而不需要你每次手动提供。

### MCP 服务器的按需连接模式

```
# 调试阶段：连接 Sentry MCP，获取生产环境错误详情
# 调试完成：断开 Sentry MCP，避免后续无关任务意外调用

# API 集成阶段：连接 Stripe OpenAPI MCP
# 集成完成：断开，避免 AI 在不相关任务中"自作主张"查询 API
```

MCP 不是"永久挂载"，而是"按场景挂载"。

### 错误日志的预处理

对于特别长的错误日志，先做一次轻量预处理再传给 AI：

```bash
# 只传最后 50 行（通常包含最关键的错误信息）
tail -50 error.log | claude "分析根本原因"

# 过滤掉已知的无关警告
grep -v "DeprecationWarning" error.log | claude "..."

# 提取特定的错误类型
grep "ERROR\|FATAL" app.log | claude "..."
```

---

## 个人 / 协作注意

**个人使用**：养成用 @ 引用而不是粘贴的习惯需要一点时间，但一旦形成，会显著减少每次对话的 Token 消耗和返工率。特别是对于频繁集成新 API 的场景，建立 MCP 连接的一次性投入能带来持续的质量提升。

**团队协作**：
- 团队共享的 MCP 配置文件（.claude/mcp.json）应该纳入版本控制
- 建立"禁用 Web Search 默认开启"的团队规范，只在明确需要时手动开启
- 对于团队常用的第三方 API，建立标准的 MCP 接入配置，避免每个人重复配置

---

## 延伸调查方向

1. **@ 引用 vs 直接粘贴的 Token 效率对比**：在相同的任务下，使用 @File 引用 vs 复制粘贴文件内容，AI 的输出质量和 Token 消耗分别是多少？Cursor 的后台 RAG 截取了多少比例的文件内容？

2. **llms.txt 的实际采用率**：目前主流框架（Next.js、FastAPI、Stripe 等）中，有多少已经提供了 llms.txt？质量如何？与官方 HTML 文档相比，信息覆盖率有多大差距？

3. **MCP 在 API 集成场景的成功率提升**：有没有系统性的数据，对比"使用 OpenAPI MCP"和"使用 Web Search"两种方式在 API 集成代码第一次运行成功率上的差异？

4. **管道传入 vs 手动复制的信息完整性**：在实际的调试场景中，手动复制错误日志丢失关键信息的频率有多高？有没有研究这一现象对 AI 诊断准确率的影响？
