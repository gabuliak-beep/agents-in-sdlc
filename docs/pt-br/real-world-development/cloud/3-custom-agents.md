---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou domínios do fluxo de desenvolvimento. Defina agentes em arquivos Markdown na pasta `.github/agents` do repositório para fornecer ao Copilot instruções focadas, boas práticas, padrões de código e conhecimento específico do domínio. As equipes podem codificar sua experiência em agentes reutilizáveis: por exemplo, um agente de acessibilidade que verifica a conformidade com [WCAG][wcag], um agente de segurança que segue práticas de código seguro ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou, globalmente, em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA na execução de tarefas. A distinção mais simples é: o **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Agentes personalizados têm sua própria janela de contexto e coordenam skills (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade; ele poderá chamar skills, como uma skill de fluxo de pull request ou uma que execute e gerencie testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você explorará:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente da visão ou das preferências visuais. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro por causa do contraste insuficiente entre o texto e o plano de fundo. A equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é essencial, você usará um agente personalizado para gerar a funcionalidade rapidamente.

## Revisar o agente de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e revise o arquivo do agente:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que apresentam:
   - responsabilidades ao gerar código para um site acessível;
   - boas práticas de acessibilidade;
   - exemplos de código HTML, CSS e JavaScript;
   - armadilhas e erros comuns.

## Criar e atribuir um issue

O centro de controle é o lugar principal para trabalhar com agentes. Nele, você pode atribuir tarefas ao agente de nuvem do Copilot, monitorá-las e redirecionar ou complementar as orientações. Vamos atribuir ao Copilot a criação do modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia de issues.
3. Selecione **New issue** e depois **Blank issue**.
4. Defina **Title** como `Add high contrast mode to website`.
5. Defina **Description** como:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

6. Selecione **Create** para criar o issue.
7. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.
8. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Atribuição do agente de nuvem com o agente personalizado e a opção de acessibilidade destacados](../../../_images/ex5-select-custom-agent.png)

9. Selecione **Assign**. O Copilot começará a trabalhar em segundo plano.

## Resumo e próximos passos

Esta lição apresentou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com eles, você pode codificar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, você pode [monitorar e orientar a sessão do agente][next-lesson] no centro de controle.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar-se para usar agentes personalizados na organização][org-custom-agents]
- [Preparar-se para usar agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
