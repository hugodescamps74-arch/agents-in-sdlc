---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发人员，你通常需要找到所有相关文件、进行更改、运行测试、调试、确定漏改的文件，再次修改……这类工作往往会不断重复。

Copilot 代理模式可以帮助你处理这些工作。

代理模式能在集成开发环境（IDE）中更自主地工作。它的行为类似开发人员：先探索现有项目结构，再执行所需更新、运行测试等任务，并自动修复发现的问题。接下来，我们将了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云代理虽然名称相似，但适用于两种不同的体验。代理模式在 IDE 中执行任务，便于快速反馈和交互。云代理则像团队成员一样异步工作，通过问题和拉取请求与你协作。

本练习将介绍如何：

- 使用 Copilot 代理模式探索项目、找到相关文件并协调多处更改；
- 使用 GitHub Copilot 代理模式跨用户界面（UI）和数据层实现新功能；
- 在将代码合并到代码库之前，审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户可以按类别和发行商筛选游戏。上一练习已添加发行商辅助函数；现在将使用 Copilot 代理模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

在修改代码前，先查看 Tailspin Toys 网站，了解它目前的功能。

该网站是一个带开发人员主题的桌游众筹平台，可供用户浏览游戏列表和游戏详情。整个网站是一个 Astro 应用，会在构建时将页面渲染为静态超文本标记语言（HTML）。页面在 frontmatter 中使用 Drizzle 对象关系映射（ORM）直接查询本地 SQLite 数据库；没有单独的后端应用程序编程接口（API）或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过范围有限的 Astro `<script>` 和标准文档对象模型（DOM）API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤，在 GitHub Codespace 中运行网站：

1. 返回 codespace。上一练习结束后，你应该已切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
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

   Astro 开发服务器准备就绪后，你会看到类似以下内容的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，选择 Codespace 终端中的 localhost 地址会自动跳转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace（现在正运行 Web 服务器）的私有隧道。

### 浏览网站

网站启动后，你可以查看其中的功能：

- **主页**：显示桌游列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含游戏标题、说明、发行商和类别等信息。

## 使用 Copilot 浏览待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新会话。每个练习会说明需要使用的模式和模型。

网站的初始实现已经可以使用，但我们希望添加新功能。先查看待办列表。根据模板设置项目时，系统已自动为你创建一组问题；请让 GitHub Copilot 显示这些问题。

1. 在 Chat 视图的代理下拉列表中选择 **Agent**。Agent 代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时，我们使用了 Claude Sonnet 4.5，因此在说明中列出该模型，希望由此获得相对一致的代码建议。但人工智能（AI）和大型语言模型（LLM）的结果具有概率性，你可能会看到与说明不同的建议。这完全正常，也在预期之中。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的模型上下文协议（MCP）命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示词，询问待办问题：

   ```plaintext
   显示我的 GitHub 存储库中的待办事项。请根据对用户的帮助程度，协助我确定优先级。
   ```

4. 选择 **Continue**，运行列出所有问题的命令。
5. 查看生成的问题列表。

留意 Copilot 还会根据它对用户价值的判断，为问题排序。

## 查看指令文件

让代理生成代码前，先查看用于向 Copilot 提供工作上下文的指令文件。本练习将使用 [user interface (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 文件，其中介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意文件中的整体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍原则；
   - 指向各类文件专用指令的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置，因此可以复用指令、将大型指令拆分成较小且更易管理的部分，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用程序进行三项更改：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页面，加入筛选 UI。

此外，合并更改前还需要运行并通过测试。Copilot 代理模式可以替你完成这些任务。现在开始添加筛选功能。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui**，将该指令文件添加为上下文。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉列表中仍选中 **Agent**。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示词，请 Copilot 根据待办列表中的相关问题实现功能：

   ```plaintext
   请根据待办列表中相关 GitHub 问题的要求，更新网站以支持按发行商和类别筛选。上一练习已添加发行商辅助函数；请在完成其余数据层、UI 和测试工作的同时保留并按需完善该实现。完成前确保所有测试都通过。服务器已经在运行，无需重新启动。
   ```

6. 观察 Copilot 如何先探索项目并找到相关文件。你会看到它查找数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示相应的 **Keep** 或 **Undo** 按钮；工作进行时无需选择这些按钮。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中要求用户确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 处理任务时，可能会暂停，也可能出现测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复调整，直到完成任务且未检测到错误。

   ![Copilot 代理模式完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习中，你完成了以下工作：

- 使用 Copilot 代理模式探索了项目、找到相关文件并协调了多处更改；
- 使用 GitHub Copilot 代理模式跨用户界面（UI）和数据层实现了新功能；
- 在将代码合并到代码库之前，审查了 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 拓展练习 — 实现分页

随着游戏列表不断增长，网站需要支持分页。运用本练习中的技能，提示 Copilot 更新网站以实现分页。编写提示词时可以考虑：

- 遵循现有最佳实践，包括使用已有的指令文件；
- 规划分页方式，例如是否允许用户选择页面大小，还是使用固定页面大小；
- 提供足够的指导，让 Copilot 按照你的设想实现分页；
- 视情况继续与 GitHub Copilot 迭代，提供上下文并要求它修改。这是与 Copilot 协作时的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
