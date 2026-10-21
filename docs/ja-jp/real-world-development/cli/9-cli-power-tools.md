---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

優れた CLI ツールと同じように、GitHub Copilot CLI にも操作用のスラッシュ コマンドが用意されています。これらのコマンドで、高度な機能、内部の処理情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートしたり共有したりする方法を学びます。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の基本的なワークフローを終えました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、いくつかの機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキスト ウィンドウの上限に達する場合があります。Copilot CLI は必要に応じて会話を自動的にコンパクト化します。スラッシュ コマンドを使うと状態を確認し、自分でコンパクト化することもできます。

1. Codespace に戻り、Copilot CLI がまだ開いていない場合はリポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分配されているかを確認します。
4. 会話をコンパクト化します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合は、大きな変化がないこともあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的にコンパクト化します。タイミングを自分で選びたい場合は `/compact` を使います。関係のない別のタスクに移り、新しい会話を始めたい場合は `/clear` または `/new` を使います。

## モデルを選ぶ

モデルにはそれぞれ得意分野があり、開発者ごとに好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用できるモデルと使用状況の情報を確認します。
3. 現在のモデルをそのまま使うか、別のモデルを選ぶか、または <kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで作業して知見を共有すると、AI ツールの使い方を互いに改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、目的に合う保存先を選択します。たとえば、ローカルの Markdown ファイルにエクスポートするには `/share file` を使います。
3. エクスポートした内容を確認してから、ほかの人に送信したり公開したりします。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡張または委任する

ワークショップの基本編は完了しました。大きなタスクでは、Copilot CLI の次のコマンドも使えます。

- `/fleet` は、独立したサブタスクをサブエージェントに分け、並行して実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期で作業し、プルリクエストを開く場合もある Copilot cloud agent にタスクを送信します。

これらのコマンドは追加の worktree やリモート作業を作成する場合があるため、任意です。試す前に、新しい会話を開始して、範囲を明確にしたタスクを用意してください。結果は通常のワークフローと同じようにレビューします。非同期エージェントをさらに詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、構成を変更し、セッションを共有し、内部の状態を確認できます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートしたり共有したりする方法を学びました。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュ コマンドがあります。次は[これまでに学んだことと今後の手順を確認します][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
