---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリングを検証するために実行します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発作業ではありません。手動でコードの動作を確認し、指示ファイルを使って標準に沿っていることを確認しました。では、テスト、lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

こうしたタスクには、**エージェント スキル**が適しています。スキルは、テストなどの操作を適切に実行する方法を Copilot に伝えます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルは、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたもので、エージェントが必要なときに読み込みます。基本的には、スキル名を付けたフォルダーと、その中にある `SKILL.md` で構成されます。Markdown にはスキルの名前と説明を定義する frontmatter、動作の概要、呼び出すタイミングに関する指示を記載します。呼び出されたときに使うスクリプトやリソースをサブフォルダーに含めることもできます。

> [!NOTE]
> スキルにサブフォルダーやファイルを追加する必要はありません。Tailspin Toys の `quality-checks` スキルは既存のプロジェクト コマンドを使うため、`SKILL.md` のみを含みます。

スキルは、チームで共有して再利用できるリポジトリの `.github/skills` フォルダー、またはユーザーのスキル フォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認します。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの先頭にある `name` と `description` を読みます。説明は、Copilot がスキルを呼び出すタイミングを判断する助けになります。
3. 指示を読み、テストと lint の手順を Copilot にどう伝えているか確認します。
4. このスキルには **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接スキルを呼び出すことも、自然な言葉で依頼することもできます。Copilot に 3 つのチェックを実行してもらいます。

1. Interactive モードのフィルタリングの会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行したチェック、成功したかどうか、ツールから実際に報告された内容が分かるレポートに改善します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルを実行して、新しいレポートを確認します。Copilot CLI は、会話を再起動しなくても編集したスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前と**まったく同じプロンプト**を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使いました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更はフィルタリングの機能 PR に含まれます。次は[Playwright MCP サーバー][next-lesson]を使い、Copilot がサイトを直接操作できるようにします。

## スキルの例

コミュニティで公開されている次の例は参考情報であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
