---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードにドキュメントを追加し、デザイン チーム向けに関連ゲーム機能を作成し、ハイ コントラストとライト モードを含むアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督を受けずに作業するため、安全性を保つためのセキュリティ制約が設けられています。

- Copilot はリポジトリへの読み取りアクセスを持ち、書き込みはコード作成に使うブランチ**のみに**許可されます。
- cloud agent は GitHub Actions 内で動作し、作業用に分離された一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が作成した最初のプル リクエスト (PR)、つまりコードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を開いたときに、GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。Copilot は MCP サーバーへの接続を含む外部リソースへのアクセスが既定で制限されているため、**想定される動作です**。必要に応じて [Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。作業が完了するまで待ちます。数分かかる場合があるため、休憩したり、ここまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選び、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc doc comment やドキュメントなど、更新されたコードを確認します。変更内容は状況によって異なります。レビューでは、演習 2 の冒頭で確認したカスタム指示ファイルによる TSDoc doc comment、TypeScript の規約、コメント ヘッダーを確認します。
6. 更新をレビューして問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローの承認待ちを示すインジケーターがあるか確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに入ったことを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了には数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーに依頼するように、PR またはコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、必ず特定の文面を指定することはできません。たとえば、次のような更新を依頼できます。

- 各コード ファイルの先頭に、役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc doc comment を追加する。
- Astro アプリの構造を説明する README を作成する。

1. **@copilot** をユーザーと同じようにメンションして、生成されたドキュメントの変更をコメントで依頼します。上記の例、またはコードベースのドキュメントに関する別の提案を使います。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新用に新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しい commit が追加されます。
5. **Files changed** タブを選択し、変更をレビューします。

問題がなければ、満足するまで改善を繰り返します。準備ができたら、PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新を確認して問題がなければ、**Conversation** タブに戻って下へスクロールします。
6. ワークフローの承認待ちを示すインジケーターを確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローがキューに入ったことを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了には数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。Codespace に移動してターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を調整する、コメント ヘッダーや TSDoc doc comment を追加する、といった依頼ができます (この作業はカスタム指示を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の mission control から追加を依頼したライト モードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が付いています。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 次の点に注意して実装を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存する local storage の使用
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更
   - ARIA ラベルやキーボード操作などのアクセシビリティ属性
   - モードの切り替えを管理する JavaScript/TypeScript コード
6. 更新をレビューして問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローの承認待ちを示すインジケーターがあるか確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションでワークフローがキューに入ったことを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了には数分かかる場合があります。
10. **任意:** Codespace でブランチを切り替えて、アクセシビリティ機能を手動テストできます。Codespace に移動してターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動し、ハイ コントラストとライト モードの切り替えをブラウザーで試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントは、アクセシビリティのベスト プラクティスに沿って Copilot が機能を実装できるようにガイドしました。改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent には、実際のバックログ項目を渡して別の作業に取り組む方法が適しています。この習慣を身につけるために、リポジトリにいくつか issue を作成して Copilot に割り当てます。たとえば次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、後で cloud agent の出力にどう反映されるかを把握しました。
- issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、実行中のアクセシビリティ セッションを監視して方向付けました。
- Copilot が作成したプル リクエストをレビューして改善し、**@copilot** に変更を依頼して、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、ほかのハーネスも役立ちます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot の各モード、agent skill、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、レビュー済みの 2 つの変更をターミナルからリリースします。

自分のリポジトリでは、次のことを試してください。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野のカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
