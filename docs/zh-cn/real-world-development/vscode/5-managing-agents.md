---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

使用 GitHub Copilot 代理模式时，它会自主探索代码库、提出更改建议、编辑文件并运行命令。由于这些工作是在你的计算机上实时完成的，VS Code 中的 Copilot Chat 会持续显示工具调用和文件编辑。你可以在聊天中查看每个差异、接受或拒绝单项更改，并通过后续提示引导对话，以完善或扩展工作，无需离开编辑器。

本练习将介绍如何：

- 实时监控无障碍代理的工作。
- 审查并接受建议的文件编辑。
- 在会话中引导代理，添加浅色模式切换功能。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行期间，设计团队又提出了一项需求：为觉得默认主题过暗的用户添加浅色模式切换。你无需重新开始对话，而是可以引导现有会话扩展工作范围。

## 监控 Copilot 的进度

Copilot Chat 视图会显示代理执行的每一步，包括探索的文件、运行的命令和提出的编辑。每次文件编辑都会以内联差异和 **Keep**/**Undo** 控件的形式显示。

1. 返回练习 4 使用的 Copilot Chat 视图。
2. 代理工作时滚动聊天记录，查看每次工具调用。你会看到文件读取、文件编辑以及代理请求运行的终端命令。
3. 代理提出文件编辑时，聊天中会显示差异：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让代理再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看代理在此会话中修改的所有文件。

> [!NOTE]
> 如果工具调用要求授权（例如运行终端命令），代理会暂停并显示 **Allow** 提示。仅批准你信任的命令。

5. 等待代理完成高对比度模式实现。完成后应会看到总结更改内容的最终消息。

## 引导代理添加浅色模式

高对比度模式已实现，现在在同一对话中添加浅色模式切换。继续使用当前会话，代理就能保留之前工作的上下文，包括修改的组件、采用的样式方法以及设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会继续之前的工作，探索已有更改，并提出浅色模式的补充方案。
3. 和高对比度模式一样，逐项审查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果代理的方向不符合预期（例如试图以你不喜欢的方式重构切换 UI），可以使用聊天输入框旁的 **Stop** 按钮停止工作，再发送澄清提示。

> [!TIP]
> 引导正在进行的会话比重新开始更快。使用后续提示进行调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换开关，把浅色模式整合到现有设置面板中”）或扩展范围（“还要为两个切换开关添加键盘快捷键”）。

## 审查工作文件

提交工作前，快速检查代理修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查已更改文件列表。应能看到 Astro 组件、样式和相关测试等更新。
3. 打开几个已更改文件并查看差异。确认无障碍代理实现了相应模式，例如 ARIA 属性、键盘导航、语义 HTML 和本地存储。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监控并引导正在进行的会话：审查每次文件编辑、接受或拒绝更改，并在保留上下文的同时通过后续提示扩展工作。

本课中，你了解了如何：

- 实时监控代理工作。
- 接受和拒绝单项文件编辑。
- 使用后续提示引导进行中的会话。

在[下一课][next-lesson]中，你将通过拉取请求发布无障碍功能，并审查本实验中本地自定义代理生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
