---
title: "レッスン 8 - 機能のプルリクエスト (PR) を作成してマージする"
description: "フィルタリング、指示、スキルの更新、品質保証 (QA) プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめて確認し、プルリクエストを開く段階です。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージのプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントをいくつかの PR に分けます。このワークショップでは作業を簡略化するため、フィルタリングと品質確認の一連の作業を 1 つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージまでのプロセスを自動化する方法を確認します。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の開発では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、マージの許可を開発者の管理下に置いたまま、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージ可能な状態にするための残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題に対処します。たとえば、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなどを行い、リポジトリで許可されている場合は GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せることもできます。Agent Merge を有効にする前に、作業を確認し、マージできる状態であると判断してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体を確認して PR を作成し、その後の作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 選択肢の一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションの進行状況を監視します。すべて成功すると、マージが実行されます。
7. 「PR #14 was squash-merged successfully.」のようなメッセージが Copilot から表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリの設定、権限不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合はレビュー済みのマージを手動で完了してください。

## まとめと次のステップ

コードの生成、テストと検証、そしてプルリクエストのプロセスなど、開発作業のいくつかを自動化しました。具体的には、次のことを行いました。

- Agent Merge と、マージまでのプロセスを自動化する方法を確認しました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使える[その他の Copilot CLI スラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
