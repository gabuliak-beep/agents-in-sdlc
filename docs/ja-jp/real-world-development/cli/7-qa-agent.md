---
title: "レッスン 7 - QA エージェントを作成して使用する"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP を使ってブラウザーでフィルタリングの動作を確認しました。次は、これらの機能を明確な QA プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールを使う方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルタリングの Issue に基づく結果を確認します。

## シナリオ

Tailspin Toys はプルリクエスト (PR) を開く前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタム エージェントに QA プロセスを調整させ、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルには、エージェントの目的、指示、利用できるツールを記載します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

ここまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルは、繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、それらの機能をどのように使うかを Copilot に伝えます。これらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを作成するよう Copilot に依頼します。QA が行うチェックと従うべき制約を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のプロンプトで新しいカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと制約をプロファイルに反映したことを確認します。検証だけを依頼したのに機能を変更するような、不完全または範囲が広すぎるエージェントになるのを防ぎます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略できますが、指定するとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件を起点にし、リポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を含む結果を報告し、実装コードを変更する前に確認を求め、変更をコミットせず、プルリクエストを作成しないことを確認します。
5. 必要な責任や制約が欠けている場合は、続ける前に既定のエージェントにプロファイルを修正するよう依頼します。

## Issue に基づいて QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、フィルタリングの会話を再開します。既存の Issue と計画の決定をコンテキストに含めるためです。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、利用可能なエージェントの一覧にまだ表示されない場合があります。次のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が終わったらレポートを確認します。

## まとめと次のステップ

ワークフローに再利用可能な専門家の役割を追加し、その作業を確認しました。このレッスンでは、次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールを使う方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリングの Issue に基づく結果を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートの準備ができました。次は、これらを[機能の PR にまとめて Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
