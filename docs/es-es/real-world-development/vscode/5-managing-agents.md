---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Cuando utilizas el modo Agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como ese trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code te muestra cada llamada a herramientas y cada edición de archivo a medida que se producen. Puedes revisar cada diff en línea, aceptar o rechazar cambios concretos y dirigir la conversación con indicaciones de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás los cambios propuestos en los archivos;
- dirigirás al agente durante la sesión para añadir un selector del modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has utilizado en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha hecho una petición adicional: también quiere un selector del modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En vez de empezar una conversación desde cero, dirigirás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, el diff aparecerá en línea:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertir el cambio y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados por el agente durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios realizados.

## Pedir al agente que añada el modo claro

Ahora que ya está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un selector del modo claro. Como continuarás en la misma sesión de chat, el agente conservará todo el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. Envía esta indicación de seguimiento en el cuadro de Copilot Chat:

    ```plaintext
    ¡Buen trabajo! Ahora añade un selector del modo claro junto al selector de alto contraste. El modo claro debe seguir el mismo patrón: conservarse mediante el almacenamiento local y estar disponible en la misma zona de la interfaz de usuario. El usuario debe poder activar el modo de alto contraste y el modo claro de forma independiente.
    ```

2. El agente retomará el trabajo, revisará los cambios realizados y propondrá los cambios necesarios para añadir el modo claro.
3. Revisa cada cambio nuevo cuando aparezca, igual que hiciste con el modo de alto contraste. Utiliza **Keep** o **Undo** para cada diff.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta refactorizar la interfaz de los selectores de una forma que no te convence—, puedes detenerlo con el botón **Stop** junto al cuadro de chat y enviarle una indicación para aclarar lo que necesitas.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Utiliza indicaciones de seguimiento para pedir ajustes («simplifica las hojas de estilo en cascada (CSS) y utiliza variables CSS en vez de duplicar selectores»), corregir el rumbo («no añadas un tercer selector; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos selectores»).

## Revisar el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa sus diffs. Comprueba que siguen las pautas de accesibilidad del agente personalizado: atributos de Accessible Rich Internet Applications (ARIA, aplicaciones de Internet enriquecidas accesibles), navegación con el teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia mediante el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo Agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición a medida que aparecía, aceptado o rechazado cambios y enviado indicaciones de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has explorado:

- supervisar el trabajo de un agente en tiempo real;
- aceptar y rechazar ediciones individuales de archivos;
- dirigir una sesión en curso con indicaciones de seguimiento.

En la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de cambios y revisarás los cambios generados por el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Utilizar el modo Agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
