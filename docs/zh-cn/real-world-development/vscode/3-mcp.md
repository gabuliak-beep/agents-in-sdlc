---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能正常。你无需亲自点击操作，而是可以连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习将介绍如何：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当代理模式可以访问编辑器之外的资源时，它的能力会大幅提升。Copilot 通过 Model Context Protocol (MCP) 实现这一点。MCP 是代理与外部工具和服务通信的标准方式。

![代理模式内部工作原理示意图，显示其如何与上下文、LLM 和工具交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 代理提供了与外部工具和服务通信的方式。通过 MCP，AI 代理可以实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁，负责管理 AI 代理与外部工具之间的通信，例如现有 API 或 NPM 包等本地工具。每个 MCP 服务器都提供一组 AI 代理可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，让 AI 代理可以创建存储库、更新现有存储库以及管理 issue 和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：提供基于 Playwright 的浏览器自动化功能，让 AI 代理可以浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可提供不同工具和资源。GitHub 提供 [MCP 注册表](https://github.com/mcp)，便于发现生态系统中的资源并参与贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细审查其源代码、验证发布者，并考虑相关安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件配置了 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板中已包含此 `github` 条目，因此 Copilot 能在上一练习中读取待办 issue。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容；VS Code 会通过 HTTP 连接，Copilot 首次使用服务器工具时，你需要使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控浏览器，正好可用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，最终内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志会让 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面环境显示窗口的 codespace 来说是必需的。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 运行端到端测试，因此通常已安装浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并要求你确认是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它立即启动服务器，确认配置正确。
2. 如果 VS Code 询问你是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选操作。如果跳过，VS Code 会在下一步 Copilot 首次需要服务器工具时启动服务器，并提示你确认信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可让 Copilot 驱动真实浏览器。代理可以打开页面、导航、应用筛选并读取结果，然后总结观察到的行为。无需离开对话即可快速确认功能是否符合预期。

在幕后，Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]，而不是屏幕截图。这意味着代理会根据结构化且有标签的元素（按钮、链接和列表项）进行推理，与辅助技术的工作方式相同。因此，快速的功能检查也能顺便检查基本无障碍行为。

服务器已连接且应用正在运行时，请 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行测试并报告结果。对照 issue 中的验收标准查看报告。如果结果不符合预期，可以提出后续问题，或要求它先修复代码再创建拉取请求。

> [!NOTE]
> 此测试要求应用在 `http://localhost:4321` 上运行。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），让团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示已更改文件的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 和 GitHub 命令行界面（CLI）等多种方式创建拉取请求。既然正在使用 GitHub Copilot，不妨让它帮你创建 PR。它可以查找相关 issue，并创建与 issue 关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认代理下拉菜单中已选中 **Agent**，让 Copilot 能使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行所需操作来收集信息和完成任务。首次使用 GitHub 工具时，系统可能会提示你进行 GitHub 身份验证，请按提示授权。
5. 观察 Copilot 如何搜索 issue、找到正确的 issue 并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但暂时**不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
