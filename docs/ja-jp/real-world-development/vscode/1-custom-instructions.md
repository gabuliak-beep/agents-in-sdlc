---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot が知っておくべき背景情報がある場合は、その情報を参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。中でも重要なのが[指示ファイル][instruction-files]です。コード生成の方針を Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を使って、フィルタリング用の最初のデータ スライス (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトを再度送信し、生成されたコードに新しい標準が反映されることを確認します。
- 指示の更新と変更を `main` にコミットして、次の演習で Copilot が新しい指針を使用できるようにします。

> [!CAUTION]
> 生成されたコードは、設定した標準と異なる場合があります。Copilot の動作は確率的です。この演習では、出力を文字単位で一致させるのではなく、指示を更新した後に動作がどう変化するかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発作業に関するガイドラインと要件があります。

- データ層には必ず単体テストが必要です。
- UI はダーク モードで、現代的な見た目にします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭には、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って Copilot に必要な情報を伝え、これらの実践に沿ってタスクを進められるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝えられます。これにより、より適切な提案やコード スニペットが得られます。好みのコーディング規約、ライブラリ、コメントのスタイルなどを指定できます。リポジトリ全体に適用する指示や、ファイルの種類など特定のタスクに適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発内容の概要、ベスト プラクティス、全般的な指針など、ほとんどのチャットや CLI のリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類を対象にできます。TypeScript などの言語や、UI コンポーネントや単体テストの作成などのタスク向けに指針を用意できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使用されます。コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` frontmatter を含む `*.instructions.md` ファイルの両方を使用します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[指示ファイルの対応状況][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成についてすべて説明することはこの演習の範囲外ですが、サンプル プロジェクトのファイルは代表的な方法を示しています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、開発するものの説明、プロジェクト構成、全体に適用するコーディング標準など、プロジェクト レベルの情報を記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ層など、ファイルの種類や特定のタスクに関する指針を記載します。
- 自然な言葉を使い、明確な例を示して、わかりやすい指示にします。

指示ファイルの作成方法に決まった正解はありません。AI のほかの使い方と同様に、プロジェクトやシナリオに合う方法を試して見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスクを対象にしたファイルがあります。
>
> Copilot に指示ファイルの作成を依頼することもできます。各環境で操作方法は異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使用します。該当するレッスンで詳しく説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェント、その他のリソースを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。中核となる `copilot-instructions.md` と、さまざまなタスクを対象とする複数の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の中にある **GitHub Actions Workflows** の指針も確認してください。Copilot とのすべてのやり取りに関係する内容です。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データ層、テストなどを対象にした指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示が適用されるファイルを決める、リポジトリ ルートからの glob パターンです。たとえば、`**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトの単体テストを作成するための指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認してください。大きな指示セットを再利用可能な小さなファイルに分け、Copilot が参考にできる例を示すことができます。ここにあるパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ここでは、変更用のブランチを作成するベスト プラクティスに従います。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は、各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルのままプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディター タブをすべて閉じ、Copilot が必要なコンテキストだけを使用するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に知らせます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択欄を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパー ファイルとテストなどのファイルに変更を適用します。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、publisher の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約によるものです。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル コメント ヘッダーがないことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なので、指示がなくてもドキュメント コメントが追加される場合があります。その場合も問題ありません。ここでは、指示の更新後に動作が一貫して改善することを確認します。

## 新しいリポジトリ標準を追加する

`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に伝えるファイルです。コード提案を改善できるよう、コーディング標準を追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。通常は 27 行目付近にあります。プロジェクトのコーディング標準が書かれていますが、コード内ドキュメントのルールはまだありません。そのため、生成されたヘルパーにはドキュメント コメントが含まれていません。
3. 既存の標準の直後に、次の Markdown を追加します。ファイル コメント ヘッダーと TSDoc のドキュメント コメントを Copilot に指示する内容です。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、指示ファイルは `.github/copilot-instructions.md` のようにリポジトリ レベルで作成して全体に適用することも、`*.instructions.md` のように言語、ファイルの種類、タスクごとに作成することもできます。ドキュメント コメントのルールのようにプロジェクト全体に適用する標準は、リポジトリ レベルのファイルに記載します。

## プロンプトを再度送信して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を開き、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しい標準を Copilot に伝え、次の演習で使う実際のコードに適用できました。

## 指示ファイルをコミットしてブランチをプッシュする

指示ファイルも、ほかのアセットと同じようにソース管理で扱います。指示の更新をリポジトリにコミットしてプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
2. ターミナルで、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、Copilot がヘルパーの基盤を作る際に変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、指示ファイルの標準に関する Issue と関連付けたプルリクエストを作成します。手動で作成する代わりに、プロジェクトに接続済みの GitHub ツールを使い、Issue を検索して PR を作成するよう Copilot に依頼できます。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは、これが最初の演習です。プロジェクト テンプレートでは GitHub のツールがすでにワークスペースに接続されています。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。後の演習では、この接続方法である Model Context Protocol (MCP) を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択欄で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、新しい publishers ヘルパーの基盤の両方を説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を検索して PR を作成します。
5. GitHub の操作を許可するよう求められたら、コマンドを確認し、適切な場合は **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ローカルのブランチを `main` に戻します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェル コマンドの実行を許可するよう求められたら、内容を確認し、適切な場合は **Allow** を選択します。

GitHub Copilot の支援を受けてプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストをどのように取得するかを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示を使ってコード生成プロンプトを送信しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加しました。
- 同じプロンプトを再送信し、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で作業を続けられるよう、指示とヘルパーの基盤を `main` にコミットしました。

次は[エージェント モード][next-lesson]を使い、Copilot にコードベース全体の新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのよりよいカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
