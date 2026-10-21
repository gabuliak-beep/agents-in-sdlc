---
slug: zh-cn/real-world-development/cli
title: "GitHub Copilot CLI"
description: "了解 Copilot CLI 的模式、自定义功能、MCP 工具和拉取请求自动化，并交付 Tailspin Toys 的两项更改。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** 将 GitHub Copilot 带入终端，作为代理式编码助手。它可以探索代码库、生成代码、运行命令并连接外部工具，所有操作都在命令行中完成，无需切换到图形编辑器。

本工作坊将带你完成 Tailspin Toys 的连续工作流：

- 在 GitHub Codespaces 中准备项目、安装 Copilot CLI 并熟悉项目。
- 完成一项范围明确的星级评分更改，在浏览器中审查，并手动合并第一个拉取请求（PR）。
- 从筛选 issue 开始，在 Plan 模式中确定方案，在 Autopilot 模式中构建功能，并在 Interactive 模式中审查。
- 更新存储库指令，并将其应用于筛选功能。
- 自定义现有的 `quality-checks` 技能，并使用它运行项目检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并在浏览器中体验筛选功能。
- 创建 QA 自定义代理，审查需求、覆盖率和验证证据。
- 审查完整的筛选功能更改，并通过 Agent Merge 处理筛选 PR。
- 探索用于管理上下文、模型、共享和可选云端委派的实用斜杠命令。

本工作坊会创建两个 PR：星级评分，以及包含筛选功能、指令更新、技能更新、QA 配置文件和测试的更改。筛选功能和质量工作流使用同一对话和分支，便于你在探索工具时基于已有工作继续推进。

## 课程

| 课程 | 主题 | 说明 |
| ------ | ----- | ----------- |
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 安装 Copilot CLI][ex1] | 安装 | 安装并验证 Copilot CLI，然后熟悉它 |
| [2. 添加星级评分：快速上手][ex2] | 首次更改 | 显示现有评分和未评分时的备用文本，然后合并第一个 PR |
| [3. 代理模式：Plan 和 Autopilot][ex3] | 代理模式 | 根据 issue 规划功能，使用 Autopilot 构建，再通过 Interactive 模式审查 |
| [4. 使用自定义指令引导 Copilot][ex4] | 上下文 | 探索并更新指令，然后将其应用到筛选功能 |
| [5. 自定义并使用 quality-checks 技能][ex5] | 可重复检查 | 探索现有技能、更改其报告格式并运行它 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 浏览器观察 | 在 CLI 中配置 MCP 并检查筛选行为 |
| [7. 创建并使用 QA 代理][ex7] | 需求和覆盖率 | 创建并选择专用配置文件，然后收集最终验证证据 |
| [8. 创建并合并功能 PR][ex8] | 审查和合并 | 审查完整更改、创建 PR 并使用 Agent Merge |
| [9. GitHub Copilot CLI 中的斜杠命令][ex9] | CLI 功能 | 探索上下文、模型、共享和可选的 cloud agent 委派 |
| [10. 总结和后续步骤][ex10] | 总结 | 回顾工作流、可复用自定义功能和后续资源 |
| [可选：集成 Foundry][foundry] | 托管代理 | 准备模型、部署基于目录的代理并将其连接到网站 |

## 先决条件

参加本工作坊前，请确保已具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 有权限创建存储库和 Codespace
- [ ] 熟悉基本的终端或命令行操作

> [!TIP]
> 没有付费计划？通过 [GitHub Education][student-plan] 验证身份的学生可以免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊所用的代理、MCP、代码审查和 Copilot CLI 功能。

> [!NOTE]
> 如果使用 Copilot Business 或 Copilot Enterprise，请确认管理员已为你启用 Copilot CLI。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
