# 12 交互控制层命令

这一章不是收容“没法映射的零碎 slash commands”，而是把 Codex CLI 里那层真正的交互控制面单独拉出来。

## 先分清 Codex 的控制面

在 Codex 里，控制动作不只在一个地方：

- 会话内 slash commands
- shell 外的 `codex status`、`codex apply` 这类命令
- cloud / resume 这类更长程的 detached surface

这章只收交互式 TUI 内这一层：

- 会话控制
- 状态观察
- 环境微调
- 旁路动作

如果动作的目标不是“让模型产出内容”，而是“调整当前会话怎么运行”，优先走 command surface，不要把它塞进主提示词。

## 当前这章收什么

- `/apps`
- `/copy`
- `/feedback`
- `/sandbox-add-read-dir`
- `/statusline`

以及其他明显属于会话控制、而不是 shell 外命令替代的 slash commands。

## 为什么单独成章

这些命令的共同点不是“没有外部等价物”，而是：

- 它们都服务于当前会话控制
- 它们会直接影响环境边界或观察方式
- 它们更像旁路动作，不是产出型请求

所以更稳的分层是：

- 产出型请求：写 prompt
- 控制型动作：优先走 command surface

## 当前收录

### `/apps`

- 用途：进入 app 相关交互入口
- 更像：控制面导航，不是内容请求
- 映射类型：No direct external mapping

### `/copy`

- 用途：快速复制会话产物
- 更像：产物已经确定后的旁路动作，不该混进实现 prompt
- 映射类型：No direct external mapping

### `/feedback`

- 用途：提交反馈
- 更像：把当前体验送回产品反馈回路，不属于主任务意图
- 映射类型：No direct external mapping

### `/sandbox-add-read-dir`

- 用途：在当前会话里补充只读目录访问
- 更像：调整环境边界，不是补充实现要求
- 映射类型：No direct external mapping

### `/statusline`

- 用途：控制或查看会话状态条相关信息
- 更像：观察层和界面层操作，不是任务内容层
- 映射类型：No direct external mapping

## 为什么 `/mention` 和 `/ps` 不放这里

- `/mention` 更接近输入引用层，所以放在 `05_io_and_handoff_commands.md`
- `/ps` 更接近长程运行观察层，所以放在 `13_advanced_agentic_coordination_commands.md`

## 使用建议

- 已有正式命令面时，不要先写自然语言让模型“帮你调一下设置”
- 需要调整环境边界、查看状态、做旁路动作时，先想有没有 slash command
- 这章只是 Codex 更大控制面的交互子层；cloud / apply / status 等外部入口仍在别章处理
- 如果以后发现其中某些命令能更自然归回前面章节，再移动也可以
