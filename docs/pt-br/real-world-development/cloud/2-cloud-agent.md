---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações deixam de enfrentar dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficam no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Quando conclui o trabalho, ele cria um PR em rascunho para revisão de uma pessoa desenvolvedora. Isso transfere tarefas repetitivas e acelera o desenvolvimento, permitindo que a equipe se concentre em questões mais amplas.

Com o agente de nuvem do Copilot, você explorará:

- a personalização do ambiente de geração de código.
- como garantir que as operações sejam realizadas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívida técnica a resolver. A documentação deixada pelos prestadores que criaram a primeira versão do site está em péssimo estado — na verdade, ela está completamente ausente. Como primeiro passo, a equipe quer comentários TSDoc em todas as funções exportadas.

Além disso, a equipe de design quer melhorar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar outros títulos da mesma categoria, para que as pessoas possam continuar navegando. Ainda não é necessária uma implementação refinada; basta algo que permita testes de aceitação da experiência do usuário. Embora seja um bloqueio, há issues de prioridade maior no momento.

Essas tarefas podem ser facilmente adiadas e são boas candidatas para atribuir ao agente de nuvem. Ele trabalha de forma assíncrona enquanto a pessoa desenvolvedora cuida de outras tarefas e, depois, revisa o resultado.

## Conhecer o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Você pode, por exemplo, [atribuir um issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request em rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão durante a execução ou depois que ela terminar. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo claro

Não há mágica no GitHub Copilot: você ainda precisa pensar. Até operações aparentemente simples podem ser complexas. Por isso, [defina cuidadosamente o escopo das tarefas do agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas. Se você já concluiu o exercício prático, viu `.github/copilot-instructions.md` em ação; caso contrário, aproveite para consultá-lo. Antes de atribuir trabalho ao agente de nuvem, examine os arquivos de instruções já incluídos no repositório:

- `.github/copilot-instructions.md` — revise **Code standards**, especialmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Ao atribuir o issue `Code lacks documentation`, observe no pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário; essas orientações vêm dos arquivos de instruções. Você voltará a examiná-los em uma lição posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

O desenvolvimento geralmente exige um ambiente específico e scripts de configuração. O mesmo vale para o Copilot, que trabalha de forma semelhante a uma pessoa engenheira de software.

O agente de nuvem usa [GitHub Actions][github-actions]. Você pode personalizar o ambiente com um [workflow de configuração][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, executado antes de o agente começar. O workflow deste laboratório já está configurado para disponibilizar Node.js, dependências do projeto, navegadores para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido do aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina apenas as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente quando elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (o aplicativo inteiro agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Como em outros workflows do GitHub, observe:

- Há um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot iniciar o trabalho no pull request.
- O gatilho `workflow_dispatch` permite executar manualmente o workflow na guia **Actions**, para testar se ele funciona sem esperar que o Copilot o inicie.

## Adicionar documentação

Muitos projetos têm informações desatualizadas ou documentação insuficiente, o que reduz a produtividade e dificulta a manutenção e a integração de novas pessoas à equipe. O Copilot pode ajudar a gerar documentação, portanto esse é um bom issue para atribuir ao agente de nuvem. Você revisará o trabalho em um exercício futuro.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues** e depois **New issue**.
3. Selecione **Blank issue**.
4. Defina **Title** como `Code lacks documentation`.
5. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

6. Selecione **Create** para criar o issue.
7. À direita, selecione **Assign to Copilot** e, na caixa de diálogo, selecione **Assign**.

    ![Atribuir um issue ao Copilot](../../../_images/shared-assign-copilot.png)

    ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

8. Selecione **Pull Requests** e abra o PR recém-gerado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele não aparecer, aguarde um pouco e atualize a página.
9. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer. O Copilot está configurando o ambiente, executando o workflow mencionado, analisando o projeto e decidindo como abordar a tarefa.

10. Revise a lista e as tarefas que serão realizadas.
11. Percorra a linha do tempo do pull request até encontrar a atualização que informa que o Copilot começou a trabalhar no issue.
12. Selecione **View session**.

    ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

13. Percorra a sessão em tempo real e observe como o Copilot explora o código, entende o estado do projeto, planeja e cria código.

Isso pode levar alguns minutos. Enquanto o agente trabalha de forma assíncrona, você pode atribuir outra tarefa e se dedicar ao desenvolvimento de um recurso.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma vantagem do agente de nuvem é dividir o trabalho: você pode cuidar de uma tarefa enquanto o agente realiza outra. Vamos atribuir a solicitação da equipe de design ao Copilot.

1. Volte ao repositório no github.com.
2. Selecione **Issues**, depois **New issue** e **Blank issue**.
3. Defina **Title** como `Show related games on the game details page`.
4. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

5. Selecione **Create**. À direita, selecione **Assign to Copilot** e depois **Assign**.

    ![Atribuir um issue ao Copilot](../../../_images/shared-assign-copilot.png)

Logo aparecerá um emoji 👀 no primeiro comentário do issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando no issue](../../../_images/ex4-issue-eyes-emoji.png)

O agente de nuvem trabalha de maneira semelhante a uma pessoa engenheira de software. Você não precisa acompanhá-lo continuamente; revise o trabalho quando estiver concluído. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Esta lição apresentou o [agente de nuvem do GitHub Copilot][copilot-agents]. Você pode atribuir issues ao agente para que ele trabalhe de forma assíncrona, resolva dívida técnica, crie recursos ou ajude a migrar código entre frameworks.

Você explorou:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam realizadas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

Enquanto o agente trabalha, você pode criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e recebe instruções personalizadas, apresentadas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
