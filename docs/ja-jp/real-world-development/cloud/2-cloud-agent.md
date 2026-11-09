---
title: "演習2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン：カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残り続ける機能リクエストなどがあります。GitHub Copilotのcloud agentは、コードの更新や機能の追加などを自律的に行います。作業が完了すると、人間の開発者が確認できるドラフトのプルリクエスト（PR）を作成します。手間のかかるタスクを任せることで、開発プロセスを加速し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agentを使って次のことを行います。

- コード生成用の環境をカスタマイズします。
- 操作が安全に行われるようにします。
- 明確に範囲を定めたIssueの重要性を確認します。
- IssueをCopilotに割り当てます。

## シナリオ

Tailspin Toysには対応したい技術的負債があります。最初のサイトを作成した委託先は、ドキュメントを十分に整備していませんでした。実際、ドキュメントがほとんどありません。まず、アプリケーションのすべてのエクスポート関数にTSDocのドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する別のゲームを表示し、閲覧を続けられるようにします。現時点では完成度の高い実装は必要なく、ユーザーエクスペリエンス（UX）の受け入れテストに使えるものを求めています。この作業は現在のブロッカーですが、ほかに優先度の高いIssueがあります。

どちらも優先順位が下がりやすいタスクの例で、Copilot cloud agentに任せるのに適しています。Copilot cloud agentに非同期で作業させれば、開発者は別の作業に集中し、あとから戻って作業内容を確認できます。

## GitHub Copilot cloud agentの概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同じようにバックグラウンドで作業できます。人間の開発者に仕事を依頼するときと同じく、[GitHub IssueをCopilotに割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法などがあります。割り当てると、Copilotは進捗を追跡するドラフトプルリクエストを作成し、環境を準備してタスクに取りかかります。作業中も完了後もCopilotのセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲を定めた指示の重要性

GitHub Copilotに魔法はありません。考える手間を省けるわけではありません。一見簡単そうな作業でも、詳しく調べると複雑なことがあります。[Copilot cloud agentに依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。人工知能（AI）ペアプログラマーとして扱い、段階を踏んで学び、試し、調整します。生成AIを追加しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示とCopilotへの影響を確認しました。カスタム指示を実際に試した場合は、`.github/copilot-instructions.md`の効果をすでに確認しています。まだの場合は、ここで簡単に確認してください。Copilot cloud agentに作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその影響を見分けられるようにします。

GitHubのWebユーザーインターフェース（UI）または起動中のcodespaceで、次のファイルを開きます。

- `.github/copilot-instructions.md`：**Code standards**セクション、特にTypeScriptの規約とTSDocのドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`：`applyTo` front matterを確認します。この指示は`**/*.test.ts`ファイルに適用されます。

次のセクションで`Code lacks documentation` Issueをcloud agentに割り当てたら、作成されたプルリクエストでTSDocのドキュメントコメント、TypeScriptの規約、コメントヘッダーを確認してください。これらは指示ファイルによるものです。後の演習でPRをレビューするときに、改めて取り上げます。

## Copilot cloud agentの開発環境を設定する

誰がコードを書く場合でも、通常は環境の準備とセットアップスクリプトの実行が必要です。タスクをCopilotに割り当てる場合も同様です。Copilotもソフトウェアエンジニアのように作業します。

Cloud agentは、作業環境として[GitHub Actions][github-actions]を使います。`.github/workflows/copilot-setup-steps.yml`に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に実行する処理をカスタマイズできます。必要な開発ツールや依存関係を利用できるようになります。このラボの流れを支援し、学習機会を提供するために、事前に設定されています。単一のAstroアプリ向けに、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを適用したSQLiteデータベースをCopilotが利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilotの操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read`権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後にCopilotが自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド／アプリのセットアップ - Node.js（アプリ全体はAstro + Drizzle/libSQLです）
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

      # ビルドとテストでデータを使えるように、ローカルのSQLiteデータベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかのGitHubワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps`という単一のジョブが含まれています。このジョブは、Copilotがプルリクエストの作業を始める前にGitHub Actionsで実行されます。
- `workflow_dispatch`トリガーにより、リポジトリの**Actions**タブからワークフローを手動実行できます。Copilotの作業を待たずにワークフローが正常に実行されるかテストするのに役立ちます。

## ドキュメントを追加する

ドキュメントが重要だと理解していても、多くのプロジェクトでは情報が古かったり、そもそもなかったりします。こうした技術的負債は対応されないままになり、開発の効率を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilotはドキュメント作成が得意なため、Copilot cloud agentに割り当てるのに適したIssueです。バックグラウンドで必要なドキュメントを作成し、後の演習で作業を確認します。

1. 新しいブラウザータブでgithub.comのリポジトリを開きます。
2. **Issues**タブを選択します。
3. **New issue**を選択して、新しいIssueのダイアログを開きます。
4. **Blank issue**を選択して、新しいIssueを作成します。
5. **Title**に`Code lacks documentation`と入力します。
6. **Description**に次の内容を入力します。

    ```plaintext
    組織では、必要に応じて関数やメソッドにTSDocのドキュメントコメントを含めることが求められています。しかし、最近の更新ではこの標準に従っていません。動作を明確にする箇所にドキュメントコメントが含まれるよう、既存のコードを更新する必要があります。
    ```

7. **Create**を選択してIssueを作成します。
8. 右側の**Assign to Copilot**を選択して、割り当てダイアログを開きます。

  ![CopilotをIssueに割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign**を選択します。
10. **Pull Requests**タブを選択します。
11. 新しく作成されたプルリクエスト（PR）を開きます。タイトルは`[WIP]: Code lacks documentation`のようになります。PRが一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、CopilotがToDoリストを作成したことを確認します。

> [!NOTE]
> CopilotのToDoリストがPRに表示されるまで、数分かかることがあります。Copilotは環境を作成し（前述のワークフローを実行します）、プロジェクトを分析して、問題への最適な取り組み方を検討しています。

13. リストを確認し、Copilotが実行するタスクを把握します。
14. プルリクエストのタイムラインを下にスクロールし、CopilotがIssueの作業を開始したことを示す更新を確認します。
15. **View session**ボタンを選択します。

  ![Copilotのセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するために、ウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールして、Copilotが問題を解決する様子を確認します。コードを調べて状態を把握する様子、計画を考えて適切な方法を決める様子、コードを作成する様子を確認できます。

作業には数分かかることがあります。Copilot cloud agentの主な目的は、作業を非同期に行い、開発者が別のタスクに集中できるようにすることです。この機能を活用するため、もうひとつのタスクをCopilot cloud agentに割り当ててから、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agentの利点のひとつは作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業に長い時間がかからないとしても、その時間を別の作業に使えます。Copilot cloud agentに割り当てましょう。

1. github.comのリポジトリに戻ります。
2. **Issues**タブを選択します。
3. **New issue**を選択して、新しいIssueのダイアログを開きます。
4. **Blank issue**を選択して、空のテンプレートを使います。
5. **Title**に`ゲーム詳細ページに関連ゲームを表示する`と入力します。
6. **Description**に次の内容を入力します。

    ```markdown
    ゲーム詳細ページに関連ゲームを表示し、ほかのゲームを見つけやすくします。デザインチームはユーザーエクスペリエンス（UX）を確認し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/`に、指定したゲームを除き、同じカテゴリに属するほかのゲームを返すデータアクセスヘルパーを追加する
   - そのヘルパーを使う「関連ゲーム」セクションをゲーム詳細ページに表示する
   - 関連ゲームがない場合にも対応する
   - 新しいヘルパーのユニットテストを作成する
   - PRを作成する前に、すべてのテストが成功することを確認する
   ```

7. **Create**を選択してIssueを作成します。
8. 右側の**Assign to Copilot**を選択して、割り当てダイアログを開きます。

  ![CopilotをIssueに割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign**を選択します。

まもなく、Issueの最初のコメントに👀が表示され、Copilotが作業を始めたことがわかります。

![IssueでCopilotが作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilotは2つ目の依頼にも取り組んでいます。Copilot cloud agentはソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後に確認できます。それでは、カスタムエージェントの作成と利用に進みましょう。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]について確認しました。cloud agentにIssueを割り当てると、非同期に作業させることができます。技術的負債への対応、新機能の作成、別のフレームワークからのコード移行などに利用できます。

次の内容を確認しました。

- コード生成用の環境をカスタマイズする方法
- 操作が安全に行われるようにする方法
- 明確に範囲を定めたIssueの重要性
- IssueをCopilotに割り当てる方法

cloud agentがバックグラウンドで作業している間に、カスタムエージェントの作成と利用に進みます。[Copilot cloud agentではModel Context Protocol（MCP）サーバーも利用でき][cloud-agent-mcp]、前のモジュールで扱ったカスタム指示にも対応しています。

## リソース

- [Copilot cloud agentについて][copilot-agents]
- [GitHub IssueをCopilotに割り当てる][assign-issue]
- [Copilot cloud agentのセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン：カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
