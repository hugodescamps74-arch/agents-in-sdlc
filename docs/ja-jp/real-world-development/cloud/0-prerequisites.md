---
title: "演習0：前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agentの演習を始める前に、必要な環境を準備します。Tailspin Toysリポジトリをご自身のリポジトリとして複製し、[codespace][codespaces]を起動します。codespaceでは、指示ファイルの編集やcloud agentが作成した成果物の確認を行います。

## ラボ用リポジトリのセットアップ

コードを作成するためのリポジトリを用意するには、[テンプレート][template-repository]から新しいリポジトリを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれ、各演習で使用します。

1. 新しいブラウザーウィンドウで[Tailspin Toysテンプレート][tailspin-template]を開きます。
2. ラボリポジトリのページで**Use this template**を選択し、**Create a new repository**を選択して、ご自身のリポジトリを作成します。

    ![テンプレートを使用するボタン](../../../_images/ex0-use-template.png)

3. GitHubまたはMicrosoftが主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilotを利用できる組織に新しいリポジトリを作成できます。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えてください。このラボの後半で使用します。

## codespaceの作成

続いて、ラボの演習に使用するcodespaceを作成します。

[GitHub Codespaces][codespaces]は、ブラウザー上で直接コードを記述、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応する、フル機能の統合開発環境（IDE）を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の**Code**ボタンを選択します。

    ![Codeボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces**タブを選択し、**+**ボタンを選択して新しいCodespaceを作成します。

    ![新しいcodespaceを作成する画面](../../../_images/ex0-create-codespace.png)

codespaceの作成には数分かかりますが、すべてのサービスを手動でインストールするよりもはるかに短時間で準備できます。その間、GitHub Copilotのほかの機能を確認してみてください。次の演習で利用します。

> [!CAUTION]
> 今後の演習でcodespaceに戻ります。今はブラウザーのタブを閉じずに、そのまま開いておいてください。

> [!NOTE]
> このワークショップは、codespaceまたはローカルの[開発コンテナー][dev-containers]内で実行できるように構成されています。どちらの環境にも、スムーズに進めるために必要な前提条件があらかじめインストールされています。ローカルで実行する場合は、クローンしたリポジトリをVS Codeで開き、プロンプトが表示されたら**Reopen in Container**を選択してください。codespaceと同じ開発コンテナーが構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成し、Copilot cloud agentと並行して作業するときに使うcodespaceの作成を開始しました。

## 次のステップ

cloud agentが従うカスタム指示を追加します。[演習1 - カスタム指示][next-lesson]に進んでください。

## リソース

- [GitHub Codespacesの概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespacesの使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
