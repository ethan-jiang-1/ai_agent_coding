# Claude 成本优化的习惯导向拆解

这不是对原文的覆盖改写，而是把原文 10 个习惯按“更好执行”的方式重新整理，并与当前目录结构保持同步。

这份重构版现在遵循两个原则：

- 习惯定义严格跟原文走，不再额外发明新习惯
- 链接、命令映射、参考资料全部对齐当前目录

## 入口

- [原始文章](file:///Users/bowhead/ai_agent_coding/claude_code/raw_saving_cost/Datawhale_不再触发Claude使用限制，大幅降低Token的10个有效习惯.md)
- [习惯目录导航](file:///Users/bowhead/ai_agent_coding/claude_code/习惯导向_Claude省成本/00_导航.md)
- [Claude Code 命令映射](file:///Users/bowhead/ai_agent_coding/claude_code/习惯导向_Claude省成本/90_Claude_Code命令映射.md)

## 一句话理解

真正省成本，不是机械地少发消息，而是：

- 少制造无效上下文
- 少重复上传和重复交代
- 让稳定背景尽量复用
- 让能力档位和任务强度匹配

## 适用范围怎么读

- 通用：大多数 coding agent 都适用
- Claude 生态：更依赖 Claude 网页版、Claude App、Claude Code 或 Anthropic 的产品机制
- Claude Code 可落地：在 Claude Code 里有比较直接的命令入口或落地方式

---

## 习惯 1：多去编辑你的提示词

### 原文意思

当 Claude 没理解你的意思时，不要继续加“不是这个意思”“再改一下”这类新消息，优先编辑原始提示词后重新生成。

### 适用范围

- 通用
- Claude Code 可落地

### 为什么有用

- 每加一条纠错消息，后续每轮都要重读更多历史
- 真正任务目标容易被补丁式追问埋掉

### 例子

低效：

```text
第 1 轮：帮我写产品介绍
第 2 轮：不对，太硬了
第 3 轮：再短一点
第 4 轮：面向大学生
```

更符合原文：

```text
编辑第 1 轮原消息：
帮我写产品介绍，面向大学生，语气轻松，控制在 120 字以内，重点突出低门槛和快速上手。
```

### 对应卡片

- [01_多去编辑你的提示词.md](file:///Users/bowhead/ai_agent_coding/claude_code/习惯导向_Claude省成本/01_多去编辑你的提示词.md)

---

## 习惯 2：每 15～20 条消息就开新对话

### 原文意思

当一个对话已经很长时，让 Claude 先总结，再开一个新对话，把总结作为第一条消息继续。

### 适用范围

- 通用
- Claude Code 可落地

### 为什么有用

- 对话越长，后续每一轮重读的历史越多
- 大量 token 会花在重复读取旧消息上

### 例子

```text
第 1～18 轮：已经来回讨论很多内容
第 19 轮：请总结当前对话的已确定结论、未解决问题、当前约束和建议下一步
新会话第 1 轮：粘贴总结，继续工作
```

### Claude Code 对应动作

- `/compact`

### 对应卡片

- [02_每15到20条消息就开新对话.md](file:///Users/bowhead/ai_agent_coding/claude_code/习惯导向_Claude省成本/02_每15到20条消息就开新对话.md)

---

## 习惯 3：把问题合并成一条消息发送

### 原文意思

多个相关任务，尽量放在一条提示词里一次发出去，不要拆成多条消息逐轮补发。

### 适用范围

- 通用
- Claude Code 可落地

### 为什么有用

- 多条独立消息意味着多次上下文加载
- 第一轮就看到完整需求，模型通常更容易做对

### 例子

低效：

```text
第 1 轮：总结这篇文章
第 2 轮：列出要点
第 3 轮：给我一个标题
```

高效：

```text
第 1 轮：总结这篇文章，列出主要要点，并给一个合适的标题建议。
```

### 对应卡片

- [03_把问题合并成一条消息发送.md](file:///Users/bowhead/ai_agent_coding/claude_code/习惯导向_Claude省成本/03_把问题合并成一条消息发送.md)

---

## 习惯 4：把常用文件上传到 Projects

### 原文意思

同一份常用文件不要在多个对话里反复上传，而要放进同一个 Project 里复用。

### 适用范围

- 通用
- Claude 生态

### 为什么有用

- 原文在 Claude 里对应 Projects
- 但底层原则是通用的：稳定资料要常驻，不要每个新会话都全量重发

### coding agent 场景的理解

- 架构说明
- 数据库表结构
- 接口文档
- 部署说明

这类材料都应该成为项目里的常驻背景，而不是每次重新贴。

### 对应卡片

- [04_把常用文件上传到Projects.md](file:///Users/bowhead/ai_agent_coding/claude_code/习惯导向_Claude省成本/04_把常用文件上传到Projects.md)

---

## 习惯 5：设置记忆与用户偏好

### 原文意思

那些每次新对话都会重复交代的角色、风格和偏好，不要每轮都重讲，尽量保存到记忆与用户偏好里。

### 适用范围

- Claude 生态
- Claude Code 可落地

### 为什么有用

- 长期信息不应该一直放在临时会话里
- 一旦沉淀下来，后续很多轮都不用重新建模

### Claude Code 对应动作

- `/memory`
- `/init`
- `CLAUDE.md`

### 对应卡片

- [05_设置记忆与用户偏好.md](file:///Users/bowhead/ai_agent_coding/claude_code/习惯导向_Claude省成本/05_设置记忆与用户偏好.md)

---

## 习惯 6：关闭不需要的功能

### 原文意思

网页搜索、连接器、探索模式、深度思考这类功能，不需要时就关掉，不要默认常开。

### 适用范围

- 通用

### 为什么有用

- 原文举的是 Claude 的功能
- 但“非必要能力默认关闭”适用于大多数 agent

### 例子

- 改标题时不必开搜索
- 润色文案时不必开外部工具
- 简单问题先别上重推理

### 对应卡片

- [06_关闭不需要的功能.md](file:///Users/bowhead/ai_agent_coding/claude_code/习惯导向_Claude省成本/06_关闭不需要的功能.md)

---

## 习惯 7：简单任务用 Haiku，复杂任务才用 Sonnet、Opus

### 原文意思

简单任务优先用 Haiku，只有复杂任务才升级到 Sonnet 或 Opus。

### 适用范围

- 通用
- Claude Code 可落地

### 为什么有用

- 模型能力和价格通常一起上升
- 轻任务长期使用高价模型，本质上就是浪费

### Claude Code 对应动作

- `/model`
- `/status`

### 对应卡片

- [07_简单任务用Haiku_复杂任务用SonnetOpus.md](file:///Users/bowhead/ai_agent_coding/claude_code/习惯导向_Claude省成本/07_简单任务用Haiku_复杂任务用SonnetOpus.md)

---

## 习惯 8：把工作分散到全天

### 原文意思

不要把大量使用集中在一个很短的时段里，而要把一天的工作拆到早上、下午、晚上几个时段。

### 适用范围

- Claude 生态

### 为什么有用

- 这条直接依赖原文提到的 Claude 滚动 5 小时窗口机制
- 同样的工作，分散做更容易错开额度压力

### 对应卡片

- [08_把工作分散到全天.md](file:///Users/bowhead/ai_agent_coding/claude_code/习惯导向_Claude省成本/08_把工作分散到全天.md)

---

## 习惯 9：避开高峰时段

### 原文意思

资源密集型任务尽量放到非高峰时段处理，不要默认在高峰时段猛跑。

### 适用范围

- Claude 生态

### 为什么有用

- 这条直接依赖 Anthropic 的高峰时段额度策略
- 更适合长会话、重推理、大材料处理这类高消耗任务

### 对应卡片

- [09_避开高峰时段.md](file:///Users/bowhead/ai_agent_coding/claude_code/习惯导向_Claude省成本/09_避开高峰时段.md)

---

## 习惯 10：开启超量使用作为兜底保障

### 原文意思

如果你的套餐支持超量使用，就把它作为兜底保障打开，避免关键时刻突然断线。

### 适用范围

- Claude 生态

### 为什么有用

- 它解决的是中断风险
- 不是直接降低单次 token 成本，而是防止关键时刻直接停摆

### 对应卡片

- [10_开启超量使用作为兜底保障.md](file:///Users/bowhead/ai_agent_coding/claude_code/习惯导向_Claude省成本/10_开启超量使用作为兜底保障.md)

---

## 当前目录下的参考资料

- [01_用量与上下文.md](file:///Users/bowhead/ai_agent_coding/claude_code/reference/01_用量与上下文.md)
- [02_缓存与项目.md](file:///Users/bowhead/ai_agent_coding/claude_code/reference/02_缓存与项目.md)
- [03_模型与价格.md](file:///Users/bowhead/ai_agent_coding/claude_code/reference/03_模型与价格.md)
- [04_记忆_偏好_命令.md](file:///Users/bowhead/ai_agent_coding/claude_code/reference/04_记忆_偏好_命令.md)
