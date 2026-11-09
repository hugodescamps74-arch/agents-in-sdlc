---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar en un navegador la funcionalidad de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, y permitir que Copilot disponga de automatizaciones adicionales. Para ello sirve Model Context Protocol (MCP): permite que Copilot vaya más allá de las funciones integradas en la interfaz de línea de comandos (CLI) y acceda a más herramientas y servicios mediante servidores MCP.

En esta lección, vas a:

- entender qué es MCP y cómo lo usa Copilot CLI.
- añadir el servidor Playwright MCP si aún no está disponible.
- pedir al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz de usuario también hay que interactuar con ella. Quieres que Copilot pueda usar el sitio web en el que estás trabajando como lo haría un usuario y automatizar así mejor la revisión de los cambios, con más garantías de que funcionan según lo previsto.

## ¿Qué es MCP?

[Model Context Protocol (MCP)][mcp-blog-post] ofrece a los agentes de inteligencia artificial una forma de comunicarse con herramientas y servicios externos. Gracias a MCP, los agentes pueden acceder a información actualizada y realizar acciones en tu nombre.

Estas herramientas y recursos se usan a través de un servidor MCP, que actúa como puente entre el agente de inteligencia artificial y las herramientas y servicios externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Estos son dos servidores MCP conocidos:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a API para gestionar repositorios, incidencias y solicitudes de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: permite automatizar el navegador con Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] que facilita descubrir servidores y contribuir al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y ten en cuenta las implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si el servidor Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos de configuración y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo para añadir un servidor MCP introduciendo este comando en Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. En el campo del nombre, introduce `playwright` y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo de MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Antes has confirmado manualmente que la funcionalidad funciona como esperabas. Ahora usaremos el servidor Playwright MCP disponible en Copilot para hacer lo mismo.

1. Usa esta indicación para pedir a Copilot que valide la funcionalidad con el servidor Playwright MCP:

   ```plaintext
   Inicia la aplicación y usa Playwright MCP para comprobar el filtrado según la incidencia y nuestro plan. Dime qué funciona y qué no, sin hacer cambios. Detén el servidor que hayas iniciado cuando termines.
   ```

> [!NOTE]
> No es necesario pedirle explícitamente a Copilot que use el servidor MCP, ya que normalmente lo detecta por sí solo. Sin embargo, como sabes qué servidor debe usar, puedes indicárselo para ayudarle a elegirlo con más coherencia y ahorrar unos cuantos tokens.

2. Observa cómo Copilot enumera los pasos que sigue en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como esperas.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio web, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el servidor Playwright MCP para explorar tu funcionalidad en un navegador real desde Copilot CLI. En resumen, has:

- aprendido qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadido el servidor Playwright MCP si aún no estaba disponible.
- pedido al agente que controlara un navegador y explorara la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de garantía de calidad (QA)][next-lesson] que combinará la skill y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añade servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
