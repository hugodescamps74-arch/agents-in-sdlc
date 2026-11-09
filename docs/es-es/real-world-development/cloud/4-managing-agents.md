---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente en la nube de Copilot que se ocupe de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, entre ellas, la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes consultar todas las tareas de los agentes que tienen solicitudes de cambios abiertas. También puedes explorar las operaciones que han realizado e incluso dirigir una sesión que esté en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- dirigirás una sesión en curso para solicitar funcionalidades adicionales.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba trabajando en actualizar el estilo del sitio y añadir la opción de cambiar de modo, parecía razonable incluir esta funcionalidad. Dirigirás el trabajo del agente para que añada también el modo claro.

## Revisar las tareas del agente en la nube de Copilot

Consulta el estado actual de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel de la izquierda. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - actualizar la documentación del código;
    - generar interfaces de programación de aplicaciones (API) para modificar juegos;
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las tareas que ha realizado Copilot, por ejemplo:
    - obtener el código del repositorio;
    - crear el entorno de trabajo de Copilot;
    - configurar servidores del Protocolo de contexto de modelo (MCP);
    - realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos de la lista variarán según el estado del trabajo de Copilot y el enfoque que haya adoptado.

4. Observa también el panel de solicitudes de cambios que aparece a la derecha. Desde ahí puedes consultar la solicitud de cambios y los archivos modificados para supervisar el trabajo.

## Dirigir el agente en la nube

Ahora que has visto las tareas activas, pídele a Copilot que añada un selector del modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión correspondiente a la incorporación de un modo de alto contraste. El título exacto dependerá del nombre que le haya dado Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe esta indicación:

    ```plaintext
    Mientras trabajamos en un modo de alto contraste, añadamos también un modo claro. También debe haber un selector para este modo, de manera que los usuarios puedan elegir el modo de visualización que prefieran.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la indicación.
5. Observa cómo Copilot confirma que la ha recibido y la incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Copilot se pondrá manos a la obra con la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

Como antes, el proceso tardará varios minutos. Puedes aprovechar el tiempo para reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control desde el que se supervisan y dirigen las tareas del agente en la nube de GitHub Copilot. Puedes utilizarla para seguir las tareas activas y completadas, revisar el trabajo en curso e incluso redirigir tareas para cambiar su alcance o proporcionar instrucciones adicionales.

Has explorado estos conceptos:

- explorar el centro de control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión en curso para solicitar funcionalidades adicionales.

Mientras Copilot completa las funcionalidades de accesibilidad, puedes [iterar sobre las solicitudes de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
