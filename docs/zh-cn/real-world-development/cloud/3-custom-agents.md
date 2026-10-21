---
title: "练习 3 - 自定义代理"
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

本练习将介绍如何：

- 了解自定义代理的定义方式。
- 将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于确保所有用户都能访问其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，部分用户认为当前暗色主题难以阅读，因为文本和背景颜色的对比度不足。为解决此无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。你将使用自定义代理来生成实现。

## 审查无障碍自定义代理

已经为你创建了无障碍自定义代理。先审查其内容，了解它将如何引导 Copilot。

返回 codespace，审查无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，重点查看：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 创建并分配 issue

Mission control 是你环境中管理代理的中心位置。你可以在此处向 Copilot cloud agent 分配任务、监控任务，也可以重定向并提供其他指导。现在将创建高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建空白 issue。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![Copilot cloud agent 分配对话框，突出显示自定义代理和无障碍选项](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域定制的 AI 助手。团队可以借助自定义代理将专业知识和标准编写为可复用配置，引导 Copilot 更有效地完成特定类型的工作。

你探索了以下概念：

- 自定义代理的定义方式。
- 将任务分配给自定义代理。

Copilot 正在实现高对比度模式，接下来你将通过 mission control [监控和引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [在组织中使用自定义代理前的准备工作][org-custom-agents]
- [在企业中使用自定义代理前的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
