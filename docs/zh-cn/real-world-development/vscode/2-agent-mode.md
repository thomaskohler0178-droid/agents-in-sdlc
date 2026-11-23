---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也涉及多个文件以及运行测试等操作。开发人员需要找到所有相关文件、修改代码、运行测试、调试、找出遗漏文件并再次修改，流程可能不断重复。

Copilot 智能体模式可以帮助处理这些工作。

Copilot 智能体模式能够在 IDE 中更自主地工作。它会像开发人员一样探索项目结构、执行必要更新、运行测试等任务，并自动修复发现的问题。现在了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于不同的工作方式。智能体模式在 IDE 中执行任务，可快速反馈并与用户交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- 如何使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 合并前如何审查智能体模式生成的变更和测试。

## 场景

游戏列表不断增长，因此需要允许用户按类别和发行商筛选。上一课已添加 publishers helper，现在使用 Copilot 智能体模式完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

修改网站前，先探索 Tailspin Toys，了解现有功能。

该网站是一个以开发者为主题的桌游众筹平台。用户可以浏览游戏列表并查看详情。它是一个单独的 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能由作用范围有限、使用标准 DOM API 的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 命令，用于启动单个 Astro 开发服务器。通过以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一课完成后应位于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 更新 `main` 并为筛选功能创建分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后会显示类似以下 URL 的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示询问是否在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有隧道，此时 Web 服务器正在其中运行。

### 探索网站

网站启动后，可以探索现有功能：

- **主页**：显示桌游列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发行商和类别等信息。

## 使用 Copilot 探索积压工作

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会在需要时说明。

网站初始版本可以正常运行，但还需要添加更多功能。先查看项目积压工作。根据模板设置项目时已自动创建一组议题，请 GitHub Copilot 列出这些事项。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。Agent 可自主规划并跨文件实现变更、运行终端命令和调用工具。
2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此按照说明使用该模型，以便获得较一致的体验。LLM 具有概率性，因此建议可能与课程示例不同，这是正常且符合预期的。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，询问积压工作中的议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据对用户的价值为议题排序。

## 查看说明文件

让智能体生成代码前，先查看用于向 Copilot 提供上下文的说明文件。项目中的 [用户界面 (UI) 说明][ui-instructions]介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 针对 Astro、Tailwind CSS 和 Drizzle 等文件类型的具体说明链接。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置。这样可以复用指导、将复杂说明拆分成较小部分，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

完成筛选至少需要对应用进行三类更新：

- 在数据层 (`src/lib/`) 添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前必须运行并通过测试。Copilot 智能体模式可以执行这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。
3. 确认 Chat 视图的智能体下拉菜单中仍选中 **Agent**。
4. 确认仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据积压工作中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并定位相关文件，包括数据层 helper、UI 和测试。之后它会修改文件并运行测试。
7. 按 Copilot 提示选择 **Continue** 运行测试。
8. Copilot 可能会暂停或遇到测试失败，这是正常的。它会在生成代码和运行测试之间迭代，直至任务完成且未发现错误。
9. 检查生成的代码是否存在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站，探索新功能。
11. 确认功能正常且已审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解了：

- 如何使用 Copilot 智能体模式跨 UI 和数据层实现功能。
- 智能体模式如何探索项目、识别相关文件并协调变更。
- 合并前如何审查智能体模式生成的变更和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，然后创建拉取请求。

### 加分探索练习 — 实现分页

游戏列表不断增长后，需要启用分页。使用本练习中学到的技能，请 Copilot 更新网站以实现分页：

- 遵循现有最佳实践，包括使用说明文件。
- 考虑分页方式，例如是否允许用户选择页面大小，还是使用固定大小。
- 编写提示时，提供足够指导，让 Copilot 按照预期实现。
- 可能需要与 GitHub Copilot 多次交互，要求修改并补充上下文。这是正常工作方式。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
