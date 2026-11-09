---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，通过 Codespaces 转发端口在浏览器中检查更改，并通过第一个拉取请求合并更改。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以开始对项目进行第一次更改了。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示评分。你将让代理显示评分、检查更改，并通过第一个拉取请求合并更改。

在本课中，你将：

- 在功能分支上启动专注于单项任务的 Copilot 对话。
- 请求代理对项目进行小幅更改。
- 使用 `/diff` 检查更改。
- 运行应用，并在通过 Codespaces 转发端口打开的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已经显示了评分。不过，主页上的游戏卡片只显示标题、类别、发行商和说明。作为热身练习，你将让代理在每张卡片上显示现有评分。这项小而独立的更改很适合首次使用。

## 对话的组成

**对话**是你与 Copilot CLI 一起完成任务的工作区。与 Copilot 应用不同，常规 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可供日后继续使用；文件和分支仍是磁盘上的常规 Git 状态。

对话中有三项内容：你的提示和代理的回复、代理探索和编辑文件时使用的工具，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

启动新对话，开始实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，请运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保启动新会话。
5. 使用以下提示请求更改：

   ```plaintext
   在游戏列表页的每张游戏卡片上显示 starRating（满分为 5 星）。如果评分为 null，则显示“No rating yet”。保持卡片布局不变，添加测试并运行相关检查。
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件，并创建所需代码。现在，你已使用 Copilot CLI 添加了一项新功能！

## 检查差异

所有人工智能（AI）生成的更改都应在合并前检查，即使是小改动也不例外。现在就在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查每个已更改的文件。
2. 确认游戏卡片在有评分时显示数值评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 检查 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 检查完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的行为具有概率性，而非确定性，因此生成的代码可能与你的不同。请检查实际行为，不要期待某一种固定实现。

## 检查更改

光看代码并不能证明功能有效。现在让 Copilot 启动网站，以便在通过 Codespaces 转发端口打开的浏览器中检查更新后的用户界面（UI）。

1. 请求 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中检查星级评分更改。告诉我 URL，并让服务器继续运行。
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为五颗星的评分。
4. 返回 Copilot，并要求它停止启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 创建并合并第一个拉取请求

功能已完成！现在创建拉取请求（PR），将新代码合并到项目中。

1. 要求默认代理提交更改：

   ```plaintext
   使用合适的提交消息提交已检查的星级评分更改。
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 检查更改的文件和检查结果。
5. 确认可以合并后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你使用 GitHub Copilot CLI 完成了第一个更改。具体来说，你：

- 在功能分支上启动了专注于单项任务的 Copilot 对话。
- 指示代理修改了游戏卡片。
- 使用 `/diff` 检查了更改。
- 运行应用，并在通过 Codespaces 转发端口打开的浏览器中确认了星级评分。
- 创建并合并了第一个拉取请求。

接下来，你将从筛选问题着手，使用 Plan 和 Autopilot 模式构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
