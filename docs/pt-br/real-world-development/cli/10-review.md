---
title: "Lição 10 - Encerramento e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, as duas pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI ao longo de um fluxo de trabalho contínuo no Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem inicial.
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e fez manualmente o merge da sua primeira pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou-a no modo Autopilot e revisou-a no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill existente `quality-checks` e usou-a para executar testes de unidade, lint e verificações de tipos.
- adicionou o servidor Playwright do Model Context Protocol (MCP) e usou-o para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia da qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou toda a alteração da filtragem e autorizou o Agent Merge para a PR da funcionalidade.
- explorou comandos de barra para gerenciar contexto, modelos e compartilhamento, além da delegação opcional a um agente de nuvem.

## O que você entregou

O workshop inclui dois marcos de PR, cada um em sua própria branch criada a partir de `main` atualizada:

1. **Avaliações por estrelas:** exibir o `starRating` existente e um estado explícito para jogos sem avaliação nos cards.
2. **Filtragem e fluxo de qualidade:** implementar a filtragem, atualizar as instruções e aplicá-las à funcionalidade, personalizar o relatório `quality-checks`, criar um perfil de QA e incluir os testes associados.

Do planejamento da filtragem à abertura da PR, você usou a mesma conversa e branch. Reunimos esse trabalho em uma única PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: com testes automatizados, inspecionando o navegador e explorando a interface com o navegador do Copilot por meio do MCP. A skill `quality-checks` executou testes de unidade, lint e verificações de tipos e apresentou os resultados no novo formato. O QA reuniu esses resultados com uma revisão dos requisitos e da cobertura de testes antes da PR.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que não exija novos testes também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas depois de qualquer alteração.

## Práticas recomendadas

O contexto e as ferramentas que você fornece ao Copilot influenciam o trabalho dele. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em outras conversas e ajuste-as conforme as necessidades da equipe mudarem. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações reais e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use o **Plan** para avaliar uma abordagem antes de implementar, o **Interactive** para acompanhar de perto alterações específicas e o **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e um modelo mais capaz para trabalhos complexos.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer criar, *por que* e *como* muda significativamente o resultado.

## Explore outros recursos

Você concluiu o fluxo de trabalho principal. Confira alguns outros recursos do CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para coordenar tarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa separada.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de progredir com qualquer ferramenta é continuar usando-a! Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você quer criar há anos. Compartilhe seus aprendizados com a equipe e aprenda com os outros. E, como sempre, explore a documentação.

Se quiser ampliar o Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
