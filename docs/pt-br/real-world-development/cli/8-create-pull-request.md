---
title: "Lição 8 - Criar e fazer merge do PR do recurso"
description: "Revise a filtragem, as instruções, a skill, o perfil de QA e os testes; depois crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora, revise tudo em conjunto e abra um pull request. Você fez merge do PR de classificação por estrelas manualmente; desta vez, deixará o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, separaríamos o recurso, as atualizações das instruções, a skill e o agente de QA em alguns PRs. Para simplificar o workshop, mantivemos todo o fluxo de qualidade e filtragem em uma conversa e branch.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar as alterações completas do recurso e as evidências de verificação.
- criar o PR da filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o restante do trabalho do PR, mantendo a autorização de merge sob controle da pessoa desenvolvedora.

## Conhecer o Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir um pull request. Quando habilitado, o Copilot resolve o que estiver impedindo o merge — corrige verificações de integração contínua (CI) com falha, responde a comentários da revisão e atualiza a branch quando necessário — e, em seguida, habilita o auto-merge do GitHub quando o repositório permite.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitá-lo.

## Usar o Agent Merge para gerenciar o PR

Com o código pronto, vamos revisá-lo em conjunto, criar o PR e deixar o Agent Merge cuidar do restante.

1. Volte ao Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie um PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará a integração contínua do PR. Quando tudo for aprovado, ele fará o merge.
7. Confirme que o Copilot informa algo como "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e conclua manualmente o merge revisado, se o repositório permitir.

## Resumo e próximos passos

Você automatizou várias etapas do desenvolvimento, incluindo geração, teste e validação de código e o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou as alterações completas e as evidências de verificação.
- criou o PR da filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou que o PR foi mesclado.

Em seguida, você [explorará outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
