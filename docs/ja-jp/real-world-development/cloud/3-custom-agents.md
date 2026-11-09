---
title: "演習3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilotの[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた、専門的な人工知能（AI）アシスタントを作成できます。リポジトリの`.github/agents`フォルダーにあるMarkdownファイルでエージェントを定義すると、特定の種類の作業をより効果的に実行できるよう、焦点を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識をCopilotに提供できます。チームの専門知識を再利用できるエージェントとして明文化できます。たとえば、[Web Content Accessibility Guidelines（WCAG）][wcag]への準拠を徹底するアクセシビリティエージェント、安全なコーディングプラクティスに従うセキュリティエージェント、テストパターンの一貫性を保つテストエージェントなどです。

カスタムエージェントは、プロジェクト内の`.github/agents`フォルダーにあるMarkdownファイル、またはグローバルな`~/.copilot/agents`に定義します。各ファイルには、少なくとも`name`と`description`を含むYAML front matterと、エージェントの動作、専門知識、指示を定義するMarkdownプロンプトを記述します。

### カスタムエージェントとエージェントスキルの比較

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が一部重なるところがあります。どちらも主にMarkdownファイルで定義し、AIに作業方法を伝えます。両者を簡単に区別するなら、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業の一部としてスキルや別のエージェントを連携させます。このラボのアクセシビリティカスタムエージェントは、アクセシビリティガイドラインに沿ってサイトをレビュー、更新します。その作業の一部として、プルリクエストワークフローのスキルやテストの実行・管理を行うスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に、唯一の正解はありません。AIのほかの使い方と同様に、環境やシナリオに合わせて試し、反復的に改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントを使って次のことを確認します。

- カスタムエージェントの定義方法
- タスクをカスタムエージェントに割り当てる方法

## シナリオ

Tailspin Toysは、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにすることに取り組んでいます。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいと感じるユーザーがいることが、フィードバックからわかりました。このアクセシビリティの課題に対処するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、この機能をできるだけ早く実装します。カスタムエージェントを使って機能を生成します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。内容を確認し、Copilotへの指示を把握しましょう。

codespaceに戻り、アクセシビリティカスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md`を開きます。
2. `name`フィールドと`description`フィールドを含むYAML front matterを確認します。

> [!CAUTION]
> `name`と`description`を含むfront matterは、カスタムエージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルなWebサイトのコードを生成するときの主な役割
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScriptのコード例
   - よくある問題と間違い

## Issueを作成して割り当てる

ミッションコントロールは、環境内のエージェントを操作する中心的な場所です。Copilot cloud agentへのタスク割り当て、タスクの監視、追加の指示による誘導ができます。まず、ハイコントラストモードの作成をCopilotに割り当てます。

1. リポジトリを開きます。
2. **Issues**タブを選択します。
3. **New issue**を選択して、新しいIssueのダイアログを開きます。
4. **Blank issue**を選択して、新しいIssueを作成します。
5. **Title**に`Webサイトにハイコントラストモードを追加する`と入力します。
6. **Description**に次の内容を入力します。

    ```plaintext
    サイトにハイコントラストモードが必要です。ユーザーがハイコントラストを切り替えられるようにしてください。設定はブラウザーのローカルストレージに保存してください。
    ```

7. **Create**を選択してIssueを作成します。
8. 右側の**Assign to Copilot**を選択して、割り当てダイアログを開きます。
9. カスタムエージェントの一覧から**Accessibility agent**を選択します。

    ![カスタムエージェントとアクセシビリティエージェントが選択されたcloud agentの割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign**を選択します。
11. Copilotがバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、GitHub Copilotの[カスタムエージェント][custom-agents]について確認しました。カスタムエージェントは、特定のタスクや分野に合わせた専門的なAIアシスタントです。チームの専門知識や標準を再利用できるエージェントとして明文化し、Copilotが特定の種類の作業をより効果的に行えるようにします。

次の内容を確認しました。

- カスタムエージェントの定義方法
- タスクをカスタムエージェントに割り当てる方法

Copilotがハイコントラストモードの実装に取り組んでいる間に、[ミッションコントロールからエージェントセッションを監視して誘導します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを利用する準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを利用する準備][enterprise-custom-agents]

---

| [← 前のレッスン：GitHub Copilot cloud agent][previous-lesson] | [次のレッスン：エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
