---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就要确保它能获取这些上下文。你可以通过多种方式向 Copilot 提供具体上下文，其中一种重要方式是使用[指令文件][instruction-files]来指导代码生成。

本练习将帮助你：

- 了解如何通过存储库自定义指令和按路径限定范围的指令文件，向 Copilot 提供项目上下文、编码准则和文档规范；
- 在现有指令下生成第一个筛选数据切片（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准；
- 重新运行相同提示词，观察生成的代码如何遵循新标准；
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合你设定的某些标准。Copilot 的结果具有非确定性。本练习的重点是观察更新指令前后 Copilot 行为的变化趋势，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践准则和要求：

- 数据层必须始终配备单元测试。
- 用户界面应采用深色模式并具有现代感。
- 应使用 TypeScript 文档（TSDoc）注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，你可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它了解你的编码风格和要求。你可以指定编码规范、库以及注释风格等偏好，从而引导 Copilot 提供更相关的建议和代码片段。你可以为整个存储库创建指令，也可以针对特定文件类型创建指令，为具体任务提供上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会将这个唯一的指令文件发送给 Copilot。文件应包含适用于大多数聊天或命令行界面（CLI）请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对具体任务或文件类型创建这些文件，例如为 TypeScript 或 Astro 提供准则，或指导如何创建用户界面（UI）组件或单元测试。

> [!NOTE]
> 在集成开发环境（IDE）中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` 前置元数据的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

详细讨论如何创建指令文件不在本研讨会的范围内，但示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 用于项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码示例说明应采用和应避免的写法。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过不断试验，你会找到最适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应配备完善的指令文件。浏览本项目的指令文件时，你可能会看到针对多种任务的文件，例如[用户界面（UI）更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式有所不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。本工作流会在相关步骤中介绍对应操作。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

先阅读存储库提供的指令文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于确定指令适用的文件。这里的规则匹配任意 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看为本项目编写单元测试时需要遵循的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小且可重复使用的文件，并为 Copilot 提供可参考的示例。（其中的路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求在代码中添加文档注释。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

按照最佳实践，先为这些更改创建分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新会话。每个练习会说明需要使用的模式和模型。

为了观察自定义指令的影响，先在当前指令文件下发送提示词。稍后更新指令后，再次发送相同提示词以比较结果。

1. 关闭之前练习留下的编辑器标签页，确保 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 向 Copilot 发送以下提示词：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加数据访问辅助函数，返回所有发行商的名称和 id。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件及其测试。
6. 留意生成的辅助函数是一个带类型的函数：它将 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层规范，该文件适用于 `src/lib/*.ts`。
7. 留意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的结果具有概率性，因此它有可能在没有相关指令时也添加文档注释。如果出现这种情况也没关系，本练习的重点仍是观察添加指令后行为的一致性变化。

## 添加存储库级标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来将编码标准写入文件，帮助 Copilot 更好地生成代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。查看其中记录的项目编码标准；目前没有关于代码文档的规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - 每个导出的函数都应包含 TSDoc 注释，说明其用途、参数和返回值。
   - 在导入语句或任何代码之前，添加说明文件用途的注释块。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可在存储库级别（`.github/copilot-instructions.md`）创建全局指导，也可使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合放置项目级标准，例如刚添加的文档注释规则。

## 重新运行提示词并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清除会话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前相同的提示词：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加数据访问辅助函数，返回所有发行商的名称和 id。应用文件更改。
   ```

4. 留意新生成的文件开头现在有类似以下内容的注释块：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 留意新生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商的 id 和名称。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为发行商对象数组。
    */
   ```

通过添加新的项目标准，你引导 Copilot 在实际代码中遵循该标准；下一练习会以这些代码为基础继续工作。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，使用常规源代码管理流程进行管理。现在，将分支提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础功能更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他配套更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求（PR），并将其关联到待办列表中的文档标准问题。你可以手动完成，也可以提示 Copilot 使用已连接到项目的 GitHub 工具查找问题、创建用于关闭问题的 PR 并合并它。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用相关工具时，VS Code 可能会提示你登录 GitHub。按照提示完成登录即可。后续练习会介绍这种连接的工作方式，即模型上下文协议（MCP）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在代理选择器中选中 **Agent**。
3. 请 Copilot 查找与更新指令文件相关的问题，并从当前分支创建 PR，说明指令更新和新建的发行商辅助函数基础功能：

   ```plaintext
   查找与更新指令文件相关的问题。从当前分支创建新的 PR，说明该 PR 会关闭此问题，并注明我们还添加了后续筛选工作所需的发行商辅助函数基础功能。
   ```

4. Copilot 会开始查找问题并创建 PR。
5. 如果 Copilot 提示你允许它代表你执行 GitHub 操作，请检查命令并在适当时选择 **Allow**。
6. 创建 PR 后，提示 Copilot 将 PR 合并到 `main`，并在本地切回 `main` 分支：

   ```plaintext
   将 PR 合并到 main。然后在本地切回 main 并拉取最新代码，确保代码已更新。
   ```

7. 如果 Copilot 提示你允许它代表你执行 GitHub 操作和 shell 命令，请检查命令并在适当时选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目中的指令文件，并在 VS Code 中使用 Copilot Chat 完成了以下工作：

- 探索了项目专用上下文、编码准则和文档规范如何通过存储库自定义指令及按路径限定范围的指令文件传递给 Copilot；
- 在现有指令下生成了第一个筛选数据切片（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加了新的存储库级标准；
- 再次发送了相同提示词，并观察了生成的代码如何遵循新标准；
- 将指令更新和筛选功能切片提交到了 `main`，让 Copilot 能在下一练习中使用更新后的指导。

接下来，将使用[代理模式][next-lesson]和这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
