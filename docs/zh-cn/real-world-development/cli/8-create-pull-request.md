---
title: "第 8 课 - 创建并合并功能拉取请求"
description: "一起检查筛选功能、说明、技能更新、质量保证配置文件和测试，然后创建拉取请求并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置文件和测试都已保存在同一分支上。现在一起检查这些更改并创建拉取请求。你已手动合并星级评分拉取请求（PR）；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常，我们会将功能、说明更新、技能更新和 QA 代理拆分为几个独立的 PR。为了简化练习，你一直在同一个对话和分支中完成筛选功能及质量工作，并将所有更改放入这个 PR。

本课将介绍如何：

- 了解 Agent Merge 及其如何自动化 PR 合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 在检查完成后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能开发过程中，你使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动完成剩余的 PR 工作，同时让开发者掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复审查意见，以及在需要时变基；随后，在存储库允许的情况下启用 GitHub 自动合并。

此前你都是亲自选择 **Merge pull request**。现在 Agent Merge 可以承担这项工作。启用 Agent Merge 前，请先检查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码已全部完成，接下来一起检查、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 激活 Agent Merge。
6. Copilot 会监视 PR 上的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限要求。如果流程受阻，请阅读报告的原因，并在存储库允许的情况下完成已检查的手动合并。

## 总结和后续步骤

你已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。你：

- 了解了 Agent Merge 及其如何自动化 PR 合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在检查完成后启用了 Agent Merge，并确认 PR 已合并。

接下来，你将[探索 Copilot CLI 中更多实用的斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
