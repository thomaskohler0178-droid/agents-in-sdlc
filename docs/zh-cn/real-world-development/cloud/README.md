---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云智能体][cloud-agent]**可以在云端异步处理工作。你可以在 GitHub 上分配任务，云智能体便会在后台探索存储库、进行更改并创建拉取请求，让你可以同时处理其他工作。

在这些练习中，你将添加云智能体会遵循的自定义说明，然后分配 GitHub 议题，让它实现相应工作。你还将审查并使用自定义智能体来调整它的工作方式，通过智能体控制面板监控并引导会话，最后审查它创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云智能体会遵循的自定义说明 |
| [2. 云智能体][ex2] | 异步智能体 | 将议题分配给 Copilot 云智能体 |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代][ex5] | 审查 | 审查拉取请求、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本研讨会前，请确认你具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为帐户或组织启用 Copilot 云智能体

> [!TIP]
> 没有付费计划？经验证的学生可以通过 [GitHub Education][student-plan] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会所需的智能体、MCP、代码审查和 Copilot CLI 功能，可完成所有工作环境的练习。

> [!NOTE]
> 云智能体适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，不适用于 Copilot Free。使用 Copilot Business 或 Enterprise 时，管理员必须为组织启用云智能体。

> [!NOTE]
> 本工作环境不涉及 MCP。若要探索如何将 MCP 服务器与 Copilot 搭配使用，请参阅 [CLI 工作环境][cli]或 [VS Code 工作环境][vscode]。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
