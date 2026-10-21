---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até atualizações simples em um aplicativo podem exigir mudanças em vários arquivos e operações como executar testes. Uma pessoa desenvolvedora precisa localizar os arquivos, fazer alterações, executar testes, depurar e descobrir se algum arquivo foi esquecido. O modo de agente do Copilot pode ajudar nesse trabalho.

O modo de agente explora a estrutura do projeto, faz alterações coordenadas, executa tarefas como testes e corrige automaticamente erros encontrados. Vamos usá-lo para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE e permite ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, interagindo por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente explora o projeto e identifica arquivos relevantes;
- o modo de agente implementa recursos na interface e na camada de dados;
- revisar as alterações e os testes gerados antes do merge.

## Cenário

À medida que a lista de jogos cresce, você quer permitir a filtragem por categoria e editora. No exercício anterior, você adicionou um helper de editoras; agora concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer alterações, explore o site para entender as funcionalidades atuais. O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele é um aplicativo Astro que renderiza páginas como HTML estático no momento da compilação. As páginas consultam diretamente um banco de dados SQLite local pelo Drizzle ORM; não há API de back-end separada nem framework de interface no cliente. Os helpers de acesso a dados ficam em `src/lib/`, e a interatividade usa pequenos elementos `<script>` do Astro com APIs DOM padrão.

### Iniciar o site

O comando `npm run dev` inicia o servidor de desenvolvimento Astro no Codespace.

1. Volte ao codespace. Após o exercício anterior, você deve estar na branch `main`.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch para a filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá uma mensagem semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo solicitando a abertura de `http://localhost:4321` no navegador, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, o link local é redirecionado automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor executado no codespace.

### Explorar o site

O site permite:

- **Página inicial**: exibe jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, abre uma página com título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa informa as opções relevantes.

O site funciona, mas queremos adicionar recursos. Vamos consultar o backlog, criado automaticamente quando você usou o modelo do projeto.

1. Na lista suspensa de agentes da visualização Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos, executa comandos e usa ferramentas.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores do workshop não estão indicando preferência por um modelo. Usamos Claude Sonnet 4.5 ao preparar o conteúdo para que as sugestões fossem relativamente consistentes. Como os LLMs são probabilísticos, suas sugestões podem ser diferentes; isso é normal.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar outro comando MCP e ainda concluir a tarefa.

3. Peça ao Copilot que mostre o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista os issues.
5. Revise a lista gerada.

O Copilot também prioriza os itens com base no que considera mais útil para as pessoas usuárias.

## Revisar arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto. O arquivo [de instruções da interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) explica como adicionar funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações sobre arquitetura, design de componentes, testabilidade, acessibilidade e links para instruções específicas de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos e exemplos do projeto. Os caminhos são relativos ao arquivo de instruções, o que permite reutilizar orientações e dividir conjuntos complexos em arquivos menores.

## Implementar a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações:

- adicionar ou aprimorar a lógica restante na camada de dados (`src/lib/`);
- adicionar ou atualizar testes para o comportamento de filtragem;
- atualizar a página de listagem de jogos com a interface de filtragem.

Os testes precisam passar antes do merge. O modo de agente do Copilot pode cuidar dessas tarefas.

1. Continue a conversa atual ou selecione **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui**.

   ![Selecionar o arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** está selecionado.
4. Confirme que **Claude Sonnet 4.5** continua selecionado.
5. Peça ao Copilot que implemente a funcionalidade com base no issue relacionado:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto, encontrar os helpers da camada de dados, a interface e os testes, modificar arquivos e executar testes.

   ![Copilot explorando arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot explorará o projeto, modificará arquivos e executará testes. Isso pode levar alguns minutos. Os botões **Keep** e **Undo** podem aparecer no editor enquanto ele trabalha. Aguarde a conclusão antes de aceitar ou desfazer as alterações.

7. Quando solicitado, selecione **Continue** para executar os testes.

   ![Solicitação para executar testes no painel do Copilot Chat](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram pausas ou falhas de teste durante o processo. O Copilot alterna entre geração de código e execução de testes até concluir a tarefa sem detectar erros.
9. Explore o código gerado e procure possíveis problemas.

   ![Sessão concluída do Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Você usou o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendeu como:

- o modo de agente implementa recursos na interface e na camada de dados;
- o Copilot explora o projeto, identifica arquivos e coordena alterações;
- revisar as alterações e os testes antes do merge.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra de exploração — implementar paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Use o que aprendeu neste exercício para pedir ao Copilot que implemente esse recurso:

- siga as boas práticas existentes, inclusive os arquivos de instruções;
- decida como a paginação deve funcionar e se a pessoa usuária poderá escolher o tamanho da página;
- forneça ao Copilot orientações suficientes no prompt;
- itere com o Copilot e forneça contexto adicional quando necessário.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
