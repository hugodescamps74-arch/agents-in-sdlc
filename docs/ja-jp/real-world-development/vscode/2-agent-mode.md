---
title: "演習2 - Copilot Agent Modeで新しい機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な更新でも、通常は複数のファイルを変更し、テストなどを実行する必要があります。開発者は通常、必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグし、更新漏れのファイルを見つけて、さらに変更を加えます。作業は次々と続きます。

Copilot Agent Modeは、このようなときに役立ちます。

Copilot Agent Modeは、統合開発環境（IDE）内でより自律的に作業できるように設計されています。開発者と同じように、まず既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Modeを使ってサイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードとcloud agentは異なる体験を提供します。エージェントモードはIDE内で作業し、すばやいフィードバックとやりとりができます。Cloud agentはチームの一員のように非同期で作業し、Issueとプルリクエストを通じて開発者とやりとりします。

この演習では、次の方法を学びます。

- Copilot Agent Modeがプロジェクトを調べ、関連ファイルを見つけ、複数のファイルにまたがる変更を行う方法
- GitHub Copilot Agent Modeがユーザーインターフェース（UI）とデータレイヤーにわたって新機能を実装する方法
- コードベースにマージする前にCopilot Agent Modeが生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや出版社で絞り込めるようにします。前の演習で出版社ヘルパーを追加しました。次はCopilot Agent Modeを使い、残りのデータレイヤー、UI、テストの作業を完了します。

## Tailspin ToysのWebサイトを実行する

変更を加える前に、Tailspin ToysのWebサイトを開いて現在の機能を確認します。

このWebサイトは、開発者向けのテーマを持つボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示できます。このサイトは単一のAstroアプリで、ビルド時にページを静的なHyperText Markup Language（HTML）として生成します。ページはDrizzle ORM（オブジェクトリレーショナルマッピング）を通じてfrontmatterからローカルのSQLiteデータベースを直接参照します。別のバックエンドApplication Programming Interface（API）やクライアントサイドのユーザーインターフェース（UI）フレームワークはありません。再利用可能なデータアクセスヘルパーは`src/lib/`にあり、インタラクティブな機能は標準のDocument Object Model（DOM）APIを使う小規模なAstroの`<script>`で追加します。

### Webサイトを起動する

Webサイトを簡単に実行できるように、単一のAstro開発サーバーを起動する`npm run dev`タスクが用意されています。GitHub Codespaceで次の手順を実行します。

1. 前の演習で`main`に戻っているはずなので、codespaceに戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または<kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、Codespace内に新しいターミナルウィンドウを開きます。
3. フィルタリング作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行してWebサイトを起動します。

   ```bash
   npm run dev
   ```

   Astro開発サーバーの準備ができると、次のようなURLを示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321`をブラウザーウィンドウで開くか尋ねるダイアログが表示された場合は、**x**を選択して閉じます。

5. ターミナルでAstroサーバーのアドレス`http://localhost:4321`を選択します。Macでは<kbd>Command</kbd>キー、Windows/Linuxでは<kbd>Ctrl</kbd>キーを押しながら選択してください。

> [!NOTE]
> codespaceでは、Codespaceのターミナルにあるlocalhost URLを選択すると、自動的に`https://<your-codespace-name>-4321.app.github.dev/`に転送されます。これは、Webサーバーを実行するcodespaceへのプライベートトンネルです。

### Webサイトを確認する

Webサイトが起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホームページ**：ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**：ゲームを選択すると、タイトル、説明、出版社、カテゴリなどの情報を掲載した詳細ページが表示されます。

## Copilotでバックログを確認する

> [!TIP]
> **Copilot Chatを開く**
>
> 次の演習を始める前に、codespaceに戻り、Copilot Chatパネルを開き、**New Chat**を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Webサイトの初期実装は動作しますが、新しい機能を追加したいと考えています。まず、バックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、Issueのバックログが自動的に作成されています。GitHub Copilotに項目を表示するよう依頼します。

1. Chatビューのエージェントのドロップダウンから**Agent**を選択します。**Agent**エージェントは、複数のファイルにわたる変更の計画と実装、ターミナルコマンドの実行、ツールの呼び出しを自律的に行います。

   ![Chatビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から**Claude Sonnet 4.5**を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップの作成にClaude Sonnet 4.5を使用したため、提案の一貫性を高め、スムーズに進められるように手順に含めています。ただし、大規模言語モデル（LLM）の出力は確率的なため、ワークショップで示す内容と異なる提案が表示されることがあります。これは正常な動作であり、想定されています。

> [!NOTE]
> 大規模言語モデル（LLM）の出力は確率的なため、Copilotが別のModel Context Protocol（MCP）コマンドを使うことがあります。その場合もタスクを完了できるはずです。

3. 次のプロンプトをCopilotに送信して、Issueのバックログを確認します。

   ```plaintext
   GitHubリポジトリのバックログ項目を表示してください。ユーザーに最も役立つ項目を基準に、優先順位付けを手伝ってください。
   ```

4. **Continue**を選択して、すべてのIssueを一覧表示するコマンドを実行します。
5. 生成されたIssue一覧を確認します。

Copilotが、ユーザーに最も役立つと思われる項目を基準に優先順位を付けたことを確認できます。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilotにコンテキストを伝えるために使う指示ファイルを確認します。Webサイトに機能を追加する方法に関するコンテキストを記載した[user interface（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)ファイルを使います。

1. codespaceで`.github/instructions/ui.instructions.md`を開きます。
2. 機能追加の進め方に関する全般的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzleなど、ファイルの種類ごとの指示ファイルへのリンク

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。これにより、再利用や、複雑な指示の小さなファイルへの分割が容易になり、コード生成の例も提示できます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の3つを個別に更新する必要があります。

- データレイヤー（`src/lib/`）の残りのフィルタリングロジックを追加または調整する
- フィルタリング動作のテストを追加または更新する
- ゲーム一覧ページにフィルタリングUIを追加する

さらに、コードベースにマージする前にテストを実行し、成功することを確認する必要があります。Copilot Agent Modeにこれらの作業を任せましょう。

1. 現在のCopilotの会話を続けるか、**New Chat**を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui**の順に選択して、指示ファイルを追加します。

   ![UI指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chatビューのエージェントのドロップダウンで、引き続き**Agent**が選択されていることを確認します。

   ![Chatビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き**Claude Sonnet 4.5**であることを確認します。
5. バックログにある関連Issueの要件に基づいて機能を実装するよう、次のプロンプトでCopilotに依頼します。

   ```plaintext
   関連するGitHub Issueの要件に基づいて、出版社とカテゴリによるフィルタリング機能をサイトに追加してください。前の演習で作成した出版社ヘルパーを維持し、必要に応じて調整しながら、残りのデータレイヤー、UI、テストを完成させてください。完了する前にすべてのテストが成功することを確認してください。サーバーはすでに実行中なので、起動する必要はありません。
   ```

6. Copilotがプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストを探した後、ファイルを変更してテストを実行します。

   ![Copilotがプロジェクトのファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilotはプロジェクトの調査、ファイルの変更、テストの実行など、複数のタスクを実行します。タスクやコードベースの複雑さによっては、数分かかることがあります。その間、エディターに**Keep**と**Undo**ボタンが表示される場合があります。Copilotが作業中にこれらを選択する必要はありません。完了すると、すべての変更に対して**Keep**または**Undo**が表示されます。

7. Copilotに求められたら、**Continue**を選択してテストを実行します。

   ![テストを実行してよいか確認するCopilot Chatペインのダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilotは、タスクが完了してエラーを検出しなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Modeとの完了したチャットセッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilotなどの人工知能（AI）ツールが生成したコードは、必ずレビューしてください。

10. Webサイトを起動したブラウザーに戻り、新しい機能を確認します。
11. 動作とコードを確認したら、Copilot Chatウィンドウで**Keep**を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Modeを使ってTailspin ToysのWebサイトに新しい機能を追加する方法を確認しました。次の方法を学びました。

- GitHub Copilot Agent ModeでUIとデータレイヤーにわたる新機能を実装する
- Copilot Agent Modeでプロジェクトを調べ、関連ファイルを見つけ、複数のファイルにまたがる変更を行う
- コードベースにマージする前にCopilot Agent Modeが生成した変更とテストをレビューする

次は[Playwright Model Context Protocol（MCP）サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 — ページ分割を実装する

ゲーム一覧が増えると、ページ分割が必要になります。この演習で学んだことを使い、Copilotにページ分割を実装するよう依頼してください。プロンプトを作成するときは、次の点を検討します。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ユーザーがページサイズを選択できるようにするか、固定にするかなど、ページ分割の方法を検討します。
- 希望する実装に必要なガイダンスをCopilotに伝えるプロンプトを作成します。
- Copilotに変更を依頼し、コンテキストを追加しながら反復的に調整することがあります。これはCopilotを使う通常のワークフローです。

## リソース

- [Copilotのask、edit、agentモード：機能と使い分け][choose-mode]
- [VS Codeのエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
