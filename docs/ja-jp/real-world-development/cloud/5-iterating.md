---
title: "演習5：GitHub Copilotの作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン：エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善と機能追加を目的に、GitHub Copilotにいくつかのタスクを依頼しました。コードへのドキュメント追加、デザインチームが試せる関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装です。コード変更を確認し、必要に応じてCopilotにフィードバックを送り、作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、人工知能（AI）を追加してもソフトウェア設計とDevOpsの基本は変わりません。生成されたコードをレビューし、通常のDevOpsプロセスに沿って作業します。チーム全体にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について、GitHub Copilotの提案を確認しましょう。

## GitHub Copilot cloud agentのセキュリティ

Copilot cloud agentは、監視を受けずに非同期でタスクを実行するため、安全性を確保するための制約があります。

- Copilotがリポジトリに対して持つ権限は読み取り専用で、コードの作業に使うブランチに限って書き込み権限を持ちます。
- Cloud agentはGitHub Actions内で実行され、作業用に分離された一時的な環境を作成します。
- GitHub Actionsワークフローを実行するには、人間による承認が必要です。
- Model Context Protocol（MCP）サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加するためにGitHub Copilot cloud agentが作成した最初のプルリクエスト（PR）を確認します。GitHub.comの標準的なPR画面を使います。

> [!NOTE]
> プルリクエスト（PR）を確認すると、GitHub Copilotがファイアウォールでブロックされたという警告が表示されることがあります。Copilotは、外部MCPサーバーへの呼び出しなど、既定では外部リソースへのアクセスが制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agentのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.comのリポジトリに戻ります。
2. **Pull Requests**を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation**またはそれに近いタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilotがまだ作業中の場合、プルリクエストのタイトルに**[WIP]**が含まれます。その場合は、Copilotが作業を終えるまで待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed**タブを選択して変更内容を確認します。

    ![Files changedタブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。新しいTSDocのドキュメントコメントや、その他のドキュメントが含まれています。変更内容は状況によって異なります。

    変更を確認しながら、TSDocのドキュメントコメント、TypeScriptの規約、コメントヘッダーを探します。これらは演習2の最初に確認したカスタム指示ファイルによるものです。

6. 更新内容に問題がないことを確認したら、**Conversation**タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows**を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されたことを確認します。すべて順調であれば、単一のAstroアプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilotに変更を依頼する

プルリクエストでCopilotと作業するとき、一方的に結果を受け取るだけではありません。チームのほかのメンバーと同じように、プルリクエストのコメントやコードへのインラインコメントでCopilotにメンションできます。Copilotはコメントを確認し、変更に対応する新しいセッションを開始します。結果は予測できないため、必ず使える定型文はありません。Copilotに依頼する変更の例を次に示します。

- 各コードファイルの先頭に、ファイルの役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScriptファイルとAstroファイルにTSDocのドキュメントコメントを追加する。
- Astroアプリの構成を説明するREADMEを作成する。

1. 上の例のいずれか、またはコードベースに必要な別の提案を使って、生成されたドキュメントの変更を依頼するコメントを追加します。ほかのユーザーと同じように**@copilot**をメンションします。
2. **View Session**を選択して、Copilotが作業する様子を確認します。Copilotが変更用の新しいセッションを開始します。
3. **Back to pull request**を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻るボタン](../../../_images/ex7-back-to-pr.png)

4. Copilotが変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed**タブを選択して、変更内容を確認します。

満足するまで、繰り返し改善できます。変更に納得できたら、PRをドラフトからレビュー可能な状態に変更して、`main`ブランチにマージします。

![PRをレビュー可能な状態に変更するボタン](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次は、ゲーム詳細ページに関連ゲームを表示するIssueに対してCopilotが作成したPRを確認します。

1. GitHub.comのリポジトリに戻ります。
2. **Pull Requests**タブを選択します。
3. **ゲーム詳細ページに関連ゲームを表示する**またはそれに近いタイトルのPRを選択します。
4. **Files changed**タブを選択して、生成されたコードを確認します。
5. 更新内容を確認して問題がなければ、**Conversation**タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows**を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが登録されたことを確認します。すべて順調であれば、単一のAstroアプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。
9. **任意：**Codespaceでこのブランチに切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespaceに移動してターミナルを開き、次のコマンドを実行します（`<branch-name>`をCopilotが作成したブランチ名（例：**copilot/fix-8**）に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilotが関連ゲーム機能を作成しました。先ほどと同じように、Copilot cloud agentに変更を依頼して反復的に改善できます。たとえば、表示する関連ゲームの数を変更したり、コメントヘッダーとTSDocのドキュメントコメントを追加したりできます（カスタム指示を更新する**前**にこのIssueを割り当てたことを思い出してください）。前と同じく、**Conversation**タブで新しいコメントを追加すると、Copilotが確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントを使って実装した機能を確認します。このPRには、演習3で依頼したハイコントラストモードと、演習4でミッションコントロールから依頼したライトモードの両方が含まれているはずです。

1. GitHub.comのリポジトリに戻ります。
2. **Pull Requests**タブを選択します。
3. **Webサイトにハイコントラストモードを追加する**またはそれに近いタイトルのPRを選択します。

> [!NOTE]
> Copilotがまだ作業中の場合、プルリクエストのタイトルに**[WIP]**が含まれます。その場合は、Copilotが作業を完了するまで数分待ちます。

4. **Files changed**タブを選択して、生成されたコードを確認します。
5. 実装を確認し、特に次の点を確認します。
   - モードを切り替えるユーザーインターフェース（UI）コンポーネント
   - ユーザー設定を保持するためのローカルストレージの使用
   - ハイコントラストモードとライトモードのCascading Style Sheets（CSS）またはスタイルの変更
   - アクセシビリティ属性（Accessible Rich Internet Applications（ARIA）ラベル、キーボード操作など）
   - モード切り替えを管理するJavaScript/TypeScriptコード

6. 更新内容を確認して問題がなければ、**Conversation**タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows**を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されたことを確認します。すべて順調であれば、単一のAstroアプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。
10. **任意：**Codespaceでこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespaceに移動してターミナルを開き、次のコマンドを実行します（`<branch-name>`をCopilotが作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティカスタムエージェントが、Copilotによる機能実装をアクセシビリティのベストプラクティスに沿って誘導したことを確認できます。アクセシビリティ上の問題や改善点があれば、前のPRと同じようにコメントで**@copilot**にメンションし、変更を依頼できます。

## 任意の演習 — さらに作業を委任する

Cloud agentは、実際のバックログ項目を任せて別の作業に取り組むときに力を発揮します。習慣にするため、リポジトリにIssueをいくつか追加し、Copilotに割り当ててみてください。次のようなタスクが考えられます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agentハーネスを完了しました。これらのレッスンでは、次のことを行いました。

- このリポジトリに用意されたカスタム指示ファイルを確認し、後でcloud agentの出力への影響を確認できるようにしました。
- IssueをCopilot cloud agentに割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して利用し、ハイコントラストモードとライトモードの切り替えを追加しました。
- Copilot agentsページをミッションコントロールとして使い、アクセシビリティセッションを監視して途中で誘導しました。
- cloud agentのプルリクエストを確認して反復的に改善し、**@copilot**に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agentハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで今回の内容を応用できます。

- 🖥️ **[VS Codeハーネス](../../vscode/)** — 統合開発環境（IDE）からCopilot ChatのエージェントモードとMCP連携を確認します。
- 💻 **[CLIハーネス](../../cli/)** — PlanモードとAutopilotモード、エージェントスキル、カスタムQAエージェント、Playwright MCP、Agent Merge、任意の`/delegate`によるcloud agentとの連携を使い、ターミナルからレビュー済みの変更を2つ作成します。

ご自身のリポジトリでは、次の作業を試してみてください。

- リファクタリングやテストカバレッジのIssueをcloud agentに割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに`copilot-setup-steps.yml`を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agentsについて][copilot-agents]
- [GitHub IssueをCopilotに割り当てる][assign-issue]
- [Copilot cloud agentのセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agentのファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン：エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
