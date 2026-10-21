---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *o que* você quer, mas também *como* o código deve ser estruturado. Agora que você desenvolveu a filtragem, vai explorar as instruções usadas pelo Copilot, adicionar um padrão de documentação e aplicá-lo ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções por caminho chegam ao agente.
- atualizar o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- Os comentários devem explicar a intenção e decisões não óbvias, sem apenas repetir o código.
- Funções exportadas em `db/` e `src/lib/` devem documentar finalidade, parâmetros e valores de retorno com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Componentes Astro reutilizáveis devem documentar os contratos `Props`, e os comentários devem permanecer atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Com arquivos de instruções, você dará ao Copilot o contexto necessário para seguir essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências para que o Copilot entenda melhor o estilo de código e os requisitos. Você pode especificar convenções, bibliotecas e comentários preferidos, criando instruções para todo o repositório ou para tipos específicos de arquivo e tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um arquivo único enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações gerais relevantes para a maioria das solicitações, como a stack, o que está sendo desenvolvido, boas práticas e orientações globais.
- `.github/instructions/*.instructions.md`, arquivos que podem orientar tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes.

> [!NOTE]
> O suporte a outros formatos de instruções varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instruções deste projeto

O projeto inicial já inclui vários arquivos de instruções. Vamos explorá-los antes de fazer uma alteração.

1. Volte ao Codespace.
2. No editor do Codespaces (não no terminal), abra `.github/copilot-instructions.md`.
3. Leia a descrição do projeto e as orientações de código. Elas se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos, incluindo as instruções para Astro, a camada de dados Drizzle e testes.
5. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` define um glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe as referências a outros arquivos de instruções e arquivos existentes do projeto.

## Atualizar as instruções da equipe

Vamos atualizar `copilot-instructions.md` para que o código TypeScript recém-gerado tenha comentários TSDoc.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, por volta da linha 35.
2. Adicione este último item:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Usar as orientações atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar a alteração, feche e retome a conversa de filtragem para que o Copilot use a nova orientação sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit`. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está ativa.
3. Peça ao Copilot que aplique a orientação atualizada:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e revise os arquivos TypeScript alterados. Confira os comentários TSDoc recém-gerados e se eles descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot usa arquivos de instruções e aplicou um novo padrão ao recurso. Em particular, você:

- explorou como as instruções do repositório e os arquivos por caminho chegam ao agente.
- atualizou o arquivo de instruções para que os padrões de código sejam seguidos.
- observou o impacto das instruções no código.

Em seguida, você personalizará e executará a skill `quality-checks` para realizar lint e testes.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
