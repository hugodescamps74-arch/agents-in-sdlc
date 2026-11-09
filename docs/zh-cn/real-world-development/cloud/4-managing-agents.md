---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几次练习中，你让 Copilot 云代理处理了三项任务，重点是改善用户体验并添加功能。云代理虽然能够异步自主工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给云代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心，你可以查看所有拥有未关闭拉取请求（PR）的代理任务，了解已执行的操作，甚至引导正在进行的会话。

本练习将帮助你：

- 浏览代理页面，监控云代理任务；
- 引导正在进行的会话，要求添加其他功能。

## 场景

将高对比度模式任务分配给代理后，团队意识到也可以同时添加浅色模式。既然正在更新网站样式并添加切换功能，同时实现浅色模式也很合理。你将引导代理，确保它在添加高对比度模式的同时也实现浅色模式。

## 查看 Copilot 云代理任务

先查看分配给 Copilot 云代理的任务当前状态。

1. 打开 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
    - 更新代码文档；
    - 生成用于修改游戏的应用程序编程接口（API）；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码；
    - 为 Copilot 创建工作环境；
    - 设置模型上下文协议（MCP）服务器；
    - 执行完成所分配任务所需的其他步骤。

> [!NOTE]
> 具体显示的步骤取决于 Copilot 的工作状态和采用的方法。

4. 留意右侧显示的拉取请求窗格。你可以通过该窗格查看 PR 及已更改的文件，以便进一步监控。

## 引导云代理

现在你已了解活动任务的状态，接下来要求 Copilot 在实现高对比度模式的同时添加浅色模式。

1. 选择用于添加高对比度模式的会话。确切标题会因 Copilot 采用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。出现类似下方的消息时，就表示已开始处理任务。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示词：

    ```plaintext
    我们正在实现高对比度模式，也请同时添加浅色模式。浅色模式也应提供一个开关，让用户选择所需的显示模式。
    ```

    ![代理页面中突出显示“Steer active session while Copilot is working”对话框的任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 留意 Copilot 如何确认收到提示词并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。完成发送消息时正在执行的具体步骤后，它会将新请求纳入工作流程。

这同样需要几分钟。你可以利用这段时间回顾目前学到和体验的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，也就是监控和引导 GitHub Copilot 云代理任务的中心位置。你可以通过任务控制中心跟踪所有正在进行和已完成的任务、查看正在执行的工作，甚至重新引导正在进行的任务，以调整范围或补充指导。

本练习中，你：

- 使用任务控制中心和代理页面监控了云代理任务；
- 重新引导了正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来，你将[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
