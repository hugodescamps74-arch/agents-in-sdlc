---
slug: real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)を使うと、GitHub Copilotがクラウド上で非同期に作業できます。GitHubで作業を割り当てると、cloud agentはバックグラウンドでリポジトリを調べ、変更を加え、プルリクエストを作成します。その間、開発者は別の作業を続けられます。

この演習では、まずcloud agentが従うカスタム指示を追加し、GitHub Issueを割り当てて実装を任せます。続いて、カスタムエージェントを確認して利用し、エージェントダッシュボードからセッションを監視・誘導します。最後に、作成されたプルリクエスト（PR）を確認し、結果を反復的に改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリとcodespaceを作成します |
| [1. カスタム指示][ex1] | コンテキスト | cloud agentが従うカスタム指示を追加します |
| [2. Cloud Agent][ex2] | 非同期エージェント | IssueをCopilot cloud agentに割り当てます |
| [3. カスタムエージェント][ex3] | 特化型エージェント | カスタムエージェントを確認して利用します |
| [4. エージェントの管理][ex4] | 監視 | エージェントセッションを監視して誘導します |
| [5. 反復的な改善][ex5] | レビュー | PRを確認し、Copilotの作業を反復的に改善して次の手順を選びます |

## 前提条件

このワークショップに参加する前に、次の条件を満たしていることを確認してください。

- [ ] 有効な**Copilot Student、Pro、Pro+、Business、または Enterprise**プランを利用できるGitHubアカウント
- [ ] アカウントまたは組織でCopilot cloud agentが有効になっていること

> [!TIP]
> 有料プランを利用していない場合でも、認定を受けた学生は[GitHub Education][callout-student-plan-education]を通じてGitHub Copilotを無料で利用できます。**Copilot Student**プランには、このワークショップで使うエージェント、Model Context Protocol（MCP）、コードレビュー、Copilot CLIが含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agentは**Copilot Student、Pro、Pro+、Business、Enterprise**で利用できます。Copilot Freeを除くすべてのプランが対象です。Copilot BusinessまたはEnterpriseでは、管理者が組織で有効にする必要があります。

> [!NOTE]
> Model Context Protocol（MCP）はこのハーネスでは扱いません。CopilotでMCPサーバーを使う方法については、[CLIハーネス](../cli/)または[VS Codeハーネス](../vscode/)をご覧ください。

## はじめに

**[演習0：前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
