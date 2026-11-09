---
title: "Lição 2 - Adicione avaliações por estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards de jogos, revise-a em um navegador encaminhado e faça o merge como sua primeira pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. A mudança será pequena: os dados dos jogos já incluem uma avaliação por estrelas, mas os cards da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a alteração e fará o merge da sua primeira pull request.

Nesta lição, você vai:

- iniciar uma conversa focada com o Copilot em uma branch de funcionalidade.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo para confirmar a alteração em um navegador encaminhado.
- abrir e fazer o merge da sua primeira pull request.

## Cenário

Cada jogo do Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes do jogo. Os cards da página inicial, porém, mostram apenas o título, a categoria, a publicadora e a descrição. Como aquecimento, você pedirá ao agente que exiba a avaliação existente em cada card — uma alteração pequena e independente, ideal para sua primeira sessão.

## Estrutura de uma conversa

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal no CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar uma árvore de trabalho dedicada. As conversas salvas permitem retomar a mesma discussão depois, enquanto os arquivos e a branch continuam sendo o estado comum do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente, a atividade do agente com ferramentas enquanto ele explora e edita arquivos e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Vamos iniciar uma nova conversa para começar a implementar a funcionalidade.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, use o comando a seguir:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Use o prompt a seguir para solicitar a alteração:

   ```plaintext
   Mostre nos cards da lista de jogos a avaliação starRating de cada jogo em uma escala de 5. Se a avaliação for null, mostre "Ainda sem avaliação". Mantenha o layout dos cards, adicione testes e execute as verificações pertinentes.
   ```

O Copilot explora o projeto, localiza os arquivos usados para exibir os detalhes dos jogos e cria o código necessário. Você acaba de adicionar uma funcionalidade com o Copilot CLI!

## Revise o diff

Todas as alterações geradas por inteligência artificial (IA) devem ser revisadas antes do merge, mesmo as pequenas. Vamos examinar as alterações diretamente no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme se o card do jogo exibe a avaliação numérica quando ela está disponível e `Ainda sem avaliação` quando `starRating` é `null`.
3. Confirme se os testes cobrem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot produz resultados probabilísticos, não determinísticos; por isso, seu código pode ser diferente. Revise o comportamento em vez de esperar uma implementação específica.

## Verifique as alterações

Não basta ler o código e presumir que ele funciona. Vamos pedir ao Copilot que inicie o site para que você examine a interface do usuário (UI) atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa inspecionar no navegador a alteração da avaliação por estrelas. Informe a URL e mantenha o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme se os cards dos jogos exibem as avaliações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça que interrompa o servidor iniciado:

   ```plaintext
   Interrompa o servidor de desenvolvimento que você iniciou.
   ```

## Abra e faça o merge da sua primeira pull request

Agora você criou a funcionalidade! É hora de criar uma pull request (PR) para integrar o novo código ao projeto.

1. Peça ao agente padrão que faça o commit da alteração:

   ```plaintext
   Faça o commit das alterações revisadas das avaliações por estrelas com uma mensagem de commit adequada.
   ```

2. Digite `/pr create`. O Copilot CLI envia o commit existente ao criar a PR e exibe a URL dela.
3. Abra a PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao seu Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI! Especificamente, você:

- iniciou uma conversa focada com o Copilot em uma branch de funcionalidade.
- orientou o agente a fazer uma pequena alteração nos cards dos jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo para confirmar a avaliação por estrelas em um navegador encaminhado.
- abriu e fez o merge da sua primeira pull request.

Em seguida, você vai [começar pela issue de filtragem e usar os modos Plan e Autopilot][next-lesson] para criar uma funcionalidade maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
