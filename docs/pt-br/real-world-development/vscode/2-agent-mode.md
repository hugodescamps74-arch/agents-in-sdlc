---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo as atualizações mais simples de um aplicativo normalmente exigem alterações em vários arquivos e a execução de operações como testes. Como desenvolvedor, seu fluxo costuma incluir localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou de fora, fazer outra atualização... e assim por diante.

É nesse ponto que o modo de agente do Copilot entra em ação.

O modo de agente do Copilot foi criado para atuar de forma mais autônoma no IDE. Ele trabalha de maneira semelhante a um desenvolvedor: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas que encontra. Vamos explorar como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem foi criado para trabalhar de forma assíncrona como um parceiro de programação da equipe, interagindo com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações;
- o modo de agente do GitHub Copilot pode implementar novos recursos na interface do usuário (UI) e na camada de dados;
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de fazer merge na base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem por categoria e editora. Você já adicionou um helper de editoras no exercício anterior e agora concluirá o restante do trabalho na camada de dados, na interface do usuário e nos testes com o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender suas funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com um tema voltado a desenvolvedores. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que gera, durante o build, páginas estáticas em HTML (linguagem de marcação de hipertexto). As páginas consultam diretamente um banco de dados SQLite local no frontmatter, por meio do mapeamento objeto-relacional (ORM) do Drizzle — não há uma interface de programação de aplicações (API) de back-end separada nem uma estrutura de interface no lado do cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno elemento `<script>` do Astro, de escopo limitado, que usa as APIs padrão do Document Object Model (DOM).

### Iniciar o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento do Astro. Siga estas etapas para executá-la no GitHub Codespace:

1. Volte ao codespace. Você deve estar na `main` depois do exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e acesse uma nova branch para este trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá um banner que indica a URL, semelhante ao exemplo abaixo:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar a abertura de uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Ao usar um codespace, selecionar um link para a URL local no terminal do Codespace redirecionará automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para seu codespace, que agora está hospedando o servidor Web.

### Explorar o site

Com o site em execução, você pode explorar suas funcionalidades. Os principais recursos incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: quando você seleciona um jogo, abre uma página de detalhes com mais informações, como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção de modelo variam em cada exercício — as etapas indicarão o que fazer quando for necessário.

A implementação inicial do site funciona, mas queremos aprimorá-la com novos recursos. Vamos começar revisando o backlog. Ao configurar o projeto a partir do template, uma lista de issues foi criada automaticamente — peça ao GitHub Copilot para mostrá-la.

1. Selecione **Agent** na lista suspensa de agentes da visualização Chat. O agente **Agent** planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos no terminal e invoca ferramentas.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão expressando preferência por um modelo em detrimento de outro. Ao criar o workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções para que as sugestões de código sejam relativamente consistentes e a experiência seja boa. No entanto, como os modelos de linguagem de grande porte (LLMs) são probabilísticos, talvez você receba sugestões diferentes das apresentadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot poderá usar um comando do Model Context Protocol (MCP) diferente, mas ainda deverá conseguir concluir a tarefa.

3. Peça ao Copilot informações sobre o backlog enviando o seguinte prompt:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude-me a priorizá-los com base na utilidade para os usuários.
   ```

4. Selecione **Continue** para executar o comando que lista todos os issues.
5. Revise a lista de issues gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para os usuários.

## Revisar os arquivos de instruções

Antes de iniciar o agente para gerar código, é uma boa hora para revisar o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contém orientações sobre como adicionar funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, que incluem:
   - uma visão geral da arquitetura;
   - princípios de design de componentes, testabilidade e acessibilidade;
   - links para arquivos de instruções específicos de vários tipos de arquivo, incluindo:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Assim, é possível reutilizar orientações, dividir conjuntos complexos de instruções em arquivos menores e mais gerenciáveis, além de fornecer exemplos e templates ao Copilot.

## Implementar a funcionalidade de filtragem

Para concluir a filtragem, serão necessárias pelo menos três atualizações distintas no aplicativo:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`);
- adicionar ou atualizar testes do comportamento de filtragem;
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes do merge de tudo na base de código. O modo de agente do Copilot pode executar essas tarefas para você. Vamos adicionar a funcionalidade.

1. Você pode continuar na conversa atual com o Copilot ou selecionar **New Chat** para iniciar uma nova.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela de um exemplo de seleção do arquivo de instruções de interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** continua selecionado na lista suspensa de agentes da visualização Chat.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot para implementar a funcionalidade com base no issue relacionado do backlog, usando o seguinte prompt:

   ```plaintext
   Atualize o site para incluir a filtragem por editora e categoria, de acordo com os requisitos do issue relacionado no backlog. Já existe um helper de editoras do exercício anterior; preserve e aprimore esse trabalho conforme necessário para concluir o restante da camada de dados, da interface e dos testes. Confirme se todos os testes foram aprovados antes de concluir. O servidor já está em execução, então não é necessário iniciá-lo.
   ```

6. Observe como o Copilot começa explorando o projeto e localizando os arquivos relacionados à funcionalidade desejada. Você deverá vê-lo encontrar os helpers da camada de dados, a interface e os testes. Em seguida, ele começará a modificar os arquivos e executar os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você verá **Keep** ou **Undo** para todas as alterações. Não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel do Copilot Chat solicitando confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez haja pausas e até falhas em alguns testes durante o processo. Não tem problema. O Copilot alterna entre a geração de código e os testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do Copilot no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Lembre-se: é sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de inteligência artificial.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximas etapas

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot pode implementar novos recursos na interface e na camada de dados;
- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações;
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de fazer merge na base de código.

Agora vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício bônus de exploração — implementar paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Use as habilidades que aprendeu neste exercício para pedir ao Copilot que atualize o site e implemente a paginação. Considere:

- seguir as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções disponíveis;
- decidir como implementar a paginação: por exemplo, se os usuários poderão escolher o tamanho da página ou se ele será fixo;
- fornecer ao Copilot as orientações necessárias no prompt para que a implementação corresponda ao que você deseja;
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
