---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、用意されていたフィルタリング issue を見つけました。
- 星評価を追加し、ポート フォワーディングされたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- フィルタリングの issue から始め、Plan mode でアプローチを定義し、Autopilot mode で実装して、Interactive mode でレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリング機能を確認しました。
- 要件、カバレッジ、スキルの結果、ブラウザーでの検証結果を評価する QA カスタム エージェントを作成して選択しました。
- フィルタリングの変更全体をレビューし、その PR に対して Agent Merge を許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## 作成したもの

ワークショップでは、更新済みの `main` からそれぞれ別のブランチを作成し、2 つの PR を段階的に進めました。

1. **星評価**: ゲーム カードに既存の `starRating` と、評価がない場合の状態を表示します。
2. **フィルタリングと品質のワークフロー**: フィルタリング機能を実装し、指示を更新して機能に適用します。`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使用しました。ワークショップを進めやすくするため、この作業は 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA エージェントは、それらの結果と要件、テスト カバレッジの確認をまとめて PR の準備状況を評価しました。

追加のテストは、実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前にコードと根拠を確認し、変更後は影響する検証結果を更新してください。

## ベスト プラクティス

Copilot に提供するコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し実行できるタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築の前にアプローチを考えるには **Plan**、範囲を絞った変更に関わり続けるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を作りたいのか、なぜ必要なのか、どのように進めるのかを明確に伝えると、結果が大きく変わります。

## さらに確認する

基本的なワークフローを学びました。Copilot CLI には、ほかにも確認しておきたい機能があります。

- `/review` でコード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールでも、上達する一番の方法は使い続けることです。本番コード、趣味のコード、何年も考えていてまだ作れていなかった小さなアプリなど、さまざまな場面で活用してください。学んだことをチームと共有し、チームからも学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
