# 习惯 4：把常用文件上传到 Projects

## 原文定义

同一份常用文件不要在多个对话里反复上传，而要放进同一个 Project 里复用。

## 适用范围

- 主标签：通用
- Claude 生态关系：原文在 Claude 里对应的是 Projects
- 迁移理解：在别的 coding agent 里，对应“知识库常驻、项目资料复用、减少重复上传”

## 为什么原文这么强调

- 同一份 PDF 如果反复上传，系统会反复处理
- 长文档反复灌入上下文，本来就不经济
- 放进 Project 后，更适合持续引用

## 场景要怎么理解

低效方式：

- 会话 A 上传一次合同
- 会话 B 再上传同一份合同
- 会话 C 继续重复上传

更符合原文意思的方式：

- 先把合同、简报、风格指南这类常用文件放进同一个 Project
- 后面多个新对话都直接在这个 Project 里继续问

如果换成 coding agent 场景，也可以这样理解：

- 架构说明、数据库表结构、接口文档、部署说明，不要每次新会话都重新贴
- 应该让这些资料成为项目里的常驻背景
- 后续任务只补这次新增的信息，而不是把老资料全量重发

## 顺着原文补充的理解

这条习惯优化的是“稳定大材料的重复使用方式”。

## Claude Code 对应动作

Claude Code 里没有网页上的 Project 按钮，但可以照着同一个原则做：

- 把长期规则沉淀到 `CLAUDE.md`
- 把项目常用资料固定放在项目目录
- 不要每个任务都重新粘贴同样的大段背景

## 对照原文

- [原始文章](file:///Users/bowhead/ai_agent_coding/claude_code/raw_saving_cost/Datawhale_不再触发Claude使用限制，大幅降低Token的10个有效习惯.md#L87-L96)
- [02_缓存与项目.md](file:///Users/bowhead/ai_agent_coding/claude_code/raw_saving_cost/reference/02_缓存与项目.md)
