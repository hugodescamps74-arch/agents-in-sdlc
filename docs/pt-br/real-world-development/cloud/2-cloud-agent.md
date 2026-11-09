---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Aula anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações — se houver alguma — não enfrentem problemas com dívida técnica. Isso pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram paradas no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um rascunho de pull request (PR) para que um desenvolvedor revise. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e liberar os desenvolvedores para se concentrarem em questões mais abrangentes.

Com o agente de nuvem do Copilot, você vai explorar:

- como personalizar o ambiente de geração de código;
- como garantir que as operações sejam realizadas com segurança;
- a importância de delimitar claramente o escopo dos issues;
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que gostaria de resolver. Os prestadores de serviço contratados inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe gostaria que comentários de documentação do TypeScript (TSDoc) fossem adicionados a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando. Por enquanto, a equipe não precisa de uma implementação refinada; basta algo que possa usar para testes de aceitação da experiência do usuário (UX). Isso está bloqueando o trabalho, mas há outros issues de prioridade mais alta no momento.

Esses são exemplos de tarefas que podem ser rapidamente rebaixadas na lista de prioridades e que são ideais para atribuir ao agente de nuvem do Copilot. O agente pode trabalhar nelas de forma assíncrona enquanto o desenvolvedor se dedica a outras tarefas e, depois, revisar o trabalho para confirmar se tudo está conforme o esperado.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de maneira semelhante a um desenvolvedor. Assim como no trabalho com uma pessoa, há várias maneiras de atribuir tarefas, incluindo [atribuir um issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois que você atribui o issue, o Copilot cria um rascunho de pull request para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode acompanhar a sessão do Copilot enquanto ela está em andamento ou depois de concluída. Quando a solução proposta estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot — você não pode deixar de pensar. Até operações aparentemente simples podem se tornar complexas quando analisadas em detalhes. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com inteligência artificial: trabalhe por etapas, aprenda, experimente e adapte a abordagem. Os fundamentos do desenvolvimento de software não mudam com a inclusão da inteligência artificial generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu uma atividade prática sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente de nuvem do Copilot, examine os arquivos de instruções incluídos neste repositório, sem alterá-los, para que você possa observar o efeito deles mais adiante.

Abra os seguintes arquivos na interface Web do GitHub ou em um codespace, se já houver um em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas sobre convenções de TypeScript e comentários de documentação TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica essas instruções aos arquivos `**/*.test.ts`.

Quando você atribuir o issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe se o pull request resultante inclui comentários de documentação TSDoc, segue as convenções de TypeScript e tem cabeçalhos nos arquivos. Esses elementos são definidos por esses arquivos de instruções. Voltaremos a esse ponto ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem do Copilot

Criar código, independentemente de quem esteja fazendo o trabalho, geralmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software (SWE).

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo criando um [workflow de configuração especial][setup-workflow], definido no arquivo `.github/workflows/copilot-setup-steps.yml`, para ser executado antes de o agente começar o trabalho. Assim, ele terá acesso às ferramentas de desenvolvimento e dependências necessárias. Esse workflow já foi configurado para facilitar o andamento do laboratório e permitir que você explore esse recurso. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários de navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para *suas etapas*. O Copilot receberá seu próprio token para as operações.
    permissions:
      # Para clonar o repositório como parte das etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório durante a configuração, o Copilot fará isso automaticamente depois que as etapas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (o aplicativo inteiro agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com qualquer workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se o workflow funciona corretamente, sem precisar esperar o Copilot executá-lo.

## Adicionar documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação alguma. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novos desenvolvedores. Felizmente, o Copilot é muito bom em criar documentação, e este é um issue ideal para atribuir ao agente de nuvem do Copilot. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para criar o novo issue.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Nossa organização exige que funções e métodos tenham comentários de documentação TSDoc quando forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários de documentação onde eles ajudarem a esclarecer o comportamento.
    ```

7. Selecione **Create** para criar o issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de um issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request recém-gerado (PR), que terá um título semelhante a `[WIP]: Code lacks documentation`. Se não aparecer um novo PR na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Após alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está criando o ambiente (executando o workflow mencionado anteriormente), analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar no issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão ao vivo e acompanhar como o Copilot está resolvendo o problema. Ele explora o código e entende o estado atual, faz pausas para avaliar e decidir o plano adequado e também cria código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é permitir que ele execute tarefas de forma assíncrona, liberando você para se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente de nuvem do Copilot e, em seguida, voltando nossa atenção para a criação de código para adicionar funcionalidades ao aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes do jogo

Como já destacamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é a possibilidade de dividir o trabalho, para que você possa se concentrar em um conjunto de tarefas enquanto ele se dedica a outro. Adicionar uma seção de jogos relacionados para a equipe de design talvez não leve muito tempo, mas esse tempo ainda poderia ser usado em outras tarefas. Vamos atribuir o trabalho ao agente de nuvem do Copilot.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    Queremos ajudar as pessoas a descobrir mais jogos, mostrando jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário e fazer testes de aceitação. Os requisitos são:

   - Adicionar um helper de acesso a dados em `src/lib/` que retorne outros jogos da mesma categoria de um jogo, sem incluir o próprio jogo
   - Mostrar uma seção "Jogos relacionados" na página de detalhes do jogo, usando o helper
   - Tratar o caso em que um jogo não tenha jogos relacionados
   - Criar testes de unidade para o novo helper
   - Antes de criar o PR, garantir que todos os testes sejam aprovados
    ```

7. Selecione **Create** para criar o issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de um issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver um conjunto de 👀 no primeiro comentário do issue, indicando que o Copilot está trabalhando nele.

![O Copilot usa o emoji de olhos para indicar que está trabalhando no issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora o Copilot está trabalhando com afinco na segunda solicitação. O agente de nuvem do Copilot trabalha de forma semelhante a um SWE, então você não precisa monitorá-lo continuamente; basta revisar o trabalho quando ele terminar. Vamos voltar nossa atenção para a criação e o uso de agentes personalizados.

## Resumo e próximas etapas

Este exercício apresentou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com o agente de nuvem, você pode atribuir issues ao Copilot para que ele trabalhe neles de forma assíncrona. Você pode usar o Copilot para resolver dívidas técnicas, criar novos recursos ou ajudar a migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente para gerar código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de delimitar claramente o escopo dos issues;
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos voltar nossa atenção para a criação e o uso de agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e tem acesso às instruções personalizadas que exploramos nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima aula: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
