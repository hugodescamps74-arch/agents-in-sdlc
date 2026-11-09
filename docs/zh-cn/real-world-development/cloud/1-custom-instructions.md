---
title: "练习 1 - 自定义指令（云代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot 云代理 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就要确保它能获取这些上下文。[指令文件][instruction-files]可提供指导，让 Copilot 不仅了解你希望它做什么，也了解你希望它如何完成。

本练习将帮助你：

- 了解如何通过存储库自定义指令和按路径限定范围的指令文件，向 Copilot 提供项目上下文、编码准则和文档规范；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和命令行界面（CLI）工作流不同，本练习不会比较提示词更新前后的结果。Copilot 云代理会异步处理 GitHub 问题，因此难以实时并排展示指令的影响。稍后审查本工作流中云代理创建的拉取请求时，你会看到指令文件发挥的作用。

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

## 添加存储库级标准

接下来要编辑一次：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后云代理处理问题时会遵循这条规则。

开始编辑前，先创建工作分支（云代理会读取问题目标分支中的指令；同时，单独提交这些更改也更整洁）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并指令更新

云代理会读取问题目标分支中的指令。下一练习中，你将把问题分配给 Copilot，它会从 `main` 分支创建新分支。因此，必须先将指令更改合并到 `main`，云代理才能使用这些指令。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，然后合并该请求。这样，云代理在下一练习中处理分配给它的问题时就能读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支操作并直接提交到 `main`。此处创建分支是为了模拟实际项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目中的指令文件，并在 `.github/copilot-instructions.md` 中添加了一条新的存储库级标准。后续练习中，你会看到云代理创建的拉取请求如何体现这条标准。

接下来，将第一个问题[分配给 Copilot 云代理][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
