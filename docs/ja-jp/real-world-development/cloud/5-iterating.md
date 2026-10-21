---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスを改善し、機能を追加するために、GitHub Copilot にいくつかのタスクを依頼しました。コードにドキュメントを追加し、デザイン チームが反復できる関連ゲーム機能を作成し、ハイ コントラスト モードとライト モードの切り替えを含むアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要であれば Copilot にフィードバックを返して作業を改善します。

### シナリオ

生成 AI を使っても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チームのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について、GitHub Copilot の提案を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ監督なしでタスクを実行するため、安全を確保するためのセキュリティ上の制約があります。

- Copilot はリポジトリに対する読み取りアクセス権を持ち、コードを書き込めるのは自身のブランチだけです。
- Cloud agent は GitHub Actions 内で実行され、作業用に一時的な環境を別途作成します。
- GitHub Actions ワークフローの実行には、人間の承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が最初に作成したプルリクエスト (PR) は、コードへのドキュメント追加です。GitHub.com の標準的な PR インターフェイスを使って作業を確認します。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールによってブロックされたという警告が表示される場合があります。既定では Copilot の外部リソースへのアクセスが制限されているため、これは想定された動作です。外部 MCP サーバーへの接続などが対象になります。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** のようなタイトルのプルリクエストを開きます。実際のタイトルはより詳しいものになる場合があります。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が表示されます。その場合は、Copilot の作業が終わるまで待ちます。数分かかることがあるので、その間に休憩したり、ここまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 更新されたコードと、新しく追加された TSDoc のドキュメント コメントなどを確認します。具体的な変更内容は異なる場合があります。

    変更を確認するときは、TSDoc のドキュメント コメント、TypeScript の規約、ファイル コメント ヘッダーを確認してください。演習 2 の最初に確認したカスタム指示ファイルから反映された内容です。

6. 変更をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。単一の Astro アプリに関するプロジェクト チェックがすべて成功することを確認します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と一緒に作業する方法は、作業を一方的に受け取るだけではありません。チームのメンバーと同じように、プルリクエストやコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確率的に変わるため、依頼する内容を一律に指定することはできません。次のような更新を依頼できます。

- 各コード ファイルの先頭に、ファイルの役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例のいずれか、またはコードベースのドキュメントに関する別の提案を使い、**@copilot** をメンションして、生成されたドキュメントへの変更を依頼します。ほかのユーザーにメンションするときと同じようにコメントします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

満足できるまで反復してかまいません。問題がなければ、ドラフト PR をレビュー可能な状態にして、`main` ブランチにマージできます。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue について、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** のようなタイトルの PR を選択します。実際のタイトルはより詳しいものになる場合があります。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローの承認待ちを示す表示があることを確認します。
7. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが追加されます。単一の Astro アプリに関するプロジェクト チェックがすべて成功することを確認します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストすることもできます。Codespace を開いてターミナルを起動し、次のコマンドを実行します。`<branch-name>` を Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。これまでと同様に、Copilot cloud agent と反復して変更を依頼できます。たとえば、表示する関連ゲームの数を変更したり、ファイル コメント ヘッダーや TSDoc のドキュメント コメントを追加したりできます。後者はカスタム指示を更新する前に割り当てたタスクだったことを思い出してください。前の PR と同じように、**Conversation** タブで新しいコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタム エージェントで実装した機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で mission control から追加を依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** のようなタイトルの PR を選択します。実際のタイトルはより詳しいものになる場合があります。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が表示されます。その場合は、Copilot の作業が終わるまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント
   - ユーザーの設定を保持する local storage の使用
   - ハイ コントラスト モードとライト モードの CSS またはスタイル変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード ナビゲーションなど)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。単一の Astro アプリに関するプロジェクト チェックがすべて成功することを確認します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace を開いてターミナルを起動し、次のコマンドを実行します。`<branch-name>` を Copilot が作成したブランチ名に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイ コントラスト モードとライト モードを切り替えます。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ用のカスタム エージェントが、アクセシビリティのベスト プラクティスに沿って Copilot の機能実装を支援しました。アクセシビリティ上の問題や改善点が見つかった場合は、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 追加のタスクを委任する

cloud agent にバックログの実際のタスクを任せ、その間に別の作業を進める方法を身に付けましょう。リポジトリに Issue を追加して Copilot に割り当ててみてください。次のようなタスクが考えられます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンでは、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、cloud agent の出力への影響を後で確かめました。
- **Issue を Copilot cloud agent に割り当て**、環境をセットアップし、非同期に計画して実行する様子を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使用し、ハイ コントラスト モードとライト モードの切り替えを追加しました。
- Copilot エージェント ページを mission control として使い、アクセシビリティ セッションを監視し、途中で指示を追加しました。
- **Copilot のプルリクエストをレビューして反復し**、`@copilot` に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。引き続き学習する場合は、ほかのハーネスも試してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP 連携を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` ブリッジを使って、ターミナルからレビュー済みの 2 つの変更を届けます。

自分のリポジトリでも、次のことを試してみてください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野のカスタム エージェントを作成する。
- 別の技術スタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
