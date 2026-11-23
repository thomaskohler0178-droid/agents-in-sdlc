---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。重要な方法の 1 つが、コード生成のガイダンスを提供する[指示ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどう渡るかを確認します。
- *現在の*指示を使って、フィルタリング用の最初のデータ スライス (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再送信し、生成コードが新しい標準を取り入れる様子を確認します。
- 指示の更新とフィルタリングの基盤を `main` に commit し、次の演習で新しい指示を利用できるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、指示の更新後に動作の傾向がどう変わるかを確認します。出力を 1 文字ずつ一致させることが目的ではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。たとえば次のとおりです。

- データ レイヤーには必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な印象にします。
- TSDoc の doc comment を使ってコードにドキュメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿って作業できるよう、必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コンテキストや設定を Copilot に渡し、コーディング スタイルや要件を理解させられます。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定のファイル向けの指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に渡される単一の指示ファイルです。技術スタック、開発中の内容、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類向けに作成します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニット テストの追加に関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示ファイルと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
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
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリ ルートからの相対 glob を設定します。この例では、`**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールし、`unit-tests.instructions.md` などのほかの指示ファイルや、既存のプロジェクト ファイルへのリンクを確認します。これにより、大きな指示を小さな再利用可能なファイルに分け、Copilot にコード例を示せます。ここにあるパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc doc comment とファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

カスタム指示の効果を確認するため、現在の指示を使ってプロンプトを送信します。その後、指示を更新し、同じプロンプトを再送信して違いを確認します。

1. Copilot が必要なコンテキストだけを使えるよう、前の演習のエディター タブを閉じます。
2. `src/lib/publishers.ts` を開き、Copilot にヘルパーの配置場所を示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、ファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパー ファイルとテストなどのコードを更新する様子を確認します。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、型付きの publisher 配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案されたコードに TSDoc doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。重要なのは指示を更新した後の一貫性の向上です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を提供します。コードの提案を改善するため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の標準は記載されていますが、コード内ドキュメントのルールがありません。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc doc comment を指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルは全体的なガイダンスを記載するリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。ここで追加した doc comment ルールのような、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再送信して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して入力内容をクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準に従うよう指示し、次の演習で利用する実際のコードに適用しました。

## 指示を commit してブランチを push する

指示ファイルもほかのリポジトリ内ファイルと同じように、ソース管理で管理します。変更を commit し、ブランチをリポジトリに push します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドで指示の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤作成で Copilot が変更したすべてのファイルを stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチを push したので、バックログにあるドキュメント標準の issue に関連付けてプル リクエストを作成します。GitHub ツールはプロジェクトに接続済みのため、Copilot に issue を見つけ、issue をクローズする PR を作成してマージするよう依頼できます。

> [!NOTE]
> この演習では Copilot に GitHub 上の操作を初めて依頼します。プロジェクト テンプレートには GitHub ツールがすでに設定されています。Copilot がツールを初めて使うとき、VS Code で GitHub へのサインインが求められる場合があります。画面の案内に従ってください。後の演習では、この接続の仕組みである Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新と、新しい publishers ヘルパーの基盤の両方を説明する PR を現在のブランチから作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を検索して PR を作成する様子を確認します。
5. Copilot に GitHub 操作を許可するよう求められたら、コマンドを確認して **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ローカル ブランチを `main` に戻します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に GitHub 操作やシェル コマンドを許可するよう求められたら、コマンドを確認して適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプル リクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- *既存の*指示を使ったコード生成を依頼し、Copilot の提案を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再送信し、提案されたコードが新しい標準を取り入れる様子を確認しました。
- 次の演習で利用できるよう、指示の更新とヘルパーの基盤を `main` に commit しました。

次は、コードベース全体に機能を追加するときに [agent mode で指示を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
