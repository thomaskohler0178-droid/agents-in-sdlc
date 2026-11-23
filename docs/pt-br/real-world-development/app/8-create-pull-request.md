---
title: "Lição 8 - Criar e fazer merge do PR da funcionalidade"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. É hora de revisar tudo em conjunto e abrir um pull request. Você fez o merge do pull request (PR) das avaliações por estrelas manualmente; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos a funcionalidade, as atualizações das instruções e da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma sessão e branch, e esse trabalho será incluído neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar o PR completo da funcionalidade e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle da pessoa desenvolvedora.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o pull request e resolve os bloqueios — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e faz o merge assim que o GitHub permite. Ele trabalha em segundo plano, continua funcionando depois que você reinicia o aplicativo e é desativado automaticamente quando o pull request é mesclado.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade, mas ainda precisa de autorização explícita para editar código e fazer merge. Revise as ações permitidas e o trabalho antes de conceder permissão de merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agentes para selecionar **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar a opção no menu suspenso, por si só, não inicia a operação.

O Agent Merge começará o trabalho. Ele poderá:

- criar o pull request com título e descrição.
- mencionar no corpo da descrição a issue relacionada, se a sessão tiver sido iniciada a partir de uma issue.
- fazer rebase ou resolver possíveis conflitos de merge com a branch de destino.
- acompanhar o processo de CI para garantir que todas as verificações sejam aprovadas.
- monitorar o PR em busca de comentários de outras pessoas desenvolvedoras ou da revisão de código do Copilot e atualizar o código para resolver esses comentários.
- opcionalmente, fazer merge automático do PR quando tudo estiver concluído.

Vamos permitir que o Agent Merge também faça merge do PR quando todas as verificações forem concluídas.

5. Revise o diff do PR e as evidências de verificação antes de conceder permissão de merge.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme se há uma marca ao lado de **Merge pull request**.
8. Peça explicitamente ao agente para fazer merge:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Acompanhe o progresso e resolva os bloqueios informados. Antes de continuar, confirme que o status do PR é **Merged**; a permissão de merge habilitada ou um agente em execução não confirmam que o merge foi concluído.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximas etapas

Você automatizou várias partes do processo de desenvolvimento, incluindo gerar, testar e validar código e gerenciar o pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou o PR completo da funcionalidade e as evidências de verificação.
- autorizou o Agent Merge somente depois da revisão e confirmou que o PR foi mesclado.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma maneira mais completa de inspecionar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
