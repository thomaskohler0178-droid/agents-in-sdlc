---
title: "练习 1 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app、连接从模板创建的存储库、熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款面向智能体开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此存储库、分支和 CI 管道都可以开箱即用。它适用于由你指挥多个智能体并行工作的流程，每个智能体都在独立工作区中运行，让你无需亲自完成所有工作，并可自动化重复任务。Node.js 已安装、项目副本也已准备好，接下来安装应用并连接存储库。

本课将：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 存储库将项目添加到应用。
- 熟悉工作区，包括模板自动创建的积压工作。
- 尝试快速聊天，了解应用本身。

## 场景

团队正开始使用 AI 智能体处理不断增长的积压工作。Copilot app 提供统一工作区，可以从中领取议题、运行智能体、审查变更并合并拉取请求。本课会完成安装和连接，并练习如何围绕项目与 Copilot 开始对话。

> [!NOTE]
> 需要符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任何付费计划（Pro、Pro+、Business 或 Enterprise）。使用 Copilot Business 或 Enterprise 时，管理员必须保持 **GitHub Copilot app** 策略处于启用状态。

## 安装并配置 GitHub Copilot app

首先安装 GitHub Copilot app。它支持 Windows、macOS 和 Linux。现在安装应用、完成身份验证并添加 Tailspin Toys 存储库。

1. 在浏览器中打开 [GitHub Copilot app 登陆页][download-app]。
2. 下载适用于当前平台的应用，并按照登陆页上的说明安装。
3. 安装完成后打开应用。
4. 选择 **Sign in to GitHub**，然后按照提示使用 GitHub.com 或 GHE.com 帐户登录。使用具有数据驻留的 GitHub Enterprise Cloud 时，请选择 **GitHub Enterprise Cloud**，并在提示时输入 `*.ghe.com` 主机名。

   使用 GitHub Enterprise Server 时，先通过云帐户完成注册。然后打开 **Settings**，选择 **Accounts**、**Add account** 和 **GitHub Enterprise Server**。按照浏览器中的提示授权应用，详情请参阅 [Enterprise Server 登录指南][ghes-sign-in]。

5. 如果注册流程提供存储库选择，请选择从模板创建的 Tailspin Toys 存储库。也可以跳过存储库选择，在注册完成后再添加项目。
6. 按提示选择主题，然后完成注册。
7. 在侧边栏的 **Sessions** 下查找存储库。如果已经列出，无需重复添加。
8. 如果尚未连接存储库，请选择 **Sessions** 旁边的 **+** 创建新会话，然后选择 **Add GitHub repository**。输入先决条件课程中记录的所有者和存储库名称。
9. 确认侧边栏中显示了存储库。在此处使用的布局中，`tailspin-toys` 等存储库文件夹会显示在 **Sessions** 下，单个会话会显示在对应存储库下方。

## 熟悉工作区

连接项目后，花些时间了解应用的布局。侧边栏将功能分为几个区域：

- **New**：从这里开始新的 Copilot 聊天会话。
- **Pull requests**：通过应用的原生 GitHub 集成查看拉取请求。可以浏览和筛选 PR、检查 CI 状态并审查 PR，无需离开应用。
- **Issues**：以相同方式查看议题，可以浏览和筛选议题，并从议题开始会话。
- **Automations**：按计划或按需运行的已保存智能体任务。适合管理待办列表、定期项目维护或其他想要委派的繁琐工作。总结课程会将其作为后续学习，而不是本研讨会的另一项练习。
- **Customize**：为 Copilot app 添加 MCP 服务器、插件、技能和其他组件。本研讨会会用它配置 Playwright MCP。
- **Chats**：用于提问和头脑风暴的轻量对话，不需要分支或工作区。开始第一次聊天后，此区域才会显示。如果未显示，请选择 **Sessions** 旁边的 **+** 开始第一次聊天。
- **Sessions**：按存储库分组显示文件夹及其智能体会话。选择 `tailspin-toys` 等存储库名称以打开其存储库视图，或选择其下的会话以返回对话。每个会话都在独立工作区中运行，因此可以同时运行多个会话而不会互相冲突。添加星级评分时会开始第一个会话。

随着研讨会继续进行，你会逐步了解工作区。

> [!TIP]
> 不确定如何操作或某项功能是否可用时，可以询问 Copilot，它会帮助你找到方法。

### 查找预先创建的积压工作

几乎每个项目都有积压工作，Tailspin Toys 也不例外。现在查看从模板创建项目时自动生成的积压工作。

1. 在侧边栏的 **Sessions** 下，选择 Tailspin Toys 存储库文件夹，而不是其下的单个会话。然后在存储库顶部导航中选择 **Issues**。
2. 根据标题查找以下议题，不要假定它们的议题编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择议题查看详情。每个议题都可以作为启动智能体会话的入口。完成第一个小变更后，会从筛选议题开始。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，只显示添加到 Copilot app 的存储库中的项目。若要查看其他存储库的工作项，请将对应存储库添加到应用。

## 尝试快速聊天

熟悉应用的一种好方法是直接询问应用本身。**快速聊天**可以提问或头脑风暴，不会创建分支或工作树，非常适合快速、一次性的问题，无需创建会话。

1. 在侧边栏的 **Chats** 旁选择 **+**，开始新聊天。如果尚未显示 **Chats**，请先选择 **Sessions** 旁边的 **+** 开始第一次聊天；之后 **Chats** 就会显示。
2. 询问应用自己的会话如何工作：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 在对话视图中阅读回答。每个会话都在独立的 Git 工作树中运行，因此可以并行运行多个智能体，而它们的变更不会冲突。可以继续当前对话，也可以随时开始新聊天。

## 总结和后续步骤

恭喜！你已安装 GitHub Copilot app、连接项目并熟悉工作区。你学会了如何：

- 安装应用并登录 GitHub。
- 从 GitHub 存储库添加项目。
- 熟悉工作区并在 **Issues** 中查找预先创建的积压工作。
- 使用快速聊天询问一次性问题。

接下来[开始第一个智能体会话][next-lesson]，让它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [开始使用 GitHub Copilot app][getting-started]
- [使用 GitHub Copilot app 中的智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
