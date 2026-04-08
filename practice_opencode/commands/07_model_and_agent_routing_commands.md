# 07 模型选择与 agent 路由

## 查看可用模型

```bash
opencode models
opencode models --refresh
opencode models --verbose
```

`--verbose` 会包含更多 metadata，比如 costs。

## 会话内对应 slash commands

```text
/models
/thinking
```

更准确的理解是：

- `/models`：会话里查看并切换模型
- `opencode models`：外部列模型清单
- `-m/--model`：启动时指定模型

所以 `/models` 更接近 combined mapping，不是单独等于 `opencode models`。

## 显式指定主模型

```bash
opencode -m anthropic/claude-sonnet-4-5

opencode run -m provider/model \
  "审查当前认证设计的风险"
```

模型 ID 的基本格式是：

```text
provider/model
```

## 配置主模型与小模型

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "anthropic/claude-sonnet-4-5",
  "small_model": "anthropic/claude-haiku-4-5"
}
```

官方 docs 当前说明：

- `model`：主模型
- `small_model`：轻量任务模型，比如 title generation

## 让不同 agent 用不同模型

```json
{
  "$schema": "https://opencode.ai/config.json",
  "agent": {
    "code-reviewer": {
      "description": "Reviews code for best practices and risks",
      "model": "anthropic/claude-sonnet-4-5",
      "prompt": "Review code changes for correctness and maintainability."
    }
  }
}
```

这比“全项目永远只绑一个模型”更实用。

## 默认 primary agent

```json
{
  "$schema": "https://opencode.ai/config.json",
  "default_agent": "plan"
}
```

官方 docs 当前说明：

- `default_agent` 必须是 primary agent
- 如果指定不存在或是 subagent，会回退到 `build`

## variants 与 `/thinking`

OpenCode 官方 docs 当前把 variants 当成正式能力：

- 有 built-in variants
- 有 `variant_cycle` keybind

另外：

- `/thinking` 只是切换思考内容的显示
- 不是模型切换入口
- 真正和 reasoning / variant 更接近的是 `ctrl+t` cycle variants

## 这章最容易写错的地方

- 不要把 `small_model` 当成主编码模型。
- 不要把 `/thinking` 误解成换模型。
- 不要把所有任务都绑在一个模型上，不看 agent 角色和任务复杂度。
