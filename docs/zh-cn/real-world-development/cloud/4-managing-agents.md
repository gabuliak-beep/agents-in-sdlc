---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几次练习中，你让 Copilot cloud agent 承担了三个与改善用户体验和添加功能有关的任务。cloud agent 可异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[代理页面][agents-page]。在此 mission control 中，可以查看所有带有未关闭拉取请求 (PR) 的代理任务、了解已执行的操作，也可以引导进行中的会话。

本课将介绍如何：

- 使用代理页面监控 cloud agent 任务。
- 引导进行中的会话，要求添加其他功能。

## 场景

将创建高对比度模式的任务分配给代理后，团队意识到也应该添加浅色模式。既然正在更新网站样式并添加切换功能，把这项功能也包含进来很合理。现在请引导代理，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应该能看到分配给 Copilot 的任务，包括：
    - 更新代码库文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成任务所需的各个步骤。

> [!NOTE]
> 具体列出的步骤会因 Copilot 的工作状态和采用的方法而异。

4. 还要留意右侧显示的拉取请求 (PR) 面板。你可以在此查看 PR 和已更改的文件，进一步监控工作。

## 引导 cloud agent

你已了解当前正在运行的任务。现在请 Copilot 在处理高对比度模式时，也添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始工作。看到类似下方的消息时，说明已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中的 cloud agent 任务截图，突出显示“在 Copilot 工作时引导活动会话”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示，并将其纳入工作流程。

## 让 Copilot 执行任务

和之前一样，Copilot 会开始处理更新后的任务！完成发送消息时正在执行的步骤后，它会将新请求纳入工作流程。

如前所述，这需要几分钟，正好可以借此时间暂停并回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心位置。借助这个 mission control，你可以跟踪所有活动和已完成的任务、审查正在执行的工作，甚至重定向进行中的任务以调整范围或提供更多指引。

你探索了以下概念：

- 使用 mission control 和代理页面监控 cloud agent 任务。
- 重定向进行中的会话，要求添加其他功能。

Copilot 正在完成无障碍功能，接下来你将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
