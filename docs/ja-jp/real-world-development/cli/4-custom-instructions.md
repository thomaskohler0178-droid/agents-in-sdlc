---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進めたい場合は、そのガイダンスを Copilot に伝えます。[指示ファイル][instruction-files]では、必要なコードだけでなく、その構成方法も説明できます。フィルタリング機能を実装したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス単位で適用される指示ファイルがエージェントにどのように渡るかを確認します。
- コーディング標準に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

優れた開発チームと同じように、Tailspin Toys にも開発プラクティスに関するガイドラインと要件があります。次のような内容です。

- コメントではコードを言い換えるのではなく、目的や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。該当する場合は、差し替え可能な `db` 引数についても記載します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変更されたらコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿って作業できるよう、必要な情報を提供します。

## 指示ファイル

カスタム指示を使うと、コンテキストや設定を Copilot に渡し、コーディング スタイルや要件を理解させられます。好みのコーディング規約、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定のファイル向けの指示も作成できます。

指示ファイルは 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に渡される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、言語、ファイル種別、タスクごとのガイドラインを提供します。

> [!NOTE]
> その他の指示形式とそのサポート状況は、環境によって異なります。特定の形式を利用する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

開始しやすいよう、スターター プロジェクトには指示ファイルがすでに含まれています。変更を加える前に、現在の内容を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この指示は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示を適用するファイルを決める glob を設定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな指示を小さく再利用しやすいファイルに分け、Copilot が参照できる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、追加できるガイダンスがあります。新しく生成する TypeScript に TSDoc コメントを追加するよう、中核となる `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. 最後の箇条書きに、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルタリングの会話を終了して再開し、機能のコンテキストを保ったまま新しいガイダンスを読み込ませます。

1. フィルタリングの会話を開いたままの場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチでリポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   求められた場合はフィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパス単位で適用される指示ファイルがエージェントにどう渡るかを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な [quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
