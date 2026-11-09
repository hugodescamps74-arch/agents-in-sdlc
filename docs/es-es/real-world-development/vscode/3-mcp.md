---
title: "Ejercicio 3: Probar la función con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo Agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la función de filtrado con el modo Agente. Antes de abrir una solicitud de cambios, comprueba que funciona en el navegador. En vez de recorrer la aplicación manualmente, conectarás el servidor Playwright, que se integra mediante el Protocolo de contexto de modelo (Model Context Protocol, MCP), y dejarás que Copilot controle un navegador real para probar la función. Después publicarás la rama y abrirás una solicitud de cambios.

En este ejercicio:

- aprenderás qué es el Protocolo de contexto de modelo (Model Context Protocol, MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- añadirás el servidor Playwright MCP al espacio de trabajo;
- pedirás a Copilot que lo utilice para probar manualmente la función de filtrado en un navegador;
- publicarás la rama y abrirás una solicitud de cambios para el trabajo de filtrado.

## ¿Qué es el Protocolo de contexto de modelo (Model Context Protocol, MCP)?

El modo Agente es mucho más potente cuando puede acceder a recursos externos al editor. Copilot lo consigue mediante el Protocolo de contexto de modelo (Model Context Protocol, MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo Agente y su interacción con el contexto, los modelos de lenguaje de gran tamaño (LLM) y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[El Protocolo de contexto de modelo (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de inteligencia artificial (IA) se comuniquen con herramientas y servicios externos. Gracias a MCP, los agentes pueden comunicarse con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de inteligencia artificial (IA) y las herramientas y los servicios externos. El servidor MCP gestiona la comunicación entre el agente de IA y las herramientas externas, como las interfaces de programación de aplicaciones (API) existentes o las herramientas locales, por ejemplo, los paquetes del gestor de paquetes npm. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente de IA.

Algunos servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server):** proporciona acceso a un conjunto de API para gestionar repositorios de GitHub. Permite que el agente de IA realice acciones como crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp):** permite automatizar el navegador con Playwright. El agente de IA puede realizar acciones como ir a páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a diferentes herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) que facilita descubrir servidores y contribuir al ecosistema.

> [!CAUTION]
> Desde el punto de vista de la seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar un servidor MCP, revisa detenidamente su código fuente, verifica quién lo publica y ten en cuenta las implicaciones de seguridad. Utiliza solo servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Deberías encontrar un servidor `github` ya configurado:

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

La entrada `github` viene incluida en la plantilla del proyecto. Por eso Copilot pudo consultar la lista de incidencias en el ejercicio anterior. Utiliza el [servidor GitHub MCP remoto][remote-github-mcp-server], así que no hay que instalarlo localmente. VS Code se conecta mediante el protocolo de transferencia de hipertexto (HTTP) y te autenticas con GitHub la primera vez que Copilot utiliza una de sus herramientas.

## Añadir el servidor Playwright MCP

Ahora añadirás un segundo servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la función.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

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

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo mediante `npx @playwright/mcp@latest`. El indicador `--headless` hace que Playwright ejecute el navegador sin mostrar una ventana, algo necesario en un codespace, que no dispone de un escritorio para mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador necesario ya estará instalado. Si Copilot indica más adelante que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Iniciar y autorizar el servidor

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot utiliza las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar ahora el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pide que confirmes que confías en el servidor, revisa la configuración y confirma que confías en él para que se inicie.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code lo iniciará y te pedirá que confirmes que confías en él la primera vez que Copilot necesite sus herramientas.

## Probar la función de filtrado

El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En vez de recorrer la aplicación para comprobar el trabajo, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados. Después, puede resumirte lo que ha observado. Es una forma rápida de comprobar que una función se comporta como esperas sin salir de la conversación.

Internamente, el servidor Playwright MCP utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en vez de capturas de pantalla. Así, el agente trabaja con elementos estructurados y etiquetados —botones, enlaces y elementos de lista—, como lo hace la tecnología de asistencia. Por tanto, una comprobación funcional rápida también permite comprobar algunos aspectos básicos de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la función de filtrado que acabas de crear.

1. En Copilot Chat, envía esta indicación:

   ```text
   Utiliza el servidor Playwright MCP para abrir un navegador en la aplicación que está en ejecución en http://localhost:4321 y comprobar la nueva función de filtrado de juegos:

   1. Ve a la página de juegos y anota cuántos aparecen.
   2. Aplica un filtro de categoría y comprueba que la lista solo muestra los juegos de esa categoría.
   3. Quita el filtro y, después, aplica un filtro de editorial. Comprueba que la lista solo muestra los juegos de esa editorial.
   4. Combina un filtro de categoría con uno de editorial y comprueba que los resultados cumplen ambos criterios.

   Describe lo que observes en cada paso e indica cualquier comportamiento inesperado.
   ```

Copilot abrirá un navegador mediante el servidor Playwright MCP, recorrerá los pasos y te explicará lo que haya encontrado. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pídele que corrija el código antes de abrir una solicitud de cambios.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar la indicación. La primera vez que Copilot utilice el servidor Playwright MCP, quizá tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de cambios

Ahora que has comprobado que la función funciona, puedes abrir una solicitud de cambios (PR) para que el equipo la revise. El primer paso es publicar la rama `filtering-vscode`.

1. Ve al panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para publicar la rama en el repositorio.

## Crear la solicitud de cambios

Hay varias formas de crear una solicitud de cambios, por ejemplo, en github.com o mediante la interfaz de línea de comandos de GitHub (CLI). Como ya estás trabajando con GitHub Copilot, deja que la cree por ti. Puede buscar la incidencia correspondiente y vincularla a la solicitud.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** esté seleccionado en el menú desplegable de agentes para que Copilot pueda utilizar las herramientas de GitHub.
3. Pídele a Copilot que cree una solicitud de cambios:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editorial. Crea una solicitud de cambios para la rama actual y vincúlala a la incidencia correcta.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones necesarias. La primera vez que utilice una herramienta de GitHub, quizá tengas que autenticarte. Sigue las instrucciones para autorizarlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la solicitud de cambios.
6. Selecciona el enlace que genera Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- has aprendido qué es el Protocolo de contexto de modelo (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- has añadido el servidor Playwright MCP al espacio de trabajo;
- lo has utilizado para probar manualmente la función de filtrado en un navegador antes de publicarla;
- has publicado la rama y abierto una solicitud de cambios para el trabajo de filtrado.

Has utilizado un servidor MCP para *probar* una función, pero MCP es solo una de las formas de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
