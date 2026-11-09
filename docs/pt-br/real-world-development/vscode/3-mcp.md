---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Aula anterior: Modo de agente][previous-lesson] · [Próxima aula: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request (PR), confirme se ele realmente funciona no navegador. Em vez de navegar pelo aplicativo manualmente, você conectará o **servidor MCP do Playwright** e permitirá que o Copilot controle um navegador real para testar o recurso. Em seguida, publicará sua branch e abrirá o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot com novas ferramentas;
- adicionar o servidor MCP do Playwright ao espaço de trabalho;
- pedir ao Copilot para usá-lo e testar manualmente o recurso de filtragem em um navegador;
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) permite que o Copilot faça isso: é uma maneira padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama que mostra o funcionamento interno do modo de agente e como ele interage com o contexto, o modelo de linguagem de grande porte (LLM) e as ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de inteligência artificial se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes podem acessar informações atualizadas (usando recursos) e executar ações em seu nome (usando ferramentas).

Essas ferramentas e recursos são acessados por meio de um servidor MCP, que atua como uma ponte entre o agente de inteligência artificial e as ferramentas e os serviços externos. O servidor MCP gerencia a comunicação entre o agente e as ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP bastante usados:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: oferece acesso a um conjunto de interfaces de programação de aplicações (APIs) para gerenciar repositórios do GitHub. Ele permite que o agente execute ações como criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador com o Playwright. Ele permite que o agente execute ações como acessar páginas Web, preencher formulários e selecionar botões.

Há muitos outros servidores MCP que oferecem acesso a diferentes ferramentas e recursos. O GitHub hospeda um [registro MCP](https://github.com/mcp) para facilitar a descoberta de servidores e contribuições para o ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, analise cuidadosamente o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use apenas servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem do template do projeto e explica por que o Copilot conseguiu ler seu backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalá-lo localmente. O VS Code se conecta a ele pelo protocolo de transferência de hipertexto (HTTP), e você se autentica no GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicionar o servidor MCP do Playwright

Agora você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, exatamente o que você precisa para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. O sinalizador `--headless` faz com que o Playwright execute o navegador sem uma janela visível, o que é necessário dentro de um codespace, onde não há uma área de trabalho para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar mais tarde que o navegador está ausente, peça para executar `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisar das ferramentas de um servidor, o VS Code o iniciará e solicitará sua confirmação de que você confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar se a configuração está correta.
2. Se o VS Code pedir que você confirme que confia no servidor, revise a configuração e escolha a opção para confiar nele e permitir que o servidor seja iniciado.

Iniciar o servidor manualmente é opcional. Se você ignorar esta etapa, o VS Code o iniciará — e solicitará que você confirme se confia nele — na primeira vez que o Copilot precisar de suas ferramentas na próxima etapa.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] permite que o Copilot controle um navegador real. Em vez de navegar pelo aplicativo para verificar seu trabalho, o agente pode abrir uma página, navegar pelo site, aplicar filtros, ler os resultados e informar o que encontrou. Essa é a maneira mais rápida de confirmar se um recurso funciona como esperado, sem sair da conversa.

Nos bastidores, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Isso significa que o agente raciocina com base em elementos estruturados e rotulados (botões, links, itens de lista), como faz a tecnologia assistiva. Assim, uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para testar o recurso de filtragem que você acabou de criar:

```text
Usando o servidor MCP do Playwright, abra um navegador para o aplicativo em execução em http://localhost:4321 e verifique o novo recurso de filtragem de jogos:

1. Acesse a página de jogos e observe quantos jogos estão listados.
2. Aplique um filtro de categoria e confirme se a lista é atualizada para exibir somente jogos dessa categoria.
3. Limpe o filtro e aplique um filtro de editora. Confirme se a lista é atualizada para exibir somente os jogos dessa editora.
4. Combine um filtro de categoria e um de editora e confirme se os resultados respeitam ambos.

Relate o que você observou em cada etapa e indique qualquer comportamento inesperado.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá cada etapa e informará o que encontrou. Compare o resumo com os critérios de aceitação do issue. Se algo não estiver certo, faça perguntas complementares ou peça ao Copilot para corrigir o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para este teste. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que o navegador está ausente, peça para executar `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Agora que confirmou que o recurso funciona, você está pronto para abrir um pull request (PR) para que a equipe o revise. Primeiro, publique a branch `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, incluindo o github.com e a interface de linha de comando do GitHub (CLI). Como você já está trabalhando com o GitHub Copilot, vamos pedir a ele que crie o PR. Ele pode localizar o issue relacionado e associá-lo ao PR.

1. Acesse o painel do Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme se **Agent** está selecionado na lista suspensa de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot para criar um PR:

   ```plaintext
   Encontre no repositório o issue relacionado à filtragem por categoria e editora. Crie um novo pull request (PR) para a branch atual e associe-o ao issue correto.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot execute as tarefas necessárias para obter informações e realizar operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar-se no GitHub. Siga as instruções para permitir o acesso.
5. Observe como o Copilot pesquisa os issues, encontra o correto e cria o PR.
6. Selecione o link fornecido pelo Copilot para revisar o pull request, mas **não faça merge ainda**.

## Resumo e próximas etapas

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot com novas ferramentas;
- adicionou o servidor MCP do Playwright ao espaço de trabalho;
- usou o servidor para testar manualmente o recurso de filtragem em um navegador antes de publicá-lo;
- publicou sua branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso — mas o MCP é apenas uma das maneiras de ampliar os recursos do Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para facilitar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
