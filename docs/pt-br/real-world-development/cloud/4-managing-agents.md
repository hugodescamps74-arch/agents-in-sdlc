---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot para executar três tarefas diferentes, todas voltadas a melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem tenha sido criado para trabalhar de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas disponíveis para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel de controle, você pode ver todas as tarefas dos agentes que têm pull requests (PRs) abertos. Você pode explorar as operações realizadas e até orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem;
- orientar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a tarefa de criar um modo de alto contraste, a equipe percebeu que seria uma boa oportunidade para adicionar também um modo claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar controles de alternância, pareceu lógico incluir essa funcionalidade. Você orientará o trabalho do agente para garantir que ele adicione um modo claro além do alto contraste.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos conferir o status atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código;
   - gerar interfaces de programação de aplicações (APIs) para modificar jogos;
   - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em execução. Revise as tarefas realizadas pelo Copilot, que podem incluir:
   - clonar o código do repositório;
   - criar o ambiente de trabalho do Copilot;
   - configurar servidores do Model Context Protocol (MCP);
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas listadas variam de acordo com o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull requests (PRs), que aparece à direita. Ele permite acessar o PR e os arquivos alterados para monitorar melhor o trabalho.

## Orientar o agente de nuvem

Agora que você viu as tarefas em andamento, vamos pedir ao Copilot que inclua um controle para o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à adição do modo de alto contraste. O título exato varia de acordo com o nome usado pelo Copilot e com o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você saberá que isso aconteceu quando começar a ver mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

    ```plaintext
    Enquanto trabalhamos em um modo de alto contraste, vamos adicionar também um modo claro. Deve haver um controle para esse modo, para que os usuários possam selecionar o modo de exibição desejado.
    ```

    ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo Steer active session while Copilot is working destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo de trabalho depois de concluir a etapa específica que estava executando quando você enviou a mensagem.

Como antes, isso levará alguns minutos. É um bom momento para fazer uma pausa e refletir sobre tudo o que você aprendeu e explorou até agora.

## Resumo e próximas etapas

Este exercício apresentou a página de agentes do Copilot, o ponto central para monitorar e orientar as tarefas do agente de nuvem do GitHub Copilot. Nesse painel, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho em andamento e até redirecionar tarefas em execução para ajustar o escopo ou fornecer orientações adicionais.

Você explorou estes conceitos:

- explorou o painel de controle e a página de agentes para monitorar as tarefas do agente de nuvem;
- redirecionou uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos voltar nossa atenção para [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Aula anterior: Agentes personalizados][previous-lesson] | [Próxima aula: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
