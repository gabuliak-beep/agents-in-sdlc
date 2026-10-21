---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習で、ユーザー エクスペリエンスを改善し、機能を追加するために、GitHub Copilot cloud agent に 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、作業を監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールが用意されています。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行された作業を調べ、進行中のセッションに指示を追加できます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を追加し、機能を依頼します。

## シナリオ

エージェントにハイ コントラスト モードの作成を依頼した後、チームはライト モードも追加するよい機会だと気付きました。サイトのスタイルを更新して切り替え機能を追加しているため、ライト モードも実装するのが自然です。エージェントがライト モードも追加するよう、作業に指示を追加します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot エージェント ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左側のペインに、タスクの一覧が表示されます。割り当てたタスクのうち、次の項目があることを確認します。
    - コードベースのドキュメントを更新する
    - ゲームを変更する API を生成する
    - Web サイトにハイ コントラスト モードを追加する
3. 実行中のタスクを選択し、Copilot が実施した作業を確認します。次のような操作があります。
    - リポジトリからコードをチェックアウトする
    - Copilot が作業する環境を作成する
    - MCP サーバーをセットアップする
    - 割り当てられたタスクを完了するためのさまざまな手順を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここから PR と変更されたファイルを確認し、作業の進行状況を監視できます。

## Cloud agent に指示を追加する

実行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードの切り替え機能も追加するよう Copilot に依頼します。

1. ハイ コントラスト モードの追加に関するセッションを選択します。Copilot が設定するタイトルや作業状況によって、表示される名前は異なります。

    ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セッションを数分間確認し、セットアップが完了して作業が始まるまで待ちます。次のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のエージェント タスクに指示を追加するダイアログのスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に加えることを確認します。

## Copilot の作業を続ける

これまでと同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中の手順が完了すると、新しい依頼が作業に追加されます。

この処理にも数分かかります。その間、ここまでに学んだことを振り返ってもよいでしょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視し、指示を追加するための中心的な場所である Copilot エージェント ページを確認しました。ここでは、進行中または完了したすべてのタスクを追跡し、実行内容をレビューできます。進行中のタスクの範囲を調整したり、追加の指示を出したりすることもできます。

このレッスンでは、次のことを行いました。

- mission control とエージェント ページを使って cloud agent のタスクを監視しました。
- 進行中のセッションに指示を追加し、機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は [Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot エージェント ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
