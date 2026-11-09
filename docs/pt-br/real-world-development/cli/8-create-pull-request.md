---
title: "Lição 8 - Crie e faça o merge da pull request da funcionalidade"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de garantia da qualidade e os testes; depois, crie uma pull request e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia da qualidade (QA) e os testes estão salvos em uma única branch. É hora de revisar tudo em conjunto e abrir uma pull request. Você fez o merge da pull request (PR) das avaliações por estrelas manualmente; desta vez, deixará que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos a funcionalidade, as atualizações das instruções, a atualização da skill e o agente de QA em algumas PRs separadas. Para simplificar o workshop, mantivemos todo o trabalho da funcionalidade de filtragem e do fluxo de qualidade em uma única conversa e branch, e incluiremos tudo nesta PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar toda a alteração da funcionalidade e as evidências de verificação.
- criar a PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar se o merge foi concluído.

## Cenário

Ao longo do fluxo de trabalho da filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora, a Tailspin Toys quer automatizar o trabalho restante da PR, mantendo a autorização para o merge sob o controle do desenvolvedor.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para integrar uma pull request. Quando você o habilita, o Copilot verifica o que está impedindo o merge da PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e, em seguida, habilita o merge automático do GitHub, se o repositório permitir.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que ele está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar a PR

Com todo o código pronto, vamos revisá-lo em conjunto, criar a PR e deixar que o Agent Merge gerencie o restante do processo.

1. Volte ao seu Codespace.
2. Digite `/agent` para abrir a caixa de diálogo de agentes.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie uma nova PR com o comando `/pr create`.
5. Ative o Agent Merge com o comando `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua da PR. Quando tudo for concluído com sucesso, ele fará o merge.
7. Confirme se você vê uma mensagem do Copilot semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e, quando o repositório permitir, faça manualmente o merge revisado.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento: geração, teste e validação de código e, agora, o processo da pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou toda a alteração da funcionalidade e as evidências de verificação.
- criou a PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou se o merge foi concluído.

Em seguida, você vai [explorar outros comandos de barra úteis do Copilot CLI][next-lesson] para gerenciar o contexto, os modelos e o compartilhamento, além da delegação opcional a um agente de nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
