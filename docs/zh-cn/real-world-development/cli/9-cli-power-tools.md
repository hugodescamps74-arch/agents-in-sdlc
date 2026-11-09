---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "了解用于管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 包含许多斜杠命令。它们提供高级功能、幕后信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来了解其他实用命令。

本课将介绍如何：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 了解可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行处理、工作树和委派任务给云端代理的可选命令。

## 场景

核心 CLI 工作流已完成。现在来了解其他功能，包括管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云端代理][about-cloud-agent]。

## 了解 Copilot CLI 上下文

处理规模较大或较复杂的任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以通过斜杠命令自行查看或压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意模型、当前令牌用量，以及上下文在系统说明、工具、消息和剩余空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 当上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。想自行决定压缩时机时，可使用 `/compact`。切换到无关任务并需要全新对话时，可使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发者也有不同偏好。Copilot CLI 支持列出并选择所需模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型及其用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作和分享经验有助于每个人提升 AI 工具的使用水平。`/share` 命令可以将会话导出为 Markdown 或超文本标记语言（HTML）文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看你所用版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标，例如 `/share file`，将会话导出为本地 Markdown 文件。
3. 将导出内容发送给他人或发布之前，先检查内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展处理规模或委派任务

核心工作流已完成。Copilot CLI 还为更大规模的任务提供了以下命令：

- `/fleet` 可以将相互独立的子任务分配给多个子代理，并行运行。
- `/worktree` 可以为单独的任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot 云端代理，由它异步处理并可能创建拉取请求。

这些命令是可选的，因为它们可能创建额外工作树或触发远程工作。尝试前，请先开始一个范围明确的新任务，并按照常规工作流检查结果。如果想进一步了解异步代理工作，可以继续学习[云端代理研讨会][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 的斜杠命令，可以配置工具、共享会话并了解其幕后运行情况。本课中，你：

- 使用 `/context` 和 `/compact` 了解了 Copilot 如何管理对话上下文。
- 使用 `/model` 了解了可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行处理、工作树和委派任务给云端代理的可选命令。

Copilot CLI 还有更多斜杠命令值得探索！接下来，通过[回顾所学内容][next-lesson]并了解后续学习资源，为本次学习之旅收尾。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云端代理][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
