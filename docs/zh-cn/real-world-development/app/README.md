---
slug: zh-cn/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** 是基于 Copilot CLI 构建的桌面应用，可在统一、专注的工作区中使用智能体进行开发。它提供并行智能体会话、可切换的会话模式、共享 Canvas，以及原生 GitHub 议题和拉取请求管理功能。其中包括 **Agent Merge**，可引导拉取请求完成变基、处理审查反馈、修复持续集成 (CI) 问题并合并。

本研讨会围绕 Tailspin Toys 的连续工作流程展开：

1. 准备项目、安装应用、连接存储库，并熟悉工作区和预先创建的积压工作。
2. 添加星级评分，在浏览器中审查变更，然后手动合并第一个拉取请求（PR）。
3. 从筛选议题开始，在 **Plan** 模式下确定方案，在 **Autopilot** 模式下构建，然后在 **Interactive** 模式下审查。
4. 更新存储库说明，并将其应用于筛选功能。
5. 自定义现有 `quality-checks` 技能，并使用它运行项目检查。
6. 添加 Playwright Model Context Protocol (MCP) 服务器，并在浏览器中探索筛选功能。
7. 创建质量保证 (QA) 自定义智能体，审查需求、覆盖率和验证证据。
8. 审查完整的筛选功能变更，并使用 Agent Merge 处理第二个 PR。
9. 检查 Canvas 是否可用，然后使用 Database Explorer 创建并测试基于存储库的分诊 Canvas。如果所需功能不可用，请记录受阻原因并使用只读替代方案，或直接进入总结。

为集中学习核心概念，你将创建两个 PR：一个用于星级评分，另一个用于筛选功能以及说明更新、技能更新、QA 配置和测试。每个 PR 都从更新后的 `main` 分支开始。筛选和质量工作流程会共享一个会话、工作树和分支，让你可以在探索工具时持续迭代。最后的 Canvas 练习会保留在自己的会话中，让你专注于创建和测试共享界面，而不必重复 PR 流程。

## 课程

| 课程 | 主题 | 说明 |
|--------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 安装 Node.js，并创建 Tailspin Toys 项目副本 |
| [1. 安装 Copilot app][ex1] | 设置 | 安装应用、连接项目并熟悉工作区 |
| [2. 添加星级评分：快速改进][ex2] | 首次更改 | 显示现有评分和未评分时的备用文本，然后合并 PR 1 |
| [3. 智能体模式：Plan 和 Autopilot][ex3] | 智能体模式 | 从议题规划功能，使用 Autopilot 构建，然后在 Interactive 模式下审查 |
| [4. 使用自定义说明引导 Copilot][ex4] | 上下文 | 探索并更新说明，然后将其应用于筛选功能 |
| [5. 自定义并使用 quality-checks 技能][ex5] | 可重复检查 | 探索现有技能、修改报告格式并运行技能 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 浏览器观察 | 通过 Customize 配置 MCP 并检查筛选行为 |
| [7. 创建并使用 QA 智能体][ex7] | 需求和覆盖率 | 创建并选择专用配置，然后收集最终验证证据 |
| [8. 创建并合并功能 PR][ex8] | 审查与合并 | 审查筛选功能、说明、技能、QA 配置和测试，然后使用 Agent Merge 处理第二个 PR |
| [9. 探索并创建 Canvas][ex9] | 协作 | 使用 Database Explorer 创建并测试基于存储库的分诊 Canvas |
| [10. 回顾与后续步骤][ex10] | 总结 | 回顾工作流程、成果和更多资源 |

## 先决条件

参加本研讨会前，请确认你具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 运行 **macOS、Linux 或 Windows** 的计算机
- [ ] 已在计算机上[安装 Git][install-git]

> [!TIP]
> 没有付费计划？经验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会使用的智能体、MCP、代码审查和 Copilot CLI 功能，可完成所有工作环境的练习。

> [!NOTE]
> Copilot app 在本机运行，而不是在 Codespace 中运行，因此[先决条件练习][ex0]会介绍如何安装 Node.js，并在安装应用前创建项目副本。

> [!NOTE]
> 使用 Copilot Business 或 Copilot Enterprise 时，管理员必须保持 **GitHub Copilot app** 策略处于启用状态。此策略与 Copilot CLI 策略相互独立。

## 开始

**[从先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
