---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 协作完成了多项任务，以改善用户体验并添加功能。你让 Copilot 为代码添加文档、为设计团队开发可供迭代的相关游戏功能，还让它实现无障碍功能，包括高对比度和浅色模式切换。现在，我们来查看代码更改；必要时，也可以向 Copilot 提供反馈，改进它的工作。

### 场景

如前所述，引入生成式人工智能（AI）并不会改变软件设计和开发运维（DevOps）的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。接下来，在为团队其他成员启用审查之前，先查看 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改。

## 安全性和 GitHub Copilot 云代理

Copilot 云代理会在没有监督的情况下异步执行任务，因此我们设置了一些安全限制，确保工作过程安全：

- Copilot 仅对存储库具有读取权限，对代码只有写入其工作分支的权限。
- 云代理在 GitHub Actions 中运行，并在独立的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认情况下，云代理对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括对模型上下文协议（MCP）服务器的访问。

## 审查生成的文档

先查看 GitHub Copilot 云代理创建的第一个拉取请求（PR），其中包含为代码添加的文档。你将使用 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，你可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云代理防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 的拉取请求，标题也可能更具体。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作。这可能需要几分钟，你可以稍作休息或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的 TSDoc 文档注释和其他文档。具体更改可能会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 规范和文件注释头。这些内容来自你在练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求检查列表的等待队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向交流。你可以像在拉取请求中提及团队成员一样，在评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新会话来处理请求。由于结果具有非确定性，我们无法提供固定的提示词。你可以要求 Copilot 更新以下内容：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TypeScript 文档（TSDoc）注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，请求 Copilot 修改生成的文档；像提及其他用户一样提及 **@copilot**。你可以使用上面的任一建议，也可以提出其他希望在代码库中看到的文档改进。
2. 选择 **View Session**，查看 Copilot 如何执行修改。留意 Copilot 会启动新会话来完成更新。
3. 选择 **Back to pull request**，返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成修改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

你可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿转换为准备审查状态，再合并到主分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在，返回 Copilot 为“在游戏详情页显示相关游戏”问题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **在游戏详情页显示相关游戏** 的 PR，标题也可能更具体。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入拉取请求检查列表的等待队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！与之前一样，你可以继续迭代并要求 Copilot 更新。例如，可以要求它调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令）。如前所述，可以在 **Conversation** 选项卡中添加评论来提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义代理实现的无障碍功能。这个 PR 应包含你在练习 3 中分配的高对比度模式，以及在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **为网站添加高对比度模式** 的 PR，标题也可能更具体。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现，重点关注：
   - 用于切换模式的用户界面（UI）组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式的层叠样式表（CSS）或样式更改；
   - 无障碍属性（无障碍富互联网应用（ARIA）标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求检查列表的等待队列。如果一切正常，项目检查就会通过。这可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式开关，确认功能正常且在重新加载页面后仍然生效。

留意无障碍自定义代理如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现任何无障碍问题或改进点，可以像处理前面的 PR 一样，在评论中提及 **@copilot** 并请求修改。

## 可选练习 — 继续委派任务

云代理适合处理真实的待办事项，让你能抽身处理其他工作。可以在自己的存储库中创建更多问题并分配给 Copilot，借此培养这种工作习惯。以下是一些建议：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云代理工作流。在这些课程中，你：

- **查看了存储库提供的自定义指令文件**，以便之后观察云代理如何遵循这些指令。
- **将问题分配给 Copilot 云代理**，并观察它如何设置环境、制定计划并异步执行任务。
- **创建并使用自定义代理**来实现无障碍功能，添加高对比度和浅色模式切换。
- **使用 Copilot 代理页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代云代理创建的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成云代理工作流。如果想继续探索，其他工作流可以让你从不同产品界面体验相关功能：

- 🖥️ **[VS Code 工作流](../../vscode/)** — 在集成开发环境（IDE）中直接使用 Copilot Chat 代理模式并集成 MCP。
- 💻 **[CLI 工作流](../../cli/)** — 在终端中交付两项经过审查的更改，使用命令行界面（CLI）中的 Plan 和 Autopilot 模式、代理技能、自定义质量保证（QA）代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云代理桥接功能。

你还可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率问题分配给云代理。
- 为其他领域创建自定义代理。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
