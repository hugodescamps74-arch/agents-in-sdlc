---
title: "演習3 - Playwright MCPサーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン：エージェントモード][previous-lesson] · [次のレッスン：カスタムエージェント →][next-lesson]

エージェントモードでフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認しましょう。アプリを手動で操作する代わりに、**Playwright MCPサーバー**を接続し、Copilotに実際のブラウザーで機能をテストさせます。その後、ブランチを公開してPRを作成します。

この演習では、次のことを行います。

- Model Context Protocol（MCP）とは何か、MCPサーバーでCopilotにツールを追加する方法を学びます。
- Playwright MCPサーバーをワークスペースに追加します。
- CopilotにPlaywright MCPサーバーを使わせ、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol（MCP）とは

エージェントモードは、エディターの外部に接続できると、さらに力を発揮します。Copilotが外部のツールやサービスと通信するための標準的な方法がModel Context Protocol（MCP）です。

![エージェントモードの内部構造と、コンテキスト、大規模言語モデル（LLM）、MCPサーバーやVS Code拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)を使うと、人工知能（AI）エージェントが外部のツールやサービスと通信できます。MCPを使うことで、AIエージェントは外部のツールやサービスとリアルタイムにやりとりできます。これにより、リソースを使って最新情報にアクセスしたり、ツールを使って開発者の代わりに操作したりできます。

こうしたツールやリソースには、AIエージェントと外部のツールやサービスをつなぐMCPサーバーを通じてアクセスします。MCPサーバーは、既存のアプリケーションプログラミングインターフェース（API）やnpmパッケージなどのローカルツールを含む外部ツールとの通信を管理します。各MCPサーバーは、AIエージェントがアクセスできるツールとリソースのセットを提供します。

よく使われているMCPサーバーの例を挙げます。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：GitHubリポジトリを管理するApplication Programming Interface（API）にアクセスできます。新しいリポジトリの作成、既存のリポジトリの更新、Issueやプルリクエストの管理などを人工知能（AI）エージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：Playwrightを使ったブラウザーの自動操作機能を提供します。Webページへの移動、フォームへの入力、ボタンの選択などをAIエージェントが実行できます。

ほかにもさまざまなツールやリソースを提供するMCPサーバーがあります。GitHubがホストする[MCPレジストリ](https://github.com/mcp)で、見つけやすさを高めたり、エコシステムに貢献したりできます。

> [!CAUTION]
> セキュリティの観点から、MCPサーバーはほかの依存関係と同様に扱ってください。利用する前に、ソースコードを慎重に確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できるMCPサーバーだけを使い、機密リソースや操作へのアクセスを許可するときは注意してください。

## MCPの構成を確認する

`.vscode/mcp.json`ファイルで、このVS Codeワークスペースで利用するMCPサーバーを設定します。

1. codespaceで`.vscode/mcp.json`を開きます。
2. `github`サーバーはすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この`github`エントリはプロジェクトテンプレートに含まれています。そのため、前の演習でCopilotがIssueのバックログを確認できました。[リモートGitHub MCPサーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS CodeはHTTP経由で接続し、Copilotがツールを初めて使うときにGitHubで認証します。

## Playwright MCPサーバーを追加する

次に、2つ目のサーバーを追加します。[Playwright MCPサーバー][playwright-mcp-server]を使うと、Copilotが操作できるブラウザーを利用できます。これを使って機能をテストします。

1. `.vscode/mcp.json`に`github`と並べて`playwright`エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの`github`サーバーとは異なり、`playwright`は**ローカル**サーバーです。VS Codeが`npx @playwright/mcp@latest`を実行して起動します。`--headless`フラグを指定すると、Playwrightはウィンドウを表示せずにブラウザーを実行します。デスクトップ画面がないcodespace内では、この設定が必要です。

> [!NOTE]
> Tailspin ToysプロジェクトはエンドツーエンドテストにPlaywrightを使っているため、通常、Playwrightに必要なブラウザーはすでにインストールされています。Copilotがブラウザーが見つからないと報告した場合は、`npx playwright install chromium`を実行させてからもう一度試してください。

## サーバーを起動して信頼する

VS Codeは必要に応じてMCPサーバーを起動します。Copilotがサーバーのツールを初めて必要としたときに、VS Codeがサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json`で`playwright`エントリの上に表示される**Start**アクションを選択し、サーバーを起動します。構成が正しいことを確認できます。
2. VS Codeからサーバーを信頼するか確認されたら、構成を確認して、サーバーを起動できるように信頼します。

手動で起動するのは任意です。省略した場合は、次の手順でCopilotがツールを必要としたときにVS Codeがサーバーを起動し、信頼の確認を表示します。

## フィルタリング機能をテストする

[Playwright MCPサーバー][playwright-mcp-server]を使うと、Copilotが実際のブラウザーを操作できます。開発者がアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用して、結果を確認し、見つけた内容をまとめます。会話を続けたまま、機能が期待どおりに動作するかをすばやく確認できます。

Playwright MCPサーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。つまり、エージェントはボタン、リンク、リスト項目など、ラベル付きの構造化された要素に基づいて動作します。支援技術と同じように情報を扱うため、簡単な機能テストがアクセシビリティの基本確認にもなります。

サーバーを接続し、アプリを実行した状態で、次のプロンプトをCopilotに送信してフィルタリング機能をテストします。

```text
Playwright MCPサーバーを使い、実行中のアプリ（http://localhost:4321）をブラウザーで開いて、新しいゲームのフィルタリング機能を確認してください。

1. ゲームページに移動し、一覧に表示されているゲームの数を確認します。
2. カテゴリフィルターを適用し、一覧にそのカテゴリのゲームだけが表示されることを確認します。
3. フィルターを解除してから出版社フィルターを適用し、その出版社のゲームだけが表示されることを確認します。
4. カテゴリと出版社のフィルターを組み合わせ、両方の条件が結果に反映されることを確認します。

各手順で確認した内容を報告し、期待どおりに動作しなかった点があれば指摘してください。
```

CopilotはPlaywright MCPサーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。Issueの受け入れ条件に照らして報告を確認してください。問題がある場合は、追加の質問をするか、プルリクエストを作成する前にコードの修正を依頼します。

> [!NOTE]
> このテストには、`http://localhost:4321`でアプリが実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動してください。CopilotがPlaywright MCPサーバーを初めて使うとき、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium`を実行させてからもう一度試します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームのレビュー用にプルリクエスト（PR）を作成します。まず、`filtering-vscode`ブランチを公開します。

1. codespaceの**Source Control**パネルに移動し、Copilotが加えた変更を確認します。
2. **+**アイコンを選択して、変更をステージングします。
3. **Sparkle**ボタンを選択して、コミットメッセージを生成します。

   ![変更内容を示すSource Controlパネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish**を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.comやGitHub command-line interface（CLI）など、プルリクエストの作成方法はいくつかあります。GitHub Copilotを使っているので、PRの作成も依頼しましょう。Copilotは関連するIssueを探し、Issueに関連付けてPRを作成できます。

1. Copilot Chatパネルに移動し、**New Chat**を選択して新しいセッションを始めます。
2. エージェントのドロップダウンから**Agent**を選択します。これでCopilotがGitHubツールを使えるようになります。
3. 次のプロンプトでPRの作成を依頼します。

   ```plaintext
   リポジトリでカテゴリと出版社によるフィルタリングに関するIssueを探してください。現在のブランチ用に新しいプルリクエストを作成し、正しいIssueに関連付けてください。
   ```

4. 情報の取得や操作を行うために必要に応じて**Continue**を選択します。GitHubツールを初めて使うとき、GitHubでの認証を求められることがあります。画面の指示に従って許可します。
5. CopilotがIssueを検索し、該当するものを見つけてPRを作成することを確認します。
6. Copilotが生成したリンクを選択してプルリクエストを確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol（MCP）とは何か、MCPサーバーでCopilotにツールを追加する方法を学びました。
- Playwright MCPサーバーをワークスペースに追加しました。
- MCPサーバーを使って、機能を公開する前にブラウザーで手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

MCPサーバーを使って機能を**テスト**しましたが、MCPはCopilotを拡張する方法のひとつです。次は[カスタムエージェント][next-lesson]を作成し、アクセシビリティレビューなど、対象を絞ったタスクを効率化します。

## リソース

- [MCPとは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS CodeのMCPサーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
