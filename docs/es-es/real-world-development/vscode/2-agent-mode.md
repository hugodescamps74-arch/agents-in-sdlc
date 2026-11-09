---
title: "Ejercicio 2: Añadir funcionalidad con el modo Agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones, como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y realizar otra actualización. Y así sucesivamente.

Para eso sirve el modo Agente de Copilot.

El modo Agente de Copilot funciona de forma más autónoma en el entorno de desarrollo integrado (IDE). Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizar el modo Agente para añadir nuevas funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo Agente y el agente en la nube ofrecen experiencias distintas. El modo Agente realiza tareas en el IDE, con ciclos rápidos de comentarios e interacción. El agente en la nube trabaja de forma asíncrona, como un miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás cómo:

- el modo Agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y realizar cambios coordinados;
- el modo Agente de GitHub Copilot puede implementar funciones en la interfaz de usuario y la capa de datos;
- revisar los cambios y las pruebas generados por el modo Agente de Copilot antes de combinar los cambios en el código.

## Escenario

Como la lista de juegos sigue creciendo, quieres que los usuarios puedan filtrarla por categoría y editorial. En el ejercicio anterior ya añadiste una función auxiliar para consultar editoriales. Ahora completarás el trabajo restante en la capa de datos, la interfaz de usuario y las pruebas con el modo Agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con un tema de desarrollo. Permite mostrar listas de juegos y ver sus detalles. Se trata de una única aplicación Astro que genera páginas de lenguaje de marcado de hipertexto (HTML) estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante el mapeador objeto-relacional (ORM) Drizzle en su frontmatter; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz de usuario del lado del cliente. Las funciones auxiliares reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con ámbito propio y las API estándar del modelo de objetos del documento (DOM).

### Iniciar el sitio web

Para facilitar la ejecución del sitio web, se ha preparado la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarlo en tu GitHub Codespace siguiendo estos pasos:

1. Vuelve al codespace. Deberías estar en `main` tras completar el ejercicio anterior.
2. Abre una ventana nueva del terminal en el codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella para trabajar en el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio web manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> Si utilizas un codespace, al seleccionar el enlace a la URL localhost del terminal se abrirá automáticamente `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Ahora que el sitio está en ejecución, puedes explorar sus funciones. Entre las principales se encuentran:

- **Página de inicio:** muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego:** al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar la lista de tareas pendientes con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

La implementación inicial del sitio funciona, pero queremos mejorarla con nuevas funciones. Empezaremos revisando la lista de tareas pendientes. Al configurar el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias. Pídele a GitHub Copilot que te muestre esos elementos.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente puede planificar e implementar cambios en varios archivos de forma autónoma, ejecutar comandos en el terminal e invocar herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no muestran preferencia por un modelo concreto. Al preparar el taller utilizamos Claude Sonnet 4.5 y por eso lo mencionamos en estas instrucciones: esperamos que así las sugerencias de código sean relativamente coherentes y la experiencia resulte adecuada. Sin embargo, como los modelos de lenguaje de gran tamaño (LLM) son probabilísticos, es posible que las sugerencias no coincidan con las que se describen en el taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede utilizar un comando del Protocolo de contexto de modelo (Model Context Protocol, MCP) diferente y aun así completar la tarea.

3. Envía a Copilot esta indicación para que consulte las incidencias pendientes:

   ```plaintext
   Muéstrame la lista de incidencias pendientes del repositorio de GitHub. Ayúdame a priorizarlas según cuáles sean más útiles para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista de incidencias generada.

Observa que Copilot también ha priorizado los elementos según cuáles cree que serán más útiles para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedirle al agente que genere código, revisa el archivo de instrucciones que utilizarás para proporcionar contexto a Copilot. Usarás el archivo de la [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En el codespace, ve a `.github/instructions/ui.instructions.md`.
2. Fíjate en las pautas generales para añadir funciones, entre ellas:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones para distintos tipos de archivo, como:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar pautas, dividir conjuntos complejos en partes más manejables y proporcionar a Copilot ejemplos y plantillas.

## Implementar la función de filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios distintos en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código. El modo Agente de Copilot puede encargarse de estas tareas. Añadamos la funcionalidad.

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de la selección del archivo de instrucciones de la interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** siga seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** siga seleccionado como modelo.
5. Pídele a Copilot que implemente la funcionalidad según la incidencia relacionada de la lista de tareas pendientes:

   ```plaintext
   Actualiza el sitio para añadir filtros por editorial y categoría según los requisitos de la incidencia relacionada de GitHub en la lista de tareas pendientes. Ya hay una función auxiliar para consultar editoriales del ejercicio anterior, así que consérvala y ajústala si hace falta mientras completas el trabajo restante en la capa de datos, la interfaz de usuario y las pruebas. Comprueba que todas las pruebas se superen antes de terminar. El servidor ya está en ejecución; no hace falta que lo inicies.
   ```

6. Observa cómo Copilot empieza explorando el proyecto y localizando los archivos relacionados con la funcionalidad. Debería encontrar las funciones auxiliares de la capa de datos, la interfaz de usuario y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y del código. Mientras trabaja, pueden aparecer los botones **Keep** y **Undo** en el editor. Al terminar, habrá un botón **Keep** o **Undo** para cada cambio. No es necesario seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo de Copilot Chat que pide confirmación para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Captura de pantalla de una sesión completa del modo Agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde está abierto el sitio web y explora la nueva función.
11. Cuando hayas comprobado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

En este ejercicio has explorado cómo utilizar el modo Agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Has aprendido cómo:

- el modo Agente de GitHub Copilot puede implementar funciones en la interfaz de usuario y la capa de datos;
- el modo Agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y realizar cambios coordinados;
- revisar los cambios y las pruebas generados por el modo Agente de Copilot antes de combinar los cambios en el código.

Ahora, [prueba la función con el servidor Playwright MCP][next-lesson] y abre una solicitud de cambios.

### Ejercicio de exploración adicional: implementar la paginación

Al crecer la lista de juegos, será necesario habilitar la paginación. Utiliza lo que has aprendido en este ejercicio para pedir a Copilot que actualice el sitio e implemente la paginación. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones;
- decide cómo quieres implementar la paginación y si quieres permitir que el usuario seleccione el tamaño de página o prefieres fijarlo en el código;
- al crear la indicación, proporciona a Copilot la orientación necesaria para obtener la implementación que buscas;
- quizá tengas que iterar con GitHub Copilot, solicitar cambios y proporcionar contexto. Este es el flujo habitual cuando trabajas con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; para qué sirven y cuándo utilizarlos][choose-mode]
- [Modo Agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
