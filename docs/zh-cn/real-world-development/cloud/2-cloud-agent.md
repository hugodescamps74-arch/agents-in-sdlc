---
title: "练习 2 - GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云代理可以自主执行代码更新、添加功能等任务。完成后，它会创建草稿拉取请求（PR），供开发人员审查。这样可以将繁琐任务交给代理，加快开发进度，让开发人员专注于更重要的工作。

本练习将介绍如何使用 Copilot 云代理：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 编写范围明确的问题；
- 将问题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上，代码中完全没有文档。团队希望首先为应用程序中所有导出的函数添加 TypeScript 文档（TSDoc）注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页显示相关游戏，也就是同一类别中的其他游戏，让用户可以继续浏览。团队目前不需要完善的实现，只需要一个可用于用户体验验收测试的版本。虽然这项工作目前受阻，但还有其他优先级更高的问题。

这些都是容易被降级处理的任务，适合交给 Copilot 云代理。它可以异步处理任务，让开发人员先处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot 云代理

[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。你可以通过多种方式与它协作，例如[将 GitHub 问题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在会话进行期间或完成后查看详情。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 编写范围明确的指令

GitHub Copilot 并非魔法工具，使用它并不能省去思考。即使看似简单的操作，深入分析后也会发现复杂性，因此要[认真界定云代理任务的范围][cloud-agent-best-practices]。把它当作人工智能（AI）结对编程伙伴：分阶段工作、学习、试验并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成自定义指令的练习，就已了解 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。将任务分配给云代理之前，先以只读方式查看存储库中已有的指令文件，稍后就能观察它们的影响。

在存储库的 GitHub 网页界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` 前置元数据，它会将这些指令限定在 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 问题分配给云代理时，观察它生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 规范和文件注释头。这些内容来自上述指令文件。后续练习审查拉取请求时也会再次查看。

## 为 Copilot 云代理设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保环境就绪。将任务分配给 Copilot 时也是如此，因为它的工作方式与软件工程师类似。

云代理使用 [GitHub Actions][github-actions] 执行任务。你可以创建[专用设置工作流][setup-workflow]，在 `.github/workflows/copilot-setup-steps.yml` 中配置，让工作流在云代理开始工作前运行，以便安装所需开发工具和依赖项。实验已预先配置该工作流，以确保流程顺畅并提供实践机会。工作流会为 Copilot 安装 Node.js、项目依赖项和端到端测试所需的浏览器二进制文件，并为单个 Astro 应用迁移和填充 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你在存储库的 "Actions" 选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为你的步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果设置步骤不克隆存储库，完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

这与其他 GitHub 工作流文件类似，但有几个关键点：

- 工作流只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器。它允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试工作流是否成功运行，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档的重要性，但大多数项目中的文档要么已经过时，要么根本不存在。这类技术债务往往得不到处理，降低了工作效率，也增加了维护代码库或让新开发人员加入团队的难度。Copilot 擅长创建文档，因此很适合把这项工作分配给 Copilot 云代理。它会在后台生成所需文档，你将在后续练习中审查结果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 创建问题。
5. 将 **Title** 设置为 `代码缺少文档`。
6. 将 **Description** 设置为：

    ```plaintext
    我们组织要求在有助于说明行为时，为函数和方法添加 TSDoc 文档注释。但近期更新没有遵循这项标准。我们需要更新现有代码，在文档注释能够澄清行为时添加注释。
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给问题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求，标题可能类似于 `[WIP]: 代码缺少文档`。如果列表中还没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中创建待办事项列表。它需要先创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表和 Copilot 即将完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理问题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 你可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停以思考并决定合适的计划，以及编写代码。

这项工作可能需要几分钟。Copilot 云代理的主要目标之一是异步执行任务，让你能够专注于其他工作。接下来，我们会利用这一点：先把另一项任务分配给云代理，再编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云代理的一大优势是可以分工协作：你处理一组任务时，它可以处理另一组任务。设计团队提出的相关游戏部分也许不需要很长时间，但这段时间仍可用于其他工作。现在就把任务分配给 Copilot 云代理。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `在游戏详情页显示相关游戏`。
6. 将 **Description** 设置为：

    ```markdown
    我们希望在每款游戏的详情页显示相关游戏，帮助用户发现更多游戏。设计团队希望探索用户体验并进行验收测试。要求如下：

   - 在 `src/lib/` 中添加数据访问辅助函数，返回与指定游戏属于同一类别的其他游戏，并排除指定游戏本身
   - 在游戏详情页添加“相关游戏”部分，并使用该辅助函数
   - 处理游戏没有相关游戏的情况
   - 为新辅助函数创建单元测试
   - 创建 PR 前，确保所有测试都通过
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给问题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

很快，你会在问题的第一条评论中看到一组 👀，表示 Copilot 已开始处理问题。

![Copilot 使用眼睛表情符号表示正在处理问题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！Copilot 云代理的工作方式类似软件工程师，因此你不需要持续监控它；等任务完成后再审查即可。接下来，我们将了解如何创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云代理][copilot-agents]。你可以将问题分配给云代理，让它异步处理。它可以帮助解决技术债务、开发新功能，或协助将代码迁移到其他框架。

本练习中，你：

- 自定义了代码生成环境；
- 确保了操作安全执行；
- 编写了范围明确的问题；
- 将问题分配给了 Copilot。

云代理正在后台处理任务，接下来你可以创建和使用自定义代理。[Copilot 云代理也支持使用模型上下文协议（MCP）服务器][cloud-agent-mcp]，并可使用之前练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云代理][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
