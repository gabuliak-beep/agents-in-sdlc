---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要遵循特定方式，或者 Copilot 应了解某些背景信息，就应确保它能获取这些信息。[指令文件][instruction-files]可提供相关指引，让 Copilot 不仅了解要做什么，也知道应该如何完成。

本练习将介绍如何：

- 探索项目专属上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，此处不会运行“前后对比”提示。Copilot cloud agent 会在 GitHub issue 上异步工作，因此很难实时并排展示影响。稍后在本环境中审查 cloud agent 创建的拉取请求时，你会看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，其中包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式，并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头应有一段注释，说明文件用途。

借助指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。它可以引导 Copilot 给出更相关的建议和代码片段。你可以指定首选编码约定、库，甚至希望加入的注释类型。你可以为整个存储库或特定文件类型和任务创建指令。

指令文件分为两种：

- `.github/copilot-instructions.md`：每次处理该存储库的请求时，都会将这个唯一的指令文件发送给 Copilot。此文件应包含项目级信息，即大多数 Copilot Chat 或 CLI 请求都需要了解的背景，例如技术栈、项目概况、最佳实践和全局指引。
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

## 添加新的存储库标准

接下来只需进行一次编辑：添加一条项目级规则，要求使用 TSDoc 文档注释和文件级注释头记录代码文档。稍后 cloud agent 处理本环境中分配的 issue 时，会使用这条规则。

编辑前，先创建一个分支（cloud agent 会从 issue 目标分支读取指令，但仍建议将编辑单独提交）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。注意此处记录了项目编码标准，但还没有代码内文档规则，这正是生成的辅助函数缺少文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件既可以放在存储库级别（`.github/copilot-instructions.md`），提供全局指引，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指引。项目级标准（例如文档注释规则）应放在存储库级指令文件中。

## 提交、推送并合并指令更新

cloud agent 会读取 issue 目标分支中的指令。下一练习将把 issue 分配给 Copilot，Copilot 会从 `main` 创建分支，因此必须先将指令更改合并到 `main`，cloud agent 才能使用它。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。这样 cloud agent 在下一练习中处理 issue 时就能读取这些指令。

> [!TIP]
> 如果你希望直接在 `main` 分支上完成本工作坊，可以跳过分支步骤并直接提交到 `main`。这里使用分支，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已探索 Copilot 如何读取此项目中的指令文件，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。稍后在本环境中审查 cloud agent 创建的拉取请求时，你会看到该标准的应用情况。

接下来，[将第一个 issue 分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
