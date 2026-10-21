---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要遵循特定方式，就应确保 Copilot 能获取相关背景信息。[指令文件][instruction-files]是提供此类指引的重要方式，可指导 Copilot 如何生成代码。

本练习将介绍如何：

- 探索项目专属上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 在现有指令生效时，使用 Copilot 生成筛选功能所需的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次发送相同提示，观察生成代码如何遵循新标准。
- 将指令更新和筛选功能切片提交到 `main`，供下一练习中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能不完全遵循你设定的标准，因为 Copilot 的输出具有概率性。本练习的目标是观察更新指令后行为的整体变化，而非要求逐字节匹配示例输出。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求：

- 数据层始终需要单元测试。
- UI 应采用暗色模式，并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头应有一段注释，说明文件用途。

借助指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。它可以引导 Copilot 给出更相关的建议和代码片段。你可以指定首选编码约定、库，甚至希望加入的注释类型。你可以为整个存储库或特定文件类型和任务创建指令。

指令文件分为两种：

- `.github/copilot-instructions.md`：每次处理该存储库的请求时，都会将这个唯一的指令文件发送给 Copilot。此文件应包含项目级信息，即大多数 Copilot 请求都需要了解的上下文，例如技术栈、项目概况、最佳实践和全局指引。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的指令文件。你可以为特定语言（例如 TypeScript 或 Astro）编写指引，也可以为创建 UI 组件或单元测试等任务编写指引。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时会使用存储库级指令，以及包含 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

全面讨论如何创建指令文件超出了本练习的范围。不过，示例项目展示了具有代表性的方法。总体来说：

- 将 `copilot-instructions.md` 中的内容集中在项目级指引上，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供详细指引。
- 使用自然语言，保持指引清晰，并提供代码应如何（以及不应如何）编写的示例。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过实践，你会逐渐找到适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一组完善的指令文件。浏览此项目的指令文件时，你会看到许多任务专用文件，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成指令文件。不同界面提供的操作有所不同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。相关练习会在适当时机介绍这些操作。
>
> 想寻找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览项目中的自定义指令文件

花些时间阅读此存储库附带的指令文件：一个核心 `copilot-instructions.md` 和一组针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指引。这些信息适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹，浏览其中内容。你会看到 Astro 文件、Drizzle 数据层、测试等方面的指引。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 决定哪些文件适用。例如，任何符合 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 注意为本项目创建单元测试的具体指引。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和现有项目文件的链接。这样可以把较大的指令集拆分成更小、可复用的文件，并通过示例引导 Copilot。此处的路径相对于指令文件，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求添加代码内文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 返回上一个练习中使用的 codespace。
2. 在 Codespaces 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。模式和模型的选择因练习而异；相关步骤会在适当时说明。

要了解自定义指令的影响，先在当前指令文件生效时发送提示。稍后更新指令文件后，再次发送相同提示并比较结果。

1. 关闭之前练习中打开的编辑器选项卡，确保 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，让 Copilot 可以应用文件更改。

   ![显示 Chat 视图代理选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会涉及辅助函数文件和测试。
6. 注意建议的辅助函数是类型化函数，接收 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定（适用于 `src/lib/*.ts`）。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有要求，它也可能添加文档注释。如果出现这种情况也没关系，本练习仍是为了观察更新指令后行为是否更一致。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。注意这里记录了项目编码标准，但尚未要求添加代码内文档，这正是生成的辅助函数缺少文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件既可以放在存储库级别（`.github/copilot-instructions.md`），提供全局指引，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指引。项目级标准（例如文档注释规则）应放在存储库级指令文件中。

## 重新发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清空对话。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 再次发送相同提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意建议文件现在会以类似以下内容的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 注意建议的函数现在会包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回包含所有发行商 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为发行商对象数组。
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于下一练习会使用的真实代码。

## 提交指令并推送分支

指令文件与存储库中的其他文件一样，使用相同的源代码管理流程进行维护。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他配套更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求并关联到待办列表中与更新指令相关的 issue。Copilot 可以使用已连接到项目的 GitHub 工具代为查找 issue、创建 PR 并关闭 issue。

> [!NOTE]
> 这是本练习中首次让 Copilot 代表你操作 GitHub。项目模板已连接 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub；按提示授权即可。稍后会在 Model Context Protocol 练习中介绍此连接的工作方式。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认代理选择器中选中了 **Agent**。
3. 请 Copilot 查找与更新指令文件相关的 issue，并从当前分支创建 PR，同时说明指令更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，请先审查命令，再按需选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR 并将分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代表你执行 GitHub 操作和 shell 命令时，请先审查命令，再按需选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结和后续步骤

你已探索 Copilot 如何读取指令文件中的上下文，并在 VS Code 的 Copilot Chat 中：

- 发送代码生成提示，观察 Copilot 在现有指令下生成的内容。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次发送相同提示，观察建议代码如何遵循新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，在[代理模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
