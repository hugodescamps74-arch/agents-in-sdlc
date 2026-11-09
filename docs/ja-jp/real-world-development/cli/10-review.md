---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連の開発ワークフローに取り組みました。具体的には、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されていたフィルタリングの Issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始め、Plan モードで進め方を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright 用の Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) のカスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証根拠を評価しました。
- フィルタリングの変更全体を確認し、フィルタリングの PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` から作成したそれぞれのブランチで、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と、評価がないことを示す状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザーでの確認、MCP を使った Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルは、ユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果に加えて、要件とテストカバレッジもレビューし、PR の前に結果をまとめました。

追加するテストは、実際に不足しているカバレッジを補うものである必要があります。新しいテストが不要という QA の結果が正しい場合もあります。マージを許可する前に、コードと検証の根拠を確認し、変更後は影響を受ける検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業内容を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクをまとめ、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った **モードとモデル** を選びます。実装前に進め方を検討するには **Plan**、対象を絞った変更に継続して関わるには **Interactive**、範囲が明確なタスクを任せるには **Autopilot** を使います。定型的な編集には応答の速いモデルを選び、複雑な作業にはより高性能なモデルを選びます。人工知能 (AI) ツールでは、コンテキストもインフラストラクチャと同じくらい重要です。

何を、なぜ、どのように作りたいかを明確に伝えると、結果が大きく変わります。

## さらに確認する

主要なワークフローを確認しました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題について話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを独立した環境で作業します。
- `/delegate` で Copilot cloud agent に作業を依頼します。

## 次のステップ

ツールの使い方を上達させるには、使い続けるのが一番です。本番コード、趣味のコード、何年も作ろうと思いながら手をつけていなかった小さなアプリなどで活用してください。チームと知見を共有し、互いに学びましょう。そして、いつものようにドキュメントを確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
