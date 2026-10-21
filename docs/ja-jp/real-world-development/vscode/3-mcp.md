---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードを使ってフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作してテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールを追加して Copilot の機能を広げる方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に MCP サーバーを使ってブラウザーでフィルタリング機能を手動テストするよう依頼します。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外にある機能を使うと、さらに強力になります。Model Context Protocol (MCP) は、Copilot が外部のツールやサービスと通信するための標準規格です。

![エージェント モードとコンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP により、AI エージェントはリアルタイムで外部ツールやサービスとやり取りできます。たとえば、リソースを使って最新情報にアクセスし、ツールを使ってユーザーに代わって操作できます。

ツールとリソースへのアクセスは、AI エージェントと外部のツールやサービスの間をつなぐ MCP サーバーを通じて行います。MCP サーバーは、AI エージェントと外部ツール (既存の API や NPM パッケージなどのローカル ツール) 間の通信を管理します。各 MCP サーバーは、AI エージェントが利用できる異なるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理するための API にアクセスできます。リポジトリの作成や更新、Issue とプルリクエストの管理などを AI エージェントに依頼できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページの移動、フォームの入力、ボタンの選択などを AI エージェントに依頼できます。

ほかにも、さまざまな MCP サーバーが利用できます。エコシステムで利用できるサーバーを見つけたり、公開したりするには、GitHub の [MCP レジストリ](https://github.com/mcp)を確認してください。

> [!CAUTION]
> セキュリティに関しては、MCP サーバーをほかの依存関係と同じように扱ってください。使用する前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使用し、機密リソースや操作へのアクセスを許可する場合は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot がバックログの Issue を読み取ることができました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot に操作可能なブラウザーを提供します。作成した機能のテストに使います。

1. `.vscode/mcp.json` で、`github` と並べて `playwright` エントリを追加します。ファイルは次のようになります。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモート サーバーの `github` とは異なり、`playwright` はローカル サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、画面に表示せずにブラウザーを実行するよう Playwright に指示します。Codespace には表示用のデスクトップがないため、ヘッドレス モードが必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストで Playwright をすでに使用しているため、必要なブラウザーは通常インストール済みです。Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行してから再試行するよう依頼してください。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot が初めてサーバーのツールを使うときに、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認された場合は、構成を確認して信頼することを選び、サーバーを起動します。

手動で起動する必要はありません。ここで起動しない場合は、次の手順で Copilot がツールを初めて使うときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot に実際のブラウザーを操作してもらえます。自分でアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を読み取って概要を報告します。会話を離れずに、機能が想定どおりに動作するかをすばやく確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントは支援技術と同じように、ラベルの付いたボタン、リンク、リスト項目などの構造化された要素を処理します。そのため、簡単な機能テストをアクセシビリティの初期確認にも活用できます。

サーバーに接続し、アプリを実行した状態で、次のプロンプトを Copilot に送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、手順を実行して結果を報告します。報告を Issue の受け入れ条件と照合してください。期待と異なる動作があれば、質問を追加したり、PR を作成する前にコードを修正するよう依頼したりします。

> [!NOTE]
> このテストでは、アプリを `http://localhost:4321` で実行しておく必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再度起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーをダウンロードする場合があります。ブラウザーが見つからないと報告されたら、`npx playwright install chromium` を実行するよう依頼して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームがレビューできるようにプルリクエスト (PR) を開きます。まず、`filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルを開き、Copilot が変更したファイルを確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを選択してコミット メッセージを生成します。

   ![変更されたファイルを表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。ここでは GitHub Copilot に作成を依頼します。関連する Issue を検索し、関連付けられた PR を作成できます。

1. Copilot Chat パネルに移動して **New Chat** を選択し、新しいセッションを開始します。
2. GitHub ツールを使えるよう、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報の取得や操作を行うために必要に応じて **Continue** を選択します。GitHub ツールを初めて使用するときに GitHub の認証を求められた場合は、画面の指示に従って許可します。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成することを確認します。
6. Copilot が作成したリンクを選択してプルリクエストをレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot に新しいツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーで機能を公開する前に、MCP サーバーを使ってフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業の PR を作成しました。

MCP サーバーを使って機能をテストしましたが、Copilot の機能を広げる方法は MCP だけではありません。次は[カスタム エージェント][next-lesson]を作成し、アクセシビリティ レビューなどの対象を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
