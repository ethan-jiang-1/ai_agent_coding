# 12 Interactive-Only Commands

这一章承接那些很重要、但没有合理 external 命令对应、也不适合硬塞进前面章节的 Codex CLI 交互式 slash commands。

## 这章收什么

- `/bug`
- `/doctor`
- `/help`
- `/terminal-setup`

以及其他明显属于会话内增强、而不是 shell 外命令替代的能力。

## 为什么单独放一章

这些命令有几个共同点：

- 它们明显属于交互态
- 很难和已有外部命令一一对应
- 它们确实会影响真实使用体验
- 但把它们硬塞回某一条习惯里会让结构变脏

所以按 command-surface mapping 计划，它们放在最后一个补充兜底章更干净。

## 当前收录

### `/help`

- 用途：查看当前可用 slash commands 和交互命令帮助
- 映射类型：No direct external mapping

### `/bug`

- 用途：反馈问题或进入 bug-reporting 工作流
- 映射类型：No direct external mapping

### `/doctor`

- 用途：做交互态环境诊断或故障排查入口
- 映射类型：No direct external mapping

### `/terminal-setup`

- 用途：帮助用户完成交互终端相关的配置或初始化
- 映射类型：No direct external mapping

## 为什么 `/mentions` 暂时不放这里

`/mentions` 虽然也偏交互态，但它和“引用输入、在会话里高效指向文件或实体”关系很强，所以目前先放在 `05_io_and_handoff_commands.md` 更合适。

## 使用建议

- 这些命令优先在交互式 TUI 内使用
- 它们更像“交互工作流增强”，不是 shell 命令替代品
- 如果以后发现其中某些命令能更自然归回前面章节，再移动也可以
