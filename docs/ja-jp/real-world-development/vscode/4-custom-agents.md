---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義すると、特定の作業をより効果的に行うための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に与えられます。チームの専門知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどです。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter があり、その後にエージェントの動作、専門分野、指示を定義する Markdown プロンプトが続きます。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、ある程度の共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡単に言うと、**カスタム エージェントは作業者で、スキルはツール**です。

カスタム エージェントには専用のコンテキスト ウィンドウがあり、作業の一部としてスキルやほかのエージェントを調整できます。このラボでは、アクセシビリティのカスタム エージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業で、プルリクエスト ワークフロー用やテストの実行・管理用のスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合う方法を試してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックで、現在のダーク テーマはテキストと背景色のコントラストが不十分で、読みにくいという意見がありました。このアクセシビリティ上の問題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、機能をできるだけ早く作成します。カスタム エージェントに実装を依頼します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。内容を確認し、Copilot への指示を把握します。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します。演習 3 のフィルタリング PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。次の内容が含まれています。
   - アクセシブルな Web サイトのコードを生成する際の主な責任
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある落とし穴や間違い

## Copilot Chat でカスタム エージェントを使用する

VS Code では、`.github/agents` に定義したすべてのカスタム エージェントが Copilot Chat ビューの下部にあるエージェント ドロップダウンに表示されます。カスタム エージェントを選択すると、その指示とツールに合わせてチャット セッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は、各手順で説明します。

1. Copilot Chat のエージェント ドロップダウンで **Agent** が選択されていることを確認します。選択されていない場合は **Agent** を選択します。

   ![Chat ビューのエージェント選択欄を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェントのドロップダウンを選択します。既定では、現在のエージェントとして **default** が表示されます。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat が作業を開始し、コードベースを調べて変更を提案し、プロジェクト ファイルを編集します。各編集はチャット内にファイル パスと差分とともに表示されるので、確認できます。
6. 編集が進むと **Files changed** の表示が更新され、エージェントが変更したファイルが示されます。

> [!NOTE]
> この処理には数分かかる場合があります。Copilot はリポジトリに実際の変更を加えます。Astro コンポーネント、CSS、関連するテストなど、既存のファイルを編集します。

次のレッスンで、作業中の内容を確認し、追加の指示を出します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。特定のタスクや分野に合わせたアシスタントを使うと、チームの専門知識と標準を再利用可能な形でまとめ、Copilot の作業を導けます。

この演習では、次のことを行いました。

- カスタム エージェントの定義方法を確認しました。
- Copilot Chat のエージェント モードでカスタム エージェントを使用しました。

次に、[エージェントの作業を監視して指示を追加します][next-lesson]。作業中の変更を確認し、同じセッションにライト モードの切り替え機能も追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
