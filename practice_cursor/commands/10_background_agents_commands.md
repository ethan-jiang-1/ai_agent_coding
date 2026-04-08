# 10 Tabs 与 Background Agents

## Background Agents 的本质

Cursor 的 Background Agents 不是本地多开一个聊天窗口，而是远程异步 agent 环境。

官方文档能确认的关键点包括：

- 运行在隔离的 Ubuntu 环境
- 有 internet access
- 通过 GitHub 克隆仓库
- 在独立 branch 上工作

## 适合的任务

- 长时间实现任务
- 大范围代码探索
- 异步处理 issue / PR
- 和前台主 tab 分离的审查或实验

## 前台 tabs 的作用

前台多个 tabs 更适合：

- 轻量角色隔离
- 对比不同方案
- 保留主线与旁线分离

## 更稳的组合

- Ask / 主 tab：做方向确认
- Agent / Manual：做前台受控执行
- Background Agent：做异步独立执行

## 工作前的边界

- 明确仓库或 branch
- 明确任务边界
- 明确输出是 diff、PR，还是摘要

## 这章最容易写错的地方

- 不要把 Background Agents 写成本地后台线程。
- 不要让两个执行通道同时改同一组文件。
- 不要因为它在后台跑，就忽略安全和成本边界。
