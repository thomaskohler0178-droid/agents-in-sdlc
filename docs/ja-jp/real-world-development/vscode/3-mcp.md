---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode でフィルタリング機能を作成しました。プル リクエストを開く前に、ブラウザーで実際に動くことを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルタリング機能を手動テストさせます。
- ブランチを公開し、フィルタリング作業のプル リクエストを開きます。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部にアクセスできると、さらに便利になります。Copilot が外部のツールやサービスと通信するための標準的な方法が、Model Context Protocol (MCP) です。

![agent mode の内部構成と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

ツールやリソースは、AI エージェントと外部サービスをつなぐ MCP サーバーを通じて利用します。MCP サーバーは既存の API や NPM パッケージなどのローカル ツールとの通信を管理し、AI エージェントがアクセスできるツールやリソースのセットを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、issue、プル リクエストを管理する API にアクセスできます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ブラウザーの移動、フォームへの入力、ボタンの選択などを自動化できます。

ほかにもさまざまなツールやリソースを提供する MCP サーバーがあります。GitHub は、見つけやすさとエコシステムへの貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同じように扱ってください。使用前にソース コードを確認し、発行元を検証し、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで使える MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot が issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できるため、機能をテストできます。

1. `.vscode/mcp.json` で `github` の隣に `playwright` エントリを追加し、次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカル サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグは、Codespace には表示用デスクトップがないため、画面を表示せずにブラウザーを実行するよう Playwright に指示します。

> [!NOTE]
> Tailspin Toys ではエンドツーエンド テストに Playwright を使っているため、通常はブラウザーがすでにインストールされています。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行するよう依頼し、もう一度試します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot が初めてサーバーのツールを使うとき、VS Code はサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** が表示されたら選択し、サーバーを起動して設定を確認します。
2. VS Code からサーバーを信頼するか尋ねられたら、設定を確認して信頼を選択します。

手動で起動する必要はありません。次の手順で Copilot が初めてツールを使うときに、VS Code が起動して信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]から Copilot が実際のブラウザーを操作できます。エージェントはページを開き、移動し、フィルターを適用して、その結果を確認して報告できます。会話を離れずに、機能が期待どおりに動作するかを確かめる方法です。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントはボタン、リンク、リスト項目など、ラベル付きの構造化要素をもとに判断します。支援技術と同じ情報を使うため、簡単な機能確認がアクセシビリティの基本的なチェックにもなります。

サーバーを接続し、アプリを起動した状態で、Copilot に次の内容を依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件に照らして結果を確認してください。問題があれば、追加で質問するか、PR を開く前にコードの修正を依頼します。

> [!NOTE]
> テストするには、アプリを `http://localhost:4321` で実行しておく必要があります。開発サーバーを停止している場合は、プロンプトを送信する前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーをダウンロードする場合があります。不足していると報告されたら、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

機能が動作することを確認できたので、チームがレビューできるようにプル リクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンを使って commit メッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリに push します。

## プル リクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) などから PR を作成できます。ここでは GitHub Copilot に PR を作成してもらい、関連する issue を見つけて紐付けます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot に情報収集や操作を許可します。GitHub ツールを初めて使うときに認証を求められたら、画面の案内に従います。
5. Copilot が issue を検索して該当するものを見つけ、PR を作成したことを確認します。
6. Copilot が作成したリンクを選択して PR を確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- MCP と MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルタリング機能をテストし、公開前に確認しました。
- ブランチを公開して、フィルタリング作業のプル リクエストを開きました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューのような作業を効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
