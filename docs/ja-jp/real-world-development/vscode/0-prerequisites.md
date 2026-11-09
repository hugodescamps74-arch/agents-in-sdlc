---
title: "演習0：前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Codeの演習を始める前に、必要な環境を準備します。Tailspin Toysリポジトリをご自身のリポジトリとして複製し、作業用の[codespace][codespaces]を起動します。また、エディターでGitHub Copilot Chatが動作することを確認します。

## ラボ用リポジトリのセットアップ

コードを作成するためのリポジトリを用意するには、[テンプレート][template-repository]から新しいリポジトリを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれ、各演習で使用します。

1. 新しいブラウザーウィンドウで[Tailspin Toysテンプレート][tailspin-template]を開きます。
2. ラボリポジトリのページで**Use this template**を選択し、**Create a new repository**を選択して、ご自身のリポジトリを作成します。

    ![テンプレートを使用するボタン](../../../_images/ex0-use-template.png)

3. GitHubまたはMicrosoftが主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilotを利用できる組織に新しいリポジトリを作成できます。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えてください。このラボの後半で使用します。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub Issueのバックログも自動的に作成されます。ワークショップではこのIssueを使います。自分でIssueを作成する必要はありません。

## codespaceの作成

続いて、ラボの演習に使用するcodespaceを作成します。

[GitHub Codespaces][codespaces]は、ブラウザー上で直接コードを記述、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応する、フル機能の統合開発環境（IDE）を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の**Code**ボタンを選択します。

    ![Codeボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces**タブを選択し、**+**ボタンを選択して新しいCodespaceを作成します。

    ![新しいcodespaceを作成する画面](../../../_images/ex0-create-codespace.png)

codespaceの作成には数分かかりますが、すべてのサービスを手動でインストールするよりもはるかに短時間で準備できます。その間、次の演習で使うGitHub Copilotのほかの機能を確認してみてください。

> [!CAUTION]
> 今後の演習でcodespaceに戻ります。今はブラウザーのタブを閉じずに、そのまま開いておいてください。

> [!NOTE]
> このワークショップは、codespaceまたはローカルの[開発コンテナー][dev-containers]内で実行できるように構成されています。どちらの環境にも、スムーズに進めるために必要な前提条件があらかじめインストールされています。ローカルで実行する場合は、クローンしたリポジトリをVS Codeで開き、プロンプトが表示されたら**Reopen in Container**を選択してください。codespaceと同じ開発コンテナーが構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chatとエージェントモードを使う

GitHub Copilot Chatのエージェントモードを使うには、統合開発環境（IDE）にGitHub Copilot Chat拡張機能がインストールされている必要があります。GitHub Codespaceを使っている場合は、すでにインストールされているはずです。

> [!TIP]
> GitHub Copilot Chat拡張機能がインストールされていない場合は、[Visual Studio Code Marketplaceからインストール][copilot-chat-extension]できます。または、Visual Studio Codeの**Extensions**ビューを開き、**GitHub Copilot Chat**を検索して**Install**を選択します。

拡張機能をインストールした後、GitHubアカウントで認証して有効にする必要がある場合があります。

1. codespaceに戻ります。
2. エディターの右側にCopilot Chatが表示されていない場合は、codespaceの上部にある**Copilot Chat**アイコンを選択します。
3. Copilot Chatウィンドウに「Hello world」のようなメッセージを入力し、Enterキーを押します。Copilot Chatが起動します。
4. 認証されていない場合は、GitHubアカウントへのサインインを求められます。画面の指示に従って認証します。

    ![Copilot Chatの認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証が完了すると、Copilot Chatウィンドウが表示されます。

## まとめ

ラボ用リポジトリのコピーを作成し、コードを書き始めるときに使用するcodespaceの作成を開始しました。

## 次のステップ

Copilotを活用しましょう。[演習1 - カスタム指示][next-lesson]に進み、Copilotにプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespacesの概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespacesの使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
