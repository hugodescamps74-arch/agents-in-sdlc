---
title: "Lição 7 - Crie e use um agente de garantia da qualidade"
description: "Crie um agente personalizado de garantia da qualidade que combine os requisitos da issue, a skill quality-checks e o servidor Playwright do Model Context Protocol (MCP)."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o servidor Playwright do Model Context Protocol (MCP) para observar a experiência de filtragem no navegador. Agora, vai reunir esses recursos em um agente personalizado com um processo de garantia da qualidade (QA) bem definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de garantia da qualidade (QA).
- selecionar o agente de QA e revisar as descobertas dele com base na issue de filtragem.

## Cenário

A Tailspin Toys quer revisar de forma consistente os requisitos, a qualidade do código, as verificações automatizadas, a cobertura de testes e o comportamento no navegador antes de abrir uma pull request (PR). Um agente personalizado pode coordenar esse processo de QA e gerar um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis para o agente. Neste workshop, você definirá uma função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações usadas até aqui têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O Playwright MCP fornece ferramentas de navegador. O perfil de QA orienta o Copilot sobre como usar esses recursos para avaliar os requisitos e relatar as descobertas. Ele não substitui esses recursos nem exige outra conversa.

## Crie o perfil de QA

Antes de abrir a PR da funcionalidade, você pedirá ao Copilot que crie um perfil de QA reutilizável. O perfil definirá as verificações que o agente de QA realizará e os limites que deverá respeitar.

1. Volte ao seu Codespace e confirme se a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o novo agente personalizado:

   ```plaintext
   Crie um agente personalizado chamado QA em .github/agents/qa.agent.md. Ele deve comparar as funcionalidades com as respectivas issues e os requisitos acordados, seguir as instruções do repositório, executar a skill quality-checks, usar o Playwright MCP para verificar o comportamento e adicionar testes quando a cobertura estiver incompleta.

   Peça que o agente informe cada requisito como aprovado, reprovado ou bloqueado, com evidências de apoio. Ele deve pedir autorização antes de alterar o código de implementação e não deve fazer commits nem abrir pull requests.

   Por enquanto, apenas crie o perfil para que eu possa revisá-lo.
   ```

## Inspecione o perfil

Antes de usar o novo agente, revise o perfil para confirmar se o Copilot registrou o fluxo de QA e os limites pretendidos. Isso evita um perfil incompleto ou amplo demais que altere a funcionalidade quando você só quer verificá-la.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas sua inclusão dá ao agente um nome de exibição claro.
3. Leia as instruções do perfil e confirme se o agente de QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o Playwright MCP.
4. Confirme se o agente de QA apresenta evidências de apoio, pede autorização antes de alterar o código de implementação e não faz commits nem abre pull requests.
5. Se o perfil gerado não incluir alguma dessas responsabilidades ou algum desses limites, peça ao agente padrão que o revise antes de continuar.

## Execute o QA com base na issue

O Copilot CLI carrega os agentes do projeto quando uma conversa é iniciada. Depois de criar o perfil, retome a mesma conversa de filtragem e selecione o agente de QA para que ele possa usar a issue e as decisões de planejamento já presentes no contexto.

1. Ative o agente com o prompt a seguir:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele não apareça na lista de agentes disponíveis. Mesmo assim, o comando acima o ativará.

2. Use o prompt a seguir para pedir ao agente de QA que revise a funcionalidade:

   ```plaintext
   Revise a funcionalidade de filtragem com base na issue e nas decisões do nosso plano. Ela está pronta para uma PR?
   ```

3. O agente de QA iniciará o trabalho.
4. Quando terminar, leia o relatório.

## Resumo e próximos passos

Você adicionou uma função especializada reutilizável e revisou o trabalho dela. Nesta lição, você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia da qualidade.
- selecionou o agente de QA e revisou as descobertas dele com base na issue de filtragem.

Agora, você tem a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Em seguida, você vai [reunir tudo em uma PR da funcionalidade e usar o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
