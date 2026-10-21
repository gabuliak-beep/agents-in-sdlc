---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際は、コンテキストが重要です。タスクを特定の方法で進める必要がある場合は、Copilot が指針を利用できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、コードの構成方法も説明できます。フィルタリング機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス単位の指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発作業に関するガイドラインと要件があります。

- コメントにはコードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` にあるエクスポートされた関数には、目的、パラメーター、戻り値 (存在する場合は注入可能な `db` 引数も含む) を記載した TSDoc/JSDoc を追加します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載し、関連コードを変更したときにコメントを最新の状態に保ちます。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使って Copilot に必要な情報を伝え、これらの実践に沿ってタスクを進められるようにします。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝えられます。好みのコーディング規約、ライブラリ、コメントのスタイルなどを指定できます。リポジトリ全体に適用する指示や、ファイルの種類やタスクに応じた指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関する**すべて**のリクエストで Copilot に送信されます。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスク向けの指針を提供します。

> [!NOTE]
> ほかの指示形式と対応状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示の対応リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに複数の指示ファイルがあります。変更を加える前に確認して、その影響を理解します。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく)、`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングの指針を確認します。この指示は、リポジトリで Copilot を使うすべての操作に適用されます。
4. `.github/instructions` フォルダーを開いてファイルを確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルを決める glob パターンです。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな指示セットを再利用可能な小さなファイルに分け、Copilot に例を示せます。

## チームの指針に合わせて指示ファイルを更新する

既存のファイルは出発点として適していますが、改善の余地があります。中核となる `copilot-instructions.md` ファイルを変更して、新しく生成する TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。通常は 35 行目付近です。
2. 次の内容を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新された指針を使用する

Copilot CLI は会話を開始するときにリポジトリの指示を読み込みます。指示を更新した後、会話を終了してフィルタリングの会話を再開します。機能のコンテキストを失わずに、新しい指示を読み込めます。

1. フィルタリングの会話が開いている場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトで、更新した指針を適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパス単位の指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次に、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
