---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Aula anterior: Pré-requisitos][previous-lesson] · [Próxima aula: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com inteligência artificial generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se o Copilot precisa conhecer informações de contexto —, esse contexto deve estar acessível. Os [arquivos de instruções][instruction-files] fornecem essas orientações para que o Copilot entenda não apenas *o que* você quer que ele faça, mas também *como* deve fazer.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções com escopo de caminho;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente das trilhas do VS Code e da interface de linha de comando (CLI), aqui você não usará um prompt *antes e depois*. O agente de nuvem do Copilot trabalha de forma assíncrona com issues do GitHub, por isso é mais difícil demonstrar seu impacto lado a lado, em tempo real. Você verá a influência do arquivo de instruções mais adiante nesta trilha, ao revisar os pull requests produzidos pelo agente de nuvem.

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

## Adicionar um novo padrão para o repositório

Agora você fará a única edição deste exercício: adicionar um padrão para todo o projeto, exigindo que a documentação fique no código em comentários de documentação TSDoc e em cabeçalhos de comentários nos arquivos. O agente de nuvem usará esse padrão quando trabalhar nos issues que você atribuir mais adiante nesta trilha.

Antes de editar, prepare uma branch de trabalho. O agente de nuvem lerá as instruções da branch de destino do issue, mas você fará o commit das alterações de forma organizada.

1. No terminal do codespace, crie e acesse uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## Fazer commit, push e merge da atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino do issue. Quando você atribuir um issue ao Copilot no próximo exercício, o Copilot criará uma branch a partir de `main`. Portanto, as alterações nas instruções precisam chegar à `main` para que o agente de nuvem possa usá-las.

1. Prepare e faça commit das alterações:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente de nuvem poderá ler essas instruções quando trabalhar nos issues que você atribuir no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente na `main` durante este workshop, ignore a etapa da branch e faça commit diretamente na `main`. A etapa da branch está incluída para que o workshop reproduza a maneira como você trabalharia em um projeto real.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e adicionou um novo padrão para todo o repositório a `.github/copilot-instructions.md`. Você verá esse padrão aplicado nos pull requests que o agente de nuvem gerar no restante desta trilha.

Em seguida, [atribua seu primeiro issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
