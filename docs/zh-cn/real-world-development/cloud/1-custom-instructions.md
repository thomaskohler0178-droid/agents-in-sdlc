---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解一些背景信息，就应确保它能够获取相关上下文。[说明文件][instruction-files]可提供这些指导，让 Copilot 不仅了解你希望它做*什么*，还了解*如何*完成。

本练习将：

- 探索项目上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件提供给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一项全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会在修改前后分别发送提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示效果。后续审查云智能体生成的拉取请求时，可以看到说明文件的影响。

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

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送这一个说明文件。它应包含适用于大多数 Copilot 聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中工作时，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持符合其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

全面讨论如何创建说明文件超出了本研讨会范围。示例项目展示了一种有代表性的做法：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供指导。
- 使用清晰的自然语言，并提供代码应如何编写或不应如何编写的示例。

说明文件没有唯一的编写方式，AI 的使用方式也不止一种。通过实验，可以找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的说明文件。浏览本项目时，可以看到其中包含针对多种任务的文件，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面的操作方式各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应环境的课程会在相关位置介绍。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义说明文件

花些时间阅读本存储库附带的说明文件：一个核心 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 嵌套指导，这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览，查看其中针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob，用于决定说明适用范围。此处与 `**/*.test.ts` 匹配的 TypeScript 测试文件都会匹配。
5. 查看其中针对本项目单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的说明集拆分成更小、可复用的文件，并为 Copilot 提供生成代码时可参考的示例。路径相对于说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

本练习只需进行一项编辑：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后分配议题时，云智能体会读取这项规则。

编辑前先创建分支。云智能体会读取议题目标分支中的说明，但仍建议单独提交本次修改：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到约在第 27 行的 **Code formatting requirements** 部分。注意其中记录了项目编码标准，但还没有代码文档要求，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件可以创建在存储库级别（`.github/copilot-instructions.md`），用于全局指导；也可以创建为 `*.instructions.md` 文件，针对特定语言、文件类型或任务提供指导。刚刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明。下一课将把议题分配给 Copilot，而它会从 `main` 创建分支，因此必须先将说明变更合并到 `main`，云智能体才能读取。

1. 暂存并提交变更：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。之后，云智能体处理下一课分配的议题时就会读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你探索了 Copilot 如何从项目中的说明文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了一项全库标准。后续练习中，可以在云智能体创建的拉取请求中看到该标准的效果。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
