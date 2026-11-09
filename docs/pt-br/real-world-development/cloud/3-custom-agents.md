---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de inteligência artificial especializados para tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes com arquivos Markdown na pasta `.github/agents` do repositório, você pode fornecer ao Copilot instruções focadas, práticas recomendadas, padrões de programação e conhecimento específico do domínio para orientar a execução de determinados tipos de trabalho. As equipes podem registrar seu conhecimento em agentes reutilizáveis — por exemplo, um agente de acessibilidade que garante a conformidade com as Diretrizes de Acessibilidade para Conteúdo Web (WCAG), um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões de teste consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem um frontmatter YAML com pelo menos os campos `name` e `description`, seguidos de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Comparação entre agentes personalizados e habilidades de agente

Há alguma sobreposição entre agentes personalizados e [habilidades de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma inteligência artificial sobre como executar operações. A maneira mais clara de diferenciá-los é: um **agente personalizado** é o executor, enquanto as **habilidades** são ferramentas.

Os agentes personalizados têm sua própria janela de contexto e foram criados para orquestrar habilidades (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com as diretrizes de acessibilidade. Como parte desse trabalho, ele poderia chamar habilidades, como uma para o fluxo de trabalho de pull requests ou outra para executar e gerenciar testes.

> [!NOTE]
> Não há uma única maneira certa de criar um agente personalizado. Como em qualquer trabalho com inteligência artificial, teste e itere para encontrar o que funciona melhor nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Com os agentes personalizados, você vai explorar:

- como definir agentes personalizados;
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todos os usuários, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários apontaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou um modo de alto contraste que os usuários possam ativar e desativar.

Como a acessibilidade é essencial, você quer implementar esse recurso o mais rápido possível. Para gerar a funcionalidade, você usará um agente personalizado.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e revise o arquivo do agente personalizado de acessibilidade:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com os campos `name` e `description` é obrigatório para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível;
   - práticas recomendadas de acessibilidade;
   - exemplos de código para linguagem de marcação de hipertexto (HTML), folhas de estilo em cascata (CSS) e JavaScript;
   - uma lista de erros e armadilhas comuns.

## Criar e atribuir um issue

O painel de agentes é o local central para trabalhar com todos os agentes do seu ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, monitorar o trabalho e até redirecionar uma sessão em andamento e fornecer orientações adicionais. Vamos começar atribuindo ao Copilot uma tarefa para criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para criar o novo issue.
5. Defina **Title** como `Add high contrast mode to website`.
6. Defina **Description** como:

    ```plaintext
    Precisamos de um modo de alto contraste para o site. Deve haver um controle que o usuário possa ativar para usar o alto contraste. A configuração deve ser armazenada no armazenamento local do navegador.
    ```

7. Selecione **Create** para criar o issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Captura de tela da atribuição do agente de nuvem, com o agente personalizado de acessibilidade destacado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximas etapas

Este exercício apresentou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de inteligência artificial especializados em tarefas e domínios específicos. Com agentes personalizados, você pode registrar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot a executar determinados tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como os agentes personalizados são definidos;
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot trabalha na implementação do modo de alto contraste, vamos voltar nossa atenção para [monitorar e direcionar a sessão do agente][next-lesson] pelo painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar-se para usar agentes personalizados na organização][org-custom-agents]
- [Preparar-se para usar agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Aula anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima aula: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
