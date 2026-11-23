---
title: "练习 0 - 先决条件"
description: "为 GitHub Copilot app 课程做好准备：安装 Tailspin Toys 项目所需的 Node.js，并从模板创建自己的存储库副本。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app 是一款桌面应用，可作为 Copilot 和 GitHub 的中心工作区。它便于访问议题和拉取请求，也支持使用 GitHub Copilot 构建项目。本研讨会将在本机使用 GitHub Copilot app 更新基于 Astro 构建的 Tailspin Toys 应用。开始前先确认本机已安装 Node.js，然后再安装 Copilot app。

本课将：

- 安装 Node.js，以便在本机运行项目测试。
- 根据模板创建 Tailspin Toys 项目的副本。

## 安装 Node.js

一些课程会要求智能体在本机构建功能并运行 Tailspin Toys 测试套件，这需要项目唯一的运行时 **[Node.js][nodejs]**。请安装当前的 **LTS** 版本。

所有平台最简单的安装方式是使用官方安装程序：

1. 在操作系统中打开终端，例如 Windows Terminal、macOS 终端或常用的其他终端。
2. 运行以下命令，检查已安装的 Node.js 版本：

   ```shell
   node --version
   npm --version
   ```

3. 如果两个命令都能运行，并且 Node.js 满足项目 README 和 `package.json` 中的要求，可以跳到下一节。

> [!TIP]
> 只有尚未安装 Node.js 或需要更新版本时，才需要执行这些步骤。

4. 打开 [Node.js 下载页面][node-download]。
5. 下载适用于操作系统的 **LTS** 版本。
6. 运行安装程序并接受默认选项。在 Windows 上，保持 **Add to PATH** 选中。
7. 安装完成后，打开一个新的终端窗口。
8. 在新终端中运行以下命令，确认安装成功：

   ```bash
   node --version
   npm --version
   ```

9. 应看到已安装的 Node.js 和 npm 版本。

> [!IMPORTANT]
> Copilot app 的智能体也必须能够运行 `node --version` 和 `npm --version`。常用终端中运行成功，并不表示 app 一定能找到它们。安装 Node.js 或更改 PATH 后，请完全退出并重新打开 app。如果会话中任一命令仍然失败，请先按照[会话环境故障排除步骤][session-environment]处理。

> [!IMPORTANT]
> 每个工作树也需要项目依赖项和 Playwright Chromium 才能运行端到端 (E2E) 检查。准备工作树时请遵循 Tailspin Toys 存储库 README，并在批准任何安装请求前先审查。

## 设置实验存储库

你将在自己的 Tailspin Toys 项目副本上完成练习。现在从[模板存储库][template-repository]创建副本。新存储库包含实验所需的全部文件，安装应用后会连接到该存储库。

1. 在新的浏览器窗口中打开本实验的 GitHub 存储库：`https://github.com/github-samples/tailspin-toys`。
2. 在实验存储库页面上选择 **Use this template**，然后选择 **Create a new repository**，创建自己的存储库副本。

   ![从菜单中选择 Create a new repository 的 Use this template 按钮](../../../_images/app-0-use-template.png)

3. 如果你参加的是 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可以在拥有 GitHub Copilot 使用权限的组织中创建新存储库。

   ![将 github-samples/tailspin-toys 设置为模板并填写存储库名称的 Create a new repository 表单](../../../_images/app-0-create-repository.png)

4. 记下所创建存储库的路径（**organization-or-user-name/repository-name**），稍后会用到。

> [!NOTE]
> 从模板创建存储库时，系统会自动为你创建一组 GitHub 议题。整个研讨会都会使用这些议题，无需自行创建。

请使用研讨会模板的新副本。它包含存储库说明、应用代码、测试、quality-checks 技能和现有 Canvas 扩展。研讨会期间会自定义技能并创建 QA 智能体。如果使用旧副本，请向活动导师确认其中包含所需文件。

## 总结和后续步骤

环境已准备就绪！本课中，你：

- 安装了 Node.js，以便在本机运行项目构建和测试。
- 根据模板创建了 Tailspin Toys 存储库副本。

接下来[安装 GitHub Copilot app][next-lesson]、连接刚创建的存储库并熟悉工作区。

## 资源

- [下载 Node.js][node-download]
- [从模板创建存储库][template-repository]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#start-a-session-from-the-issue
