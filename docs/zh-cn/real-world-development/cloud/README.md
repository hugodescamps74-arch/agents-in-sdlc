---
slug: real-world-development/cloud
title: "GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)可在云端异步处理任务。你可以在 GitHub 上分配工作，云代理会在后台接手任务、探索存储库、进行更改并创建拉取请求（PR），你则可以继续处理其他工作。

完成这些练习后，你将为云代理添加自定义指令，分配 GitHub 问题并让它实现需求；还将查看和使用自定义代理来引导它的工作方式，在代理页面监控和调整会话，最后审查它创建的拉取请求并继续迭代。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|------|
| [0. 准备工作][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加云代理将遵循的自定义指令 |
| [2. 云代理][ex2] | 异步代理 | 将问题分配给 Copilot 云代理 |
| [3. 自定义代理][ex3] | 专用代理 | 查看并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作并选择后续步骤 |

## 准备工作

参加本研讨会前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为你的帐户或组织启用 Copilot 云代理

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education] 验证身份的学生可免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会使用的代理、模型上下文协议（MCP）、代码审查和 Copilot 命令行界面（Copilot CLI）功能，因此可用于完成所有工作流。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Student、Pro、Pro+、Business 和 Enterprise 均可使用云代理；只有 Copilot Free 计划不支持。在 Copilot Business 或 Enterprise 计划中，管理员必须为组织启用云代理。

> [!NOTE]
> 本工作流不涉及 MCP。若要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 工作流](../cli/)或 [VS Code 工作流](../vscode/)。

## 开始

**[从练习 0：准备工作开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
