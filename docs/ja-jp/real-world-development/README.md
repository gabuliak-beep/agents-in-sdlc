---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共通のバックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を段階的に計画してコードを作成し、テスト、プルリクエストのレビュー、エージェントを使った作業管理を行います。

## シナリオ

架空の企業 Tailspin Toys に新しく加わった開発者として、ボードゲームのクラウドファンディングを提供するサービスに取り組みます。チームのバックログは GitHub Issue として登録済みです。フィルターやページネーションなどの機能に加え、アクセシビリティやコーディング規約などの品質改善にも取り組みます。

## 作業環境を選ぶ

GitHub Copilot は、さまざまな作業環境で利用できます。開発スタイルに合った環境を選んでください。各ワークショップには専用のセットアップ手順があり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使用します。Plan モードと Autopilot モード、指示、スキル、カスタム エージェント、Playwright MCP、Agent Merge、実用的なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

デスクトップ アプリで複数のエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業を行い、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を進め、進行状況を確認し、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使用します。エディターを離れずに Copilot Chat のエージェント モード、MCP サーバー、カスタム エージェントを扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
