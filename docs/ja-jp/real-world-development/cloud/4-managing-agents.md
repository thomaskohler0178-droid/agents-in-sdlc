---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習で、GitHub Copilot cloud agent にユーザー体験の改善と機能追加を目的とする 3 つのタスクを依頼しました。cloud agent は非同期に自律して動作しますが、タスクを監視することも重要です。

GitHub.com の[agents ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつかあります。この mission control では、開いているプル リクエスト (PR) があるエージェント タスクをすべて確認できます。実行内容を調べたり、進行中のセッションに追加の指示を出したりできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 実行中のセッションに追加機能を依頼します。

## シナリオ

エージェントにハイ コントラスト モードを依頼した後、チームはライト モードも追加するよい機会だと考えました。サイトのスタイルと切り替え機能を更新しているため、ライト モードも含めるのが自然です。エージェントに指示を追加し、ハイ コントラストに加えてライト モードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインの両方に表示されるタスク一覧を確認します。コードのドキュメント更新、ゲームを変更する API の生成、Web サイトへのハイ コントラスト モード追加などのタスクが表示されます。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。リポジトリのチェックアウト、作業環境の作成、MCP サーバーの設定、タスク完了に向けた手順などが含まれます。

> [!NOTE]
> 表示される手順は、作業の状態や Copilot が選択した方法によって異なります。

4. 右側に表示されるプル リクエスト (PR) ペインも確認します。PR と変更されたファイルを表示し、進行状況を監視できます。

## cloud agent に追加の指示を出す

実行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードを追加するセッションを選択します。タイトルは Copilot が付けた名前や作業状態によって異なります。

   ![mission control に表示されたアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが分かるまで数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェント ページのタスクで、作業中のセッションに指示するダイアログを強調表示したスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り込むことを確認します。

## Copilot に作業を続けてもらう

更新されたタスクに Copilot が取り組みます。メッセージを送信した時点で実行中だった手順が終わると、新しい要望が作業に追加されます。

今回も数分かかります。ここまでに学んだことを振り返りながら待ちましょう。

## まとめと次のステップ

このレッスンでは、Copilot agents ページを使って GitHub Copilot cloud agent のタスクを監視し、方向付けました。この mission control では、実行中と完了済みのタスクを追跡し、行われている作業を確認して、進行中のタスクに指示を追加できます。

このレッスンでは、次のことを行いました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 実行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成したプル リクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
