---
title: "Exercício 3 - Testar o recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ele funciona no navegador. Em vez de navegar manualmente pelo aplicativo, conecte o **servidor MCP do Playwright** e deixe o Copilot testar o recurso em um navegador real. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como servidores MCP ampliam as ferramentas do Copilot;
- adicionar o servidor MCP do Playwright ao workspace;
- pedir ao Copilot que teste manualmente o recurso de filtragem no navegador;
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é um padrão que permite ao Copilot se comunicar com ferramentas e serviços externos.

![Diagrama do funcionamento interno do modo de agente e de como ele interage com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real, acessem informações atualizadas por meio de recursos e executem ações em seu nome.

As ferramentas e os recursos são acessados por meio de um servidor MCP, que atua como ponte entre o agente e ferramentas externas, como APIs existentes ou ferramentas locais, incluindo pacotes NPM. Cada servidor MCP oferece um conjunto próprio de ferramentas e recursos.

Alguns servidores conhecidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: oferece acesso a APIs para gerenciar repositórios do GitHub, issues e pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador para navegar por páginas, preencher formulários e selecionar botões.

Há muitos servidores MCP disponíveis. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. O servidor `github` já deve estar configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

A entrada `github` vem do modelo do projeto e permitiu ao Copilot ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server]; não é necessário instalá-lo localmente. O VS Code conecta-se por HTTP, e você se autentica na primeira utilização de uma ferramenta.

## Adicionar o servidor MCP do Playwright

O [servidor MCP do Playwright][playwright-mcp-server] dá ao Copilot um navegador controlável para testar o recurso.

1. No arquivo `.vscode/mcp.json`, adicione uma entrada `playwright` junto a `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é local: o VS Code o inicia executando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, necessária em um codespace sem desktop.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright em testes de ponta a ponta, então o navegador geralmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa de uma ferramenta, o VS Code inicia o servidor e solicita confirmação de confiança.

1. No `.vscode/mcp.json`, selecione a ação **Start** que aparece acima da entrada `playwright` para iniciar o servidor e confirmar a configuração.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme para iniciá-lo.

Iniciar o servidor manualmente é opcional. Se você pular essa etapa, o VS Code o iniciará e solicitará confiança quando o Copilot precisar da ferramenta.

## Testar o recurso de filtragem

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, em vez de capturas de tela. Assim, o agente analisa elementos estruturados e rotulados, como botões, links e itens de lista, de maneira semelhante à tecnologia assistiva. Uma verificação funcional também serve como uma avaliação básica de acessibilidade.

O Copilot abrirá um navegador, executará as etapas e relatará os resultados. Compare o relatório com os critérios de aceitação do issue. Se algo estiver errado, faça perguntas ou peça que o agente corrija o código antes de abrir o PR.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor tiver sido interrompido, inicie-o antes do prompt. Na primeira utilização do MCP do Playwright, talvez seja necessário baixar um navegador; se faltar, peça ao Copilot que execute `npx playwright install chromium`.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Depois de confirmar que o recurso funciona, publique a branch `filtering-vscode` para abrir um pull request (PR) para revisão da equipe.

1. Abra o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit com o botão **Sparkle**.

   ![Painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Você pode criar um PR no github.com ou pela interface de linha de comando do GitHub. Vamos pedir ao Copilot que localize o issue correspondente e crie o PR associado.

1. Abra o painel Copilot Chat e selecione **New Chat**.
2. Confirme que **Agent** está selecionado para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie um PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Selecione **Continue** quando necessário para permitir que o Copilot reúna informações e execute operações. Na primeira utilização de uma ferramenta GitHub, talvez seja necessário autenticar-se.
5. Observe o Copilot procurar os issues, encontrar o correto e criar o PR.
6. Selecione o link gerado pelo Copilot para revisar o pull request, mas **não faça merge ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é MCP e como servidores MCP ampliam as ferramentas do Copilot;
- adicionou o servidor MCP do Playwright ao workspace;
- usou-o para testar o recurso de filtragem no navegador antes de publicá-lo;
- publicou a branch e abriu um pull request.

Você usou um servidor MCP para *testar* um recurso, mas há outras maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
