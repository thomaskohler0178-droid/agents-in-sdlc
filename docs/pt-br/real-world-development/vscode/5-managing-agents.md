---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você usa o modo de agente do GitHub Copilot, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições de arquivos à medida que ocorrem. Você pode revisar cada diff no próprio chat, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts de acompanhamento sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivo propostas.
- orientar o agente durante a sessão para adicionar uma opção de modo claro junto com o alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design solicita também uma opção de modo claro para pessoas que acham o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão atual para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A exibição Copilot Chat mostra cada etapa do agente — os arquivos explorados, os comandos executados e as edições propostas. Cada alteração de arquivo aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra o chat para acompanhar as chamadas de ferramenta. Você verá leituras e edições de arquivos, além de comandos de terminal cuja execução poderá ser solicitada.
3. Quando o agente propuser uma edição, o diff aparecerá no chat:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Abra também o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão (por exemplo, para executar um comando do terminal), o agente pausará e exibirá uma solicitação **Allow**. Aprove apenas comandos com os quais você se sinta confortável.

5. Aguarde o agente concluir o modo de alto contraste. Ele deverá exibir uma mensagem final que resume as alterações.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, amplie a mesma conversa para adicionar uma opção de modo claro. Como você continuará na mesma sessão, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo adições para o modo claro.
3. Revise cada nova edição quando aparecer, como fez com o modo de alto contraste. Selecione **Keep** ou **Undo** para cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface das opções de alternância — pare-o com o botão **Stop** ao lado do campo de chat e envie um prompt de esclarecimento.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes ("simplify the CSS — use CSS variables instead of duplicating selectors"), corrigir a direção ("don't add a third toggle, integrate light mode into the existing settings panel") ou ampliar o escopo ("also add keyboard shortcuts for both toggles").

## Revise os arquivos alterados

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se as práticas de acessibilidade do agente personalizado estão presentes: atributos ARIA, navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações pelo painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Nesta lição, você:

- acompanhou o trabalho de um agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local ao longo deste laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
