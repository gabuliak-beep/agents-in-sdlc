---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、登録済みのフィルタリング Issue を見つけました。
- 星評価を追加して、転送されたブラウザーで結果を確認し、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装して、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズし、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの根拠を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使うスラッシュ コマンドを確認しました。

## 公開した内容

このワークショップでは、更新済みの `main` からそれぞれ別のブランチを作成し、2 つの PR を作成しました。

1. **星評価:** ゲーム カードに既存の `starRating` と、未評価であることを示す状態を表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズして QA プロファイルを作成し、関連するテストを含めます。

フィルタリングの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップの手順を簡略化するため、この作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザーの確認、MCP を通じた Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA エージェントは PR 作成前に、要件とテスト カバレッジのレビューに結果をまとめました。

追加したテストは、実際に不足していた箇所を補うものです。カバレッジが十分なため、QA の実行で新しいテストが不要になる場合もあります。マージを許可する前に、コードと根拠をレビューします。変更後は、影響を受ける検証を再度実行してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールは、作業に影響します。このワークショップでは、指示の更新、スキルのカスタマイズ、QA プロファイルの作成、MCP サーバーの構成を行いました。こうしたカスタマイズは複数の会話で再利用でき、チームの必要に合わせて調整できます。指示は標準を設定し、スキルは繰り返し行うタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。作成前に方針を考えるには **Plan**、対象を絞った変更に参加し続けるには **Interactive**、範囲が明確な作業には **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

ツールや環境だけでなく、コンテキストも重要です。作成するもの、目的、方法を明確に伝えると、結果が大きく変わります。

## さらに試す

基本的なワークフローを学びました。Copilot CLI の次の機能も試してください。

- `/review` を使って、コードレビュー エージェントに変更の分析を依頼します。
- `/rubber-duck` を使って問題を話し合い、別の視点を得ます。
- `/fleet` を使って、独立したサブタスクを並行して調整します。
- `/worktree` を使って、別のタスクを分離します。
- `/delegate` を使って、Copilot cloud agent にタスクを送ります。

## 次のステップ

ツールの使い方を上達させるには、使い続けることが一番です。プロダクション コード、趣味のコード、何年も作りたいと思っていた小さなアプリなど、さまざまな場面で使ってください。チームで知見を共有し、互いの経験から学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
