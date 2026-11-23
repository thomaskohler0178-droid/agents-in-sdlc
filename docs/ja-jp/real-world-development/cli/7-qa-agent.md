---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルタリング機能を確認しました。次は、明確に定義した QA プロセスを持つカスタム エージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールをどのように扱うかを確認します。
- 再利用可能な品質保証プロファイルを作成して調べます。
- QA エージェントを選択し、フィルタリングの issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタム エージェントを使うと、QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する専門化された Copilot です。プロファイルでは、エージェントの目的、指示、利用できるツールを説明します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

ここまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し実行できるチェックをまとめ、Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告するよう Copilot に指示します。ほかの機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルには、QA で実行するチェックと、その作業範囲を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive mode であることを確認します。
2. 既定のエージェントに、新しいカスタム エージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA の流れと制約をプロファイルに反映したことを確認します。これにより、検証だけを行いたいのに、内容が不完全だったり範囲が広すぎたりするエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は省略できますが、含めるとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件から始め、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. 根拠を示した報告を行い、実装コードを変更する前に確認を求め、変更を commit したりプル リクエストを開いたりしないことを確認します。
5. これらの責務や制約が抜けている場合は、続ける前に既定のエージェントに修正を依頼します。

## issue を QA で確認する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成したら同じフィルタリングの会話を再開し、QA を選択して、会話にある issue と計画の判断を活用できるようにします。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、使用可能なエージェントの一覧にまだ表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業する様子を確認します。
4. 作業が終わったら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門的な役割をワークフローに加え、その結果を確認しました。このレッスンでは、次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールをどう扱うかを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリングの issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、[機能の PR を作成して Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
