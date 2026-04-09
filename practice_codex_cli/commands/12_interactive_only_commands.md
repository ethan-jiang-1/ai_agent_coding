# 12 Interactive-Only Commands

这一章承接那些很重要、但没有合理 external 命令对应、也不适合硬塞进前面章节的 Codex CLI 交互式 slash commands。

## 这章收什么

- `/apps`
- `/copy`
- `/feedback`
- `/sandbox-add-read-dir`
- `/statusline`

以及其他明显属于会话内增强、而不是 shell 外命令替代的能力。

## 为什么单独放一章

这些命令有几个共同点：

- 它们明显属于交互态
- 很难和已有外部命令一一对应
- 它们确实会影响真实使用体验
- 但把它们硬塞回某一条习惯里会让结构变脏

所以按 command-surface mapping 计划，它们放在最后一个补充兜底章更干净。

## 当前收录

### `/apps`

- 用途：会话内访问 app 相关能力
- 映射类型：No direct external mapping

### `/copy`

- 用途：快速复制会话产物
- 映射类型：No direct external mapping

### `/feedback`

- 用途：提交反馈
- 映射类型：No direct external mapping

### `/sandbox-add-read-dir`

- 用途：在当前会话里补充只读目录访问
- 映射类型：No direct external mapping

### `/statusline`

- 用途：控制或查看会话状态条相关信息
- 映射类型：No direct external mapping

## 为什么 `/mention` 和 `/ps` 不放这里

- `/mention` 更接近输入引用层，所以放在 `05_io_and_handoff_commands.md`
- `/ps` 更接近长程运行观察层，所以放在 `13_advanced_agentic_coordination_commands.md`

## 使用建议

- 这些命令优先在交互式 TUI 内使用
- 它们更像“交互工作流增强”，不是 shell 命令替代品
- 如果以后发现其中某些命令能更自然归回前面章节，再移动也可以
