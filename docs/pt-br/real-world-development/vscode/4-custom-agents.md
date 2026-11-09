---
title: "Exercício 4 - Agentes personalizados"
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

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todos os usuários, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários apontaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou um modo de alto contraste que os usuários possam ativar e desativar.

Como a acessibilidade é essencial, você quer implementar esse recurso o mais rápido possível. Você usará um agente personalizado para gerar a funcionalidade.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente;
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e abra um terminal. Em seguida, acesse uma branch nova baseada em `main` para o trabalho de acessibilidade (assim, o pull request (PR) de filtragem do Exercício 3 ficará separado):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com os campos `name` e `description` é obrigatório para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível;
   - práticas recomendadas de acessibilidade;
   - exemplos de código para linguagem de marcação de hipertexto (HTML), folhas de estilo em cascata (CSS) e JavaScript;
   - uma lista de erros e armadilhas comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code exibe todos os agentes personalizados definidos em `.github/agents` na lista suspensa de agentes, na parte inferior da visualização do Copilot Chat. Você pode selecionar um agente personalizado para limitar uma sessão de chat às instruções e ferramentas desse agente.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção de modelo variam em cada exercício — as etapas indicarão o que fazer quando for necessário.

1. Selecione **Agent** na lista suspensa de agentes da visualização Chat, caso ainda não esteja selecionado.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione a lista suspensa de agentes na parte inferior da visualização de chat (ela mostra o agente ativo, que por padrão é **default**).
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie o seguinte prompt ao agente de acessibilidade:

    ```plaintext
    Adicione um modo de alto contraste ao site. Deve haver um controle que o usuário possa ativar para usar o alto contraste, e a configuração deve continuar valendo após recarregar a página, usando o armazenamento local do navegador.
    ```

5. O Copilot Chat começará a trabalhar: explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para você revisar.
6. À medida que as edições forem feitas, o indicador **Files changed** será atualizado para mostrar o conjunto de arquivos modificados pelo agente.

> [!NOTE]
> Esse processo provavelmente levará alguns minutos. O Copilot está fazendo alterações reais no repositório: editará arquivos existentes, como componentes Astro, CSS e testes relacionados, se houver.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximas etapas

Este exercício apresentou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de inteligência artificial especializados em tarefas e domínios específicos. Com agentes personalizados, você pode registrar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot a executar determinados tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como os agentes personalizados são definidos;
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [monitorará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que são feitas e adicionando um controle para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Aula anterior: Testar seu recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima aula: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
