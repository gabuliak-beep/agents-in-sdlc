---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを構成し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages にデプロイして、Space Quiz を誰でも利用できるようにします。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのはユーザーの役割です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages のデプロイを構成するよう依頼します。
- マージする前にワークフローとプルリクエストをレビューします。
- デプロイを確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz は GitHub リポジトリにありますが、まだ簡単に試せる方法がありません。単一ファイルのサイトを GitHub Pages で公開すると、サーバーやランタイム依存関係を追加せずに公開 URL からアクセスできるようになります。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3 段階の図: マージ済みの space-quiz プルリクエスト、Build website・Upload artifact・Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ に公開された Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトでは、エージェントがプルリクエストを作成してマージし、リポジトリの Pages 設定を変更します。作業を進める前に、差分、デプロイ権限、GitHub の承認プロンプトを確認してください。

## ワークフローをレビューしてデプロイを確認する

1. README は、リポジトリにまだ存在しない場合に限り追加されていることを確認します。
2. プルリクエストの変更内容を確認します。ワークフローのビルド手順とデプロイ手順も確認してください。
3. 必須のプルリクエスト チェックに合格した後にのみマージします。
4. GitHub Actions のデプロイ実行を開き、成功するまで確認します。
5. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを実行できない場合は、示された手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまでは、タスクを完了したとみなさないでください。

## まとめと次のステップ

Space Quiz をサイトとして公開し、エージェントの報告だけに頼らず、実際にサイトが動作することを確認しました。続けて、[レッスン 13: レビューと次のステップ][next-lesson]に進んでください。

[next-lesson]: ../13-review/
