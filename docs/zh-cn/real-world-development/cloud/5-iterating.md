---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作

在本实验中，你已与 GitHub Copilot 一起完成多个改善用户体验和添加功能的任务。你请 Copilot 为代码添加文档、创建相关游戏功能以便设计团队进行迭代，并实现包含高对比度和浅色模式切换功能的无障碍特性。现在来检查代码更改，必要时向 Copilot 提供反馈以改进工作。

### 场景

正如前面多次提到的，引入生成式 AI 不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在为团队其他成员开启审查前，我们先查看 GitHub Copilot 对创建文档、实现相关游戏功能和无障碍功能的建议。

## 安全与 GitHub Copilot cloud agent

Copilot cloud agent 会异步、无人监督地执行任务，因此设置了一些安全限制：

- Copilot 只能读取存储库，并且只能写入它用于生成代码的分支。
- cloud agent 在 GitHub Actions 中运行，并在独立的临时环境中工作。
- 任何 GitHub Actions 工作流都必须先由人工批准才能运行。
- 默认情况下，[对外部资源的访问受限](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先探索 GitHub Copilot cloud agent 创建的第一个拉取请求（PR），即为代码添加文档的 PR。此任务将通过 GitHub.com 的标准 PR 界面完成。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**：Copilot 默认只能有限访问外部资源，包括调用外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。你可以趁此机会休息一下，或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 浏览更新后的代码，其中包含新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自练习 2 开始时审查过的自定义指令文件。

6. 审查更改并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求的检查部分。正常情况下，单体 Astro 应用的项目检查会通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 配合处理拉取请求并非单向过程。你可以像提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动另一个会话来处理请求。由于生成结果不确定，无法提供规定好的提示文本。可以要求 Copilot：

- 在每个代码文件顶部添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用的结构。

1. 添加评论，要求更改生成的文档，并像提及其他用户一样提及 **@copilot**。可以使用上述建议，或提出其他你希望添加到代码库的文档要求。
2. 选择 **View Session**，观察 Copilot 执行工作。注意 Copilot 会启动新会话来完成更改。
3. 选择 **Back to pull request**，返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。确认完成后，可以将 PR 从草稿转换为可审查状态，并合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看 Copilot 为解决“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更改并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入拉取请求的检查部分。正常情况下，单体 Astro 应用的项目检查会通过。此过程可能需要几分钟。
9. **可选：** 你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 中的终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能！和之前一样，你可以迭代改进 Copilot cloud agent 的工作。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住：分配此任务时，你还没有更新自定义指令）。和之前一样，可以在 **Conversation** 选项卡中添加评论来提出要求，Copilot 会看到评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义代理实现的无障碍功能。此 PR 应包含你在练习 3 中分配的高对比度模式，以及在练习 4 的 mission control 中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，重点关注：
   - 用于切换显示模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更改并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求的检查部分。正常情况下，单体 Astro 应用的项目检查会通过。此过程可能需要几分钟。
10. **可选：** 你可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后设置仍然有效。

请注意，无障碍自定义代理如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍问题或改进空间，可以像之前处理 PR 一样，在评论中提及 **@copilot** 并要求它更新。

## 可选练习 — 继续委派任务

cloud agent 最适合处理实际待办事项，让你能够同时转向其他工作。可以在存储库中创建一些 issue 并将它们分配给 Copilot，以培养这种工作习惯。以下是一些想法：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页面实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 环境的学习。在这些课程中，你：

- **检查了存储库附带的自定义指令文件**，以便了解它们对 cloud agent 后续输出的影响。
- **将 issue 分配给 Copilot cloud agent**，并观察它设置环境、规划和异步执行任务。
- **创建并使用了无障碍自定义代理**，添加高对比度和浅色模式切换功能。
- **使用 Copilot agents 页面作为 mission control**，监控并引导无障碍会话。
- **审查并迭代改进了 cloud agent 的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 环境的学习。如果想继续探索，其他环境提供了相关练习：

- 🖥️ **[VS Code 环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 在终端中使用 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云端委派功能，完成两个经过审查的更改。

也可以在自己的存储库中尝试以下后续练习：

- 将重构或测试覆盖率 issue 分配给 cloud agent。
- 为其他领域创建自定义代理。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
