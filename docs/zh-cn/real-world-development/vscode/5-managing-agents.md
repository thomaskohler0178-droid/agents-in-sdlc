---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

启用 GitHub Copilot 智能体模式后，它会自主探索代码库、提出变更、编辑文件并运行命令。由于 Copilot 在本机实时工作，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以内联审查每个差异、接受或拒绝单项更改，并通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

本练习将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中引导智能体，除了高对比度模式外再添加浅色模式切换。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作期间，设计团队又提出了一项需求：为觉得默认主题太暗的用户添加浅色模式切换。无需从头开始新对话，可以引导现有会话扩展工作。

## 监控 Copilot 进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联方式显示差异及 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容查看每次工具调用。你会看到文件读取、文件编辑，以及它请求运行的终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
   - 选择 **Keep** 接受变更。
   - 选择 **Undo** 撤销变更，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在会话期间修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式实现。完成后会显示总结变更的最终消息。

## 引导智能体添加浅色模式

高对比度模式已经就绪，现在在同一对话中添加浅色模式切换。由于继续使用同一会话，智能体会保留之前工作的上下文，包括修改的组件、样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会检查现有变更并继续添加浅色模式。
3. 像审查高对比度模式一样，逐项检查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的实现方向不符合预期，例如试图重构切换 UI，可以选择聊天输入框旁边的 **Stop** 按钮停止任务，然后发送更明确的后续消息。

> [!TIP]
> 引导进行中的会话比从头开始更快。可以使用后续提示调整工作（例如“简化 CSS，改用 CSS 变量，避免重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式放入现有设置面板”）或扩展范围（“为两个开关添加键盘快捷键”）。

## 审查工作集

提交前快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看已更改文件列表。应该能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改的文件并检查差异。确认体现了自定义智能体要求的无障碍模式，例如 ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 确认无误后，在 Source Control 面板中暂存并提交变更。下一课会发布这些变更。

## 总结和后续步骤

你使用 Copilot Chat 智能体模式监控并引导了正在进行的会话：逐项审查文件编辑、接受或拒绝变更，并通过后续提示扩展工作，同时保留上下文。

本课介绍了以下概念：

- 实时监控智能体工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示引导进行中的会话。

在[下一课][next-lesson]中，将无障碍工作发布为拉取请求，并审查本地自定义智能体在整个实验中的成果。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
