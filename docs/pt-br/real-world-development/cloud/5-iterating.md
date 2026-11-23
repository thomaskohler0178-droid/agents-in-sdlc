---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot para documentar o código, criar uma funcionalidade de jogos relacionados para a equipe de design experimentar e implementar recursos de acessibilidade, incluindo opções de alto contraste e modo claro. Agora, vamos explorar as alterações e, se necessário, enviar comentários ao Copilot para aprimorar o resultado.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a IA generativa. Sempre revise o código gerado e siga o processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para a documentação, os jogos relacionados e a acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para manter o ambiente protegido. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de escrita **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- Os fluxos de trabalho do GitHub Actions precisam de aprovação humana antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revise a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: a adição de documentação ao código. Você fará essa revisão usando a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao explorar o PR, você poderá encontrar um aviso de que o GitHub Copilot está bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a indicação **[WIP]**. Aguarde até que ele termine. Isso pode levar alguns minutos; aproveite o tempo para descansar ou refletir sobre o que aprendeu.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os comentários TSDoc recém-criados e outras documentações. As alterações exatas variam.

    Ao revisar as alterações, procure comentários TSDoc, convenções TypeScript e cabeçalhos de comentário. Eles seguem os arquivos de instruções personalizados vistos no início do Exercício 2.

6. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Solicite alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencioná-lo em comentários — assim como faria com outra pessoa da equipe — no PR ou em comentários em linha no código. O Copilot lerá esses comentários e iniciará outra sessão para resolvê-los. Como o resultado não é determinístico, não há um texto prescritivo a usar. Algumas ideias de solicitações:

- adicionar cabeçalhos de comentário no início de cada arquivo de código, descrevendo brevemente sua finalidade.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README com uma descrição da estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot** como faria com outra pessoa. Use uma das ideias acima ou outra sugestão.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar, deverá haver um novo commit no pull request.
5. Selecione a guia **Files changed** e revise as alterações.

Continue iterando até ficar satisfeito. Depois, você pode marcar o PR como pronto para revisão e fazer o merge na branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise a funcionalidade de jogos relacionados

Agora, vamos voltar ao PR que o Copilot criou para resolver a issue de exibir jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** e revise o código gerado.
5. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns fluxos de trabalho aguardam aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** mude para esta branch no Codespace para testar manualmente a funcionalidade de jogos relacionados. Acesse o Codespace, abra o terminal e execute estes comandos, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou a funcionalidade de jogos relacionados. Como antes, você pode iterar no trabalho do agente de nuvem e pedir atualizações, por exemplo, ajustar a quantidade de jogos relacionados ou adicionar cabeçalhos de comentário e comentários TSDoc. Como a tarefa foi atribuída **antes** da atualização das instruções personalizadas, talvez seja necessário pedir isso em um comentário na guia **Conversation**. O Copilot verá o comentário e iniciará outra sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado. Esse PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado na central de controle durante o Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a indicação **[WIP]**. Aguarde até que ele termine; isso pode levar alguns minutos.

4. Selecione a guia **Files changed** e revise o código gerado.
5. Examine a implementação, prestando atenção especial a:
   - componentes de alternância entre modos.
   - uso do armazenamento local para manter as preferências.
   - alterações de CSS ou estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade (rótulos ARIA, navegação por teclado etc.).
   - código JavaScript/TypeScript que controla a alternância entre modos.
6. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** mude para esta branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute estes comandos, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os modos de alto contraste e claro no navegador. Confirme se eles funcionam e se as preferências continuam salvas depois de recarregar a página.

Observe como o agente personalizado de acessibilidade ajudou a orientar a implementação segundo boas práticas. Se encontrar problemas ou melhorias de acessibilidade, mencione **@copilot** em um comentário e peça as atualizações, como nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem funciona melhor quando você pode delegar itens reais do backlog e se concentrar em outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes dos jogos.
- implementar paginação na página de listagem de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu a trilha do agente de nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizadas** do repositório para observar posteriormente o efeito deles nos resultados do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando opções de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como central de controle** para acompanhar e orientar a sessão de acessibilidade.
- **Revisou e iterou nos pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximas etapas

Você concluiu a trilha do agente de nuvem. Para continuar explorando, as outras trilhas complementam o que praticou:

- 🖥️ **[Trilha do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Trilha de CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente de QA personalizado, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

No seu repositório, experimente estas ideias:

- Atribua ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
