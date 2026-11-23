---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調べ、変更を行い、プル リクエストを作成します。その間、別の作業を進められます。

各演習では、まず cloud agent が従うカスタム指示を追加し、GitHub issue を割り当てて実装を依頼します。カスタム エージェントを確認して使い、進め方を調整します。agents ダッシュボードからセッションの進捗を監視して指示を出し、最後に作成されたプル リクエストをレビューしながら結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | issue を GitHub Copilot cloud agent に割り当てる |
| [3. カスタム エージェント][ex3] | 専門エージェント | カスタム エージェントを確認して使う |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視して方向付ける |
| [5. 繰り返し改善する][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して、次の手順を選ぶ |

## 前提条件

このワークショップに参加する前に、次の条件を満たしていることを確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランを利用していない場合でも、認定を受けた学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コード レビュー、Copilot CLI の機能が含まれているため、どのハーネスでも最後まで完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> cloud agent は Copilot Free を除く **Copilot Student、Pro、Pro+、Business、Enterprise** のすべてのプランで利用できます。Copilot Business または Enterprise の場合、組織の管理者が有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
