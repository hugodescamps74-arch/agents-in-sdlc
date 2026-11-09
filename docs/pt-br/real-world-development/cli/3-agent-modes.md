---
title: "Lição 3 - Modos do agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para criar a filtragem a partir de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto. Mudanças maiores, porém, exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi desenvolvido para se integrar ao fluxo de trabalho existente da organização e ajudar a criar as coisas certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes: começará com uma issue para gerar uma funcionalidade, verificará se o código está correto, confirmará se ela funciona como esperado e, por fim, fará o merge no projeto.

> [!NOTE]
> Você usará a mesma conversa e branch ao longo do fluxo de trabalho da funcionalidade. Em geral, você usaria branches ou pull requests diferentes para os vários tipos de arquivos com que trabalharia, mas faremos um atalho para nos concentrarmos nos conceitos principais.

Para começar, nesta lição você vai:

- iniciar uma nova conversa do Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar a funcionalidade usando o modo Autopilot.
- revisar o código.
- validar a funcionalidade manualmente em um navegador encaminhado.

À medida que continuar trabalhando nessa funcionalidade, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação com o Model Context Protocol (MCP), criará um agente de garantia da qualidade (QA) e abrirá a PR da funcionalidade.

## Cenário

O catálogo do Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e publicadora. A issue do backlog descreve a funcionalidade, mas detalhes como a combinação de categorias precisam ser acordados antes de escrever o código. Você usará o modo Plan para resolver essas decisões e, em seguida, autorizará uma implementação com escopo definido pelo Autopilot.

## Contexto

Introduzir agentes de programação com IA no seu fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes! O fluxo de trabalho da maioria dos desenvolvedores é parecido com este:

1. Começar com uma issue registrada que descreva o trabalho necessário.
2. Criar um plano do que precisa ser implementado.
3. Implementar e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar uma pull request (PR).
7. Depois que o código for revisado e o processo de integração contínua for concluído com sucesso, fazer o merge do código.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria dos fluxos segue alguma variação do processo acima.

Ao seguir essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos definidos e passe pelo mesmo processo de validação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e faz iterações até considerar a tarefa concluída.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Comece pela issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confirme se o repositório e o terminal estão prontos.

1. Volte ao seu Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme se o merge da PR das avaliações por estrelas foi concluído.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite incluir no contexto uma issue ou pull request (PR) do GitHub.

## Planeje a funcionalidade de filtragem

O planejamento permite definir a abordagem para implementar uma funcionalidade ou realizar tarefas antes de delegá-las ao Copilot. Planejar por alguns instantes é sempre uma boa ideia quando a tarefa é complexa. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme se o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite o prompt a seguir:

   ```plaintext
   Crie um plano para implementar esta funcionalidade.
   ```

O Copilot começará a elaborar o plano! Primeiro, ele explorará o projeto e, em seguida, determinará a melhor abordagem.

3. Durante o processo, o Copilot poderá fazer perguntas sobre o funcionamento dos filtros. Responda com base nas suas preferências. Não há respostas erradas!
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano para revisá-lo.
6. Peça ao Copilot que revise qualquer parte do plano que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo! Vamos deixar o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot faça iterações até considerar o problema resolvido.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção com rótulo semelhante na versão instalada.
2. Confirme se o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe as iterações do Copilot com base no plano definido, a geração de código e a execução dos testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente; por isso, revise o plano antes. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Depois que o código for gerado, revise-o antes de fazer o merge, como faria com qualquer outro código. Vamos revisar o código e executar o site para confirmar se tudo está funcionando bem.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme se o indicador não mostra mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise a saída das verificações do projeto e peça ao Copilot que corrija eventuais falhas.

## Explore a nova funcionalidade

O código parece correto — mas será que funciona? Vamos iniciar o aplicativo como fizemos antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa testar a funcionalidade de filtragem no navegador. Informe a URL e mantenha o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria e por publicadora, além das combinações acordadas no plano.
4. Confirme se o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que interrompa o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma nova conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou a funcionalidade usando o modo Autopilot.
- revisou o código.
- validou a funcionalidade manualmente em um navegador encaminhado.

Em seguida, vamos aprofundar como o código é gerado e garantir que ele siga as práticas documentadas [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
