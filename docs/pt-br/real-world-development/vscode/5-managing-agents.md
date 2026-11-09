---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Aula anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você coloca o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho ocorre em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição de arquivo à medida que acontecem. Você pode revisar cada diff no próprio chat, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts complementares para aprimorar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade;
- revisar e aceitar as edições de arquivos propostas;
- orientar o agente durante a sessão para adicionar um controle para o modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade que você usou no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design fez uma solicitação adicional: também gostaria de um controle para o modo claro, para usuários que consideram o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão atual para ampliar o trabalho.

## Monitorar o progresso do Copilot

A visualização do Copilot Chat mostra cada etapa executada pelo agente: os arquivos que ele explora, os comandos que executa e as edições que propõe. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização do Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos, além de comandos de terminal cuja execução ele solicitar.
3. Quando o agente propuser uma edição de arquivo, o diff aparecerá na conversa:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed**, na parte superior do chat, para ver todos os arquivos modificados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão (por exemplo, para executar um comando de terminal), o agente pausará o trabalho e exibirá uma solicitação **Allow**. Aprove apenas os comandos que você considera seguros.

5. Aguarde até que o agente termine de implementar o modo de alto contraste. Você deverá ver uma mensagem final que resume as alterações realizadas.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você ampliará a mesma conversa para adicionar um controle para o modo claro. Como continuará na mesma sessão de chat, o agente manterá o contexto do trabalho anterior, incluindo os componentes que alterou, a abordagem de estilo adotada e o padrão de armazenamento local configurado.

1. No campo de entrada do Copilot Chat, envie o seguinte prompt complementar:

    ```plaintext
    Bom trabalho! Agora adicione um controle para o modo claro junto com o controle de alto contraste. O modo claro deve seguir o mesmo padrão: a configuração deve ser persistida no armazenamento local e o controle deve ficar na mesma área da interface. O usuário deve poder ativar o alto contraste e o modo claro de forma independente.
    ```

2. O agente continuará o trabalho, explorando as alterações já feitas e propondo adições para o modo claro.
3. Revise cada nova edição à medida que ela aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, se tentar refatorar os controles de um jeito que não agrada a você —, interrompa-o com o botão **Stop** ao lado do campo de entrada do chat e envie uma mensagem complementar para esclarecer o que você quer.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Envie prompts complementares para pedir ajustes ("simplifique as folhas de estilo em cascata (CSS) — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro controle; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para os dois controles").

## Revisar o conjunto de alterações

Antes de fazer commit do trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em testes relacionados.
3. Abra alguns dos arquivos alterados e analise os diffs. Confirme se a implementação segue os padrões de acessibilidade do agente personalizado: atributos da especificação Accessible Rich Internet Applications (ARIA), navegação pelo teclado, linguagem de marcação de hipertexto (HTML) semântica e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará [na próxima aula][next-lesson].

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou cada edição de arquivo à medida que era feita, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Neste exercício, você explorou:

- como monitorar o trabalho de um agente em tempo real;
- como aceitar ou rejeitar edições individuais de arquivos;
- como orientar uma sessão em andamento com prompts complementares.

Na [próxima aula][next-lesson], você publicará seu trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local durante este laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Aula anterior: Agentes personalizados][previous-lesson] | [Próxima aula: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
