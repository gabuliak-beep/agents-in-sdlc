---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o Plan para definir uma abordagem, o Autopilot para criar filtragem a partir de um issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto, mas mudanças maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi desenvolvido para se integrar ao fluxo de trabalho existente da organização e ajudar a construir as coisas certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: começará com um issue para gerar um recurso, verificará se o código é válido e se o recurso funciona como esperado e, por fim, fará merge no projeto.

> [!NOTE]
> Você usará a mesma conversa e branch ao longo do fluxo de trabalho do recurso. Em geral, você usaria branches ou PRs diferentes para os vários tipos de arquivo, mas vamos simplificar para focar nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de um issue do GitHub.
- definir requisitos no modo Plan.
- implementar o novo recurso no modo Autopilot.
- revisar o código.
- validar o recurso manualmente em um navegador encaminhado.

Ao longo do desenvolvimento, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação com MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. O issue do backlog descreve o recurso, mas detalhes como combinar categorias precisam ser definidos antes de programar. Você usará o modo Plan para tomar essas decisões e, em seguida, autorizará uma implementação delimitada com o Autopilot.

## Contexto

Adicionar agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, torna-os ainda mais importantes. Um processo comum é:

1. Começar por um issue registrado que descreva o trabalho necessário.
2. Criar um plano do que precisa ser desenvolvido.
3. Desenvolver e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, fazer merge.

> [!NOTE]
> Os detalhes variam conforme a equipe e a organização, mas a maioria dos processos segue uma variação desse padrão.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria primeiro um plano e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Começar pelo issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o.
2. Confirme que o PR de classificação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todos os issues.
8. Use as teclas de seta para destacar o issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar o issue ao prompt e voltar à guia **Session**.

Observe que agora o prompt começa com `#7` (ou um número semelhante). O caractere `#` adiciona um issue ou pull request do GitHub ao contexto.

## Planejar o recurso de filtragem

O planejamento permite definir como implementar um recurso antes de delegar o trabalho ao Copilot. Para tarefas complexas, vale a pena dedicar algum tempo a isso. Vamos pedir ao Copilot que crie um plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência ao issue adicionada na etapa anterior, digite:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot explorará o projeto e determinará uma abordagem.

3. Durante o processo, o Copilot poderá perguntar como a filtragem deve funcionar. Responda de acordo com suas preferências; não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Use o Autopilot para deixar o Copilot trabalhar.

O Autopilot permite que o Copilot itere até considerar a tarefa concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme que o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot executar o plano, gerar código e rodar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente; revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Como qualquer outro código, o código gerado precisa ser revisado antes do merge. Vamos revisar as alterações e executar o site para conferir o resultado.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com o issue e com as decisões do planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que resolva as falhas.

## Explorar a nova funcionalidade

O código parece bom, mas será que funciona? Vamos iniciar o aplicativo e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros de categoria e editora, além das combinações definidas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde ao issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de um issue do GitHub.
- definiu requisitos no modo Plan.
- implementou o recurso no modo Autopilot.
- revisou o código.
- validou o recurso manualmente em um navegador encaminhado.

Em seguida, vamos aprofundar a geração de código e garantir que ela siga as práticas documentadas usando [instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
