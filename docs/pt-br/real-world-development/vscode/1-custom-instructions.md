---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Aula anterior: Pré-requisitos][previous-lesson] · [Próxima aula: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com inteligência artificial generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se há informações de contexto que o Copilot precisa conhecer —, esse contexto deve estar acessível. Há várias maneiras de compartilhar um contexto específico com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções com escopo de caminho;
- gerar o primeiro componente da camada de dados para filtragem (um helper de editoras) com as instruções *atuais*;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`;
- executar novamente o mesmo prompt e observar como o código gerado passa a seguir o novo padrão;
- fazer commit das atualizações das instruções e das alterações de filtragem na `main` para que o Copilot possa usar as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões que você definiu. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança no comportamento após a atualização das instruções, não obter exatamente o mesmo resultado caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa ter testes de unidade.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários de documentação do TypeScript (TSDoc).
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso permite orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar as convenções de programação, as bibliotecas e até os tipos de comentários que prefere. Também pode criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md` é um único arquivo de instruções enviado ao Copilot para **todas** as solicitações feitas no repositório. Ele deve conter informações sobre o projeto que sejam relevantes para a maioria das solicitações feitas ao Copilot Chat ou ao CLI.
- Você pode criar arquivos `.github/instructions/*.instructions.md` para tarefas ou tipos de arquivo específicos. Use-os para fornecer diretrizes para linguagens específicas, como TypeScript ou Astro, ou para tarefas como criar um componente de interface ou um novo conjunto de testes de unidade.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instruções são usados apenas para geração de código no Copilot Chat — não para conclusões de código nem sugestões de edição seguinte.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto os arquivos de instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [aceita arquivos de instruções em outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma discussão completa sobre como criar arquivos de instruções está fora do escopo deste workshop. No entanto, os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções em `copilot-instructions.md` focadas em orientações para o projeto, como uma descrição do que está sendo criado, a estrutura do projeto e padrões gerais de programação.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo, como testes de unidade, componentes Astro e camada de dados, ou para tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve (e não deve) ser escrito.

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira certa de usar inteligência artificial. Com a experimentação, você descobrirá o que funciona melhor para o projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, você talvez perceba que há arquivos para vários tipos de tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de um jeito diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI) — a aula da interface que você estiver usando indicará quando for relevante.
>
> Quer encontrar templates ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções fornecidos por este repositório — há um arquivo principal, `copilot-instructions.md`, e um conjunto de arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Observe que há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo — ele define um padrão glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript, por exemplo, um que corresponda a `**/*.test.ts`, será incluído.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis, além de indicar ao Copilot exemplos para seguir ao gerar código. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários de documentação TSDoc e cabeçalhos de comentários nos arquivos.

## Criar uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as alterações.

1. Volte ao codespace usado no exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e acesse uma nova branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção de modelo variam em cada exercício — as etapas indicarão o que fazer quando for necessário.

Para observar o impacto das instruções personalizadas, primeiro envie um prompt com o arquivo de instruções atual. Depois, você o atualizará e enviará o mesmo prompt novamente para comparar os resultados.

1. Feche as guias abertas no editor para que o Copilot use apenas o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar ao Copilot onde o helper deve ficar.
3. Selecione **Agent** na lista suspensa de agentes da visualização Chat para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o seguinte prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, com o nome e o identificador (ID) de cada uma. Aplique as alterações nos arquivos.
   ```

5. O Copilot explorará o projeto e aplicará atualizações de código, muitas vezes abrangendo o arquivo do helper e os testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras — isso vem das convenções da camada de dados definidas em `.github/instructions/drizzle.instructions.md` (que se aplica a `src/lib/*.ts`).
7. Observe que o código proposto **não tem** comentários de documentação TSDoc nem um cabeçalho de comentários no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários de documentação mesmo sem essa orientação. Se isso acontecer, tudo bem — o objetivo continua sendo observar a melhoria de consistência após a atualização das instruções.

## Adicionar um novo padrão para o repositório

Como vimos, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de programação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões de programação do projeto — mas ainda não contém regras para documentação no código, por isso o helper gerado não tinha comentários de documentação.
3. Adicione as seguintes linhas de Markdown logo abaixo dos padrões existentes, para orientar o Copilot a adicionar cabeçalhos de comentários nos arquivos e comentários de documentação TSDoc:

   ```markdown
   - Toda função exportada deve ter um comentário TSDoc que descreva sua finalidade, seus parâmetros e seu valor de retorno.
   - Antes das importações ou de qualquer código, adicione ao arquivo um bloco de comentários que explique sua finalidade.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na aula anterior, é possível criar arquivos de instruções no nível do repositório (`.github/copilot-instructions.md`) para orientações gerais ou arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo no nível do repositório é o lugar certo para padrões aplicáveis a todo o projeto, como a regra de comentários de documentação que você acabou de adicionar.

## Executar novamente o prompt e observar a diferença

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o histórico da conversa.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o **mesmo prompt** de antes:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, com o nome e o identificador (ID) de cada uma. Aplique as alterações nos arquivos.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Helpers de acesso a dados de editoras para a plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com suas IDs e seus nomes.
    *
    * @param db - O cliente do banco de dados Drizzle.
    * @returns Uma promessa que é resolvida com uma matriz de objetos de editora.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão e aplicá-lo ao código real que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são recursos do repositório como qualquer outro e, por isso, são gerenciados com o mesmo fluxo de controle de versão. Vamos fazer commit das alterações e enviar a branch ao repositório.

1. Abra uma nova janela de terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme se as alterações nas instruções e no helper estão presentes:

   ```bash
   git status
   ```

3. No terminal, prepare e faça commit das alterações nas instruções e de todos os arquivos modificados pelo Copilot para criar a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras atualizações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e fazer merge de um pull request (PR)

Com a branch enviada, vamos criar um pull request e vinculá-lo ao issue sobre os padrões de documentação no backlog. Em vez de fazer isso manualmente, podemos pedir ao Copilot, que já tem acesso às ferramentas do GitHub conectadas ao projeto, para localizar o issue, criar um PR para fechá-lo e, depois, fazer o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot age no GitHub por você. O template do projeto já conecta as ferramentas do GitHub ao seu espaço de trabalho, então não é necessário configurar nada — na primeira vez que o Copilot usar uma delas, o VS Code talvez solicite que você entre no GitHub. Siga as instruções para autorizar o acesso. Você aprenderá como essa conexão funciona (Model Context Protocol (MCP)) em um exercício posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização do Copilot Chat e confirme se **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot para localizar o issue relacionado à atualização dos arquivos de instruções e criar um PR da branch atual, descrevendo as atualizações nas instruções e a nova base do helper de editoras:

   ```
   Encontre o issue relacionado à atualização do arquivo de instruções. Crie um novo PR a partir da branch atual, indique que o PR fecha esse issue e mencione também que adicionamos a base do helper de editoras para o próximo trabalho de filtragem.
   ```

4. O Copilot começará a localizar o issue e criar o PR.
5. Quando o Copilot pedir permissão para executar ações do GitHub em seu nome, revise o comando e selecione **Allow** se for apropriado.
6. Depois que o PR for criado, peça ao Copilot para fazer o merge do PR e retornar sua branch para `main` usando o seguinte prompt:

   ```
   Faça o merge do PR em main. Em seguida, volte localmente para main e baixe o código mais recente para deixar tudo atualizado.
   ```

7. Quando o Copilot pedir permissão para executar ações do GitHub e comandos do shell em seu nome, revise o comando e selecione **Allow** se for apropriado.

Você acabou de criar e fazer merge de um pull request com a ajuda do GitHub Copilot.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e, em seguida, usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *atuais*;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`;
- enviar novamente o mesmo prompt e observar como o código proposto passa a seguir o novo padrão;
- fazer commit das alterações nas instruções e da base do helper na `main` para que o próximo exercício possa usá-las.

Em seguida, você colocará essas instruções em prática no [modo de agente][next-lesson], enquanto o Copilot adiciona um novo recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
