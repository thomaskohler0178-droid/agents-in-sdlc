---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーからフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを使ったり、Copilot に追加の自動化機能を提供したりする必要があります。ここで MCP サーバーが役立ちます。MCP サーバーを使うと、Copilot は CLI に組み込まれている機能以外のツールやサービスも利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を確認します。
- まだ使用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認します。

## シナリオ

ユニット テストとエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot が作業中のサイトをユーザーのように利用できるようにすれば、変更が期待どおりに動作するかを、より確実に確認できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、エージェントは最新の情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

これらのツールやリソースは、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じて利用します。各 MCP サーバーは、エージェントがアクセスできるツールとリソースのセットを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プル リクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ってブラウザーを自動操作できます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献できるように [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、ほかの依存関係と同じように扱ってください。使用前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーのようにサイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、セットアップを飛ばして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP サーバーの追加画面を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd>、次に <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を確認させる

これまでは、機能が期待どおりに動作することを手動で確認しました。今度は Copilot で利用できる Playwright MCP サーバーを使って、同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は MCP サーバーを自動的に判断するため、明示的に指定しなくてもかまいません。ただし、使用するツールが分かっている場合は、プロンプトで伝えると結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで操作を行い、機能を確認する様子を見ます。
3. レポートを読み、すべて期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作した後、サーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。振り返ると、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を確認しました。
- まだ利用できない場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認しました。

次は、スキルとブラウザー ツールを専門的な役割にまとめる [QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
