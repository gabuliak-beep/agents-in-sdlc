---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを 1 つのブランチに保存しました。変更をまとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを管理してもらいます。

> [!NOTE]
> 通常は、機能、指示、スキルの更新、QA エージェントをいくつかの PR に分けます。ワークショップを簡潔に進めるため、フィルタリングと品質ワークフロー全体を 1 つの会話とブランチにまとめ、この PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージのライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後に Agent Merge を有効にして、PR がマージされたことを確認します。

## シナリオ

フィルタリングのワークフローを通じて、Copilot に機能の計画、実装、検証を依頼しました。次は Tailspin Toys の残りの PR 作業を自動化します。ただし、マージを許可するかどうかは開発者が管理します。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題に対応します。継続的インテグレーション (CI) のチェック失敗を修正し、レビュー コメントに対応し、必要な場合はリベースした後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは、**Merge pull request** を自分で選択していました。Agent Merge にその作業を任せることができます。有効にする前に、作業をレビューし、マージ可能であることを確認してください。

## Agent Merge で PR を管理する

コードをすべて作成したので、まとめてレビューして PR を作成し、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. オプションから **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って、新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。作業がブロックされた場合は、報告された理由を読み、リポジトリで許可されていれば、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストの作成など、開発プロセスの一部を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge とは何か、マージのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にして、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に役立つ[ほかの Copilot CLI スラッシュ コマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
