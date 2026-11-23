---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プル リクエストのレビュー、エージェントを使った作業管理を繰り返しながら進めます。

## シナリオ

ボード ゲームのクラウドファンディングを手掛ける架空の会社 Tailspin Toys に、新しい開発者として参加します。チームのバックログは GitHub issue として登録済みで、すぐに着手できます。フィルタリングやページネーションなどの機能開発に加え、アクセシビリティやコーディング標準の改善も含まれています。

## 環境を選ぶ

GitHub Copilot はさまざまな作業環境で利用できます。自分の開発スタイルに合った環境を選んでください。どのワークショップも独自のセットアップ手順から始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタム エージェント、Playwright MCP、Agent Merge、実用的なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub issue とプル リクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を指示し、進行状況を監視して、作成されたプル リクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat の agent mode、MCP サーバー、カスタム エージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
