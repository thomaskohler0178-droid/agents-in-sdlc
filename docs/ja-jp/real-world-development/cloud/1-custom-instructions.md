---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、何をしてほしいかだけでなく、どのように進めてほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどう渡るかを確認します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では指示を変更する前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を非同期に処理するため、リアルタイムで並べて効果を示すのが難しいためです。後の演習で cloud agent が作成したプル リクエストをレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。たとえば次のとおりです。

- データ レイヤーには必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な印象にします。
- TSDoc の doc comment を使ってコードにドキュメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿って作業できるように必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コンテキストや設定を Copilot に伝え、コーディング スタイルや要件を理解させられます。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定のファイル向けの指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に渡される単一の指示ファイルです。技術スタック、開発中の内容、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、タスクやファイルの種類ごとに作成します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニット テストの追加に関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示ファイルと `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。サンプル プロジェクトには代表的な方法が含まれています。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` には、ファイル種別 (ユニット テスト、Astro コンポーネント、データ レイヤー) や特定のタスク向けの指示を記載します。
- 自然言語で明確に書き、望ましいコードと望ましくないコードの例を示します。

指示ファイルの作成方法や AI の使い方に唯一の正解はありません。プロジェクトに合う方法を試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示ファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、surface ごとに方法が異なります。必要な場合は、該当する演習で説明します。
>
> テンプレートを探している場合は、指示ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意されている指示ファイルを読みます。中核となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** に関するガイダンスも確認してください。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリ ルートからの相対 glob を設定します。この例では、`**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールし、`unit-tests.instructions.md` などのほかの指示ファイルや、既存のプロジェクト ファイルへのリンクを確認します。これにより、大きな指示を小さな再利用可能なファイルに分け、Copilot にコード例を示せます。ここにあるパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、現在のコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc doc comment とファイル コメント ヘッダーのルールを追加します。

## リポジトリ標準を追加する

ここで行う編集は 1 つです。コード内のドキュメントを TSDoc doc comment とファイル レベルのコメント ヘッダーで記述する、プロジェクト全体のルールを追加します。後の演習で issue を割り当てると、cloud agent がこのルールを読み取ります。

編集の前に作業ブランチを作成します。cloud agent は issue の対象ブランチから指示を読み取りますが、ここでは変更をきれいに commit しておきます。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、プロジェクト レベルの情報を Copilot に提供します。コードの提案を改善するため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在の標準は記載されていますが、コード内ドキュメントのルールがありません。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc doc comment を指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルは、全体的なガイダンスを記載するリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。ここで追加した doc comment ルールのような、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## 指示の更新を commit、push、マージする

cloud agent は issue が対象とするブランチの指示ファイルを読み取ります。次の演習で Copilot に issue を割り当てると、`main` からブランチが作成されます。cloud agent が指示を読み取れるよう、変更を `main` にマージする必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを開き、マージします。次の演習で issue を割り当てると、cloud agent はこれらの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチを省略して `main` に直接 commit できます。ブランチの手順は、実際のプロジェクトでの作業方法に近づけるために用意しています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りのハーネスで cloud agent が作成するプル リクエストを確認すると、この標準が反映されます。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
