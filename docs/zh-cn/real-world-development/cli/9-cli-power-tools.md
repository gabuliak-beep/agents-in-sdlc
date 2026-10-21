---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀 CLI 工具一样，GitHub Copilot CLI 提供了许多可与其交互的斜杠命令。这些命令提供高级功能、幕后信息和其他配置选项。你已使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来看看其他实用命令。

本课将介绍如何：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 了解可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和委派给 cloud agent 的可选命令。

## 场景

核心 CLI 工作流已经完成，现在来了解一些其他功能：管理上下文、切换模型、共享会话，以及将工作可选地委派给 [Copilot cloud agent][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，你也可以使用斜杠命令检查或手动压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请在存储库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及上下文在系统指令、工具、消息和空闲空间之间的分配。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就不长，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口逐渐填满时自动压缩上下文。想自行选择压缩时机时，请使用 `/compact`。如果要切换到无关任务并开启新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发人员也有各自偏好。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型及其用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和分享经验有助于大家更好地使用 AI 工具。`/share` 命令可将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看已安装版本中 `/share` 的选项。
2. 如果要共享此会话，请选择符合需求的目标，例如使用 `/share file` 将会话导出为本地 Markdown 文件。
3. 发送给他人或发布前，请先审查导出内容。会话导出内容可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展规模或委派任务

核心工作坊已经完成。Copilot CLI 还提供了适用于大型任务的命令：

- `/fleet` 可将相互独立的子任务分配给子代理并行执行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot cloud agent，由它异步处理并可能创建拉取请求。

这些命令是可选的，因为它们可能创建额外的工作树或远程工作。尝试之前，请开启新的对话并明确任务范围，然后按照常规工作流审查结果。如果想深入了解异步代理工作方式，请继续学习 [cloud agent 工作坊][cloud-workshop]。

## 总结和后续步骤

通过 Copilot CLI 中的斜杠命令，你可以配置工具、共享会话并了解幕后情况。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 了解可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索了用于并行工作、工作树和委派给 cloud agent 的可选命令。

Copilot CLI 还有更多斜杠命令和功能等待探索！接下来，[回顾所学内容][next-lesson]，并了解后续学习方向。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
