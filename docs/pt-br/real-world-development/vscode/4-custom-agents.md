---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou domínios do fluxo de desenvolvimento. Defina agentes em arquivos Markdown na pasta `.github/agents` do repositório para fornecer instruções focadas, boas práticas, padrões de código e conhecimento específico. As equipes podem codificar sua experiência em agentes reutilizáveis — por exemplo, um agente de acessibilidade que verifica conformidade com [WCAG][wcag], um agente de segurança ou um agente de testes.

Agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou, globalmente, em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos usam principalmente arquivos Markdown para orientar uma IA na execução de operações. A distinção mais simples é que o **agente personalizado** executa o trabalho e as **skills** fornecem ferramentas e procedimentos reutilizáveis.

Agentes personalizados têm sua própria janela de contexto e coordenam skills (e até outros agentes). Neste laboratório, o agente de acessibilidade revisa e atualiza o site conforme diretrizes de acessibilidade; durante o trabalho, ele pode usar skills de fluxo de pull request ou de execução e gerenciamento de testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Teste e itere para encontrar o que funciona para seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente da visão ou das preferências visuais. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro devido ao contraste insuficiente entre texto e plano de fundo. A equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente;
- usar o agente no Copilot Chat para implementar um modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente de acessibilidade já foi criado. Volte ao codespace, abra um terminal e crie uma branch nova a partir de `main` para manter o trabalho de acessibilidade separado do PR de filtragem do Exercício 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Examine as seções seguintes:
   - responsabilidades ao gerar código para um site acessível;
   - boas práticas de acessibilidade;
   - exemplos de HTML, CSS e JavaScript;
   - armadilhas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code apresenta todos os agentes definidos em `.github/agents` na lista suspensa de agentes, na parte inferior da visualização Copilot Chat. Selecione um agente personalizado para aplicar suas instruções e ferramentas à sessão.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa informa as opções relevantes.

1. Na lista de agentes da visualização Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Abra a lista suspensa na parte inferior da visualização Chat, que mostra o agente ativo (por padrão, **default**).
3. Selecione **Accessibility agent**.
4. Envie este prompt ao agente:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot explorará o código, proporá alterações e editará os arquivos. Cada edição aparecerá no chat com o caminho do arquivo e um diff para revisão.
6. O indicador **Files changed** será atualizado conforme o agente modificar arquivos.

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot fará alterações reais no repositório, como componentes Astro, CSS e testes relacionados.

Você acompanhará e orientará o trabalho na próxima lição.

## Resumo e próximos passos

Esta lição apresentou os [agentes personalizados][custom-agents], assistentes especializados em tarefas e domínios. Com eles, você codifica a experiência e os padrões da equipe em agentes reutilizáveis.

Você explorou:

- como os agentes personalizados são definidos;
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [monitorará e orientará o trabalho do agente][next-lesson], revisará as alterações enquanto são feitas e solicitará um controle de modo claro na mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar seu recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
