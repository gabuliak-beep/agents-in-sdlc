---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que assumisse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse centro de controle, você vê todas as tarefas de agentes com pull requests (PRs) abertos, explora as operações realizadas e orienta uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar tarefas do agente de nuvem.
- orientar uma sessão em andamento e solicitar outra funcionalidade.

## Cenário

Depois de atribuir ao agente a criação do modo de alto contraste, a equipe decidiu que também seria útil adicionar um modo claro. Como o trabalho já atualiza o estilo do site e adiciona controles de alternância, você orientará o agente para incluir também o modo claro.

## Revisar tarefas do agente de nuvem

Vamos conferir o estado atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas nos painéis principal e esquerdo. Você deverá ver as tarefas atribuídas ao Copilot, como:
   - atualizar a documentação do código;
   - gerar APIs para modificar jogos;
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em execução e revise as etapas realizadas pelo Copilot, que podem incluir:
   - obter o código do repositório;
   - configurar o ambiente;
   - configurar servidores MCP;
   - executar outras etapas para concluir a tarefa.

> [!NOTE]
> As etapas listadas variam conforme o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull request à direita, que mostra o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você viu as tarefas ativas, peça ao Copilot que adicione o controle do modo claro enquanto implementa o modo de alto contraste.

1. Selecione a sessão que corresponde à criação do modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado do trabalho.

    ![Sessão de acessibilidade no centro de controle](../../../_images/ex6-accessibility-session.png)

2. Aguarde alguns minutos, até a configuração terminar e o agente começar a trabalhar. Você verá mensagens semelhantes às apresentadas no exercício.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarefa do agente de nuvem na página de agentes com a caixa de orientação destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot reconhece a orientação e a inclui no fluxo de trabalho.

## Deixar o Copilot trabalhar

O Copilot continuará com a tarefa atualizada. Ele incorporará a nova solicitação depois de concluir a etapa em andamento no momento em que você enviou a mensagem.

Isso levará alguns minutos; aproveite para refletir sobre o que aprendeu e explorou até agora.

## Resumo e próximos passos

Esta lição apresentou a página de agentes do Copilot, o centro de controle para monitorar e orientar tarefas do agente de nuvem. Nela, você acompanha tarefas ativas e concluídas, revisa o trabalho e redireciona tarefas em andamento para ajustar o escopo ou fornecer orientação.

Você explorou:

- o centro de controle e a página de agentes para monitorar tarefas do agente de nuvem.
- como redirecionar uma sessão em andamento para solicitar outra funcionalidade.

Com o Copilot concluindo os recursos de acessibilidade, agora você pode [iterar nos pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
