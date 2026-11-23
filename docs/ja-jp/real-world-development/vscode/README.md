---
slug: ja-jp/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** を使うと、普段使っているコード エディターで GitHub Copilot を利用できます。Visual Studio Code と GitHub Codespaces で Copilot Chat の agent mode を使い、MCP を通じて外部ツールに接続し、カスタム エージェントを活用します。Copilot はファイル、ターミナル、問題をすべて確認できるため、IDE を離れる必要はありません。

まずカスタム指示を追加し、Copilot がそれに従うことを確認します。次に agent mode を使って、UI、データ レイヤー、テストにまたがるフィルタリング機能を実装します。続いて Playwright MCP サーバーを接続し、Copilot にブラウザーを操作させて機能をテストしてから、プル リクエストを作成します。最後にアクセシビリティ対応のカスタム エージェントを確認して使い、エディターを離れずに Copilot の変更を監視、調整、改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | VS Code でカスタム指示を追加して確認する |
| [2. Agent mode][ex2] | コード生成 | agent mode でフィルタリング機能を構築する |
| [3. Playwright を使った MCP][ex3] | 外部ツール | Playwright MCP サーバーを使い、ブラウザーで機能をテストする |
| [4. カスタム エージェント][ex4] | 専門エージェント | カスタム エージェントを確認して使う |
| [5. エージェントの管理][ex5] | 監視 | エージェント セッションを監視して方向付ける |
| [6. 繰り返し改善する][ex6] | レビュー | Copilot の作業をローカルでレビューし、次の手順を選ぶ |
| [任意: Foundry を組み込む][foundry-toolkit] | AI エージェント | VS Code と Foundry Toolkit でモデルを準備し、エージェントをデプロイしてサイトに接続する (3 モジュール) |

## 前提条件

このワークショップに参加する前に、次の条件を満たしていることを確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] GitHub Codespaces にアクセスできること

> [!TIP]
> 有料プランを利用していない場合でも、認定を受けた学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コード レビュー、Copilot CLI の機能が含まれているため、どのハーネスでも最後まで完了できます。

[callout-student-plan-education]: https://github.com/education/students

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
