---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所说，编写代码不仅仅是写代码。我们还需要处理数据和外部服务，并允许 Copilot 使用其他自动化功能。MCP 服务器正是为此而存在：它们扩展 Copilot CLI 的内置能力，为其提供更多工具和服务。

本课将介绍如何：

- 了解 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 在尚未配置时添加 Playwright MCP 服务器。
- 请代理驱动浏览器并体验筛选功能。

## 场景

单元测试和端到端测试都很重要，但验证 UI 更新还需要实际操作界面。你希望 Copilot 能像用户一样使用正在开发的网站，从而进一步自动化更改，并更有把握地确认更新符合预期。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 代理提供了与外部工具和服务通信的方式。通过 MCP，AI 代理可以实时访问最新信息，并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁。每个 MCP 服务器都提供一组 AI 代理可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、issue 和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：提供基于 Playwright 的浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，便于发现生态系统中的资源并参与贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请审查其源代码、验证发布者，并考虑相关安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请 Copilot 使用 Playwright 体验功能

之前你已手动确认功能符合预期。现在使用 Copilot 中的 Playwright MCP 服务器完成同样的操作。

1. 使用以下提示，请 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常你不必明确要求 Copilot 使用 MCP 服务器，它会自行判断。不过，既然你知道应该使用哪个工具，不妨在提示中说明。这有助于获得更一致的结果，还能节省一些 token。

2. 观察 Copilot 如何列出在浏览器中执行的步骤，以确认功能正常。
3. 阅读报告，确保所有行为均符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，并向你提供报告。

## 总结和后续步骤

恭喜！你已通过 Copilot CLI 使用 Playwright MCP 服务器，在真实浏览器中体验功能。回顾一下，你：

- 了解了 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 在尚未配置时添加了 Playwright MCP 服务器。
- 请代理驱动浏览器并体验筛选功能。

接下来，你将[创建 QA 自定义代理][next-lesson]，把技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
