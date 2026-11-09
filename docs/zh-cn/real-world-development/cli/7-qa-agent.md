---
title: "第 7 课 - 创建并使用质量保证（QA）代理"
description: "创建质量保证自定义代理，将问题要求、quality-checks 技能和 Playwright 浏览器工具结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并通过 Playwright 模型上下文协议（MCP）服务器在浏览器中观察筛选体验。现在将这些能力整合到一个自定义代理中，建立清晰、可复用的 QA 流程。

本课将介绍如何：

- 了解自定义代理如何配合说明、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 代理，并根据筛选问题检查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，一致地检查要求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义代理可以协调这一 QA 流程，并提供可复用的报告。

## 什么是自定义代理？

自定义代理是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。在本练习中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的各种自定义功能各有用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何利用这些能力评估要求并报告发现。它不会取代这些功能，也不需要另开对话。

## 创建 QA 配置文件

在创建功能 PR 前，请让 Copilot 创建可复用的 QA 配置文件。配置文件会定义 QA 执行的检查及其必须遵循的边界。

1. 返回 Codespace，并确认筛选功能对话处于 Interactive 模式。
2. 请求默认代理创建新的自定义代理：

   ```plaintext
   创建一个名为 QA 的自定义代理，保存到 .github/agents/qa.agent.md。它应根据问题和已达成一致的要求检查功能，遵循存储库说明，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   它应将每项要求标记为通过、失败或受阻，并提供支持证据。修改实现代码前必须先询问，而且不得提交更改或创建拉取请求。

   现在只创建配置文件，供我检查。
   ```

## 检查配置文件

使用新代理前，先检查其配置文件，确认 Copilot 捕捉到了预期 QA 流程和边界。这样可避免配置不完整或权限过宽的代理在你只想验证功能时修改代码。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读前置元数据。`description` 为必填项；`name` 是可选项，但添加后能清楚显示代理名称。
3. 阅读配置文件中的说明，确认 QA 会从要求着手，遵循存储库说明，运行 `quality-checks` 技能，并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件遗漏了上述职责或边界，请在继续之前要求默认代理修改。

## 根据问题运行 QA

创建配置文件后，Copilot CLI 会在对话启动时加载项目代理。创建配置文件后，恢复同一个筛选功能对话，再选择 QA，以便它能使用已有的问题和规划决定。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于代理是刚创建的，它可能不会显示在可用代理列表中。即使如此，以上命令仍会启用它。

2. 使用以下提示，让 QA 代理检查功能：

   ```plaintext
   根据问题和计划中达成的决定检查筛选功能。它可以准备好创建 PR 了吗？
   ```

3. QA 代理将开始工作！
4. 工作完成后，阅读它提供的报告。

## 总结和后续步骤

你已添加可复用的专用角色，并检查了它的工作。本课中，你：

- 了解了自定义代理如何配合说明、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 代理，并根据筛选问题检查了它的发现。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告都已准备就绪，可以进行检查。接下来，将这些内容整理到功能 PR 中，并使用 Agent Merge。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
