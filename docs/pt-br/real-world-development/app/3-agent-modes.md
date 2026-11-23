---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando uma pequena funcionalidade ao projeto, mas alterações maiores exigem um processo mais robusto. Felizmente, o aplicativo GitHub Copilot foi criado para trabalhar com o fluxo existente de uma organização e ajudar a criar as soluções certas da maneira certa. Nesta e nas próximas lições, você seguirá um processo típico de desenvolvimento orientado por agentes: começará com uma issue para criar uma funcionalidade, confirmará que o código é válido e que a funcionalidade funciona como esperado e, por fim, fará o merge ao projeto.

> [!NOTE]
> Você continuará o fluxo de trabalho da funcionalidade usando a mesma sessão. Normalmente, você usaria sessões ou PRs diferentes para os vários tipos de arquivo, mas faremos um atalho para manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma sessão de agente a partir de uma issue do GitHub.
- definir os requisitos no modo **Plan**.
- implementar a funcionalidade no modo **Autopilot**.
- revisar o código.
- validar a funcionalidade manualmente em um canvas de navegador.

Ao continuar este fluxo, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará a validação por MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo e as pessoas visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como a combinação de categorias precisam ser acordados antes da programação. Você usará o modo Plan para resolver essas decisões e, em seguida, autorizará uma implementação com escopo definido pelo Autopilot.

## Contexto

Introduzir agentes de programação de IA no fluxo de desenvolvimento não muda os fundamentos — na verdade, eles se tornam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo semelhante:

1. Abrir uma issue registrada com os detalhes do trabalho necessário.
2. Planejar o que será criado.
3. Desenvolver e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois que o código for revisado e o processo de integração contínua for concluído com êxito, fazer merge do código.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria segue uma variação do fluxo listado acima.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode defini-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua entrada antes de continuar.
- **Plan**: o agente cria um plano primeiro. Você revisa e aprova o plano antes da execução.
- **Autopilot**: o agente trabalha de forma totalmente autônoma, escrevendo código, executando testes e iterando sem aguardar sua entrada.

Comece no modo Plan, revise o plano e use o Autopilot para implementá-lo.

## Inicie uma sessão a partir da issue

Antes de começar, confirme que o PR das avaliações por estrelas foi mesclado e que sua branch `main` local está atualizada.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e depois **Issues** na navegação superior do repositório. Abra a issue **Allow users to filter games by category and publisher**. Essa issue predefinida já existe; não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na `main` atualizada.

    ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme que a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente para executar `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique o PATH do aplicativo ou a configuração do gerenciador de versões e feche e reabra completamente o aplicativo. Tente novamente a operação e as verificações com falha; não reinstale o npm apenas porque o agente não consegue encontrá-lo.

## Planeje a funcionalidade de filtragem

O planejamento permite revisar a abordagem antes de o Copilot escrever código. Como você começou pela issue, o Copilot já tem a solicitação como contexto. Envie:

```plaintext
Build this feature.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme se ele inclui a filtragem por categoria e editora, controles acessíveis, alterações na camada de acesso a dados e testes. Discuta os comportamentos que não estiverem claros, como combinar várias categorias ou o que acontece quando nenhum jogo corresponde aos filtros.

O plano deve incluir lint, testes unitários, testes E2E e verificação de tipos com as ferramentas existentes do projeto. Mantenha-o focado em implementar e testar a filtragem; você criará o PR depois de concluir o fluxo de qualidade. Peça alterações no plano antes de aprová-lo e mantenha à mão a URL da issue e quaisquer esclarecimentos acordados para a validação posterior.

## Aprove o Autopilot explicitamente

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Confira o modo ativo em vez de presumir que a aprovação o alterou. Confirme que o indicador mostra **Autopilot**.

O Copilot começará a implementação e iterará pelo plano, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, então revise o plano antes. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas. Pare apenas os servidores que você iniciou.

## Revise e verifique a implementação

Depois que o código for gerado, revise-o antes de fazer merge, como faria com qualquer outro código.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com a issue e com os esclarecimentos aprovados, incluindo combinações de categorias e editoras. Verifique se as alterações seguem as instruções existentes do repositório.
3. Examine os resultados de lint, testes unitários, testes E2E e verificação de tipos. Uma verificação ignorada não conta como aprovação.
4. Resolva as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright compila e serve uma versão de pré-visualização e pode reutilizar um servidor local; confirme que o servidor testado pertence a este worktree, não a uma lição anterior.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site em um canvas de navegador.

1. Use este prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme se os resultados correspondem a ela. Limpe o filtro e selecione uma editora; confirme se os resultados correspondem a essa editora.
4. Combine os filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento acordado para a combinação.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme que a lista completa volta a aparecer.
6. Quando terminar, peça ao Copilot para encerrar o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Mantenha esta sessão para as próximas lições

Ainda não crie nem faça merge do PR de filtragem. Mantenha esta sessão, o worktree e a branch para as Lições 4 a 8. Em seguida, você aplicará instruções personalizadas ao código de filtragem e adicionará melhorias à skill e ao QA antes de criar e fazer merge do PR combinado na Lição 8. Depois de parar o servidor de desenvolvimento, você pode deixar a sessão inativa e retomá-la na próxima lição.

## Resumo e próximas etapas

Você usou modos diferentes de agente para criar e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma sessão de agente a partir de uma issue do GitHub.
- definiu os requisitos no modo **Plan**.
- implementou a funcionalidade no modo **Autopilot**.
- revisou o código.
- validou a funcionalidade manualmente em um canvas de navegador.

Em seguida, você aprofundará como o código é gerado e garantirá que ele siga as práticas documentadas [usando instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
