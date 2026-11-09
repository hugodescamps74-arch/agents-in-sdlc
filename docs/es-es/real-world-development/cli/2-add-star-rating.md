---
title: "Lección 2 - Añade valoraciones con estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revisarlo en el navegador mediante el reenvío de puertos y fusionarlo como tu primera solicitud de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado la interfaz de línea de comandos (CLI) de GitHub Copilot y has probado una conversación, es hora de hacer tu primer cambio en el proyecto. Será algo pequeño: los juegos ya incluyen una valoración con estrellas en sus datos, pero aún no se muestra en las tarjetas de la página de inicio. Pedirás al agente que la muestre, revisarás el cambio y lo fusionarás como tu primera solicitud de cambios.

En esta lección, vas a:

- iniciar una conversación específica de Copilot en una rama de funcionalidad.
- pedir al agente que haga un pequeño cambio en el proyecto.
- revisar el cambio con `/diff`.
- ejecutar la aplicación para confirmar el cambio en el navegador a través del puerto reenviado.
- abrir y fusionar tu primera solicitud de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles del juego. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre la valoración existente en cada tarjeta: un cambio pequeño y autónomo, perfecto para tu primera sesión.

## Estructura de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación de Copilot, una conversación normal de la CLI usa el repositorio y la rama de Git que están actualmente seleccionados en el terminal, en lugar de crear un árbol de trabajo específico. Las conversaciones guardadas te permiten retomar el mismo intercambio más adelante, mientras que los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás tres elementos: tus indicaciones y las respuestas del agente, la actividad de las herramientas que usa el agente para explorar y editar archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

Inicia una conversación nueva para empezar a implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot aún no está abierto, inicia la CLI con este comando:

   ```bash
   copilot --yolo
   ```

4. Asegúrate de iniciar una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa esta indicación para solicitar el cambio:

   ```plaintext
   Muestra el valor de starRating de cada juego sobre 5 en las tarjetas de la página de listado. Si la valoración es null, muestra «Sin valoración todavía». Mantén el diseño de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explorará el proyecto, localizará los archivos que se usan para mostrar los detalles del juego y creará el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisa las diferencias

Antes de fusionar cualquier cambio generado por inteligencia artificial, por pequeño que sea, debes revisarlo. Vamos a inspeccionar los cambios directamente en Copilot CLI.

1. Introduce `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta del juego muestra la valoración numérica cuando existe y «Sin valoración todavía» cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que haya ejecutado Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Copilot, como todas las herramientas de inteligencia artificial generativa, es probabilístico, no determinista, por lo que el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación concreta.

## Comprueba los cambios

No basta con leer el código y dar por hecho que funciona. Pidamos a Copilot que inicie el sitio para que podamos examinar la interfaz de usuario (UI) actualizada en el navegador que se abre a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda inspeccionar en el navegador el cambio de las valoraciones con estrellas. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de los juegos muestran sus valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abre y fusiona tu primera solicitud de cambios

Ya has creado la funcionalidad. Ahora toca crear una solicitud de cambios (pull request, PR) para integrar el nuevo código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Confirma los cambios revisados de las valoraciones con estrellas con un mensaje de confirmación adecuado.
   ```

2. Introduce `/pr create`. Copilot CLI enviará la confirmación existente al crear la PR y mostrará la URL de la PR.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y, después, confirma la fusión.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto, has:

- iniciado una conversación específica de Copilot en una rama de funcionalidad.
- indicado al agente que hiciera un pequeño cambio en las tarjetas de los juegos.
- revisado el cambio con `/diff`.
- ejecutado la aplicación para confirmar la valoración con estrellas en el navegador a través del puerto reenviado.
- abierto y fusionado tu primera solicitud de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
