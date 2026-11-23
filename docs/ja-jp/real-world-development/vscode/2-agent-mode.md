---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイル変更やテスト実行などが必要です。開発者は、必要なファイルを探し、変更し、テストを実行し、デバッグし、更新漏れのファイルを見つけて、さらに変更を加えます。作業は次々と続きます。

ここで Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に作業するように設計されています。開発者のように既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、問題が見つかった場合は自動的に修正します。Agent Mode を使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けです。agent mode は IDE 内で動作し、すばやいフィードバックと対話を行います。cloud agent はチーム メンバーのように非同期で作業し、issue とプル リクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定して、複数のファイルを連携して変更する方法。
- GitHub Copilot Agent Mode で UI とデータ レイヤーにまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行者で絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。次は Copilot Agent Mode で、残りのデータ レイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームを一覧表示し、詳細を確認できます。単一の Astro アプリであり、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次のように実行します。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. `main` を更新し、フィルタリング用ブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` のブラウザー ウィンドウを開くか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Codespace 内で Web サーバーをホストするためのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主なページは次のとおりです。

- **ホーム ページ**: タイトル、画像、説明を含むボード ゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行者、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると issue のバックログが自動的に作成されるため、GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** は複数ファイルにまたがる変更を計画して実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは、特定のモデルを推奨するものではありません。作成時には Claude Sonnet 4.5 を使ったため、手順に記載しています。提案の一貫性を高め、スムーズに体験できるようにするためです。ただし、LLM は確率的に動作するため、ワークショップの記載と異なる提案が出る場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは同様に完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、コマンドで issue の一覧を取得します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も役立つと考えた順に、項目を優先順位付けしたことを確認します。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に提供する指示ファイルを確認します。Web サイトに機能を追加する際の指針が含まれる [user interface (UI) ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの指示ファイルへのリンク

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、複雑な内容を管理しやすい単位に分け、Copilot に例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリング機能の完成には、最低でも次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) に残りのフィルタリング ロジックを追加または調整する
- フィルタリングの動作を確認するテストを追加または更新する
- ゲーム一覧ページにフィルタリング UI を追加する

コードベースにマージする前に、テストを実行して成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** を選択して指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログ内の関連 issue に基づいて機能を実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要なファイルを見つける様子を確認します。データ レイヤーのヘルパー、UI、テストを特定し、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調べている様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更してテストを実行します。タスクやコードベースの複雑さによっては、数分かかることがあります。作業中にコード エディターで **Keep** と **Undo** が表示される場合があります。Copilot が完了するとすべての変更に対する **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![テストを実行してよいか確認する Copilot Chat のダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はエラーを検出しなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了済み Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot を含む AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. すべて動作することを確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。この演習では、次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータ レイヤーにまたがる機能を実装する方法。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プル リクエストを開きます。

### ボーナス演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだ方法を使い、Copilot にページネーションを実装するよう依頼します。

- 既存の指示ファイルを含め、現在のベスト プラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定値にするかなど、ページネーションの動作を決めます。
- 希望する実装に必要なガイダンスをプロンプトで Copilot に伝えます。
- 希望する結果になるまで、追加の変更やコンテキストを伝えて Copilot と反復する必要がある場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
