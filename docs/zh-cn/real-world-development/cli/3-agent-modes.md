---
title: "练习 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认实现方案，再通过 Autopilot 根据议题构建筛选功能，最后在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一个小功能，但更大的改动需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流程，帮助我们以正确的方式构建正确的内容。在接下来的几课中，你将按照典型的智能体开发流程，先从议题开始实现新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续功能流程会继续使用同一对话和分支。实际工作中，通常会按不同文件类型使用不同分支或 PR；这里会简化流程，以便聚焦核心概念。

本课将：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式下定义需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

在继续实现这个功能时，你还将更新存储库说明、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。积压工作中的议题描述了该功能，但例如是否允许组合多个类别等细节需要先达成一致。你将使用 Plan 模式确定这些决策，然后授权 Autopilot 在明确范围内实现。

## 背景

在开发流程中引入 AI 编码智能体并不会改变基本原则。事实上，这些原则可能更加重要！大多数开发人员的工作流程大致如下：

1. 从描述需求的已创建议题开始。
2. 规划要构建的内容。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述方式类似。

遵循这一标准流程，可以确保 AI 生成的代码满足需求，并经过与手写代码相同的验证。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可循环切换模式：

- **Interactive**：你与智能体协作。智能体提出变更建议，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，且不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并不断迭代，直至任务完成。

先从 Plan 模式开始并审查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选功能之前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd>，打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，然后返回 **Session** 选项卡。

此时会看到提示以 `#7`（或类似编号）开头。使用 `#` 可将 GitHub 议题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划可以让你在交由 Copilot 处理前，先确定功能或任务的实现方案。对于复杂工作，花些时间规划总是值得的。切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定最佳方案，以制定计划。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有唯一正确的答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 浏览计划。
6. 对不符合你决定的部分，要求 Copilot 修改。

## 批准 Autopilot

计划已经撰写并审查完毕，现在开始实现。让 Copilot 使用 Autopilot 完成工作。

Autopilot 会让 Copilot 持续迭代，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何根据既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能立即开始，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再判断检查是否完成。

## 审查并验证实现

生成代码后，和其他代码一样，必须在合并前审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与议题及规划时作出的决定进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复任何失败项。

## 探索新功能

代码看起来没问题，但能否正常运行？像之前一样启动应用，并通过 Codespaces 转发的端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别、发行商筛选，以及计划中约定的组合方式。
4. 确认重置和无结果时的行为符合议题和约定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并审查功能。本课中，你：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式下定义需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，[使用自定义说明][next-lesson]深入了解代码生成，确保生成的代码符合已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
