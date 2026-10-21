---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都已保存在同一分支中。现在可以一起审查它们并创建拉取请求。之前你手动合并了星级评分拉取请求（PR）；这次将由 **Agent Merge** 管理该流程。

> [!NOTE]
> 通常我们会把功能、指令更新、技能更新和 QA 代理拆分到几个独立的 PR 中。为简化本工作坊，你已将完整的筛选和质量工作流放在同一个对话和分支中，并在此 PR 中一并提交。

本课将介绍如何：

- 了解 Agent Merge 及其如何自动处理合并生命周期。
- 检查完整功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你已使用 Copilot 规划、实现并验证功能。Tailspin Toys 现在希望自动处理剩余 PR 工作，同时由开发人员掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的后续工作。启用后，Copilot 会处理 PR 的阻塞事项，例如修复失败的持续集成 (CI) 检查、回复审查意见，以及在必要时进行变基；之后，如果存储库允许，它会启用 GitHub 自动合并。

到目前为止，都是由你自己选择 **Merge pull request**。Agent Merge 可以承担这项工作。启用 Agent Merge 前，请审查工作并确认其已准备就绪。

## 使用 Agent Merge 管理 PR

代码全部完成后，一起审查代码、创建 PR，并让 Agent Merge 处理剩余流程。

1. 返回 Codespace。
2. 输入 `/agent`，打开代理对话框。
3. 从选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果合并受阻，请阅读报告的原因，并在存储库允许的情况下，手动完成已审查的合并。

## 总结和后续步骤

你已自动处理开发流程的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动处理合并生命周期。
- 检查了完整功能更改和验证证据。
- 创建了筛选功能 PR。
- 审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，你将[探索 GitHub Copilot CLI 中其他实用的斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
