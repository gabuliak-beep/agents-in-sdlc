---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou há informações de contexto que o Copilot deve conhecer —, disponibilize esse contexto. Os [arquivos de instruções][instruction-files] são uma maneira importante de orientar a geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções por caminho;
- gerar a primeira fatia de dados para filtragem, um helper de editoras, usando as instruções atuais;
- adicionar um padrão geral em `.github/copilot-instructions.md`;
- repetir o mesmo prompt e observar o código gerado incorporar o novo padrão;
- fazer commit das instruções e da fatia de filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é não determinístico. O objetivo é observar a tendência após atualizar as instruções, não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar tema escuro e ter uma aparência moderna.
- O código deve conter comentários de documentação TSDoc.
- Um bloco de comentários no início de cada arquivo deve explicar sua finalidade.

Com arquivos de instruções, você dará ao Copilot o contexto necessário para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências para que o Copilot entenda melhor o estilo de código e os requisitos. Você pode especificar convenções, bibliotecas e comentários preferidos, criando instruções para todo o repositório ou para tipos específicos de arquivo e tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um arquivo único enviado ao Copilot em **todas** as solicitações do repositório, com informações gerais como a stack, o que está sendo desenvolvido, boas práticas e orientações globais.
- `.github/instructions/*.instructions.md`, arquivos que podem orientar tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também oferece suporte a arquivos de instruções em outros padrões, como AGENTS.md e CLAUDE.md. O suporte varia conforme o ambiente; consulte a [referência de suporte a instruções personalizadas][custom-instructions-support].

### Boas práticas para gerenciar arquivos de instruções

Os exemplos do projeto demonstram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado em orientações gerais, como a descrição e a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para tipos específicos, como testes unitários, componentes Astro e camada de dados, ou para tarefas específicas.
- Use linguagem natural, orientações claras e exemplos do que o código deve ou não deve fazer.

Não há uma única maneira correta de criar arquivos de instruções nem de usar IA. A experimentação ajudará a descobrir o que funciona para o projeto.

> [!TIP]
> Projetos que usam o GitHub Copilot devem ter um conjunto robusto de arquivos de instruções. Este projeto inclui arquivos para várias tarefas, como [atualizações da interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode gerar arquivos de instruções. Cada ambiente oferece essa opção de um jeito diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI.
>
> Para modelos e exemplos, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizados deste projeto

O projeto inicial já inclui um arquivo principal `copilot-instructions.md` e arquivos `*.instructions.md` para várias tarefas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas de **GitHub Actions Workflows**.
3. Explore `.github/instructions`. Há instruções para Astro, Drizzle, testes e outros temas.
4. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` define um glob relativo à raiz do repositório, que corresponde a arquivos TypeScript de teste, como `**/*.test.ts`.
5. Observe as instruções para criar testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos existentes do projeto. Os caminhos são relativos ao arquivo de instruções.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` descreve os padrões de código, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará orientações para comentários TSDoc e cabeçalhos de comentário.

## Criar uma branch para as alterações

Vamos criar uma branch para as alterações.

1. Volte ao codespace.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa especificará as opções relevantes.

Para observar o impacto das instruções, envie um prompt com o arquivo atual. Depois de atualizá-lo, você enviará o mesmo prompt novamente.

1. Feche as guias de editor abertas nos exercícios anteriores para limitar o contexto.
2. Abra `src/lib/publishers.ts` para indicar ao Copilot onde o helper deve ficar.
3. Na lista suspensa de agentes da visualização Chat, selecione **Agent**.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará as alterações, que podem incluir o helper e seus testes.
6. Observe que o helper proposto usa tipos, recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicável a `src/lib/*.ts`.
7. Observe que o código proposto **não tem** comentários TSDoc nem cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é não determinístico, talvez ele adicione comentários mesmo sem orientação explícita. Tudo bem: o objetivo é observar a consistência depois de atualizar as instruções.

## Adicionar um novo padrão ao repositório

`copilot-instructions.md` fornece informações gerais ao Copilot. Vamos documentar os padrões de código para melhorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize **Code formatting requirements**, próxima da linha 27. Ela não tem uma regra de documentação no código.
3. Adicione estas linhas Markdown logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como na lição anterior, instruções globais ficam em `.github/copilot-instructions.md`; arquivos `*.instructions.md` podem atender linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é apropriado para padrões gerais, como comentários de documentação.

## Repetir o prompt e observar as alterações

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie novamente o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentário semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função agora tem um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo a código que será usado no próximo exercício.

## Fazer commit das instruções e enviar a branch

Arquivos de instruções são controlados pelo mesmo processo de controle de versão que os outros arquivos do repositório. Vamos fazer commit e enviar a branch.

1. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirme que as alterações das instruções e do helper estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit das instruções e de todos os arquivos alterados pelo Copilot para o helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar um pull request

Depois de enviar a branch, abra um pull request associado ao issue de padrões de documentação no backlog. O Copilot pode localizar o issue, criar o PR e fechá-lo com o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot interage com o GitHub em seu nome. O modelo do projeto já configura as ferramentas do GitHub; na primeira utilização, o VS Code pode solicitar autenticação. Siga as instruções. Você aprenderá sobre essa conexão (Model Context Protocol) em outro exercício.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot que encontre o issue relacionado às instruções e abra um PR da branch atual, descrevendo também o helper de editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Quando o Copilot solicitar autorização para executar ações no GitHub, revise o comando e selecione **Allow**, se apropriado.
5. Depois que o PR for criado, peça ao Copilot que faça merge e retorne à branch `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Revise os comandos e selecione **Allow** para as ações no GitHub e os comandos de shell apropriados.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot usa arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar a saída com as instruções existentes;
- adicionar um padrão geral a `.github/copilot-instructions.md`;
- repetir o prompt e observar o código proposto adotar o novo padrão;
- fazer commit das instruções e do helper em `main`, preparando a base para o próximo exercício.

Em seguida, você usará essas instruções no [modo de agente][next-lesson] para implementar um recurso.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas do GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
