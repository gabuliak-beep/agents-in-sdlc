---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys のコピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な準備をします。Tailspin Toys リポジトリのコピーを作成し、[Codespace][codespaces] を起動します。次のレッスンでは、Codespace の統合ターミナルを使って Copilot CLI をインストールして実行します。

このレッスンでは、次のことを行います。

- テンプレートから Tailspin Toys プロジェクトのコピーを作成します。
- Codespace を作成し、プロジェクトの準備ができていることを確認します。

## ラボ用リポジトリをセットアップする

自分用にコピーした Tailspin Toys プロジェクトを使います。[テンプレート リポジトリ][tailspin-template]から今すぐ作成してください。新しいリポジトリにはラボに必要なファイルがすべて含まれています。

1. 新しいブラウザー ウィンドウで [Tailspin Toys のテンプレート][tailspin-template]を開きます。
2. **Use this template** を選択し、**Create a new repository** を選択してリポジトリのコピーを作成します。
3. GitHub または Microsoft が主催するイベントでワークショップを受講している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。
4. 作成したリポジトリのパス (`organization-or-user-name/repository-name`) を控えてください。ワークショップの後半で参照します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこれらの Issue を使用します。自分で Issue を登録する必要はありません。

ワークショップ テンプレートの新しいコピーを使ってください。リポジトリには、指示ファイル、アプリケーション コード、テスト、`quality-checks` スキル、演習で使うバックログが含まれています。古いコピーを使う場合は、必要なファイルが揃っているか、担当者に確認してください。

## Codespace を作成する

次に、Codespace を使ってワークショップを進めます。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境です。ブラウザーでコードの作成、実行、デバッグができます。複数のプログラミング言語、拡張機能、ツールに対応した高機能なエディターを使用できます。

1. 新しく作成したリポジトリを開きます。
2. **Code** を選択します。
3. **Codespaces** タブを開き、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートがプロジェクトの依存関係、Playwright Chromium、ローカル データベースをインストールします。
5. **Do you trust the authors of the files in this folder?** と確認された場合は、**Trust Folder & Continue** を選択します。
6. リポジトリのルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が利用可能と表示されたら **Open in Browser** を選択し、Tailspin Toys のサイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> を押して開発サーバーを停止します。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- テンプレートから Tailspin Toys プロジェクトのコピーを作成しました。
- Codespace を作成し、プロジェクトの準備ができていることを確認しました。

次は Codespace に[GitHub Copilot CLI をインストール][next-lesson]し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
