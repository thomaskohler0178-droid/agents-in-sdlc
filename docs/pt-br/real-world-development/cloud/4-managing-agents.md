---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nessa central de controle, você vê as tarefas dos agentes que têm pull requests abertos, explora as operações executadas e orienta uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a tarefa de criar um modo de alto contraste, a equipe percebeu que seria útil adicionar também um modo claro. Como o trabalho já envolvia atualizar os estilos do site e adicionar alternância entre modos, a equipe quer incluir essa funcionalidade. Você orientará o agente para garantir que ele implemente o modo claro além do alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Vamos verificar o estado atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá encontrar as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em execução e revise as operações realizadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas listadas variam conforme o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull request que aparece à direita. Ele permite acompanhar o PR e os arquivos alterados.

## Oriente o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot para incluir a alternância do modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à criação do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade na central de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o trabalho comece. Você verá mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot reconhecer o prompt e incluí-lo no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot continuará trabalhando na tarefa atualizada. Ele incorporará a nova solicitação depois de concluir a etapa em andamento no momento em que você enviou a mensagem.

Esse processo levará alguns minutos; aproveite esse tempo para refletir sobre o que aprendeu até aqui.

## Resumo e próximas etapas

Esta lição apresentou a página de agentes do Copilot, a central para acompanhar e orientar as tarefas do agente de nuvem do GitHub Copilot. Nela, você acompanha tarefas ativas e concluídas, revisa o trabalho em andamento e redireciona sessões para alterar o escopo ou fornecer mais orientações.

Você explorou estes conceitos:

- usar a central de controle e a página de agentes para acompanhar tarefas do agente de nuvem.
- redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho de acessibilidade, você pode [iterar nos pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
