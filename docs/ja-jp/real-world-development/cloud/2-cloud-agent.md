---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなど、さまざまな形があります。GitHub Copilot cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。これにより、手間のかかるタスクを任せて開発を加速し、より大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 範囲を明確にした issue の重要性
- Copilot への issue の割り当て

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託先がドキュメントを適切に整備しておらず、実際にはほとんど存在しません。まず、アプリケーションのすべてのエクスポート関数に TSDoc doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを表示し、閲覧を続けられるようにします。完成された実装はまだ必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度の高い issue がほかにあります。

どちらも優先順位が下がりやすいタスクの例で、Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期に作業している間、開発者は別のタスクに取り組み、後から Copilot の変更をレビューして期待どおりか確認できます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。[GitHub issue を Copilot に割り当てる][assign-issue]方法などがあります。割り当てると、Copilot は進捗を追跡するドラフト プル リクエストを作成し、環境をセットアップしてタスクを開始します。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プル リクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見単純な操作でも、詳しく見ると複雑さが分かります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に進めながら学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、ここで確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認しておくと、後で影響を判断できます。

GitHub の Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter が `**/*.test.ts` ファイルに指示を適用していることを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、作成されたプル リクエストで TSDoc doc comment、TypeScript の規約、コメント ヘッダーを確認します。これらは指示ファイルから反映されます。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

コードを作成するには、通常、適切な状態を保つための環境とセットアップ スクリプトが必要です。Copilot にタスクを割り当てる場合も同様で、cloud agent はソフトウェア エンジニアのように作業します。

cloud agent は [GitHub Actions][github-actions] を使って環境内で作業します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を利用できるよう環境をカスタマイズできます。このラボでは学習を進めやすくするため、あらかじめ設定されています。単一の Astro アプリで必要な Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシード済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同じ構造ですが、次の点に注意してください。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプル リクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古い、またはドキュメントが存在しないプロジェクトも多くあります。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意としているため、この issue は cloud agent に割り当てるのに適しています。後の演習で作業結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプル リクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかる場合があります。Copilot は先ほどのワークフローを実行して環境を作り、プロジェクトを分析して、問題への最適な取り組み方を判断しています。

13. リストと、Copilot が実行するタスクを確認します。
14. プル リクエストのタイムラインを下へスクロールし、Copilot が issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション ビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するには、ページを更新する必要がある場合があります。

16. ライブ セッションをスクロールして Copilot の作業を確認します。コードを調べて状態を理解し、計画を考えて判断した後、コードを作成する過程が表示されます。

作業には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期に実行し、その間に別の作業を進められるようにすることです。この機能を活用して、Copilot にもう 1 つのタスクを割り当てた後、アプリに機能を追加する作業に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けて、エージェントが別のタスクに取り組んでいる間に自分の作業を進められることです。デザイン チームが求めている関連ゲームの表示は、短時間で実装できるかもしれませんが、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![issue の作業中であることを示すため、Copilot が目の絵文字を使っている画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタム エージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を使うと、非同期で処理する issue を Copilot に割り当てられます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに活用できます。

このレッスンでは、次のことを確認しました。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 範囲を明確にした issue の重要性
- Copilot への issue の割り当て

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで学んだカスタム指示も使えます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
