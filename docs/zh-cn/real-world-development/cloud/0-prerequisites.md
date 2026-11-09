---
title: "练习 0：准备工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云代理练习前，需要先准备好所需资源。你将创建 Tailspin Toys 存储库的副本，并启动一个 [codespace][codespaces]，用于编辑指令文件和审查云代理生成的工作。

## 设置实验存储库

你将从[模板][template-repository]创建存储库副本。新存储库包含实验所需的全部文件，你将在后续练习中使用它。

1. 在新的浏览器窗口中，打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面上选择 **Use this template**，然后选择 **Create a new repository**，创建存储库副本。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![存储库模板设置输入界面](../../../_images/ex0-repository-settings.png)

4. 记下新建存储库的路径（**organization-or-user-name/repository-name**），稍后会用到。

## 创建 codespace

接下来，你将使用 codespace 完成本实验中的练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的集成开发环境（IDE），支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

codespace 需要几分钟才能创建完成，不过这仍比手动安装所有服务快得多。等待期间，你可以了解 GitHub Copilot 的其他功能，接下来就会用到它们。

> [!CAUTION]
> 你会在后续练习中返回这个 codespace。目前请在浏览器标签页中保持它打开。

> [!NOTE]
> 本研讨会可在 codespace 或本地[开发容器][dev-containers]中运行。两者都已准备好所需的先决条件。如果你希望在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

你已创建实验存储库的副本，并开始创建 codespace。后续练习中，你将在该环境中与 Copilot 云代理协作。

## 下一步

接下来，为云代理添加自定义指令。继续学习[练习 1 - 自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
