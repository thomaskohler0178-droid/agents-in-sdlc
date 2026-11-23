---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、対象を絞った指示、ベスト プラクティス、コーディング パターン、専門知識を Copilot に提供できます。チームの知識を再利用可能なエージェントにまとめることで、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、セキュア コーディングに従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントなどを作成できます。

カスタム エージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept] には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを調整しながら作業します。このラボのアクセシビリティ エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビュー、更新します。その作業で、プル リクエスト ワークフローやテスト実行を管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを使えるようにしたいと考えています。最近のフィードバックでは、現在のダーク テーマは文字と背景のコントラストが不十分で、読みづらいという意見がありました。このアクセシビリティの問題に対処するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、カスタム エージェントを使って機能をすばやく実装します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Codespace に戻り、アクセシビリティ用のカスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、アクセシブルな Web サイトのコード生成に関する中核的な責務、ベスト プラクティス、HTML/CSS/JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントのドロップダウンには、`.github/agents` で定義されたカスタム エージェントが表示されます。エージェントを選択すると、その指示とツールに沿ったチャット セッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

1. Chat ビューのエージェント ドロップダウンで、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを開きます。既定では、現在のエージェント (**default**) が表示されます。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクト ファイルを編集します。編集ごとに、ファイル パスとレビューできる差分がチャットに表示されます。
6. 編集が適用されると、**Files changed** の表示が更新され、このセッション中にエージェントが変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連するテストなどの既存ファイルを編集します。

次の演習では、作業中のセッションをレビューし、方向付けます。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]は、特定のタスクや分野に合わせた専門的な AI アシスタントです。カスタム エージェントにチームの知識と標準をまとめ、Copilot が作業を効果的に進めるために利用できます。

この演習では、次のことを行いました。

- カスタム エージェントの定義方法を確認しました。
- Copilot Chat の agent mode でカスタム エージェントを使いました。

次は、同じセッションにライト モードの切り替えを追加するよう依頼しながら、[エージェントの作業を監視して方向付けます][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの設定][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
