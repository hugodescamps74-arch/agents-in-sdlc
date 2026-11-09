---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的人工智能（AI）助手。你可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，为其提供明确的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编写成可重复使用的代理，例如确保符合 [Web 内容无障碍指南（WCAG）][wcag]要求的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局目录 `~/.copilot/agents` 中定义。每个文件都包含 YAML 前置元数据，至少要有 `name` 和 `description` 字段，后面是用于定义代理行为、专长和指令的 Markdown 提示词。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]有些相似。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理负责执行工作，技能提供工具。**

自定义代理拥有独立的上下文窗口，能够协调技能（甚至其他代理）完成工作。在本实验中，无障碍自定义代理会根据无障碍准则审查和更新网站；执行任务时，它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的正确方式。与 AI 的其他用法一样，需要不断测试和迭代，找到适合自身环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，目前的深色主题对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍功能至关重要，因此你希望尽快实现它。接下来将使用自定义代理生成此功能。

本练习将帮助你：

- 查看现有的无障碍自定义代理；
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

我们已为你创建了无障碍自定义代理。先查看文件，了解它将如何引导 Copilot。

返回 codespace，然后打开终端，从 `main` 创建新分支，用于单独处理无障碍功能（这样可以将筛选功能拉取请求（PR）与本练习分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML 前置元数据。

> [!CAUTION]
> `name` 和 `description` 前置元数据是自定义代理必需的字段。

3. 浏览后续部分，查看其中列出的内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - 超文本标记语言（HTML）、层叠样式表（CSS）和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉列表中显示 `.github/agents` 中定义的所有自定义代理。选择自定义代理后，聊天会话将使用该代理的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新会话。每个练习会说明需要使用的模式和模型。

1. 如果代理下拉列表中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉列表（其中显示当前代理，默认为 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示词：

    ```plaintext
    为网站添加高对比度模式。用户应能通过开关启用或关闭高对比度模式，并且设置应使用浏览器的本地存储持久保存，以便重新加载页面后仍然生效。
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中内联显示文件路径和差异，供你审查。
6. 随着编辑逐步应用，**Files changed** 指示器会更新，显示代理修改的文件。

> [!NOTE]
> 这个过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

你将在下一练习中审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 中的自定义代理][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。使用自定义代理可以将团队的专业知识和标准编写成可重复使用的代理，从而引导 Copilot 更有效地完成特定类型的工作。

本练习中，你：

- 查看了现有的无障碍自定义代理；
- 在 Copilot Chat 中使用无障碍代理实现了高对比度模式。

接下来，你将[监控并引导代理的工作][next-lesson]，查看实时更改，并在同一会话中添加浅色模式开关。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
