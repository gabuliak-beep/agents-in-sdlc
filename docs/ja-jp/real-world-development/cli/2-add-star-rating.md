---
title: "レッスン 2 - 星評価を追加する: すばやくできる改善"
description: "Copilot CLI を使ってゲーム カードに小さな変更を加え、転送されたブラウザーでレビューし、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、プロジェクトに最初の変更を加えます。小さな変更から始めましょう。ゲームのデータにはすでに星評価がありますが、ホーム ページのゲーム カードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot との会話を始めます。
- エージェントにプロジェクトの小さな変更を依頼します。
- `/diff` で変更をレビューします。
- 転送されたブラウザーでアプリを実行し、変更を確認します。
- 最初のプルリクエストを開いてマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードに表示されるのはタイトル、カテゴリ、発行元、説明だけです。最初の練習として、エージェントに各カードの既存の評価を表示してもらいます。小さく独立した変更なので、最初のセッションに適しています。

## 会話の構成

**会話**では、Copilot CLI を使ってタスクに取り組みます。Copilot app とは異なり、通常の CLI 会話では、専用の worktree を作らず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使います。保存された会話に戻って同じ話題を続けられますが、ファイルとブランチはディスク上の通常の Git 状態として管理されます。

会話には、プロンプトとエージェントの応答、ファイルを調査・編集するエージェントの操作、`/diff` で確認できる変更の 3 つが表示されます。

## 会話を始めて変更を依頼する

機能の実装を始めるために、新しい会話を開始します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. Copilot がまだ起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを特定して、必要なコードを作成します。Copilot CLI を使って新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージ前にレビューしてください。Copilot CLI で変更を確認します。

1. `/diff` を入力して、変更されたすべてのファイルを確認します。
2. ゲーム カードに評価がある場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストが両方の状態を対象としていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI ツールは決定論的ではなく確率的に動作するため、コードは例と異なる場合があります。実装を完全に一致させるのではなく、動作を確認してください。

## 変更を確認する

コードを読んだだけで動作すると判断するのではなく、Copilot に Web サイトを起動してもらい、Codespaces の転送ポートを使ってブラウザーで更新した UI を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら **Open in Browser** を選択します。
3. ゲーム カードに 5 点満点の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを開いてマージする

機能を作成できました。新しいコードをプロジェクトにマージするため、プルリクエスト (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI が PR を作成するときに既存のコミットをプッシュし、PR の URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLI が表示した URL を選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って最初の変更を公開しました。具体的には、次のことを行いました。

- 機能ブランチで Copilot との会話を始めました。
- ゲーム カードに小さな変更を加えるようエージェントに指示しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプルリクエストを開いてマージしました。

次は[フィルタリング Issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
