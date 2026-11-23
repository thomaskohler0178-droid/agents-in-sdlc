---
title: "Lição 0 - Pré-requisitos"
description: "Crie sua cópia do Tailspin Toys e prepare um GitHub Codespace para o workshop do Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de começar as lições do Copilot CLI, prepare tudo. Você criará sua própria cópia do repositório Tailspin Toys e iniciará um [codespace][codespaces]. O terminal integrado dele será usado para instalar e executar o Copilot CLI na próxima lição.

Nesta lição, você vai:

- criar sua própria cópia do projeto Tailspin Toys a partir do modelo.
- criar um Codespace e confirmar que o projeto está pronto.

## Configure o repositório do laboratório

Você trabalhará na sua própria cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório de modelo][tailspin-template]. O novo repositório contém todos os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [modelo da Tailspin Toys][tailspin-template].
2. Crie sua própria cópia selecionando **Use this template** e depois **Create a new repository**.
3. Se você estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções fornecidas pelas pessoas mentoras. Caso contrário, crie o novo repositório em uma organização na qual você tenha acesso ao GitHub Copilot.
4. Anote o caminho do repositório que você criou (`organization-or-user-name/repository-name`), pois você o usará mais adiante no workshop.

> [!NOTE]
> Ao criar o repositório a partir do modelo, um backlog de issues do GitHub é criado automaticamente. Você trabalhará com essas issues durante o workshop — não é necessário criar nenhuma manualmente.

Use uma cópia nova do modelo do workshop. Ela inclui instruções do repositório, código do aplicativo, testes, uma skill `quality-checks` e o backlog que você usará. Se usar uma cópia antiga, confirme com a pessoa que conduz o workshop se ela contém os arquivos necessários.

## Crie um Codespace

Em seguida, você usará um Codespace para concluir o workshop.

O [GitHub Codespaces][codespaces] oferece um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele fornece um editor completo com suporte a várias linguagens de programação, extensões e ferramentas.

1. Acesse o repositório recém-criado.
2. Selecione **Code**.
3. Selecione a guia **Codespaces** e depois **Create codespace on main**.
4. Aguarde a conclusão da configuração do Codespace. O modelo instala as dependências do projeto, o Chromium do Playwright e o banco de dados local.
5. Se a solicitação **Do you trust the authors of the files in this folder?** aparecer, selecione **Trust Folder & Continue**.
6. Abra um terminal na raiz do repositório e inicie o aplicativo:

   ```bash
   npm run dev
   ```

7. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser** e confirme que o site Tailspin Toys carrega.
8. Volte ao terminal e pare o servidor de desenvolvimento pressionando <kbd>Ctrl</kbd>+<kbd>C</kbd>.

## Resumo e próximas etapas

Tudo pronto! Nesta lição, você:

- criou sua própria cópia do projeto Tailspin Toys a partir do modelo.
- criou um Codespace e confirmou que o projeto estava pronto.

Em seguida, você [instalará o GitHub Copilot CLI][next-lesson] no Codespace e o autenticará com sua conta do GitHub.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
