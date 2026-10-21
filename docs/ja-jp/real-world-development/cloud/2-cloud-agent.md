---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織はほとんどありません。未解決のセキュリティ上の問題、更新が必要なレガシー コード、実装する時間がなくバックログに残っている機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などのタスクを自律的に実行します。作業が完了すると、人間の開発者がレビューできるドラフト PR が作成されます。面倒な作業を任せて開発を加速し、より大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成用の環境をカスタマイズします。
- 安全に処理を行う方法を確認します。
- Issue の範囲を明確にすることの重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には、対応したい技術的負債があります。最初のサイトを作った開発者は、ドキュメントを十分に整備していません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示して、閲覧を続けられるようにします。まだ洗練された実装は必要なく、UX の受け入れテストに使えるものを求めています。これは現在のブロッカーですが、ほかに優先度の高い Issue もあります。

どちらも、優先度が下がりやすいタスクの例です。Copilot cloud agent に割り当てれば、開発者はほかの作業に集中し、後から Copilot の作業をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を記録するドラフト プルリクエストを作成し、環境をセットアップしてタスクに取り組み始めます。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策をレビューできる状態になると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省けるわけではありません。一見簡単な操作でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、指示が Copilot を導く方法を確認しました。すでにカスタム指示の演習を終えている場合は、`.github/copilot-instructions.md` の効果を見ています。まだの場合は、ここで簡単に確認してください。cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその影響を確かめます。

リポジトリの GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、作成されるプルリクエストに指示ファイルからの TSDoc コメント、TypeScript の規約、ファイル コメント ヘッダーが反映されるか確認してください。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境をセットアップする

誰が作業する場合でも、コードを作成するには通常、特定の環境とセットアップが必要です。Copilot にタスクを割り当てる場合も同様です。Copilot は SWE と同じような手順で作業します。

Cloud agent は、作業用の環境として [GitHub Actions][github-actions] を使用します。作業を始める前に実行される[専用セットアップ ワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、必要な開発ツールや依存関係を利用できるように環境をカスタマイズできます。このラボの流れに沿うよう、あらかじめ設定済みです。単一の Astro アプリ用に、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、移行とシードを済ませた SQLite データベースを Copilot が利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと似ていますが、次の点に注意してください。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを確認してください。これにより、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の実行を待たずに、ワークフローが正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、多くのプロジェクトで情報が古いままになっているか、ドキュメントが不足しています。このような技術的負債は後回しになり、生産性を下げたり、コードベースの保守や新しい開発者の受け入れを難しくしたりします。Copilot はドキュメント作成を得意としているため、このタスクを Copilot cloud agent に割り当てるのに適しています。バックグラウンドでドキュメントを作成してもらい、後の演習で作業をレビューします。

1. 新しいブラウザー タブで、github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空の Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て用のダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。新しい PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が To Do リストを作成したことを確認します。

> [!NOTE]
> Copilot が PR に To Do リストを表示するまで、数分かかることがあります。Copilot は前述のワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組むための方針を決めています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するために、ウィンドウを更新する必要がある場合があります。

16. セッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調べて状況を把握し、適切な計画を検討し、コードを作成する過程を確認できます。

この処理には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期に実行して、開発者がほかの作業に集中できるようにすることです。この機能を活用して、別のタスクも cloud agent に割り当ててから、アプリケーションに機能を追加する作業に取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前述のとおり、GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チーム向けに関連ゲームのセクションを追加する作業に時間がかからない場合でも、その間に別のタスクを進められます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て用のダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることを示します。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent は SWE と同じように動作するため、常に監視する必要はありません。完了後にレビューできます。次は、カスタム エージェントの作成と使用に進みます。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使って、Issue を Copilot に割り当て、非同期に作業できることを確認しました。Copilot は技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに利用できます。

この演習では、次のことを行いました。

- コード生成用の環境をカスタマイズしました。
- 安全に処理を行う方法を確認しました。
- Issue の範囲を明確にすることの重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使用します。[Copilot cloud agent は MCP サーバーも使用できます][cloud-agent-mcp]。前のモジュールで取り上げたカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
