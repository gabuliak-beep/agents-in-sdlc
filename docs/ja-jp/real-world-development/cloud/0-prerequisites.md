---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な準備をします。Tailspin Toys リポジトリのコピーを作成し、[Codespace][codespaces] を起動します。Codespace では、指示ファイルを編集し、cloud agent が作成した変更をレビューできます。

## ラボ用リポジトリをセットアップする

作成するコードのコピーとして、[テンプレート][template-repository]からリポジトリを作成します。新しいリポジトリにはラボに必要なファイルが含まれ、演習を通じて使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys のテンプレート][tailspin-template]を開きます。
2. ラボ リポジトリのページで **Use this template** を選択し、**Create a new repository** を選択してリポジトリのコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップを受講している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えてください。このラボの後半で参照します。

## Codespace を作成する

次に、Codespace を使ってラボの演習を行います。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境です。ブラウザーでコードの作成、実行、デバッグができます。複数のプログラミング言語、拡張機能、ツールに対応した IDE を使用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを開き、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、必要なサービスを手動でインストールするよりも短時間で済みます。作成中に、これから説明する GitHub Copilot の機能を確認してもよいでしょう。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。ここでは、ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できるように構成されています。どちらにも、スムーズに演習を進めるための前提条件が含まれています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択します。Codespace と同じ dev container がビルドされます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成しました。また、Copilot cloud agent と一緒に演習を進めるための Codespace の作成を開始しました。

## 次のステップ

次に、cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の利用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
