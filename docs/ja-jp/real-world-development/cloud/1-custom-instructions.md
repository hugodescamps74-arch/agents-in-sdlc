---
title: "演習1 - カスタム指示（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン：前提条件][previous-lesson] · [次のレッスン：Copilot cloud agent →][next-lesson]

人工知能（AI）を使ううえで、コンテキストは重要です。作業の進め方に決まりがある場合や、Copilotに知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilotに「何を」するかだけでなく、「どのように」行うかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準をCopilotに伝える方法を確認します。
- `.github/copilot-instructions.md`にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Codeやコマンドラインインターフェース（CLI）のハーネスとは異なり、この演習ではプロンプトの実行前後を比較しません。Copilot cloud agentはGitHub Issueを使って非同期に動作するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半でcloud agentが作成したプルリクエストを確認すると、指示ファイルの影響を確認できます。

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
> IDEで作業する場合、指示ファイルはCopilot Chatでのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
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

## リポジトリ全体に適用する標準を追加する

この演習で編集するのは、ここで追加するプロジェクト全体のルールだけです。コードにTSDocのドキュメントコメントとファイルレベルのコメントヘッダーを追加するように指定します。後の演習でIssueを割り当てると、cloud agentがこのルールを参照します。

編集する前に、作業用のブランチを作成します（cloud agentはIssueの対象ブランチにある指示を読み取りますが、ここでは編集内容を整理してコミットします）。

1. codespaceのターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md`はCopilotにプロジェクトレベルの情報を伝えるためのファイルです。コーディング標準を記載して、コードの提案を改善しましょう。

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

## 指示の更新をコミット、プッシュ、マージする

cloud agentは、Issueが対象とするブランチの指示ファイルを読み取ります。次の演習でCopilotにIssueを割り当てると、`main`からブランチが作成されます。そのため、cloud agentが指示を参照できるように、変更を`main`に取り込む必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.comで`custom-instructions-cloud`から`main`へのプルリクエストを作成し、マージします。次の演習でcloud agentが作業するときに、これらの指示を読み取れるようになります。

> [!TIP]
> このワークショップでは`main`に直接作業してもかまいません。その場合はブランチ作成を省略して、`main`に直接コミットできます。ブランチを使う手順は、実際のプロジェクトでの作業方法を体験できるように含めています。

## まとめと次のステップ

このプロジェクトの指示ファイルからCopilotがコンテキストを取得する方法を確認し、`.github/copilot-instructions.md`にリポジトリ全体に適用する標準を追加しました。このハーネスの残りの演習でcloud agentが作成するプルリクエストを確認すると、この標準が適用されている様子を確認できます。

次は[最初のIssueをCopilot cloud agentに割り当てます][next-lesson]。

## リソース

- [GitHub Copilotのカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot向けのカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
