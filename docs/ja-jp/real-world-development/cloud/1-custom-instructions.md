---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot が知っておくべき背景情報がある場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何をするかだけでなく、どのように進めるかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは変更前と変更後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue を使って非同期に動作するため、リアルタイムで並べて比較するのは難しいためです。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

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

変更を始める前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチにある指示を読み込みますが、変更を整理してコミットするためにブランチを使います。

1. Codespace のターミナルで、ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

## 新しいリポジトリ標準を追加する

`.github/copilot-instructions.md` は、プロジェクト レベルの情報を Copilot に伝えるファイルです。コード提案を改善できるよう、コーディング標準を追加します。

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

## 指示の更新をコミット、プッシュしてマージする

Copilot cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、`main` からブランチが作成されます。したがって、cloud agent が指示を使えるように、変更を `main` に取り込む必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成してマージします。次の演習で Issue を割り当てるとき、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの手順を省略して `main` にコミットしてもかまいません。ブランチを使う手順は、実際のプロジェクトに近い流れを体験するためのものです。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストをどのように取得するかを確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストをレビューするときに、その標準が反映されているか確認できます。

次に、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのよりよいカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
