---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：自定义说明][previous-lesson]

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺乏时间而长期积压的功能请求。GitHub Copilot 云智能体可以自主更新代码和添加功能。完成任务后，它会创建草稿拉取请求，供开发人员审查。这样可以将繁琐任务交给智能体，让开发人员专注于更重要的工作。

本课将探索如何：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有为项目编写文档，团队首先希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能：在每款游戏的详情页显示同类别的其他游戏，方便用户继续浏览。团队暂时只需要可用于 UX 验收测试的实现。目前这是一个阻碍，但优先级较高的其他议题需要先处理。

这些任务容易被延后，很适合交给云智能体异步处理。开发人员可以继续处理其他工作，稍后再回来审查智能体的结果。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发人员一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始执行任务。进行中或完成后都可以查看会话。准备好审查建议的解决方案时，Copilot 会在拉取请求中提及你。

## 明确说明任务范围的重要性

GitHub Copilot 并非魔法——不能跳过思考步骤。即使看似简单的操作也可能涉及许多细节，因此要注意[如何为 Copilot 云智能体定义任务范围][cloud-agent-best-practices]。应将它视为 AI 结对程序员：分阶段工作、学习、尝试并不断调整。生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果已经完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的作用；否则可先快速查看。分配任务前，以只读方式检查存储库中已有的说明文件，以便稍后观察它们的影响：

- `.github/copilot-instructions.md`：查看 **Code standards**，特别是 TypeScript 约定和 TSDoc 文档注释。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，了解这些说明如何限定到 `**/*.test.ts` 文件。

下一节会将 `Code lacks documentation` 议题分配给云智能体。审查生成的拉取请求时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头；它们都来自这些说明文件。

## 设置云智能体开发环境

无论由谁编写代码，通常都需要准备特定环境并运行设置脚本。将任务分配给 Copilot 时也一样，它会像软件工程师一样完成工作。

云智能体使用 [GitHub Actions][github-actions] 执行任务。可以通过 `.github/workflows/copilot-setup-steps.yml` 中的[专用设置工作流][setup-workflow]自定义环境，在智能体开始工作前安装所需工具和依赖项。本实验已提前配置该工作流，其中包含 Node.js、项目依赖、端到端测试所需的浏览器，以及已迁移并填充数据的 SQLite 数据库。

此工作流包含名为 `copilot-setup-steps` 的单个作业，在 Copilot 开始处理拉取请求前运行。`workflow_dispatch` 触发器可让你从存储库的 **Actions** 选项卡手动运行工作流，以便测试其是否成功。

```yaml
name: "Copilot Setup Steps"

# 可在存储库的 Actions 选项卡中手动测试设置步骤。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅设置步骤所需的最低权限。Copilot 会获得自己的操作令牌。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read 权限。
      # 如果设置步骤不克隆存储库，步骤完成后 Copilot 会自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置：Node.js（整个应用现在都是 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

## 添加文档任务

文档对项目至关重要，但许多项目中的文档都已过时或根本不存在。这类技术债务会降低开发效率，也会增加维护代码库或帮助新成员加入团队的难度。Copilot 很适合编写文档，因此可以将此议题分配给云智能体。稍后会回来审查结果。

1. 在新浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡，然后选择 **New issue**。
3. 选择 **Blank issue**。
4. **Title** 输入 `Code lacks documentation`。
5. **Description** 输入：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

6. 选择 **Create** 创建议题。
7. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题的界面](../../../_images/shared-assign-copilot.png)

8. 选择 **Assign**。

   ![Copilot 议题分配详情](../../../_images/ex4-assign-copilot-details.png)

9. 打开 **Pull Requests** 选项卡，查找标题类似 `[WIP]: Code lacks documentation` 的新 PR。如果尚未显示，请稍等片刻并刷新页面。
10. 等待 Copilot 创建任务清单，然后查看它计划执行的工作。Copilot 需要先创建环境、分析项目并确定处理方案，因此可能需要几分钟。
11. 在 PR 时间线中查看进度，并选择 **View session** 观察 Copilot 探索代码、制定计划和编写代码。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态。

云智能体的主要优势之一是异步执行任务，让你可以处理其他工作。现在也将另一个任务分配给云智能体，同时继续探索项目。

## 为游戏详情页添加相关游戏

云智能体可以并行处理工作，让你专注于其他任务。现在将设计团队的相关游戏功能分配给它。

1. 返回 github.com 上的存储库。
2. 打开 **Issues**，选择 **New issue** 和 **Blank issue**。
3. **Title** 输入 `Show related games on the game details page`。
4. **Description** 输入：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

5. 选择 **Create** 创建议题。
6. 在右侧选择 **Assign to Copilot**，然后选择 **Assign**。

稍后会在议题的首条评论中看到 👀，表示 Copilot 已开始处理。

![Copilot 使用 👀 表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步工作。它能够处理技术债务、创建新功能，或协助将代码迁移到其他框架。

本课介绍了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

云智能体会继续在后台工作。接下来创建并使用自定义智能体。[云智能体也支持 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
