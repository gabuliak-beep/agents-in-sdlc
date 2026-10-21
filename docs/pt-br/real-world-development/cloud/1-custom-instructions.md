---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de uma maneira específica — ou há informações de contexto que o Copilot deve conhecer —, disponibilize esse contexto. Os [arquivos de instruções][instruction-files] fornecem orientações para que o Copilot entenda não apenas *o que* deve fazer, mas também *como*.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções por caminho;
- adicionar um padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente das trilhas do VS Code e do CLI, você não fará aqui um prompt de *antes e depois*: o agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, por isso é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests produzidos pelo agente.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar tema escuro e ter uma aparência moderna.
- O código deve conter comentários de documentação TSDoc.
- Um bloco de comentários no início de cada arquivo deve explicar sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências para que o Copilot entenda melhor o estilo de código e os requisitos. Você pode especificar convenções, bibliotecas e comentários preferidos, criando instruções para todo o repositório ou para tipos específicos de arquivo e tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um arquivo único enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações gerais relevantes para a maioria das solicitações, como a stack, o que está sendo desenvolvido, boas práticas e orientações globais.
- `.github/instructions/*.instructions.md`, arquivos que podem orientar tarefas ou tipos de arquivo específicos, como TypeScript ou Astro, componentes de interface ou testes.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Boas práticas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está fora do escopo deste exercício. Os exemplos do projeto demonstram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado em orientações do projeto, como sua finalidade, estrutura e padrões globais de código.
- Use arquivos `*.instructions.md` para orientações específicas de tipos de arquivo, como testes unitários, componentes Astro e camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos do que o código deve ou não deve fazer.

Não há uma única maneira correta de criar arquivos de instruções ou de usar IA. A experimentação ajudará a descobrir o que funciona melhor para o projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto robusto de arquivos de instruções. Este projeto tem arquivos para várias tarefas, incluindo [atualizações da interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa opção de um jeito diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição correspondente explicará como usar o recurso.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos no repositório: há um `copilot-instructions.md` principal e vários arquivos `*.instructions.md` para tarefas específicas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a descrição breve do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas de **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Explore a pasta `.github/instructions`. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros temas.
4. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` no início define um glob relativo à raiz do repositório. Aqui, ele corresponde a arquivos TypeScript de teste, como `**/*.test.ts`.
5. Observe as instruções para criar testes unitários.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e vá ao final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Isso permite dividir instruções extensas em arquivos menores e reutilizáveis, usando caminhos relativos ao arquivo de instruções.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de código, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará orientações para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Adicionar um novo padrão ao repositório

Você fará uma única edição: adicionar um padrão geral para que a documentação fique no código em comentários TSDoc e em um cabeçalho de comentário no nível do arquivo. O agente de nuvem usará essa orientação nas tarefas atribuídas a ele mais adiante.

Antes de editar, crie uma branch. O agente de nuvem lerá as instruções da branch de destino do issue, mas você fará commits organizados:

1. No terminal do codespace, crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abra novamente `.github/copilot-instructions.md`.
3. Localize a seção **Code formatting requirements**, próxima da linha 27. Ela documenta os padrões do projeto, mas ainda não exige documentação no código.
4. Adicione estas linhas Markdown logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, as instruções globais ficam em `.github/copilot-instructions.md`, e arquivos `*.instructions.md` podem atender a linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação recém-adicionada.

## Fazer commit, enviar e mesclar a atualização das instruções

O agente de nuvem lê as instruções da branch de destino do issue. No próximo exercício, quando você atribuir um issue ao Copilot, ele criará uma branch a partir de `main`; portanto, as instruções precisam estar em `main` para que o agente as use.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça merge. No próximo exercício, o agente de nuvem usará essas instruções nos issues atribuídos a ele.

> [!TIP]
> Se preferir trabalhar diretamente em `main` durante este workshop, pule a etapa da branch e faça commit diretamente em `main`. A branch serve para aproximar o workshop de um fluxo real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e adicionou um padrão geral a `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests gerados pelo agente de nuvem nas próximas lições.

Em seguida, você [atribuirá seu primeiro issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
