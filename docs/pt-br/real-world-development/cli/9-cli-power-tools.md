---
title: "Lição 9 - Comandos de barra do GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar o contexto, selecionar modelos, compartilhar sessões e delegar tarefas opcionalmente a um agente de nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda boa ferramenta de CLI, o GitHub Copilot CLI inclui vários comandos de barra para você interagir com ele. Esses comandos dão acesso a recursos avançados, informações internas e opções adicionais de configuração. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usar `/model` para explorar os modelos disponíveis.
- aprender como `/share` pode exportar ou compartilhar uma sessão.
- explorar comandos opcionais para trabalho em paralelo, árvores de trabalho e delegação ao agente de nuvem.

## Cenário

Você concluiu o fluxo de trabalho principal do CLI. Agora, vamos conhecer alguns recursos adicionais: gerenciamento de contexto, troca de modelos, compartilhamento de sessões e delegação opcional para o [agente de nuvem do Copilot][about-cloud-agent].

## Explore o contexto do Copilot CLI

Ao trabalhar em tarefas maiores ou mais complexas, você pode se aproximar do limite máximo de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário; com os comandos de barra, você também pode inspecionar ou compactar o contexto manualmente.

1. Volte ao seu Codespace e inicie o Copilot CLI na raiz do repositório, caso ele ainda não esteja aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre as instruções do sistema, as ferramentas, as mensagens e o espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare o resultado. Talvez não haja uma mudança significativa se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente à medida que a janela se preenche. Use `/compact` quando quiser escolher o momento. Use `/clear` ou `/new` quando mudar para uma tarefa sem relação com a conversa atual e quiser começar uma conversa nova.

## Escolha um modelo

Modelos diferentes têm pontos fortes diferentes, e cada desenvolvedor tem suas próprias preferências. O Copilot CLI permite listar e selecionar o modelo que você quer usar.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhe uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todo mundo a aproveitar melhor as ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou de linguagem de marcação de hipertexto (HTML), criar um link compartilhável ou publicar um GitHub gist.

1. Digite `/help` e revise as opções de `/share` disponíveis na versão instalada.
2. Se quiser compartilhar esta sessão, escolha o destino mais adequado, como `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações da sessão podem incluir prompts, respostas e detalhes do projeto.

A publicação de um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que sua equipe não pretenda compartilhar.

## Opcional: distribua ou delegue o trabalho

O fluxo de trabalho principal do workshop foi concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir tarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar uma árvore de trabalho Git isolada para uma tarefa separada.
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir uma pull request.

Esses comandos são opcionais porque podem criar árvores de trabalho adicionais ou iniciar trabalho remoto. Comece uma tarefa nova e bem definida antes de experimentá-los e revise o resultado com seu fluxo de trabalho habitual. Para saber mais sobre o trabalho assíncrono de agentes, continue com o [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Os comandos de barra do Copilot CLI permitem configurá-lo, compartilhar sessões e acompanhar o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usou `/model` para explorar os modelos disponíveis.
- aprendeu como `/share` pode exportar ou compartilhar uma sessão.
- explorou comandos opcionais para trabalho em paralelo, árvores de trabalho e delegação ao agente de nuvem.

Há muitos outros comandos de barra para explorar no Copilot CLI! Vamos encerrar essa jornada [revisando o que você aprendeu][next-lesson] e conhecendo os próximos passos.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
