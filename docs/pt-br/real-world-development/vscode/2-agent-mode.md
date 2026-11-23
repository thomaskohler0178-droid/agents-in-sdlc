---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma atualização simples de um aplicativo costuma exigir alterações em vários arquivos e a execução de operações como testes. O fluxo de trabalho de uma pessoa desenvolvedora envolve localizar os arquivos necessários, fazer alterações, executar testes, depurar, identificar um arquivo esquecido, atualizá-lo e assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para atuar com mais autonomia no IDE. Ele se comporta de forma semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar uma funcionalidade ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem foram criados para experiências diferentes. O modo de agente trabalha no IDE, o que permite ciclos rápidos de feedback e interação. O agente de nuvem funciona de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- revisar as alterações e os testes gerados pelo modo de agente antes de fazer merge na base de código.

## Cenário

Com o crescimento da lista de jogos, você quer permitir que as pessoas filtrem por categoria e editora. Você já adicionou um helper de editoras no exercício anterior e agora concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer alterações, explore o site Tailspin Toys para entender suas funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. As pessoas podem listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM — não há uma API de backend separada nem um framework de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com pequenos elementos `<script>` do Astro que usam as APIs padrão do DOM.

### Inicie o site

Para facilitar a execução, há uma tarefa `npm run dev` que inicia o servidor de desenvolvimento único do Astro. Você pode executá-la no GitHub Codespace:

1. Volte ao codespace. Você deve estar na branch `main` depois do exercício anterior.
2. No Codespace, abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute este comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, um banner deverá exibir a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar a abertura de `http://localhost:4321` em uma janela do navegador, feche-a selecionando o **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link local no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora hospeda o servidor web.

### Explore o site

Com o site em execução, explore suas funcionalidades principais:

- **Página inicial**: exibe uma lista de jogos de tabuleiro, com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você acessa uma página com mais informações, como título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de iniciar os exercícios a seguir, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para começar uma conversa limpa. O modo e o modelo variam por exercício — cada etapa indica a seleção necessária.

A implementação inicial do site funciona, mas queremos adicionar recursos. Primeiro, vamos revisar o backlog. Ao criar seu projeto a partir do modelo, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot para mostrar esses itens.

1. No menu de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos do terminal e chama ferramentas.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> As pessoas autoras deste workshop não estão recomendando um modelo em particular. Durante a criação do workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções para que as sugestões de código sejam relativamente consistentes. Como os LLMs são probabilísticos, é normal que as sugestões variem.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot poderá usar um comando MCP diferente, mas ainda deverá concluir a tarefa.

3. Envie o prompt a seguir para perguntar sobre o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente para gerar código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo [de instruções de interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções específicos de tipos de arquivo, como Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Assim, é possível reutilizar orientações, dividir conjuntos complexos em partes menores e incluir exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações separadas no aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos com a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes do merge. O modo de agente do Copilot pode executar essas tarefas.

1. Continue na conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado no menu de agentes da exibição Chat.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot para implementar a funcionalidade de acordo com a issue relacionada no backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade, incluindo os helpers da camada de dados, a interface e os testes. Depois, ele começará a alterar os arquivos e executar os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e executar testes. Isso pode levar alguns minutos, conforme a complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** poderão aparecer no editor. Quando o Copilot terminar, você poderá aceitar ou desfazer cada alteração; não é necessário selecionar esses botões enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat solicitando confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e falhas em alguns testes durante o processo. O Copilot alterna entre geração de código e testes até concluir a tarefa sem detectar erros.

   ![Sessão concluída do modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximas etapas

Parabéns! Neste exercício, você usou o modo de agente do GitHub Copilot para adicionar funcionalidades ao site Tailspin Toys. Você aprendeu como:

- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- o modo de agente pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar as alterações e os testes gerados pelo modo de agente antes de fazer merge na base de código.

Agora, vamos [testar sua funcionalidade com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício bônus de exploração — implemente a paginação

Com o crescimento da lista de jogos, será necessário adicionar paginação. Use as habilidades deste exercício e peça ao Copilot para implementar a paginação. Considere:

- seguir as boas práticas existentes, incluindo os arquivos de instruções do projeto.
- decidir se as pessoas usuárias poderão selecionar o tamanho da página ou se ele será fixo.
- fornecer ao Copilot as orientações necessárias para a implementação desejada.
- iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
