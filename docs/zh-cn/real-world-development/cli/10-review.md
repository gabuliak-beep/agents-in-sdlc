---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义功能及后续资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的完整工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目，并找到预先创建的筛选 issue。
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选 issue 开始，在 Plan 模式中确定方案，在 Autopilot 模式中构建功能，并在 Interactive 模式中审查。
- 使用自定义指令引导代理，随后自定义现有的 `quality-checks` 技能，并通过它运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证 (QA) 自定义代理，以评估需求、覆盖率、技能结果和浏览器证据。
- 审查了完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 你交付的内容

本工作坊包含两个 PR 里程碑，均在更新后的 `main` 上使用独立分支：

1. **星级评分：** 在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：** 实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并包含相关测试。

从规划筛选功能到创建 PR，你都使用同一对话和分支。为了简化本工作坊，我们把这部分工作放在一个 PR 中。

## 不同类型的验证

你通过多种方式检查了代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按照新格式报告结果。QA 则将这些结果与需求和测试覆盖率审查汇总起来，为 PR 提供证据。

新增测试应弥补实际缺口；QA 运行后不需要补充测试也可能是正确结果。授权合并前，请审查代码和证据；更改之后要重新生成受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置 MCP 服务器。可以在其他对话中复用这些自定义设置，并根据团队需求进行调整。指令用于设定标准，技能描述可重复执行的任务，自定义代理定义专用角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是代理的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前考虑方案，使用 **Interactive** 参与目标明确的更改，使用 **Autopilot** 处理范围明确的任务。日常编辑可以选择速度更快的模型，复杂任务则选择能力更强的模型。

上下文仍和基础设施一样重要。清楚说明要构建**什么**、**为什么**以及**如何**构建，会显著影响生成结果。

## 更多探索

你已完成核心工作流。还有一些值得了解的 CLI 功能：

- 使用 `/review` 请求代码审查代理分析更改。
- 使用 `/rubber-duck` 讨论问题并获得不同视角。
- 使用 `/fleet` 并行协调相互独立的子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务发送给 Copilot cloud agent。

## 后续步骤

持续使用工具，是提升熟练度的最佳方法。将它用于生产代码、个人项目，或你多年来一直想构建的小应用。与团队分享经验，也向他们学习。和往常一样，别忘了探索文档。

如果想通过终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
