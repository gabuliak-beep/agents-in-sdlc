---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot cloud agent 练习前，需要先做好准备。你将创建自己的 Tailspin Toys 存储库副本，并启动一个 [codespace][codespaces]，用于编辑指令文件和审查 cloud agent 生成的工作。

## 设置实验存储库

若要创建代码实验所用的存储库副本，请根据[模板][template-repository]创建一个实例。新实例包含实验所需的所有文件，你将在整个练习中使用它。

1. 在新的浏览器窗口中，打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验室存储库页面上选择 **Use this template**，创建自己的存储库副本。然后选择 **Create a new repository**。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你正在参加 GitHub 或 Microsoft 组织的活动，请遵循导师提供的说明。否则，可以在自己有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下创建的存储库路径（**organization-or-user-name/repository-name**），稍后会在实验中用到。

## 创建 codespace

接下来，使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可以直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 打开新创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，不过仍比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能，我们接下来会介绍这些功能。

> [!CAUTION]
> 后续练习还会用到这个 codespace。暂时不要关闭浏览器标签页。

> [!NOTE]
> 本工作坊支持在 codespace 或本地 [dev container][dev-containers] 中运行。两者都确保环境已安装所有必需的先决条件，便于顺利完成练习。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 会构建与 codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

恭喜，你已创建实验存储库的副本！你还启动了 codespace，后续练习中将与 Copilot cloud agent 一起使用它。

## 下一步

接下来为 cloud agent 添加它会遵循的自定义指令。继续学习[练习 1 - 自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
