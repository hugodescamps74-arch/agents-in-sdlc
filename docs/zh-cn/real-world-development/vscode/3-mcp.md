---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚才使用代理模式构建了筛选功能。在创建拉取请求（PR）之前，应先确认该功能在浏览器中可以正常运行。接下来，不必亲自操作应用，而是连接 **Playwright 模型上下文协议（MCP）服务器**，让 Copilot 在真实浏览器中测试功能；测试完成后，再发布分支并创建 PR。

本练习将帮助你：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何通过新工具扩展 Copilot；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用该服务器，在浏览器中手动测试筛选功能；
- 发布分支，并为筛选功能创建拉取请求。

## 什么是模型上下文协议（MCP）？

当代理模式能够访问编辑器以外的资源时，它的能力会大幅增强。Copilot 使用模型上下文协议（MCP）与外部工具和服务通信。MCP 是一种标准，规定代理如何与这些工具和服务交互。

![代理模式及其与上下文、大型语言模型（LLM）和工具交互的示意图，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为人工智能（AI）代理提供了与外部工具和服务通信的方式。借助 MCP，AI 代理可以实时访问最新信息（通过资源）并代表你执行操作（通过工具）。

AI 代理通过 MCP 服务器访问这些工具和资源。MCP 服务器充当代理与外部工具和服务之间的桥梁，负责管理代理与外部工具（例如现有应用程序编程接口（API）或 npm 包等本地工具）之间的通信。每个 MCP 服务器都代表 AI 代理可以访问的一组工具和资源。

一些常见的 MCP 服务器包括：

- **[GitHub MCP 服务器](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 代理可以使用这些 API 创建新存储库、更新现有存储库以及管理问题和拉取请求。
- **[Playwright MCP 服务器](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力。AI 代理可以使用它访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器提供不同的工具和资源。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，帮助用户发现并贡献 MCP 服务器。

> [!CAUTION]
> 安全方面，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细查看其源代码并确认发布者，同时考虑其安全影响。只使用你信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已为你配置的 `github` 服务器：

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

项目模板中已包含 `github` 条目，因此 Copilot 能在上一练习中读取待办问题。该服务器使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 会通过超文本传输协议（HTTP）连接到它，并在 Copilot 首次使用其工具时要求你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用于测试你的功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件内容如下：

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

与远程 `github` 服务器不同，`playwright` 是本地服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志表示 Playwright 在没有可见窗口的情况下运行浏览器；codespace 中没有可显示浏览器的桌面，因此必须使用该标志。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器：Copilot 首次需要使用服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问你是否信任该服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选操作。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要使用服务器工具时启动服务器，并提示你确认是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了可操作的真实浏览器。无需亲自操作应用来验证更改，代理可以打开网页、浏览页面、应用筛选条件并读取结果，然后向你总结观察到的情况。你无需离开当前对话，就能快速确认功能是否符合预期。

在内部，Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着代理会分析带有标签的结构化元素（按钮、链接和列表项），与辅助技术处理页面的方式相同。因此，快速功能测试也能顺便检查一些基本无障碍体验。

连接服务器并启动应用后，提示 Copilot 测试刚完成的筛选功能：

```text
使用 Playwright MCP 服务器，在 http://localhost:4321 打开正在运行的应用，并验证新游戏筛选功能：

1. 打开游戏页面并记录游戏数量。
2. 应用类别筛选条件，确认列表只显示该类别的游戏。
3. 清除筛选条件，然后应用发行商筛选条件，确认列表只显示该发行商的游戏。
4. 同时应用类别和发行商筛选条件，确认结果同时满足这两个条件。

报告每个步骤中的观察结果，并指出任何行为异常。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行测试，并报告结果。请根据问题中的验收标准审阅摘要；如果发现异常，可以提出后续问题，或要求 Copilot 修复代码，然后再创建拉取请求。

> [!NOTE]
> 测试需要应用在 `http://localhost:4321` 运行。如果开发服务器已停止，请在发送提示词前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果提示缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以发布分支并创建拉取请求（PR），供团队审查。先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，查看 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

你可以通过 github.com 或 GitHub 命令行界面（CLI）等多种方式创建拉取请求。既然正在使用 GitHub Copilot，不妨让它来创建 PR。它可以查找相关问题并创建与该问题关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认在代理下拉列表中选中 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 让 Copilot 为你创建 PR：

   ```plaintext
   在存储库中查找与按类别和发行商筛选相关的问题。根据当前分支创建新的拉取请求，并将它关联到正确的问题。
   ```

4. 按需选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你通过 GitHub 身份验证；按照提示完成操作即可。
5. 留意 Copilot 如何搜索问题、找到正确的问题并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

本练习中，你：

- 了解了模型上下文协议（MCP）以及 MCP 服务器如何通过新工具扩展 Copilot；
- 将 Playwright MCP 服务器添加到了工作区；
- 使用该服务器在浏览器中手动测试了筛选功能；
- 发布了分支，并为筛选功能创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，创建一个[自定义代理][next-lesson]，以便更有效地执行无障碍审查等专门任务。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
