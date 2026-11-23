---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な環境を準備します。Tailspin Toys リポジトリの自分用コピーを作成し、[Codespace][codespaces] を起動します。Codespace は、指示ファイルの編集や cloud agent が作成した変更のレビューに使います。

## ラボ用リポジトリをセットアップする

コードを作成するため、[テンプレート][template-repository]からリポジトリのインスタンスを作成します。新しいインスタンスにはラボに必要なファイルがすべて含まれ、各演習で使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ リポジトリのページで **Use this template** を選択し、**Create a new repository** を選択して、自分用のコピーを作成します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

   ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. ラボで後から参照できるよう、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

## Codespace を作成する

次に、Codespace を使って演習に取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、必要なサービスを手動でインストールするよりもはるかに短時間で済みます。作成中に GitHub Copilot の機能を確認しておきましょう。

> [!CAUTION]
> 後の演習で Codespace に戻ります。今はブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動作するように構成されています。どちらも必要な前提条件がインストールされているため、スムーズに作業できます。ローカル環境を使う場合は、クローンしたリポジトリを VS Code で開き、表示されたら **Reopen in Container** を選択します。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成し、今後 Copilot cloud agent と作業するための Codespace の作成を開始しました。

## 次のステップ

cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
