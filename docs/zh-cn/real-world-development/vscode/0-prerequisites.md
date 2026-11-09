---
title: "练习 0：准备工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要先准备好所需资源。你将创建 Tailspin Toys 存储库的副本，启动一个供你使用的 [codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 已正常运行。

## 设置实验存储库

你将从[模板][template-repository]创建存储库副本。新存储库包含实验所需的全部文件，你将在后续练习中使用它。

1. 在新的浏览器窗口中，打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面上选择 **Use this template**，然后选择 **Create a new repository**，创建存储库副本。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![存储库模板设置输入界面](../../../_images/ex0-repository-settings.png)

4. 记下新建存储库的路径（**organization-or-user-name/repository-name**），稍后会用到。

> [!NOTE]
> **待办列表已准备就绪**
>
> 从模板创建存储库时，系统会自动为你创建一组 GitHub 问题，作为待办事项。你将在整个研讨会中使用这些问题，无需自行创建。

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

## 使用 GitHub Copilot Chat 和代理模式

要在 IDE 中使用 GitHub Copilot Chat 代理模式，需要安装 GitHub Copilot Chat 扩展。如果你使用 GitHub Codespace，该扩展应该已经安装。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开 Extensions 视图，搜索 **GitHub Copilot Chat**，然后选择 **Install**。

安装扩展后，你可能还需要使用 GitHub 帐户进行身份验证才能启用它。

1. 返回 codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入一条消息，例如“你好，世界”，然后按 <kbd>Enter</kbd>。这会启动 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示你登录 GitHub 帐户。按照提示完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 完成身份验证后，Copilot Chat 窗口应会显示出来。

## 总结

你已创建实验存储库的副本，并开始创建 codespace。开始编写代码时，你会使用这个环境。

## 下一步

现在就让 Copilot 开始工作。继续学习[练习 1 - 自定义指令][next-lesson]，向 Copilot 介绍项目规范。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
