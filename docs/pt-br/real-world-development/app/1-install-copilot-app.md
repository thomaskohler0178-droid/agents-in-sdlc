---
title: "Lição 1 - Instalar o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento orientado por agentes. Ele foi criado com base no GitHub Copilot CLI e integra-se nativamente ao GitHub, então seus repositórios, branches e pipelines de CI funcionam sem configuração adicional. O aplicativo foi projetado para fluxos de trabalho em que você orienta vários agentes em paralelo — cada um em um espaço de trabalho isolado — em vez de executar todo o trabalho por conta própria, além de automatizar tarefas repetitivas. Com o Node.js instalado e a cópia do projeto pronta, o próximo passo é instalar o aplicativo e conectar esse repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta.
- adicionar seu projeto ao aplicativo pelo repositório do GitHub.
- conhecer o espaço de trabalho, incluindo o backlog criado pelo modelo.
- experimentar um chat rápido para saber mais sobre o próprio aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: selecionar issues, executar agentes, revisar alterações e fazer merge de pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**.

## Instale e configure o aplicativo GitHub Copilot

Para usar o aplicativo GitHub Copilot, primeiro instale-o. Há versões para Windows, macOS e Linux. Vamos instalar o aplicativo, autenticar sua conta e adicionar o repositório Tailspin Toys.

1. Em um navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo depois da instalação.
4. Selecione **Sign in to GitHub** e siga as instruções para entrar com uma conta do GitHub.com ou do GHE.com. Se você usa o GitHub Enterprise Cloud com residência de dados, escolha **GitHub Enterprise Cloud** e informe seu nome de host `*.ghe.com` quando solicitado.

   Se você usa o GitHub Enterprise Server, conclua primeiro a integração com sua conta na nuvem. Em seguida, abra **Settings**, selecione **Accounts**, selecione **Add account** e escolha **GitHub Enterprise Server**. Siga as instruções para autorizar o aplicativo no navegador, conforme descrito no [guia de entrada do Enterprise Server][ghes-sign-in].

5. Se o processo inicial oferecer a seleção de repositórios, selecione o repositório Tailspin Toys criado a partir do modelo. Você também pode ignorar essa etapa e adicionar o projeto depois.
6. Quando solicitado, escolha um tema e conclua a configuração inicial.
7. Procure seu repositório em **Sessions**, na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, em seguida, selecione **Add GitHub repository**. Use o proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme que o repositório aparece na barra lateral. No layout usado aqui, pastas de repositório como `tailspin-toys` são agrupadas em **Sessions**, com sessões individuais abaixo de cada repositório.

## Conheça o espaço de trabalho

Com o projeto conectado, conheça o espaço de trabalho. O aplicativo organiza tudo em algumas áreas na barra lateral:

- **New** — permite iniciar uma nova sessão de chat com o Copilot.
- **Pull requests** — exibe seus pull requests por meio da integração nativa com o GitHub. Você pode navegar e filtrar os PRs, verificar o status de CI e revisá-los sem sair do aplicativo.
- **Issues** — exibe suas issues. Você pode navegar e filtrar issues e iniciar uma sessão a partir de uma issue.
- **Automations** — tarefas de agente salvas, executadas em uma agenda ou sob demanda. São úteis para gerenciar listas de tarefas, manutenção regular do projeto e outras atividades repetitivas. O resumo do workshop apresenta automações como uma próxima etapa, não como outro exercício.
- **Customize** — permite adicionar recursos ao aplicativo Copilot por meio de servidores MCP, plugins, skills e outros componentes. Você usará essa área para configurar o Playwright MCP.
- **Chats** — conversas rápidas para perguntas e brainstorming que não precisam de uma branch nem de um espaço de trabalho próprio. Essa seção aparece depois que você inicia o primeiro chat. Se ela não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat.
- **Sessions** — agrupa pastas de repositórios e sessões de agentes. Selecione um repositório, como `tailspin-toys`, para abrir a visualização dele, ou selecione uma sessão abaixo do repositório para retomar a conversa. Cada sessão é executada em um espaço de trabalho isolado, permitindo executar várias sessões sem conflitos. Você iniciará sua primeira sessão ao adicionar avaliações por estrelas.

Ao longo do workshop, você explorará o espaço de trabalho.

> [!TIP]
> Na dúvida, pergunte ao Copilot. Se não souber como fazer algo ou se algo é possível, peça orientação ao Copilot.

### Encontre o backlog criado pelo modelo

Provavelmente não há projeto sem backlog, e a Tailspin Toys não é exceção. Vamos explorar o backlog criado quando você gerou o projeto a partir do modelo.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão individual abaixo dela. Em seguida, selecione **Issues** na navegação superior do repositório.
2. Localize estas issues pelo título, sem presumir seus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler os detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de concluir uma alteração inicial rápida.

> [!NOTE]
> As listas em **Issues** e **Pull requests** são filtradas automaticamente para mostrar apenas itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de outros repositórios, adicione-os ao aplicativo.

## Experimente um chat rápido

Uma ótima maneira de conhecer o aplicativo é usá-lo para aprender sobre o *próprio aplicativo*. Um **chat rápido** é ideal: você pode fazer uma pergunta ou discutir uma ideia sem criar uma branch nem um worktree, portanto não precisa iniciar uma sessão completa.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um novo chat. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá em seguida.
2. Pergunte como funcionam as sessões do aplicativo:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na visualização da conversa. Você verá que cada sessão é executada em seu próprio git worktree isolado — isso permite executar vários agentes em paralelo sem que suas alterações entrem em conflito. Você pode continuar a conversa ou iniciar outro chat a qualquer momento.

## Resumo e próximas etapas

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou seu projeto e explorou o espaço de trabalho. Você aprendeu a:

- instalar o aplicativo e entrar no GitHub.
- adicionar um projeto por meio do repositório do GitHub.
- conhecer o espaço de trabalho e localizar o backlog criado em **Issues**.
- usar um chat rápido para fazer uma pergunta simples e temporária.

Em seguida, você [iniciará sua primeira sessão de agente][next-lesson] e a usará para exibir uma avaliação por estrelas nos cards dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
