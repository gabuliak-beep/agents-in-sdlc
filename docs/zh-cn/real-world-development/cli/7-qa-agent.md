---
title: "第 7 课 - 创建并使用 QA 代理"
description: "创建 QA 自定义代理，将 issue 需求、quality-checks 技能和 Playwright MCP 整合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中查看筛选体验。现在，你将把这些能力整合到自定义代理中，并为它定义明确的 QA 流程。

本课将介绍如何：

- 探索自定义代理如何配合指令、技能和 MCP 工具使用。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 代理，并根据筛选 issue 审查其调查结果。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，对需求、代码质量、自动检查、测试覆盖率和浏览器行为进行一致的审查。自定义代理可以协调此 QA 流程，并生成可复用的报告。

## 什么是自定义代理？

自定义代理是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件描述代理的用途、指令和可用工具。本工作坊将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择该角色。

之前使用的自定义功能各有用途：存储库指令说明团队标准，`quality-checks` 技能封装可重复执行的检查，Playwright MCP 提供浏览器工具。QA 配置文件告诉 Copilot 如何使用这些能力来评估需求并报告结果。它不会取代这些功能，也不需要另外开启对话。

## 创建 QA 配置文件

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置文件。该文件会定义 QA 执行的检查和必须遵守的界限。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 请默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新代理前，先审查它的配置文件，确认 Copilot 已记录预期的 QA 工作流和约束。这样可以避免代理配置不完整或范围过大，在你只想验证功能时却修改了功能代码。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填字段；`name` 可选，但添加后会让代理有明确的显示名称。
3. 阅读配置文件中的指令，确认 QA 会从需求开始、遵循存储库指令、运行 `quality-checks` 技能，并使用 Playwright MCP。
4. 确认 QA 会报告支持证据、在修改实现代码前征求许可，且不会提交更改或创建拉取请求。
5. 如果生成的配置文件遗漏了以上职责或边界，请先要求默认代理修改，再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置文件后，恢复之前的筛选对话，再选择 QA，这样它便能使用对话中已有的 issue 和计划决策。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚创建了此代理，它可能不会显示在可用代理列表中。代理仍然存在，可以使用上述命令启用。

2. 使用以下提示，请 QA 代理审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 代理会开始工作。
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你已在工作流中添加可复用的专用角色，并审查了它的工作。本课中，你：

- 探索了自定义代理如何配合指令、技能和 MCP 工具使用。
- 创建并检查了可复用的质量保证配置文件。
- 选择 QA 代理，并根据筛选 issue 审查其调查结果。

现在，实现、技能更新、QA 配置文件、测试和验证报告都已准备就绪，可以进行审查。接下来，你将[整合这些内容到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
