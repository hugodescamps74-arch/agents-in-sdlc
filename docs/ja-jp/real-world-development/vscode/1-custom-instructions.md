---
title: "演習1 - カスタム指示（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン：前提条件][previous-lesson] · [次のレッスン：エージェントモード →][next-lesson]

人工知能（AI）を使ううえで、コンテキストは重要です。作業の進め方に決まりがある場合や、Copilotに知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilotに特定のコンテキストを伝える方法はいくつかあります。その中でも重要なのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成の指針を伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準をCopilotに伝える方法を確認します。
- 現在の指示を使って、フィルタリングに使う最初のデータスライス（出版社ヘルパー）を生成します。
- `.github/copilot-instructions.md`にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成されたコードに新しい標準が反映される様子を確認します。
- 指示の更新とフィルタリング用データスライスをコミットして`main`にプッシュし、次の演習でCopilotが更新後のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードは、設定した標準から逸脱することがあります。Copilotの出力は決定的ではありません。この演習では、指示の更新前後で出力が一字一句同じになることではなく、動作傾向の変化を確認します。

## 指示ファイル

### シナリオ

優れた開発会社であるTailspin Toysには、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには必ずユニットテストを用意します。
- ユーザーインターフェース（UI）はダークモードにし、現代的な見た目にします。
- TSDocのドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使ってCopilotに適切な情報を伝え、ここで取り上げたプラクティスに沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストや好みをCopilotに伝えられます。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できるため、より適切な提案やコードスニペットを得るのに役立ちます。リポジトリ全体に適用する指示も、特定の種類のファイルに適用する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md`は、リポジトリに対する**すべての**リクエストでCopilotに送られる単一の指示ファイルです。技術スタック、開発対象の概要、ベストプラクティス、全般的なガイダンスなど、Copilot ChatやCLIへのほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md`ファイルは、特定のタスクやファイルの種類に合わせたガイドラインを記載するために作成できます。TypeScriptやAstroなどの言語、UIコンポーネントの作成、ユニットテストの作成などを対象にできます。

> [!NOTE]
> 統合開発環境（IDE）で作業する場合、指示ファイルはCopilot Chatでのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilotコマンドラインインターフェース（CLI）、Copilot cloud agentは、リポジトリレベルの指示ファイルと、`applyTo` front matterで対象を指定した`*.instructions.md`ファイルの両方を使ってコードを生成します。
>
> Copilotは、AGENTS.mdやCLAUDE.mdなど、ほかの規格に基づく指示ファイルにも対応しています。[対応する指示ファイル][custom-instructions-support]をご覧ください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法をすべて説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトには代表的な方法が示されています。基本的には、次のようにします。

- `copilot-instructions.md`には、開発対象の説明、プロジェクト構成、全体に適用されるコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md`ファイルには、ファイルの種類（ユニットテスト、Astroコンポーネント、データレイヤーなど）や特定のタスクに関する指示を記載します。
- 自然な言葉を使います。ガイダンスは明確にし、コードのよい例と悪い例を示します。

指示ファイルの作成方法はひとつではありません。AIの使い方と同様に、試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilotを使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトの指示ファイルを確認すると、[UIの更新][ui-instructions]や[Astro][astro-instructions]など、さまざまなタスク向けのファイルが見つかります。
>
> Copilotに指示ファイルを生成させることもできます。操作方法は利用する環境によって異なります（たとえば、VS Codeでは**Configure Chat → Generate Agent Instructions**、Copilot CLIでは`/init`）。該当する演習で、必要な場合に説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めたリポジトリ、[awesome-copilot][awesome-copilot]をご覧ください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。中心となる`copilot-instructions.md`が1つあり、さまざまなタスク向けの`*.instructions.md`ファイルもあります。エディターまたはGitHubのWeb UIで開いてください。

1. `.github/copilot-instructions.md`を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure**などのセクションを確認します。**Code standards**内の**GitHub Actions Workflows**に関する項目も確認してください。これらの指示は、Copilotとのあらゆるやりとりに適用されます。
3. `.github/instructions`フォルダーを開いて内容を確認します。Astroファイル、Drizzleデータレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md`を開きます。先頭の`applyTo`フィールドは、リポジトリルートを基準としたglobを指定し、指示の適用対象を決めています。この例では、`**/*.test.ts`に一致するTypeScriptテストファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. 最後に`.github/instructions/drizzle.instructions.md`を開き、末尾までスクロールします。`unit-tests.instructions.md`などの別の指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。こうしたリンクにより、大きな指示を再利用しやすい小さなファイルに分割し、コード生成の例をCopilotに示せます（ここにあるパスはリポジトリルートではなく、指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md`の**Code formatting requirements**セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDocのドキュメントコメントとファイルヘッダーコメントのルールを追加します。

## 変更用のブランチを作成する

ここではベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使ったcodespaceに戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または<kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前にCopilot Chatを使う

> [!TIP]
> **Copilot Chatを開く**
>
> 次の演習を始める前に、codespaceに戻り、Copilot Chatパネルを開き、**New Chat**を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新した後、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilotに必要なコンテキストだけを与えます。
2. `src/lib/publishers.ts`を開き、ヘルパーの作成場所をCopilotに示します。
3. Chatビューのエージェントのドロップダウンから**Agent**を選択し、Copilotがファイルを変更できるようにします。

   ![Chatビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts に、各出版社の名前とIDを返すデータアクセスヘルパーを作成または更新してください。ファイルの変更を適用してください。
   ```

5. Copilotがプロジェクトを調べ、ヘルパーファイルとテストなどにコードの更新を適用します。
6. 提案されたヘルパーが、最初の引数として`db`クライアントを受け取り、出版社の型付き配列を返す型付き関数であることを確認します。これは`src/lib/*.ts`に適用される`.github/instructions/drizzle.instructions.md`のデータレイヤー規約によるものです。
7. 提案されたコードにTSDocのドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilotの出力は確率的なため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。指示の更新後に、動作がより一貫することを確認するのが目的です。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md`はCopilotにプロジェクトレベルの情報を伝えるためのファイルです。コーディング標準を記載して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md`を再度開きます。
2. **Code formatting requirements**セクションを探します。27行目付近にあるはずです。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーにドキュメントコメントが含まれていません。
3. 既存の標準の直後に、次のMarkdown行を追加します。これにより、ファイルヘッダーコメントとTSDocのドキュメントコメントをCopilotに指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md`を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル（`.github/copilot-instructions.md`）にも、特定の言語、ファイルの種類、タスク向けの`*.instructions.md`にも作成できます。ここで追加したドキュメントコメントのルールなど、プロジェクト全体に適用する標準にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chatに戻り、**New Chat**を選択して会話をクリアします。
2. `src/lib/publishers.ts`を再度選択し、Copilotが適切なファイルに注目できるようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts に、各出版社の名前とIDを返すデータアクセスヘルパーを作成または更新してください。ファイルの変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のようなTSDocコメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilotに新しいプロジェクト標準を伝え、次の演習で基礎として使うコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのリポジトリアセットと同じように、ソース管理で管理します。指示とブランチをリポジトリにコミットしてプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または<kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで次のコマンドを実行し、指示の更新と、ヘルパーの基礎コードとしてCopilotが変更したすべてのファイルをステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドで、ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準のIssueに関連付けてプルリクエスト（PR）を作成します。手動で作成することもできますが、GitHubツールを使えるCopilotに依頼します。CopilotにIssueを探させ、Issueをクローズするプルリクエストを作成してマージします。

> [!NOTE]
> CopilotにGitHub上の操作を依頼するのは、この演習が初めてです。プロジェクトテンプレートではGitHubのツールがワークスペースに接続済みのため、セットアップは不要です。Copilotが初めてツールを使うときに、VS CodeでGitHubへのサインインを求められることがあります。画面の指示に従って許可してください。後の演習では、この接続の仕組みであるModel Context Protocolについて学びます。

1. codespace内でCopilot Chatを開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）または<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）を押してCopilot Chatビューを開き、エージェント選択で**Agent**が選択されていることを確認します。
3. 指示ファイルの更新に関するIssueを探し、現在のブランチからプルリクエストを作成するようにCopilotに依頼します。プルリクエストには、指示の更新と、新しい出版社ヘルパーの基礎コードの両方を記載するようにします。

   ```
   指示ファイルの更新に関するIssueを探してください。現在のブランチから新しいプルリクエストを作成し、そのIssueをクローズすることを明記してください。また、今後のフィルタリング作業に備えて出版社ヘルパーの基礎コードも追加したことを記載してください。
   ```

4. CopilotがIssueを探し、プルリクエストを作成します。
5. CopilotがGitHub上で操作することを**Allow**するよう求められたら、コマンドを確認して適切な場合に**Allow**を選択します。
6. プルリクエストが作成されたら、次のプロンプトでPRをマージし、ローカルのブランチを`main`に戻すようCopilotに依頼します。

   ```
   プルリクエストをmainにマージしてください。その後、ローカルでmainに戻り、最新のコードをプルして最新状態にしてください。
   ```

7. CopilotがGitHub上で操作したり、シェルコマンドを実行したりすることを**Allow**するよう求められたら、コマンドを確認して適切な場合に**Allow**を選択します。

GitHub Copilotを使ってプルリクエストを作成し、マージしました。

## まとめと次のステップ

このプロジェクトの指示ファイルからCopilotがコンテキストを取得する方法を確認し、VS CodeのCopilot Chatを使って次の作業を行いました。

- 既存の指示を使ってコード生成プロンプトを送信し、Copilotの出力を確認しました。
- `.github/copilot-instructions.md`にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 指示の更新とヘルパーの基礎コードを`main`にコミットし、次の演習で作業を引き継げるようにしました。

次は[エージェントモード][next-lesson]で指示を活用し、Copilotにコードベース全体にわたる新機能を追加させます。

## リソース

- [GitHub Copilotのカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot向けのカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilotの個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
