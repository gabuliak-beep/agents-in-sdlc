---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas de melhoria da experiência do usuário e de implementação de funcionalidades. Pediu ao Copilot que adicionasse documentação, criasse um recurso de jogos relacionados para a equipe de design iterar e implementasse recursos de acessibilidade, incluindo controles para os modos de alto contraste e claro. Vamos explorar as alterações no código e, se necessário, enviar comentários ao Copilot antes de habilitar a revisão para o restante da equipe.

### Cenário

Os fundamentos de design de software e DevOps não mudam com a IA generativa. Sempre devemos revisar o código gerado e seguir o processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão, há restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem executa no GitHub Actions, em um ambiente separado e temporário.
- Workflows do GitHub Actions precisam de aprovação humana antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revisar a documentação gerada

Vamos começar pelo primeiro pull request (PR) gerado pelo agente de nuvem do Copilot: adicionar documentação ao código. Você fará isso pela interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, talvez você veja um aviso de que o Copilot está bloqueado por um firewall. Isso é **esperado**: o acesso a recursos externos, incluindo servidores MCP externos, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de PRs.
3. Abra o PR intitulado, por exemplo, **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde até que o trabalho termine; isso pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, incluindo os comentários TSDoc e outras documentações. As alterações exatas variam. Procure os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário, definidos pelos arquivos de instruções personalizados revisados no Exercício 2.
6. Quando terminar, volte à guia **Conversation** e role para baixo.
7. Procure o indicador de workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Confira os workflows na seção de verificações do PR. Se tudo correr bem, as verificações do projeto passarão para o aplicativo Astro. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

O trabalho com o Copilot em um pull request não precisa ser uma via de mão única. Você pode mencioná-lo em comentários do PR ou em comentários de linha do código. O Copilot lerá os comentários e iniciará outra sessão para tratá-los. Como os resultados não são determinísticos, não há um texto único de solicitação. Você pode pedir, por exemplo:

- adicionar cabeçalhos de comentário no início dos arquivos de código, com uma breve descrição.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração à documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das sugestões acima ou outra solicitação.
2. Selecione **View Session** para acompanhar o trabalho. O Copilot iniciará uma sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao PR.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar, um novo commit aparecerá no PR.
5. Selecione **Files changed** e revise as alterações.

Continue iterando até ficar satisfeito. Depois, marque o PR como pronto para revisão e faça merge na branch principal.

![Marcar o PR como pronto](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Volte ao PR criado pelo Copilot para atender ao issue de exibição de jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione **Files changed** para revisar o código gerado.
5. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
6. Procure o indicador de workflows aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Confira as verificações do PR. Se tudo correr bem, as verificações do projeto passarão para o aplicativo Astro.
9. **Opcional:** teste manualmente o recurso no Codespace. Abra o terminal e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, como **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O recurso de jogos relacionados está pronto. Você pode pedir ao agente de nuvem que altere a quantidade de jogos exibidos ou acrescente cabeçalhos e comentários TSDoc — lembre-se de que esse issue foi atribuído antes da atualização das instruções personalizadas. Adicione a solicitação como um comentário na guia **Conversation** para iniciar outra sessão.

## Revisar os recursos de acessibilidade

Por fim, revise os recursos implementados pelo agente personalizado de acessibilidade. O PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado no centro de controle do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Selecione **Files changed** para revisar o código.
5. Examine a implementação, prestando atenção a:
   - componentes de interface para alternar entre modos;
   - uso do armazenamento local para manter as preferências;
   - alterações de CSS ou estilo para os modos de alto contraste e claro;
   - atributos de acessibilidade, como rótulos ARIA e navegação pelo teclado;
   - código JavaScript/TypeScript que controla a alternância dos modos.

6. Quando terminar, volte à guia **Conversation** e role para baixo.
7. Procure o indicador de workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Confira as verificações do PR. Se tudo correr bem, as verificações do projeto passarão para o aplicativo Astro.
10. **Opcional:** teste manualmente os recursos de acessibilidade no Codespace. No terminal, execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Inicie o aplicativo e teste os controles de alto contraste e modo claro no navegador. Confirme que funcionam e que as preferências permanecem após recarregar a página.

O agente personalizado ajudou o Copilot a implementar os recursos seguindo boas práticas de acessibilidade. Se encontrar algo que possa ser melhorado, mencione **@copilot** em um comentário, como nos PRs anteriores.

## Exercício opcional — continuar delegando

O agente de nuvem é útil quando você pode delegar itens reais do backlog e cuidar de outras tarefas. Para praticar, registre mais alguns issues e atribua-os ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na página da lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu a trilha do agente de nuvem. Ao longo das lições, você:

- **Inspecionou os arquivos de instruções personalizados** do repositório para observar seu impacto no trabalho do agente.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade, adicionando controles para alto contraste e modo claro.
- **Usou a página de agentes do Copilot como centro de controle** para monitorar e orientar a sessão de acessibilidade.
- **Revisou e iterou nos pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu a trilha do agente de nuvem. Para continuar aprendendo, as outras trilhas complementam o que praticou:

- 🖥️ **[Trilha do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- 💻 **[Trilha do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente de QA personalizado, MCP do Playwright, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

No seu repositório, experimente:

- atribuir ao agente de nuvem um issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas do workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
