---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "了解存储库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够获取相关指导。[说明文件][instruction-files]不仅描述你需要什么代码，还会说明代码应如何组织。现在，筛选功能已完成，你将了解 Copilot 使用的说明、添加文档标准，并将其应用于代码。

本课将介绍如何：

- 了解存储库说明和按路径限定范围的说明文件如何提供给代理。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

作为规范的开发团队，Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 应保留现有格式规范和 lint 指南。

借助说明文件，你可以确保 Copilot 了解这些实践要求，并据此完成任务。

## 说明文件

自定义说明可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以通过说明引导它生成更符合需求的建议和代码片段，例如指定编码约定、库以及注释风格。你可以为整个存储库创建说明文件，也可以针对特定文件类型创建说明文件，为具体任务提供上下文。

说明文件有两种：

- `.github/copilot-instructions.md` 是单个说明文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md` 提供针对特定语言、文件类型或任务的指南。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。使用特定格式前，请参阅[自定义说明支持参考][custom-instructions-support]。

## 浏览项目中的自定义说明文件

起始项目已包含一组说明文件。先了解现有内容，再进行更改，以便观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解项目简介和编码指南。这些说明适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它用于设置决定说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意它指向其他说明文件和现有项目文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并指向可参考的示例。

## 更新说明文件以符合团队指南

现有文件虽然不错，但还缺少一项指导。现在修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含文档注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 将以下内容添加为该部分的最后一项：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指南

Copilot CLI 会在对话启动时加载存储库说明。保存更新后的说明后，退出并恢复筛选功能对话，以便 Copilot 在保留功能上下文的同时加载新指南。

1. 如果筛选功能对话仍处于打开状态，输入 `/exit` 将其关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请求 Copilot 应用更新后的指南：

   ```plaintext
   我们刚刚更新了说明和代码指南。请根据这些指南更新你生成的代码。
   ```

4. 输入 `/diff` 并检查已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确解释了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何读取说明文件，并将新标准应用于功能代码。具体来说，你：

- 了解了存储库说明和按路径限定范围的说明文件如何提供给代理。
- 更新了说明文件。
- 了解了说明文件对代码的影响。

接下来，你将[自定义并运行可复用的 `quality-checks` 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
