---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または構成し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまで説明したように、コードを書くだけが開発作業ではありません。データや外部サービスを扱い、Copilot に追加の自動化を利用させることもあります。そこで MCP サーバーが役立ちます。MCP サーバーを使うと、Copilot CLI に組み込まれた機能を超えて、さらに多くのツールやサービスを使えます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを学びます。
- Playwright MCP サーバーが利用できない場合は追加します。
- エージェントにブラウザーを操作してフィルタリング機能を確認してもらいます。

## シナリオ

単体テストとエンドツーエンド テストは重要ですが、UI を更新した場合は実際に操作して検証する必要があります。Copilot がユーザーと同じように Web サイトを使えるようにすると、変更内容が想定どおりに動作する確信を高めるために、作業をさらに自動化できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP を使うと、最新情報にアクセスし、ユーザーに代わって操作できます。

ツールとリソースへのアクセスは、AI エージェントと外部のツールやサービスの間をつなぐ MCP サーバーを通じて行います。各 MCP サーバーは、AI エージェントが利用できる異なるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも、さまざまな MCP サーバーがあります。エコシステムで利用できるサーバーを見つけたり公開したりするには、GitHub の [MCP レジストリ][mcp-registry]を確認してください。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同じように扱ってください。使用する前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーをまだ利用できない場合は、Copilot がユーザーのように Web サイトを操作できるよう、追加します。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに構成されている場合は、セットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類を **STDIO** にし、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは機能が想定どおりに動作することを手動で確認しました。今度は Copilot が利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使って、Playwright MCP サーバーで機能を検証するよう Copilot に伝えます。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot は MCP サーバーを自動で判断するため、明示的に指定する必要はありません。ただし、使うツールが分かっている場合は指示しておくと、結果がより一貫し、トークンも節約できます。

2. Copilot がブラウザーで機能を確認するために実行する手順を確認します。
3. レポートを読み、すべてが想定どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、サーバーを停止して、結果を報告します。

## まとめと次のステップ

Playwright MCP サーバーを使い、Copilot CLI から実際のブラウザーで機能を確認しました。ここでは、次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを学びました。
- Playwright MCP サーバーが利用できない場合は追加しました。
- エージェントにブラウザーを操作して、フィルタリング機能を確認してもらいました。

次は[QA カスタム エージェントを作成します][next-lesson]。スキルとブラウザー ツールを組み合わせ、専門的な役割を実現します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
