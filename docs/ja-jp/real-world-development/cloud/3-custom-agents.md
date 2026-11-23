---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` にある Markdown ファイルでエージェントを定義し、対象を絞った指示、ベスト プラクティス、コーディング パターン、専門知識を Copilot に提供できます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、セキュア コーディングに従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントを作成できます。

カスタム エージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記述する Markdown プロンプトを含めます。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept] には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部としてスキルやほかのエージェントを調整します。このラボのアクセシビリティ エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビュー、更新します。その作業で、プル リクエストのワークフローやテスト実行を管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを使えるようにしたいと考えています。ユーザーからは、文字と背景のコントラストが不十分で、現在のダーク テーマが読みづらいという意見が寄せられています。デザイン チームは、ユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、カスタム エージェントを使って機能をすばやく実装します。この演習では次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot に issue を割り当て、カスタム エージェントを使ってハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。Codespace に戻り、ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する責務、ベスト プラクティス、HTML/CSS/JavaScript のコード例、よくある問題が記載されています。

## issue を作成して割り当てる

Mission control では、環境内のエージェントを管理できます。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示を出せます。ハイ コントラスト モードの作成を Copilot に依頼しましょう。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

   ![カスタム エージェントと Accessibility が選択された、cloud agent の割り当て画面。](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]は、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの知識と標準を再利用可能なエージェントにまとめ、Copilot が作業を効果的に進めるために利用できます。

この演習では、次のことを行いました。

- カスタム エージェントの定義方法を確認しました。
- カスタム エージェントにタスクを割り当てました。

Copilot がハイ コントラスト モードを実装している間に、次は mission control から[エージェント セッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使う準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
