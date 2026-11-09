---
title: "演習4 - カスタムエージェント"
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

## シナリオ

Tailspin Toysは、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにすることに取り組んでいます。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいと感じるユーザーがいることが、フィードバックからわかりました。このアクセシビリティの課題に対処するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、この機能をできるだけ早く実装します。カスタムエージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティカスタムエージェントを確認します。
- Copilot Chatでアクセシビリティエージェントを使って、ハイコントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。内容を確認し、Copilotへの指示を把握しましょう。

codespaceに戻り、ターミナルを開いて、アクセシビリティ作業用に`main`から新しいブランチを作成して切り替えます（演習3のフィルタリングプルリクエスト（PR）は別にしておきます）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md`を開きます。
2. `name`フィールドと`description`フィールドを含むYAML front matterを確認します。

> [!CAUTION]
> `name`と`description`を含むfront matterは、カスタムエージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルなWebサイトのコードを生成するときの主な役割
   - アクセシビリティのベストプラクティス
   - HyperText Markup Language（HTML）、Cascading Style Sheets（CSS）、JavaScriptのコード例
   - よくある問題と間違い

## Copilot Chatでカスタムエージェントを使う

VS Codeでは、`.github/agents`に定義されている各カスタムエージェントが、Copilot Chatビューの下部にあるエージェントのドロップダウンに表示されます。カスタムエージェントを選択すると、そのエージェントの指示とツールに合わせてチャットセッションを設定できます。

> [!TIP]
> **Copilot Chatを開く**
>
> 次の演習を始める前に、codespaceに戻り、Copilot Chatパネルを開き、**New Chat**を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chatビューのエージェントのドロップダウンで、**Agent**がまだ選択されていない場合は選択します。

   ![Chatビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. チャットビューの下部にあるエージェントのドロップダウンを選択します。現在のエージェントが表示されます。既定では**default**です。
3. 利用可能なエージェントの一覧から**Accessibility agent**を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

    ```plaintext
    サイトにハイコントラストモードを追加してください。ユーザーがハイコントラストを切り替えられるようにし、ブラウザーのローカルストレージを使って設定を保存し、ページを再読み込みしても保持してください。
    ```

5. Copilot Chatがコードベースを調べて変更を提案し、プロジェクトのファイルに編集を適用します。変更ごとに、ファイルパスと差分がチャットに表示されます。
6. 編集が適用されると、エージェントが変更した作業ファイル一式に合わせて**Files changed**の表示が更新されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilotはリポジトリに実際の変更を加え、Astroコンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、作業中のセッションを確認し、指示を追加します。

## まとめと次のステップ

このレッスンでは、GitHub Copilotの[カスタムエージェント][custom-agents]について確認しました。カスタムエージェントは、特定のタスクや分野に合わせた専門的なAIアシスタントです。チームの専門知識や標準を再利用できるエージェントとして明文化し、Copilotが特定の種類の作業をより効果的に行えるようにします。

次の内容を確認しました。

- カスタムエージェントの定義方法
- Copilot Chatのエージェントモードでカスタムエージェントを使う方法

次は[エージェントの作業を監視して誘導し][next-lesson]、変更を確認しながら、同じセッションにライトモードの切り替え機能を追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDEでカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Codeのカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilotのカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン：Playwright MCPサーバーで機能をテストする][previous-lesson] | [次のレッスン：エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
