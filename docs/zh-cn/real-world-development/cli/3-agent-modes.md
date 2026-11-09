---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确认方案，使用 Autopilot 根据问题构建筛选功能，并在 Interactive 模式下检查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能。但较大的更改需要更完善的流程。GitHub Copilot CLI 能够配合组织现有的工作流，帮助我们以正确的方式构建正确的功能。接下来的几课将介绍典型的代理驱动开发流程：从问题着手创建新功能、确保代码有效并按预期运行，最终成功合并到项目中。

> [!NOTE]
> 继续开发此功能时，你将使用同一个对话和分支。通常，不同文件类型的工作会使用不同分支或拉取请求（PR）；本练习会简化这一流程，让你专注于核心概念。

本课将介绍如何：

- 从 GitHub 问题启动新的 Copilot 对话。
- 在 Plan 模式下定义要求。
- 使用 Autopilot 模式实现新功能。
- 检查代码。
- 在通过 Codespaces 转发端口打开的浏览器中手动验证功能。

继续开发此功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理，并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩充，访客需要按类别和发行商筛选游戏。待办事项中的问题描述了该功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式解决这些问题，然后授权 Autopilot 在明确范围内实现功能。

## 背景

将人工智能（AI）编码代理引入开发流程，并不会改变基本原则。事实上，这些原则变得更加重要！大多数开发者遵循的流程大致如下：

1. 从已提交的问题着手，了解需要完成的工作。
2. 制定实现计划。
3. 编写并检查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多与上述流程相似。

遵循这种标准流程，可以确保 AI 生成的代码满足要求，并像手写代码一样经过审查。

## 对话模式

**对话模式**用于控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与代理协作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试并不断迭代，直至任务完成。

先从 Plan 模式开始，检查计划，然后切换到 Autopilot 来实现。

## 从问题着手

开始筛选功能开发前，返回 Codespace，确保存储库和终端已准备就绪。

1. 返回 Codespace。如果 Codespace 已停止，请先重启。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有问题。
8. 使用箭头键突出显示标题为 **Allow users to filter games by category and publisher** 的问题。
9. 按 <kbd>C</kbd> 将问题添加到提示中，并返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的问题或拉取请求（PR）添加到上下文中。

## 规划筛选功能

在实现功能或执行任务前，规划有助于确定实现方式。对于复杂任务，花些时间规划通常很有帮助。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的问题引用后输入以下提示：

   ```plaintext
   为实现此功能制定计划。
   ```

Copilot 会开始制定计划！它会先探索项目，再确定最佳方案。

3. 在此过程中，Copilot 可能会询问筛选功能应如何工作。根据你的偏好回答，没有唯一的正确答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动检查计划。
6. 如果计划中的任何内容与你的决定不符，请要求 Copilot 修改。

## 批准 Autopilot

计划编写并检查完毕后，就可以开始实现了。现在使用 Autopilot 让 Copilot 执行。

Autopilot 会让 Copilot 持续迭代，直至它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所用版本中类似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先检查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 检查并验证实现

代码生成后，和其他代码一样，需要先检查再合并。接下来检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与问题描述以及规划时做出的决定进行比较。
4. 检查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目运行的检查结果，并要求 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但能否正常运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中试用筛选功能。告诉我 URL，并让服务器继续运行。
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别、发行商筛选，以及按计划约定的组合条件筛选。
4. 确认重置和无结果时的行为符合问题描述及你的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并检查功能。本课中，你：

- 从 GitHub 问题启动了新的 Copilot 对话。
- 在 Plan 模式下定义了要求。
- 使用 Autopilot 模式实现了新功能。
- 检查了代码。
- 在通过 Codespaces 转发端口打开的浏览器中手动验证了功能。

接下来，使用自定义说明深入了解如何生成符合文档规范的代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
