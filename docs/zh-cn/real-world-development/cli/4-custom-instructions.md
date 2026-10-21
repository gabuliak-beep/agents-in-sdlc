---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令、添加文档规范，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要遵循特定方式，就应让 Copilot 能够获取相应指引。[指令文件][instruction-files]不仅说明要编写什么代码，还说明代码应该如何组织。现在筛选功能已完成，你将了解 Copilot 使用的指令、添加文档规范，并将其应用到代码中。

本课将介绍如何：

- 探索存储库指令和按路径限定的指令文件如何传递给代理。
- 更新指令文件，确保遵循编码规范。
- 了解指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 记录用途、参数和返回值，包括其中可注入的 `db` 参数。
- 可复用 Astro 组件应记录其 `Props` 约定，相关代码更改时也应同步更新注释。
- 保留现有格式和 lint 指引。

通过指令文件，你可以确保 Copilot 获得正确的信息，按这些实践完成任务。

## 指令文件

自定义指令可为 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。这项功能可以引导 Copilot 给出更相关的建议和代码片段。你可以指定首选编码约定、库，甚至注释的编写方式。指令既可以应用于整个存储库，也可以限定到特定文件类型或任务。

指令文件分为两种：

- `.github/copilot-instructions.md`：每次处理该存储库的请求时，都会将这个唯一的指令文件发送给 Copilot。文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指南。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。依赖某种格式前，请参阅[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

启动项目时已包含一组指令文件。先看看现有文件，再进行更改，了解更改产生的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指南。该文件适用于此存储库中的所有 Copilot 交互。
4. 打开 `.github/instructions` 文件夹，查看其中的文件。你会看到 Astro 文件、Drizzle 数据层、测试等方面的指引。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` frontmatter 字段，它定义了此指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他指令文件和项目文件。这样可以将大型指令集拆分为较小、可复用的文件，并通过示例指导 Copilot。

## 更新指令文件以匹配团队指南

现有文件是一个良好的起点，但还有不足。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常位于第 35 行附近。
2. 在最后一条项目符号后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指引

Copilot CLI 会在对话开始时加载存储库指令。保存更改后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指引。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指引：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，逐个查看已更改的 TypeScript 文件。确认新生成的 TSDoc 注释准确说明代码行为。

## 总结和后续步骤

你已了解 Copilot CLI 如何获取指令文件中的上下文，并将新的规范应用到功能中。具体来说，你：

- 探索了存储库指令和按路径限定的指令文件如何传递给代理。
- 更新了指令文件，确保遵循编码规范。
- 了解了指令文件对代码的影响。

接下来，你将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件和其他资源集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
