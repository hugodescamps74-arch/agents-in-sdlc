---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace，安装 Copilot CLI，探索项目，并找到预置的筛选问题。
- 添加了星级评分，在通过 Codespaces 转发端口打开的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选问题着手，在 Plan 模式下确定方案，在 Autopilot 模式下实现，并在 Interactive 模式下检查。
- 使用自定义说明引导代理，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义代理，检查要求、测试覆盖率、技能运行结果和浏览器验证证据。
- 检查完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 交付成果

本研讨会包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选功能和质量工作流：**实现筛选功能，更新说明并将其应用于功能代码，自定义 `quality-checks` 报告，创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一个对话和分支。为简化练习，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 在浏览器中探索，采用了多种方式检查代码。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 汇总这些结果，以及对要求和测试覆盖率的检查，然后再创建 PR。

新增测试应解决实际缺口；如果 QA 检查后确认无需添加测试，也可能是正确结果。授权合并前，请检查代码和证据；更改后，请刷新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。本研讨会中，你更新了说明、自定义了技能、创建了 QA 配置文件，并配置了 MCP 服务器。可以在多个对话中重复使用这些自定义项，并根据团队需求调整。说明用于设定标准，技能用于描述可重复执行的任务，自定义代理定义专用角色，MCP 服务器则连接外部工具。请检查实际更改和工具结果，而不只是代理的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在实现前考虑方案，使用 **Interactive** 参与专注于单项任务的更改，使用 **Autopilot** 处理范围明确的任务。常规编辑可选择速度更快的模型，复杂任务则可选择能力更强的模型。

上下文依然与基础设施同样重要。清楚说明要构建的内容、原因和方式，会显著影响输出。

## 进一步探索

你已完成核心工作流。还可以了解一些 CLI 功能：

- `/review`：请求代码审查代理分析更改。
- `/rubber-duck`：讨论问题并获取另一种看法。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离单独的任务。
- `/delegate`：将任务发送给 Copilot 云端代理。

## 后续步骤

提升工具使用能力的最佳方法就是持续实践！可将其用于生产代码、个人项目，或用于构建那个多年来一直想做却未动手的小应用。与团队分享人工智能（AI）工具的使用经验，也向团队成员学习。别忘了继续探索文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他环境，可以了解 [VS Code 研讨会][vscode]、[GitHub Copilot 应用研讨会][app]或 [Copilot 云端代理研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
