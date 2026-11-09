---
title: "Lição 6 - Valide a funcionalidade com o Playwright MCP"
description: "Inspecione ou configure o servidor Playwright do Model Context Protocol (MCP) no Copilot CLI e use-o para explorar a funcionalidade de filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas escrever código. Precisamos trabalhar com dados, serviços externos e até disponibilizar automações adicionais para o Copilot. É aí que entram os servidores do Model Context Protocol (MCP). Eles permitem que o Copilot vá além dos recursos integrados ao CLI e tenha acesso a mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionar o servidor Playwright MCP, caso ele ainda não esteja disponível.
- pedir ao agente que controle um navegador e explore sua funcionalidade de filtragem.

## Cenário

Embora testes de unidade e de ponta a ponta sejam importantes, validar atualizações na interface do usuário (UI) exige interagir com ela. Você quer permitir que o Copilot use o site em desenvolvimento como um usuário faria, para automatizar ainda mais as alterações e aumentar a confiança de que elas funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas e executar ações em seu nome.

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e os serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Dois servidores MCP populares são:

- **[GitHub MCP Server][github-mcp]**: fornece acesso a interfaces de programação de aplicações (APIs) para gerenciar repositórios, issues e pull requests do GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: oferece recursos de automação de navegador usando o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do seu projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor Playwright MCP

Se o servidor Playwright MCP ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como um usuário.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas de configuração e continue na próxima seção.

1. Volte ao seu Codespace.
2. No Copilot CLI, digite o comando a seguir para abrir a caixa de diálogo de adição de um servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e depois pressione <kbd>Tab</kbd>.
5. Cole o texto a seguir na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo do MCP.

## Peça ao Copilot que explore a funcionalidade com o Playwright

Antes, você confirmou manualmente que a funcionalidade funciona como esperado. Agora, vamos usar o servidor Playwright MCP disponível no Copilot para fazer o mesmo!

1. Use o prompt a seguir para pedir ao Copilot que valide a funcionalidade com o servidor Playwright MCP:

   ```plaintext
   Inicie o aplicativo e use o Playwright MCP para verificar os filtros com base na issue e no nosso plano. Informe o que funciona e o que não funciona, sem fazer alterações. Quando terminar, interrompa o servidor que iniciou.
   ```

> [!NOTE]
> Não é necessário dizer ao Copilot que use o servidor MCP; normalmente, ele identifica por conta própria qual ferramenta usar. Mas, como você sabe qual ferramenta deve ser usada, vale a pena indicar a direção! Isso ajuda a obter resultados mais consistentes e economiza alguns tokens.

2. Observe as etapas que o Copilot executa no navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e confirme se tudo está funcionando como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, interromperá o servidor e apresentará um relatório.

## Resumo e próximos passos

Parabéns, você usou o servidor Playwright MCP para explorar sua funcionalidade em um navegador real a partir do Copilot CLI! Em resumo, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionou o servidor Playwright MCP, caso ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse sua funcionalidade de filtragem.

Em seguida, você vai [criar um agente personalizado de QA][next-lesson] que reúne a skill e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
