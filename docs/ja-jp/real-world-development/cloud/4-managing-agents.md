---
title: "演習4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、ユーザーエクスペリエンスの改善と機能追加を目的に、Copilot cloud agentに3つのタスクを依頼しました。cloud agentは非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.comの[agentsページ][agents-page]など、cloud agentに割り当てたタスクを管理するためのツールがいくつかあります。このミッションコントロールでは、オープンなプルリクエスト（PR）を伴うすべてのエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を追加したりできます。

このレッスンでは、次のことを行います。

- agentsページでcloud agentのタスクを監視します。
- 進行中のセッションに追加の機能を依頼して誘導します。

## シナリオ

ハイコントラストモードの作成をエージェントに割り当てた後、チームはライトモードも追加するよい機会だと気づきました。サイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが合理的です。エージェントの作業を誘導し、ハイコントラストモードに加えてライトモードも追加してもらいます。

## Copilot cloud agentのタスクを確認する

Copilot cloud agentに割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agentsページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインの両方にあるタスク一覧を確認します。Copilotに割り当てた次のようなタスクが表示されます。
    - コードベースのドキュメントを更新する
    - ゲームを変更するアプリケーションプログラミングインターフェース（API）を生成する
    - Webサイトにハイコントラストモードを追加する
3. 実行中のタスクを選択します。Copilotが行った次のような作業を確認します。
    - リポジトリからコードをチェックアウトする
    - Copilotが作業する環境を作成する
    - Model Context Protocol（MCP）サーバーをセットアップする
    - タスクを完了するためのさまざまな手順を実行する

> [!NOTE]
> 表示される具体的な手順は、Copilotの作業状態や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト（PR）ペインも確認します。ここでは、PRと変更されたファイルを確認し、作業状況をさらに監視できます。

## Cloud agentを誘導する

実行中のタスクを確認したので、ハイコントラストモードの作業中にライトモードも含めるよう、Copilotに依頼します。

1. ハイコントラストモードを追加するセッションを選択します。タイトルはCopilotが付けた名前や作業状態によって異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working**ダイアログに次のプロンプトを入力します。

    ```plaintext
    ハイコントラストモードの作業に加えて、ライトモードも追加してください。ユーザーが希望する表示モードを選べるように、ライトモード用の切り替え機能も用意してください。
    ```

    ![agentsページで、Steer active session while Copilot is workingダイアログが強調表示されたcloud agentのタスク画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd>を押してプロンプトを送信します。
5. Copilotがプロンプトを受け取り、作業計画に追加する様子を確認します。

## Copilotに作業を任せる

これまでと同様に、Copilotは更新されたタスクに取り組みます。メッセージを送信した時点で作業中の手順を完了した後、新しい依頼を作業の流れに追加します。

今回も数分かかるため、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agentのタスクを監視し、誘導する中心的な場所であるCopilot agentsページを確認しました。このミッションコントロールを使うと、進行中または完了したタスクの追跡、実行中の作業の確認、進行中のタスクへの指示の追加ができます。

次の内容を確認しました。

- ミッションコントロールとagentsページでcloud agentのタスクを監視しました。
- 進行中のセッションに指示を追加して、機能を依頼しました。

Copilotがアクセシビリティ機能を完成させている間に、[Copilotが作成したプルリクエストを反復的に改善します][next-lesson]。

## リソース

- [Copilot agentsページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン：カスタムエージェント][previous-lesson] | [次のレッスン：Copilotの作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
