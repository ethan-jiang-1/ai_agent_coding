# 02 重试与 checkpoint 回退

## 回退 agent 更改

Cursor 的正式回退语义是 checkpoint，但它只覆盖 agent 产生的更改。

适合：

- Agent 刚做了一轮错误修改
- 你想回到这轮 agent 执行前的状态

## 混入手工编辑时

这时不要指望 checkpoint 全包，优先用 git：

```bash
git restore --source=HEAD --worktree <target-files>
```

## 重试顺序

1. 先回到干净文件状态
2. 重写更明确的提示词
3. 再决定留在原 tab 还是开新 tab

## 更稳的重试提示词

```text
只修改 `src/auth/login.ts`
必须处理 null 和空字符串
不要改测试
参考 `src/auth/register.ts` 的风格
```

## 可以沉淀成 custom command 的重试动作

这次补查官方 `Commands` 页后，能确认一批适合这一章的示例命令方向：

- `run-all-tests-and-fix`
- `light-review-existing-diffs`
- `address-github-pr-comments`

它们的价值不是替代 checkpoint，而是把“失败后怎么再来一轮”做成稳定入口，例如：

- 先看现有 diff，再决定回退还是继续修
- 先跑完整测试，再基于失败结果做修复
- 围绕已经存在的 PR comments 做定向返工

对 Cursor 来说，这类动作更适合放进 `.cursor/commands`，而不是每次都临时重写一大段返工提示词。

## 这章最容易写错的地方

- 不要把 checkpoint 当成 git 的完全替代。
- 不要忘了 checkpoint 不回滚手工编辑。
- 不要在已经偏航的 tab 里无限追加补丁提示词。
