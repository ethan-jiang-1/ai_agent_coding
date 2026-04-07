# Cursor 官方核验摘要

最后核验日期：2026-04-07
核验方式：Cursor 官方文档搜索结果摘要

## 官方来源

- Rules：`https://docs.cursor.com/context/rules-for-ai`
- Modes：`https://docs.cursor.com/chat/manual`
- Background Agents：`https://docs.cursor.com/en/background-agents`
- Overview：`https://docs.cursor.com/chat/overview`

## 已确认事实

### 1. Rules 体系当前以 `.cursor/rules` 为主

- 官方 Rules 文档当前明确支持四类规则来源：
  - Project Rules：`.cursor/rules`
  - User Rules
  - `AGENTS.md`
  - `.cursorrules`（Legacy）
- 这意味着：
  - `.cursorrules` 不能再写成首选方案
  - `AGENTS.md` 在 Cursor 里也已是正式选项

### 2. `.mdc` 与 `alwaysApply`、`globs` 仍是核心机制

- 官方 Rules 文档写明 rule 文件使用 MDC（`.mdc`）。
- 文档明确提到的关键属性：
  - `description`
  - `globs`
  - `alwaysApply`
- 官方 rule 类型包括：
  - `Always`
  - `Auto Attached`
  - `Agent Requested`
  - `Manual`

### 3. 嵌套 `.cursor/rules` 是正式能力

- 官方文档写明子目录可以有自己的 `.cursor/rules`，并在引用该目录文件时自动附着。

### 4. Ask 是正式只读探索模式

- 官方 Modes 文档明确：
  - `Ask` 是 read-only exploration
  - 不自动改文件
  - 适合 learning / planning / questions

### 5. Cursor 当前官方模式叙事重心不是“内建 Plan Mode”

- 官方 Modes 文档重点列出：
  - `Agent`
  - `Ask`
  - `Custom`
- 官方示例里可以创建 `Plan` 这样的 custom mode。
- 这意味着正文更稳的写法是：
  - “用 Ask 做只读探索”
  - “用 Custom Mode 做 Plan/Debug/Refactor 流程”
- 不要直接断言“Cursor 内置标准 Plan Mode”。

### 6. Background Agents 是远程异步 agent

- 官方文档明确：
  - Background agents 在远程环境异步运行
  - 默认是隔离的 Ubuntu 机器
  - 有 internet access
  - 通过 GitHub 克隆仓库并在独立 branch 工作

### 7. 前台 Agent 与后台 Agent 的权限模型不同

- 官方 Background Agents 文档明确指出：
  - 背景 agent 会自动运行 terminal commands
  - 这与 foreground agent 需要用户审批每条命令不同

## 当前不应写死的点

- `AGENTS.md` 是否已经支持嵌套子目录
  - 搜索摘要里看到的是旧版本限制信息，时效性不足
- `Manual` 是否仍在所有用户界面中默认可见
  - 搜索结果不同页面表述略有差异
- 任何具体快捷键和菜单文案

## 对 round1 的直接修正建议

### 可以安全写成事实的

- `.cursor/rules/*.mdc` 是正式 project rules 体系
- `AGENTS.md` 是官方支持的简单规则替代方案
- `.cursorrules` 是 legacy
- `Ask` 是只读探索模式
- Cursor 支持 remote background agents

### 需要重写的高风险说法

- “Cursor 内置 Plan Mode”
- “`.cursorrules` 是主要规则载体”
- “Background Agent 只是本地开一个额外会话”

## 建议在正文里采用的表述

- 若讲规划阶段：
  - “Cursor 更稳的官方工作流是 Ask + Custom Modes，而不是把 Plan 当作固定内建模式名”
- 若讲规则：
  - “优先用 `.cursor/rules/*.mdc`；`AGENTS.md` 适合更简单的全局指令；`.cursorrules` 属于 legacy”
- 若讲多智能体：
  - “Background Agents 是远程异步 agent，不等同于本地多开聊天窗”

