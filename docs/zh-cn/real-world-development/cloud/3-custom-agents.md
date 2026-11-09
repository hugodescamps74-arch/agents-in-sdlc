---
title: "练习 3 - 自定义代理"
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

使用自定义代理时，你将了解：

- 自定义代理的定义方式；
- 将任务分配给自定义代理的方法。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，目前的深色主题对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍功能至关重要，因此你希望尽快实现它。接下来将使用自定义代理生成此功能。

## 查看无障碍自定义代理

我们已为你创建了无障碍自定义代理。先查看文件，了解它将如何引导 Copilot。

返回 codespace，然后查看无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML 前置元数据。

> [!CAUTION]
> `name` 和 `description` 前置元数据是自定义代理必需的字段。

3. 浏览后续部分，查看其中列出的内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - 超文本标记语言（HTML）、层叠样式表（CSS）和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 创建并分配问题

任务控制中心是管理环境中所有代理的中心位置。你可以将任务分配给 Copilot 云代理、监控任务并引导会话。现在，将实现高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 创建问题。
5. 将 **Title** 设置为 `为网站添加高对比度模式`。
6. 将 **Description** 设置为：

    ```plaintext
    我们需要为网站添加高对比度模式。用户应能通过开关启用或关闭高对比度模式。设置应存储在浏览器的本地存储中。
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![云代理分配截图，突出显示自定义代理和无障碍代理](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 中的自定义代理][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。使用自定义代理可以将团队的专业知识和标准编写成可重复使用的代理，从而引导 Copilot 更有效地完成特定类型的工作。

本练习中，你：

- 了解了自定义代理的定义方式；
- 将任务分配给了自定义代理。

Copilot 正在实现高对比度模式。接下来，你将前往任务控制中心，[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [为组织使用自定义代理做好准备][org-custom-agents]
- [为企业使用自定义代理做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云代理][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
