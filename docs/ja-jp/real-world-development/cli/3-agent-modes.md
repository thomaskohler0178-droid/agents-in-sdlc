---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan でアプローチを決め、Autopilot で issue からフィルタリング機能を実装し、Interactive mode で結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作するため、適切な方法で必要なものを構築できます。このレッスンから数回にわたり、一般的なエージェント駆動の開発プロセスを進めます。issue をもとに機能を作り、コードの妥当性と機能の動作を確認し、最終的にプロジェクトへマージします。

> [!NOTE]
> 機能開発を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは主な概念に集中できるよう簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot の会話を始めます。
- Plan mode で要件を定義します。
- Autopilot mode で機能を実装します。
- コードをレビューします。
- ポート フォワーディングされたブラウザーで機能を手動検証します。

この機能の開発を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がゲームをカテゴリや発行者で絞り込める必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細もあります。Plan mode でこれらの判断を決めてから、範囲を明確にした実装を Autopilot に許可します。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、基本がさらに重要になります。多くの開発者は、次のような流れで作業します。

1. 実装する内容を記載した issue から始めます。
2. 構築する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードがレビューされ、継続的インテグレーションが成功したら、マージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、ほとんどは上記の流れに沿っています。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビュー プロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan mode で計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルタリングの作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング用ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーを使って、**Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub 上の issue やプル リクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、機能やタスクの実装を Copilot に任せる前にアプローチを決められます。複雑な作業では、まず計画に時間をかけることをお勧めします。Plan mode に切り替えて、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan mode に切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、実装方法を検討しながら計画を作成します。

3. フィルタリングの動作について質問された場合は、希望に沿って答えます。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を上下にスクロールして確認します。
6. 判断と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を許可する

計画を作成してレビューしたら、Copilot に実装を任せます。Autopilot では、Copilot が完了したと判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様のラベルの選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まることがあります。必ず計画を先に確認してください。Copilot から依存関係が不足している、またはポートが競合していると報告された場合は、チェック完了と見なす前にセットアップの問題を解決してください。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同じようにマージ前にレビューします。コードを確認し、サイトを起動して、問題なく動作することを確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive mode に切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を issue および計画で決めた内容と照らし合わせます。
4. コードを確認したら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を試す

コードの確認はできましたが、実際に動くでしょうか。前と同じようにアプリを起動し、Codespaces のポート フォワーディングを使ってサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリと発行者のフィルタリング、およびその組み合わせを試します。
4. リセットと該当なしの場合の動作が、issue と合意内容に沿っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot の会話を始めました。
- Plan mode で要件を定義しました。
- Autopilot mode で機能を実装しました。
- コードをレビューしました。
- ポート フォワーディングされたブラウザーで機能を手動検証しました。

次は、コードが文書化されたプラクティスに沿うよう、[カスタム指示を使用します][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
