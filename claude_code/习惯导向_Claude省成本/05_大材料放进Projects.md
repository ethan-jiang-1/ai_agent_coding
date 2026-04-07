# 习惯 5：大材料放进 Projects

## 这个习惯是什么

长报告、合同、风格指南、PRD、课程资料，不要每开一个对话就重新上传一遍，而要放进同一个 Project 里持续复用。

## 为什么这会省成本

- 重复上传同一材料，等于重复让系统处理同一背景
- 大文档全量灌入上下文，本来就不经济
- 稳定资料进入项目后，更适合被检索和缓存

## 背后的机制

Anthropic 官方说明，Projects 使用 RAG，只把相关内容加载进 context window。usage best practices 也明确写到：Project 中的文档会被缓存，后续引用时只有新内容或未缓存部分计入限制。

## 手动对话例子

如果你这一周都在研究同一份 200 页行业报告：

- 低效方法：会话 A 上传一次，会话 B 再上传一次，会话 C 继续重复上传
- 高效方法：先把报告放进一个项目，后面很多轮问题都只围绕这个项目继续问

## Claude Code 对应动作

Claude Code 没有网页里的 Project 按钮，但思路一样：

- 把稳定规则写进 `CLAUDE.md`
- 把项目级知识沉淀到项目目录
- 不要每次任务都把同一段背景粘贴一遍

## 这类习惯真正优化的是什么

它优化的是“大材料复用成本”。不是把资料藏起来，而是减少无差别重复加载。

## 参考

- [01_用量与上下文.md](file:///Users/bowhead/ai_agent_coding/claude_code/raw_saving_cost/reference/01_用量与上下文.md)
- [02_缓存与项目.md](file:///Users/bowhead/ai_agent_coding/claude_code/raw_saving_cost/reference/02_缓存与项目.md)
