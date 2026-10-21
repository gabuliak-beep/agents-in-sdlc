---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora o código, propõe alterações, edita arquivos e executa comandos. Como isso acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições. Você pode revisar cada diff, aceitar ou rejeitar alterações individualmente e orientar a conversa com prompts de acompanhamento sem sair do editor.

Neste exercício, você vai:

- acompanhar o trabalho do agente de acessibilidade em tempo real;
- revisar e aceitar as edições propostas;
- orientar o agente durante a sessão para adicionar um controle de modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design solicitou também um controle para o modo claro, para as pessoas que consideram o tema padrão escuro demais. Em vez de iniciar uma conversa nova, você orientará a sessão existente.

## Acompanhar o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: arquivos explorados, comandos executados e edições propostas. Cada edição aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, acompanhe as chamadas de ferramentas, leituras e edições de arquivos e comandos de terminal solicitados.
3. Ao propor uma edição:
   - selecione **Keep** para aceitá-la;
   - selecione **Undo** para revertê-la e permitir outra tentativa.
4. Abra **Files changed**, na parte superior do chat, para ver todos os arquivos modificados nesta sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão, por exemplo, para executar um comando de terminal, o agente pausará e mostrará **Allow**. Aprove somente comandos que você considere seguros.

5. Aguarde o agente concluir o modo de alto contraste. Ele apresentará uma mensagem final com o resumo das alterações.

## Orientar o agente para adicionar o modo claro

Com o modo de alto contraste implementado, estenda a mesma conversa para incluir o modo claro. A sessão mantém o contexto do trabalho anterior, incluindo componentes, estilo e o padrão de armazenamento local.

1. No campo do Copilot Chat, envie:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente revisará as alterações anteriores e proporá a implementação do modo claro.
3. Revise cada nova edição e use **Keep** ou **Undo**.
4. Se o agente seguir uma abordagem indesejada, como refatorar o controle de alternância de uma maneira que você não quer, interrompa-o com **Stop** e envie outra orientação.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes ("simplifique o CSS e use variáveis em vez de duplicar seletores"), corrigir a direção ("não adicione um terceiro controle; integre o modo claro ao painel existente") ou ampliar o escopo ("adicione atalhos de teclado para os dois modos").

## Revisar o conjunto de alterações

Antes de fazer commit, revise tudo que o agente alterou.

1. Abra **Source Control** no VS Code.
2. Revise a lista de arquivos alterados. Ela deve incluir componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos e examine os diffs. Confirme a presença de padrões de acessibilidade, como atributos ARIA, navegação pelo teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts para ampliar o trabalho sem perder o contexto.

Nesta lição, você:

- acompanhou o trabalho do agente em tempo real;
- aceitou e rejeitou edições individuais;
- orientou uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações do agente personalizado.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
