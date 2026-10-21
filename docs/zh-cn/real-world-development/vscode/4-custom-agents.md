---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，可以提供有针对性的指令、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地完成特定工作。团队可以将专业知识编写为可复用代理，例如确保符合 [WCAG][wcag] 标准的无障碍代理、遵循安全编码实践的安全代理，或维护一致测试模式的测试代理。

自定义代理可以在项目的 `.github/agents` 文件夹中通过 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件至少包含 `name` 和 `description` 两个 YAML frontmatter 字段，后面是用于定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能的比较

自定义代理和[代理技能][agent-skills-concept]有一些相似之处：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理是执行工作的人员，技能是工具**。

自定义代理有自己的上下文窗口，负责协调技能（甚至其他代理）来完成工作。在本实验中，无障碍自定义代理会根据无障碍指南审查和更新网站；它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一“正确”的方法。与 AI 相关的其他工作一样，请通过测试和迭代找到适合你环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于确保所有用户都能访问其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，部分用户认为当前暗色主题难以阅读，因为文本和背景颜色的对比度不足。为解决此无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。你将使用自定义代理来生成实现。

本练习将介绍如何：

- 审查现有的无障碍自定义代理。
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 审查无障碍自定义代理

已经为你创建了无障碍自定义代理。先审查其内容，了解它将如何引导 Copilot。

返回 codespace，在终端中切换到 `main` 的新分支，专门处理无障碍功能（这样练习 3 中的筛选 PR 就能单独保留）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，重点查看：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的所有自定义代理。可以选择自定义代理，让聊天会话遵循该代理的指令和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。模式和模型的选择因练习而异；相关步骤会在适当时说明。

1. 如果 Chat 视图的代理下拉菜单中尚未选择 **Agent**，请选择它。

   ![显示 Chat 视图代理选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉菜单（其中显示当前代理；默认情况下为 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改建议并编辑项目文件。每项编辑都会在聊天中以内联差异形式显示文件路径和更改内容，供你审查。
6. 代理修改文件时，**Files changed** 指示器会更新，显示它已修改的工作文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会真实修改存储库，包括 Astro 组件、CSS 和相关测试等现有文件。

下一练习中，你将审查并引导这项进行中的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域定制的 AI 助手。团队可以借助自定义代理将专业知识和标准编写为可复用配置，引导 Copilot 更有效地完成特定类型的工作。

你探索了以下概念：

- 自定义代理的定义方式。
- 在 Copilot Chat 代理模式中使用自定义代理。

接下来，你将[监控并引导代理的工作][next-lesson]：审查更改，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [Awesome Copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
