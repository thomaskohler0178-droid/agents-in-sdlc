---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解一些背景信息，就应确保它能够获取相关上下文。可以通过多种方式向 Copilot 提供上下文，其中一种重要方式是使用[说明文件][instruction-files]指导代码生成。

本练习将：

- 探索项目上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件提供给 Copilot。
- 使用当前说明生成筛选功能的第一部分数据访问代码（publishers helper）。
- 向 `.github/copilot-instructions.md` 添加一项全库标准。
- 再次运行相同提示，观察生成的代码如何遵循新标准。
- 将说明更新和筛选代码提交到 `main`，供下一课中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能不符合所有设定标准。Copilot 具有概率性。此练习旨在观察更新说明后行为的*趋势*，而不是要求输出逐字符完全相同。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加注释块，说明文件的用途。

通过说明文件，可以确保 Copilot 按照这些实践执行任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和需求。你可以指定编码约定、库以及希望包含的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型创建说明，为任务提供上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送这一个说明文件。它应包含适用于大多数请求的项目级信息，例如技术栈、项目概述、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中工作时，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持符合其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供指导。
- 使用清晰的自然语言，并提供代码应如何编写或不应如何编写的示例。

说明文件没有唯一的编写方式，AI 的使用方式也不止一种。通过实验，可以找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的说明文件。本项目提供了针对多种任务的文件，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面的操作方式各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应环境的课程会介绍相关功能。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义说明文件

本存储库附带一个核心 `copilot-instructions.md` 和一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`，查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 嵌套指导，这些内容适用于与 Copilot 的任何交互。
2. 打开 `.github/instructions` 文件夹，查看适用于 Astro 文件、Drizzle 数据层、测试等内容的说明。
3. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段设置了相对于存储库根目录的 glob；例如 `**/*.test.ts` 会匹配 TypeScript 测试文件。
4. 查看其中针对本项目单元测试的说明。
5. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部，查看它指向的其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件。路径相对于说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 为变更创建分支

遵循最佳实践，为本次变更创建分支。

1. 返回上一课使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会在需要时说明。

先在当前说明生效时发送提示，以便观察自定义说明的影响。稍后会更新说明并再次发送相同提示。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放置的位置。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以允许 Copilot 修改文件。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改 helper 文件及其测试。
6. 注意生成的 helper 是带类型的函数，第一个参数接收 `db` 客户端并返回带类型的 publishers 数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系；此练习要观察的是更新说明后行为的*一致性*改善。

## 添加新的存储库标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。补充存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到约在第 27 行的 **Code formatting requirements** 部分。它已记录项目编码标准，但尚未包含代码文档规则。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件可以创建在存储库级别（`.github/copilot-instructions.md`），用于全局指导；也可以创建为 `*.instructions.md` 文件，针对特定语言、文件类型或任务提供指导。刚刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除对话上下文。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成文件现在会以类似以下内容的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在会包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

现在已引导 Copilot 遵循新的项目标准，并将其应用到下一课会继续扩展的真实代码中。

## 提交说明并推送分支

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
2. 运行以下命令，确认说明更新和 helper 变更已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新及 helper 基础：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他辅助变更（例如测试），也一并添加。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并将其关联到待办事项中的文档标准议题。Copilot 可以使用已连接到项目的 GitHub 工具查找议题、创建关闭议题的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会要求登录 GitHub。按提示授权即可。后续练习会介绍这种连接方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中 **Agent**。
3. 请 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，说明说明更新和 publishers helper 基础：

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 提示允许其代表你执行 GitHub 操作时，检查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示请求 Copilot 合并 PR 并将本地分支切回 `main`：

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 提示允许其执行 GitHub 操作和 shell 命令时，检查命令并酌情选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你探索了 Copilot 如何从说明文件中获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 如何根据*现有*说明生成内容。
- 向 `.github/copilot-instructions.md` 添加一项全库标准。
- 再次运行相同提示，观察生成代码如何遵循新标准。
- 将说明更新和 helper 基础提交到 `main`，让下一课可以继续构建。

接下来，在[智能体模式][next-lesson]中应用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
