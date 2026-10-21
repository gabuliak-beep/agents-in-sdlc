---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺乏时间而一直搁置的功能请求。GitHub Copilot cloud agent 可以自主执行更新代码、添加功能等任务。代理完成工作后，会创建草稿 PR，供开发人员审查。这样可以把繁琐任务交给代理，加快开发进程，让开发人员专注于更重要的工作。

本练习将探索如何使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保操作安全。
- 了解明确限定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 希望处理一些技术债务。最初受雇创建网站的承包商没有留下完善的文档——事实上，项目几乎完全没有文档。团队希望先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验。他们希望在每款游戏的详情页显示相关游戏，即同一类别的其他游戏，让用户能继续浏览。目前不需要精致的实现，只需要能用于用户体验验收测试的版本。此项工作目前受到阻碍，但当前还有优先级更高的 issue。

这些任务很容易被降级处理，非常适合交给 Copilot cloud agent。它可以异步完成工作，让开发人员继续处理其他任务，之后再回来审查 Copilot 的工作并确认结果符合预期。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发人员一样在后台执行任务。你可以通过多种方式使用它，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。你可以在会话进行中或结束后查看它的工作。准备好审查建议解决方案时，它会在拉取请求中提及你。

## 明确限定指令范围的重要性

GitHub Copilot 并非魔法工具，不能替你省去思考。即使看起来简单的操作，细究后也可能很复杂，因此[请认真限定 Copilot cloud agent 任务的范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、试验并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其引导 Copilot 的方式。如果你已经完成自定义指令练习，就见过 `.github/copilot-instructions.md` 的作用；如果没有，现在可以快速查看。将工作分配给 Copilot cloud agent 前，先只读浏览存储库中已有的指令文件，以便稍后观察其影响。

在存储库的 GitHub Web UI 中打开以下文件；如果 codespace 已运行，也可以在其中打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它会将这些指令限定到 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` issue 分配给 cloud agent 时，请留意生成的拉取请求中的 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述指令文件，稍后审查 PR 时还会再次提到。

## 设置 Copilot cloud agent 的开发环境

无论由谁编写代码，通常都需要特定环境和设置脚本，以确保环境正常。将任务分配给以类似软件工程师方式工作的 Copilot 时也是如此。

cloud agent 使用 [GitHub Actions][github-actions] 运行任务。你可以创建专用的[设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 中，让它在代理开始工作前运行，从而为代理提供所需的开发工具和依赖项。为配合实验流程并提供学习机会，模板已预先配置此工作流。它会为单体 Astro 应用准备 Node.js、项目依赖项、端到端测试所需的浏览器，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设为*你的步骤*所需的最低权限。Copilot 会获得自己的操作令牌。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果没有在设置步骤中克隆存储库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现为 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，让构建和测试能够使用数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件看起来和其他 GitHub 工作流类似，但有几个重点：

- 工作流只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试工作流是否成功，无需等待 Copilot 触发。

## 添加文档

大多数项目都存在文档过时或缺失的问题。这类技术债务往往长期无人处理，会降低工作效率，增加维护代码库或帮助新开发人员加入团队的难度。Copilot 擅长创建文档，因此很适合将此任务分配给 Copilot cloud agent。它会在后台生成所需文档，后续练习中再回来审查。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建空白 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请等待一两分钟并刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前文提到的工作流）、分析项目并确定合适的处理方案。

13. 审查列表及其计划执行的任务。
14. 向下滚动拉取请求时间线，应该会看到 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到状态指示器更新。

16. 滚动查看实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适计划，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们能专注于其他工作。接下来我们会利用这一点：将另一个任务分配给 Copilot cloud agent，同时着手为应用编写代码、添加功能。

## 在游戏详情页添加相关游戏部分

GitHub Copilot cloud agent 的一大优势是可以拆分工作：你处理一项任务，它同时处理另一项。为设计团队添加相关游戏部分也许不需要太久，但这些时间仍可用于其他工作。现在就把任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会看到 issue 中第一条评论旁出现一组 👀，表示 Copilot 正在处理任务。

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理第二项请求！Copilot cloud agent 的工作方式类似于软件工程师，无需一直监控；等任务完成后再审查即可。现在来创建并使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将 issue 分配给 cloud agent，由它异步处理。Copilot 可以帮助解决技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

你探索了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 明确限定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

cloud agent 正在后台工作，现在可以着手创建和使用自定义代理。[Copilot cloud agent 还可以使用 MCP 服务器][cloud-agent-mcp]，并支持前面练习中介绍的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
