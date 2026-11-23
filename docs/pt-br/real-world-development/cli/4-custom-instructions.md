---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *que* código você quer, mas também *como* ele deve ser estruturado. Agora que você criou a filtragem, explore as instruções usadas pelo Copilot, adicione um padrão de documentação e aplique-o ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções com escopo por caminho chegam ao agente.
- atualizar o arquivo de instruções para que os padrões de programação sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem um conjunto de diretrizes e requisitos para suas práticas de desenvolvimento. Entre eles:

- Os comentários devem explicar a intenção e as decisões que não são óbvias, em vez de repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, seus parâmetros e valores de retorno com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Os componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- A formatação existente e as orientações de lint devem ser preservadas.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas permitem fornecer contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Você pode especificar convenções de programação, bibliotecas e os tipos de comentários a incluir no código. As instruções podem valer para todo o repositório ou para tipos específicos de arquivo.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações no nível do projeto relevantes para a maioria das solicitações.
- Arquivos `.github/instructions/*.instructions.md`, que fornecem orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instruções varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explore os arquivos de instruções personalizadas deste projeto

O projeto inicial já inclui um conjunto de arquivos de instruções. Vamos explorá-los antes de fazer alterações para observar o impacto.

1. Volte ao Codespace.
2. No editor do Codespaces, não no terminal, abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e as orientações de programação. Essas instruções se aplicam a toda interação com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no topo — ele define um glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e para arquivos existentes no projeto. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e indicar ao Copilot exemplos para seguir.

## Atualize os arquivos de instruções para refletir as orientações da equipe

Os arquivos existentes são um bom começo, mas ainda há uma lacuna. Vamos modificar o arquivo principal `copilot-instructions.md` para garantir que novos códigos TypeScript incluam comentários TSDoc.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar perto da linha 35.
2. Adicione o item a seguir como último item da lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Use as orientações atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções atualizadas, saia e retome a conversa de filtragem para que o Copilot carregue as novas orientações sem perder o contexto da funcionalidade.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para fechá-la. O Copilot CLI salva conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot que aplique as orientações atualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e leia os arquivos TypeScript alterados. Observe os comentários TSDoc recém-gerados e confirme que eles explicam o código com precisão.

## Resumo e próximas etapas

Você explorou como o Copilot recebe contexto dos arquivos de instruções e aplicou um novo padrão à funcionalidade. Você:

- explorou como as instruções do repositório e os arquivos de instruções com escopo por caminho chegam ao agente.
- atualizou o arquivo de instruções para que os padrões de programação sejam seguidos.
- observou o impacto dos arquivos de instruções no código.

Em seguida, você [personalizará e executará a skill reutilizável quality-checks][next-lesson] para garantir que o lint e os testes sejam executados de forma consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
