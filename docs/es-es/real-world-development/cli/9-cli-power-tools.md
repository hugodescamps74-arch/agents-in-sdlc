---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de interfaz de línea de comandos (CLI), GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Estos comandos permiten acceder a funciones avanzadas, consultar información interna y configurar opciones adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Vamos a explorar otros que también resultan útiles.

En esta lección, vas a:

- usar `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usar `/model` para explorar los modelos disponibles.
- aprender cómo `/share` puede exportar o compartir una sesión.
- explorar comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas en cloud agent.

## Escenario

Has completado el flujo principal de la CLI. Ahora explorarás algunas funciones adicionales: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente en [Copilot cloud agent][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas extensas o complejas, puede que alcances el límite de la ventana de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta, y puedes inspeccionar o compactar el contexto con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Introduce:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a introducir `/context` y compara el resultado. Puede que no haya cambiado mucho si la conversación ya era breve.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando la ventana se llena. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` cuando vayas a cambiar a una tarea que no esté relacionada y quieras empezar una conversación nueva.

## Elige el modelo

Los distintos modelos tienen puntos fuertes diferentes y cada desarrollador tiene sus propias preferencias. Copilot CLI te permite mostrar y seleccionar el modelo que quieras usar.

1. Introduce:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir el trabajo y los conocimientos con el equipo ayuda a todos a aprovechar mejor las herramientas de inteligencia artificial. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Introduce `/help` y revisa las opciones de `/share` disponibles en la versión instalada.
2. Si quieres compartir la sesión, elige el destino adecuado, por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviárselo a alguien o publicarlo. Las exportaciones de sesiones pueden incluir indicaciones, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

El flujo principal del taller ha terminado. Copilot CLI también ofrece comandos para tareas más amplias:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabaja de forma asíncrona y puede abrir una solicitud de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o iniciar tareas remotas. Antes de probarlos, inicia una tarea nueva y bien delimitada, y revisa el resultado con tu flujo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller de cloud agent][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y consultar qué ocurre en segundo plano. En esta lección, has:

- usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usado `/model` para explorar los modelos disponibles.
- aprendido cómo `/share` puede exportar o compartir una sesión.
- explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas en cloud agent.

Copilot CLI incluye más comandos de barra y muchas otras posibilidades. Para terminar el recorrido, [repasarás lo aprendido][next-lesson] y verás cuáles son los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
