---
title: "Lição 2 - Adicionar classificações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cartões dos jogos, revisá-la no navegador encaminhado e mesclá-la como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já incluem uma classificação por estrelas, mas ela ainda não aparece nos cartões da página inicial. Peça ao agente que a exiba, revise a alteração e faça merge no seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de recurso;
- pedir ao agente que faça uma pequena alteração no projeto;
- revisar a alteração com `/diff`;
- executar o aplicativo e confirmar a alteração no navegador encaminhado;
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma classificação por estrelas, já exibida na página de detalhes. Os cartões da página inicial mostram somente título, categoria, editora e descrição. Como exercício inicial, peça ao agente que mostre a classificação existente em cada cartão — uma alteração pequena e independente, ideal para a primeira sessão.

## Estrutura de uma conversa

Uma **conversa** é o espaço onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e a branch Git atualmente selecionados no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a discussão; os arquivos e a branch continuam sendo o estado Git normal no disco.

Em uma conversa, você verá seus prompts e as respostas do agente, as ferramentas que ele usa para explorar e editar arquivos e as alterações que podem ser inspecionadas com `/diff`.

## Iniciar uma conversa e pedir a alteração

Vamos iniciar uma conversa nova para implementar o recurso.

1. Volte ao Codespace.
2. Se o terminal ainda não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, inicie-o:

   ```bash
   copilot --yolo
   ```

4. Inicie uma sessão nova usando o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Envie este prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto, localizará os arquivos que exibem os detalhes do jogo e criará o código necessário. Você implementou um recurso com o Copilot CLI.

## Revisar o diff

Revise todas as alterações geradas por IA antes do merge, mesmo as pequenas.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o cartão mostra a classificação numérica quando disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que corrija as falhas.
5. Depois da revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico, e o código exato pode variar. Revise o comportamento, sem esperar uma implementação idêntica.

## Verificar as alterações

Não basta ler o código e presumir que funciona. Peça ao Copilot que inicie o site para que você examine a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões exibem as classificações de zero a cinco.
4. Volte ao Copilot e peça que encerre o servidor:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir e mesclar seu primeiro pull request

Você implementou o recurso. Agora, crie um pull request (PR) para integrar o código ao projeto.

1. Peça ao agente padrão que faça commit da alteração:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. O Copilot CLI enviará o commit existente ao criar o PR e exibirá a URL.
3. Mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você publicou sua primeira alteração com o GitHub Copilot CLI. Você:

- iniciou uma conversa focada em uma branch de recurso;
- orientou o agente a fazer uma pequena alteração nos cartões;
- revisou a alteração com `/diff`;
- executou o aplicativo e confirmou as classificações no navegador encaminhado;
- abriu e mesclou seu primeiro pull request.

Em seguida, você [começará pelo issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para desenvolver um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
