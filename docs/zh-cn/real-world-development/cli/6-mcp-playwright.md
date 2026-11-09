---
title: "第 6 课 - 使用 Playwright 模型上下文协议（MCP）验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所说，编写代码并不只是写代码。我们还需要处理数据、外部服务，并允许 Copilot 使用其他自动化工具。这就是 MCP 服务器的作用。MCP 服务器让 Copilot 能够使用 CLI 内置功能之外的工具和服务。

本课将介绍如何：

- 了解 MCP 及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器。
- 让代理驱动浏览器并体验筛选功能。

## 场景

单元测试和端到端测试固然重要，但验证用户界面（UI）更新还需要实际与 UI 交互。你希望 Copilot 能像用户一样使用正在开发的网站，从而进一步自动化更改，并增强对功能符合预期的信心。

## 什么是 MCP？

[模型上下文协议（MCP）][mcp-blog-post]为人工智能（AI）代理与外部工具和服务通信提供了一种方式。借助 MCP，AI 代理可以实时与外部工具和服务通信，从而访问最新信息并代表你执行操作。

这些工具和资源通过 MCP 服务器访问，MCP 服务器充当代理与外部工具和服务之间的桥梁。每个 MCP 服务器都提供一组 AI 代理可访问的工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、问题和拉取请求的应用程序编程接口（API）。
- **[Playwright MCP Server][playwright-mcp-server]**：提供基于 Playwright 的浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，帮助开发者发现并参与生态建设。

> [!CAUTION]
> 使用 MCP 服务器时，应像使用项目中的其他依赖项一样谨慎。使用前，请检查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 将以下内容粘贴到 **Command** 对话框中：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

之前你已手动确认功能按预期运行。现在使用 Copilot 中的 Playwright MCP 服务器执行同样的操作！

1. 使用以下提示，让 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   启动应用，并使用 Playwright MCP 对照问题描述和计划检查筛选功能。告诉我哪些功能正常、哪些不正常，不要进行任何更改。完成后停止你启动的服务器。
   ```

> [!NOTE]
> 你不必明确要求 Copilot 使用 MCP 服务器，因为它通常会自行判断。但既然你知道应该使用哪个工具，明确指出也有好处！这样更有助于获得一致结果，还能节省一些令牌。

2. 观察 Copilot 如何列出在浏览器中验证功能的各项操作。
3. 阅读报告，确认一切按预期运行。

Copilot 会启动服务器，使用 Playwright 与网站交互，停止服务器，然后提供报告。

## 总结和后续步骤

恭喜！你已使用 Playwright MCP 服务器，通过 Copilot CLI 在真实浏览器中体验功能。回顾一下，你：

- 了解了 MCP 及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加了该服务器。
- 让代理驱动浏览器并体验了筛选功能。

接下来，你将[创建 QA 自定义代理][next-lesson]，将技能与浏览器工具结合起来，打造专门的质量检查角色。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
