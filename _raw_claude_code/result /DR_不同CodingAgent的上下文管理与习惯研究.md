# Deep Research 主题上下文：不同 Coding Agent 的上下文管理习惯研究

这是一份给 Deep Research 的总控入口，不是完整研究说明书。

## 总问题

研究主题是：

**不同 coding agent 中，用户如何通过“上下文管理习惯”同时降低成本并提升效果。**

这里说的“上下文管理”，请先按用户能理解的方式理解，不要一上来跑偏到纯技术定义：

- 用户怎么发第一轮任务
- 用户怎么管理多轮对话
- 用户怎么处理长会话
- 用户怎么放置稳定背景、规则、记忆、大材料
- 用户怎么决定是否开工具、联网、切模型

## 先限定研究对象

请确认研究对象是 **coding agent**，不是泛泛的 AI chat 产品。

优先覆盖这些产品：

- Claude Code
- Codex / OpenAI Codex / ChatGPT coding workflow
- Cursor
- Trae
- OpenCode
- Aider
- Continue
- Cline / Roo Code
- Windsurf
- Gemini CLI / Gemini Code Assist

## 子上下文入口

- [01_DR_通用上下文管理习惯.md](file:///Users/bowhead/ai_agent_coding/claude_code/result%20/01_DR_通用上下文管理习惯.md)
- [02_DR_产品机制与上下文承载.md](file:///Users/bowhead/ai_agent_coding/claude_code/result%20/02_DR_产品机制与上下文承载.md)
- [03_DR_成本_模型_工具_额度策略.md](file:///Users/bowhead/ai_agent_coding/claude_code/result%20/03_DR_成本_模型_工具_额度策略.md)
- [04_DR_典型任务场景与工作流.md](file:///Users/bowhead/ai_agent_coding/claude_code/result%20/04_DR_典型任务场景与工作流.md)

## 搜索时先用这些中英混合词把方向卡住

- coding agent context management
- coding agent 用户习惯
- AI 编程助手 上下文管理
- agent memory best practices
- coding workflow context
- project rules / workspace memory / session compact
- codebase indexing / retrieval / RAG
- tool calling cost / model routing / long session

## 优先去这些地方找

- 官方文档
- 官方 blog / changelog / docs
- 官方论坛、GitHub repo、GitHub issues、GitHub discussions
- 团队成员公开访谈、X 线程、演讲、播客
- 高质量用户实战复盘

如果社区内容很强，优先找那种：

- 有截图、有操作细节
- 提到具体产品机制
- 能解释为什么省 token、为什么更稳
- 不只是泛泛 prompt 技巧

## 这个总文件的作用

它只负责定总题、限定对象、给出搜索方向和分流入口。
