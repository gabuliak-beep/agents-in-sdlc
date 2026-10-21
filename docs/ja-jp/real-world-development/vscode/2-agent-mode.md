---
title: "演習 2 - Copilot エージェント モードで新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションへの簡単な変更でも、通常は複数のファイルを更新し、テストを実行する必要があります。開発者は必要なファイルを探して変更し、テストを実行し、問題をデバッグして、変更漏れを見つけてさらに更新する、といった作業を繰り返します。

こうした作業に役立つのが Copilot エージェント モードです。

Copilot エージェント モードは IDE でより自律的に作業します。開発者と同じように、まず既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行して、見つかった問題を自動で修正します。エージェント モードを使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる利用方法を提供します。エージェント モードは IDE 内で動作し、すばやくフィードバックを得ながら対話できます。Cloud agent はチームのメンバーのように非同期で作業し、Issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェント モードがプロジェクトを調査し、関連するファイルを特定して、複数のファイルを連携して変更する方法。
- GitHub Copilot エージェント モードが UI とデータ層にまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームが増えてきたため、カテゴリと発行元で絞り込めるようにします。前の演習で publishers ヘルパーを追加しました。ここでは Copilot エージェント モードを使い、データ層、UI、テストの残りの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握します。

この Web サイトは、開発者向けテーマのボードゲームを扱うクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリであり、ページはビルド時に静的 HTML としてレンダリングされます。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースに直接クエリします。別のバックエンド API やクライアント側の UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` に置き、インタラクティブな処理には標準 DOM API を使う小さな Astro `<script>` を追加します。

### Web サイトを起動する

単一 Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace から実行できます。

1. 前の演習の後に `main` に戻っていることを確認し、Codespace に戻ります。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが準備できると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは、Web サーバーを実行している Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、その機能を確認できます。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明を含むボードゲームの一覧が表示されます。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は機能しますが、新しい機能を追加したいと考えています。まずバックログを確認します。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されています。GitHub Copilot に項目を表示してもらいましょう。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは、ファイルをまたいで自律的に計画して実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択欄を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。ワークショップを作成したときに Claude Sonnet 4.5 を使用したため、手順に記載しています。参加者の体験を揃えるためですが、LLM の動作は確率的なので、記載と異なる提案が表示される場合があります。これは正常であり、想定された動作です。

> [!NOTE]
> LLM の確率的な動作により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できます。

3. 次のプロンプトを Copilot に送信し、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての Issue を一覧表示するコマンドを実行するには **Continue** を選択します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーにとって有用なものを基準に、項目に優先順位を付けたことを確認してください。

## 指示ファイルを確認する

コード生成をエージェントに依頼する前に、Copilot にコンテキストを伝える指示ファイルを確認します。Web サイトへの機能追加の方針が記載された [ユーザー インターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使用します。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の方針を確認します。次の内容が含まれています。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティに関する原則
   - Astro、Tailwind CSS、Drizzle など、ファイルの種類ごとの指示ファイルへのリンク

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。これにより、指示を再利用したり、大きな指示セットを小さなファイルに分けたり、例やテンプレートを Copilot に示したりできます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、アプリケーションに少なくとも 3 つの変更が必要です。

- データ層 (`src/lib/`) のフィルタリング ロジックを追加または調整します。
- フィルタリングの動作に関するテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストが成功することを確認します。Copilot エージェント モードにこれらの作業を依頼します。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** を選択して指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択欄を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連 Issue を基に機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを探す様子を確認します。データ層のヘルパー、UI、テストなどを見つけた後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調査する様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストなど、複数のタスクを実行します。コードベースやタスクの複雑さによっては、数分かかる場合があります。作業中にエディターで **Keep** と **Undo** が表示される場合があります。作業が終わるとすべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に確認されたら **Continue** を選択してテストを実行します。

   ![テストを実行してよいか Copilot Chat で確認するダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 処理中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを繰り返し、エラーを検出しなくなるまで修正します。

   ![Copilot エージェント モードの完全な Chat セッションのスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動しているブラウザーに戻り、新しい機能を確認します。
11. すべてが動作し、コードをレビューできたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェント モードを使い、Tailspin Toys の Web サイトに新機能を追加しました。この演習では、次のことを学びました。

- GitHub Copilot エージェント モードを使って UI とデータ層にまたがる新機能を実装する方法。
- Copilot エージェント モードがプロジェクトを調査し、関連するファイルを特定して、複数のファイルを連携して変更する方法。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法。

次に、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### おまけの演習 — ページ分割を実装する

ゲームが増えるにつれて、ページ分割が必要になります。この演習で学んだスキルを使って、Copilot にページ分割を実装するよう依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、ベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定するかなど、ページ分割の方法を検討します。
- 希望どおりの実装を作成できるよう、プロンプトで Copilot に必要な情報を伝えます。
- GitHub Copilot に変更を依頼し、コンテキストを追加して反復する必要がある場合があります。これは Copilot と作業するときの通常の流れです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
