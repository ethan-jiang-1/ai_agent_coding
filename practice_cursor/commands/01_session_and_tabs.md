# 01 Tabs、历史与任务边界

## 新任务

对 Cursor 来说，最稳的做法就是新开一个 chat tab。

官方 overview 页明确强调：每个 tab 都维护自己的 context、history 和 model selection。

适合：

- 新 feature
- 新 bug
- 新模块
- 新风险级别的任务

## 继续当前任务

只有在目标、文件范围和风险轮廓都没变时，才继续原 tab。

## 历史与旧对话

Cursor 有 chat history，但这不等于你应该默认把所有旧对话都续上。

更稳的理解是：

- history 负责找回旧上下文
- tab 负责承载当前任务

## 跨天回来时的判断

- 旧 tab 还干净：继续
- 旧 tab 已经混乱：新开 tab，再用 handoff 文件或 `@Past Chats` 补上下文

## 这章最容易写错的地方

- 不要把 Cursor 的 tab / history 写成 CLI 里的 `resume` 语义。
- 不要因为旧对话能找回，就默认应该继续。
- 不要让一个 tab 混多个大任务。
