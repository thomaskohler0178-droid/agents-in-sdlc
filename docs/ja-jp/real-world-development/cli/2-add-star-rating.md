---
title: "レッスン 2 - 星評価の追加で小さな成果を得る"
description: "Copilot CLI でゲーム カードに小さな変更を加え、ポート フォワーディングされたブラウザーで確認して、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次はプロジェクトに初めて変更を加えます。変更は小さく保ちます。ゲームのデータには星評価がすでにありますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで、目的を絞った Copilot の会話を始めます。
- エージェントにプロジェクトの小さな変更を依頼します。
- `/diff` で変更をレビューします。
- アプリを起動し、ポート フォワーディングされたブラウザーで確認します。
- 最初のプル リクエストを開いてマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードに表示されるのは、タイトル、カテゴリ、発行者、説明だけです。練習として、各カードに既存の評価を表示するようエージェントに依頼します。最初のセッションにぴったりな、小さく独立した変更です。

## 会話の構成

**会話**は、Copilot CLI を使ってタスクに取り組む場所です。Copilot app と異なり、通常の CLI 会話では、専用の worktree を作成せずに、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使用します。保存した会話を後で再開できます。ファイルとブランチはディスク上の通常の Git 状態として残ります。

会話では、プロンプトとエージェントの応答、ファイルを調査して編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を始めて変更を依頼する

新しい会話を始め、機能の実装に取りかかります。

1. Codespace に戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を使い、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを見つけ、必要なコードを作成します。Copilot CLI を使って、新しい機能を追加できました。

## 差分をレビューする

小さな変更であっても、AI が生成した変更はすべてマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたファイルをすべて確認します。
2. ゲーム カードに評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. どちらの状態もテストで確認されていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら、<kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> ほかの生成 AI ツールと同様に、Copilot は確率的に動作するため、生成されるコードは毎回異なる場合があります。特定の実装と一致することではなく、動作を確認してください。

## 変更を確認する

コードを読んだだけで動くと決めつけず、Copilot に Web サイトを起動してもらい、Codespaces でポート フォワーディングされたブラウザーから更新された UI を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプル リクエストを開いてマージする

機能を作成できました。次は、プロジェクトにコードをマージするためのプル リクエスト (PR) を作成します。

1. 既定のエージェントに変更を commit するよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存の commit を push し、PR の URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLI に表示された URL を選択して PR を開きます。
4. 変更されたファイルとチェック結果を確認します。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って、最初の変更をリリースしました。具体的には、次のことを行いました。

- 機能ブランチで目的を絞った Copilot の会話を始めました。
- ゲーム カードに小さな変更を加えるようエージェントに指示しました。
- `/diff` で変更をレビューしました。
- アプリを起動し、ポート フォワーディングされたブラウザーで星評価を確認しました。
- 最初のプル リクエストを開いてマージしました。

次は、[フィルタリング issue から始め、Plan と Autopilot の各モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
