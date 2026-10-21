---
title: "练习 2 - 使用 Copilot Agent 模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。作为开发人员，通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏文件并继续修改……工作流程可能会不断重复。

Copilot Agent 模式正是为此而设计。

Copilot Agent 模式可以在 IDE 中更自主地工作。它的工作方式类似开发人员：先探索现有项目结构，进行必要更新，运行测试等任务，并自动修复发现的问题。接下来使用 Agent 模式为网站添加新功能。

> [!NOTE]
> Agent 模式和 cloud agent 名称相似，但用途不同。Agent 模式在 IDE 中执行任务，能够快速反馈和交互；cloud agent 则像团队成员一样异步工作，通过 issue 和拉取请求与你交互。

本练习将介绍如何：

- 使用 Copilot Agent 模式探索项目、查找相关文件并协调修改。
- 使用 GitHub Copilot Agent 模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库前，审查 Copilot Agent 模式生成的更改和测试。

## 场景

游戏列表不断扩大，你希望用户可以按类别和发行商筛选。上一练习中已经添加发行商辅助函数，现在将使用 Copilot Agent 模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先查看 Tailspin Toys 网站，了解当前功能。

这是一个以开发者为主题的桌游众筹平台。用户可以查看游戏列表和游戏详情。此网站是单体 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库；它没有独立的后端 API，也没有客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过范围有限、使用标准 DOM API 的 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行网站：

1. 返回 codespace。上一练习结束时应已切换回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下 URL 的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框询问是否打开 `http://localhost:4321` 浏览器窗口，请选择 **x** 关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，从 Codespace 终端选择 localhost URL 会自动跳转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通向 codespace 的专用隧道，你的网站服务器现在可通过此隧道访问。

### 浏览网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示桌游列表、标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示游戏标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。模式和模型的选择因练习而异；相关步骤会在适当时说明。

网站的初始实现已经可以运行，但还可以添加更多功能。先查看待办列表。根据模板设置项目时，系统已自动创建一组 issue；请 GitHub Copilot 显示这些待办事项。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。Agent 代理会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图代理选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非在推荐某个特定模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此在此注明，希望建议结果相对一致，确保学习体验良好。LLM 的输出具有概率性，收到的建议可能与本工作坊示例不同，这是正常现象。

> [!NOTE]
> 由于 LLM 的概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问存储库中的待办 issue：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有 issue 的命令。
5. 查看生成的 issue 列表。

注意 Copilot 还根据对用户的价值对待办事项进行了优先级排序。

## 审查指令文件

开始让代理生成代码前，先审查将用于提供上下文的指令文件。本练习会使用[用户界面 (UI) 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的整体指引，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各种文件类型具体指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置。这便于复用内容、拆分较大指令集，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要更新三个部分：

- 在数据层（`src/lib/`）中添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，测试必须先运行并通过，才能将更改合并到代码库。Copilot Agent 模式可以完成这些任务，接下来让它实现此功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新的对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍选中 **Agent**。

   ![显示 Chat 视图代理选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办列表中的相关 issue 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目，找到相关数据层辅助函数、UI 和测试，再修改文件并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库大小，可能需要几分钟。处理过程中，代码编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo**，无需在工作进行时选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板提示用户确认是否运行测试的屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 工作过程中可能会暂停，甚至出现测试失败。这没有问题！Copilot 会在生成代码和运行测试之间迭代，直到完成任务且未发现错误。

   ![Copilot Agent 模式完整 Chat 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请务必审查 Copilot 或任何 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你使用 GitHub Copilot Agent 模式为 Tailspin Toys 网站添加新功能。你了解了如何：

- 使用 GitHub Copilot Agent 模式跨 UI 和数据层实现新功能。
- 使用 Copilot Agent 模式探索项目、查找相关文件并协调修改。
- 在将代码合并到代码库前，审查 Copilot Agent 模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分探索练习 — 实现分页

随着游戏列表不断增长，需要支持分页。使用本练习中学到的技能，请 Copilot 更新网站并实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用现有指令文件。
- 考虑分页方式，例如允许用户选择每页大小或使用固定值。
- 提供足够指引，让 Copilot 按预期实现功能。
- 可能需要与 GitHub Copilot 迭代沟通，要求修改并提供上下文。这是使用 Copilot 时的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
