---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメントの標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成系の人工知能 (AI) を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、求めるコードの内容だけでなく、コードの構成方法も説明できます。フィルタリング機能を作成したので、Copilot が使った指示を確認し、ドキュメントの標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスで対象を限定した指示ファイルがエージェントに適用される仕組みを確認します。
- コーディング標準が守られるように、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のような内容です。

- コメントでは、コードの内容を言い換えるのではなく、目的や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を TypeScript 用の TSDoc / JavaScript 用の JSDoc で記述します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントでは、`Props` の契約を文書化し、関連するコードの変更時にはコメントを最新の状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使って、Copilot がここで示した開発手法に沿って作業できるよう、必要な情報を提供します。

## 指示ファイル

カスタム指示では、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝えられます。コーディング規約、ライブラリ、記述したいコメントの形式などを指定することで、より適切な提案やコードスニペットを得やすくなります。リポジトリ全体に適用する指示ファイルも、特定のファイル種別に適用する指示ファイルも作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイダンスを提供します。

> [!NOTE]
> その他の指示形式やサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

変更を加える前に、スタータープロジェクトに用意された指示ファイルを確認して、既存の内容と変更による影響を把握しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく)、`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドでは、指示の適用対象を決める glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクトファイルへのリンクを確認します。このように大きな指示セットを小さく再利用可能なファイルに分け、参考例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、追加の指示が必要です。新しく生成する TypeScript に TSDoc コメントを追加するよう、基本の `copilot-instructions.md` ファイルを変更します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** という見出しのセクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - 新しい TypeScript には、ドキュメント用の TSDoc コメントを含めてください。
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。機能のコンテキストを保ったまま新しいガイダンスを適用するため、ファイルを保存した後に会話を終了して再開します。

1. フィルタリングの会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートから `game-filters-cli` ブランチにいることを確認し、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   メッセージが表示された場合は、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   指示とコードのガイダンスを更新しました。生成したコードをこのガイダンスに合わせて更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確かめます。

## まとめと次のステップ

Copilot CLI が指示ファイルのコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパスで対象を限定した指示ファイルがエージェントに適用される仕組みを確認しました。
- 指示ファイルを更新して、機能に新しい標準を適用しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な `quality-checks` スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
