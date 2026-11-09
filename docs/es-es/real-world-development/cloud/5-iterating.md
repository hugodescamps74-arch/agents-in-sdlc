---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que añada documentación al código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar sobre ella e implemente funciones de accesibilidad, incluidos los modos de alto contraste y claro. Vamos a revisar los cambios de código y, si hace falta, a proporcionar comentarios a Copilot para mejorar el resultado.

### Escenario

Como ya hemos indicado varias veces, los fundamentos del diseño de software, el desarrollo y las operaciones (DevOps) no cambian al incorporar inteligencia artificial generativa. Siempre queremos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con esto en mente, revisaremos las sugerencias de GitHub Copilot para crear la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para que el proceso sea seguro. Entre ellas:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que se ejecute.
- El [acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores del Protocolo de contexto de modelo (Model Context Protocol, MCP).

## Revisar la documentación generada

Empezaremos por explorar la primera solicitud de cambios (PR) que ha generado el agente en la nube de GitHub Copilot: la que añade documentación al código. Para ello, utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la solicitud de cambios, puede que veas un aviso que indica que el cortafuegos ha bloqueado GitHub Copilot. Es **lo esperado**, ya que Copilot tiene un acceso limitado a los recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente en la nube de Copilot][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de cambios.
3. Abre la solicitud de cambios titulada algo parecido a **Añadir la documentación que falta** o con un título más detallado.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud de cambios tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes tomarte un descanso o reflexionar sobre lo que has aprendido.

4. Cuando la solicitud de cambios esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que ahora incluye comentarios de documentación de TypeScript (TSDoc) y otros tipos de documentación. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas pautas proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en cola en la sección de comprobaciones de la solicitud de cambios. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. El proceso puede tardar varios minutos.

## Solicitar cambios a GitHub Copilot

Trabajar con Copilot en una solicitud de cambios no es una interacción unidireccional. Puedes mencionarlo en comentarios de la solicitud, igual que harías con cualquier otra persona del equipo, o en comentarios en línea del código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte un texto prescriptivo para la solicitud. Algunas ideas de cambios que puedes pedirle:

- Añadir encabezados de comentarios al principio de cada archivo de código con una breve descripción de su contenido.
- Añadir comentarios TSDoc a los archivos TypeScript y Astro.
- Crear un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier persona. Puedes utilizar una de las ideas anteriores u otra sugerencia para mejorar la documentación del código.
2. Selecciona **View Session** para ver el trabajo de Copilot. Observa cómo inicia una sesión nueva para realizar las actualizaciones.
3. Selecciona **Back to pull request** para volver a la solicitud de cambios.

    ![Volver a la solicitud de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya terminado los cambios, debería aparecer una nueva confirmación en la solicitud de cambios.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la PR en borrador como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

Volvamos a la solicitud de cambios que Copilot generó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud de cambios cuyo título sea parecido a **Mostrar juegos relacionados en la página de detalles del juego** o tenga un título más detallado.
4. Selecciona la pestaña **Files changed** para revisar el código que ha generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en cola en la sección de comprobaciones de la solicitud de cambios. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. El proceso puede tardar varios minutos.
9. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente la función de juegos relacionados. Ve al codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama que creó Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes trabajar con el agente en la nube de forma iterativa para solicitar cambios. Por ejemplo, puedes pedirle que modifique cuántos juegos relacionados se muestran o que añada encabezados de comentarios y comentarios TSDoc (recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas). Como antes, puedes hacer estas solicitudes con un nuevo comentario en la pestaña **Conversation**. Copilot lo verá e iniciará otra sesión.

## Revisar las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta solicitud de cambios debería incluir tanto el modo de alto contraste que asignaste en el ejercicio 3 como el modo claro que solicitaste desde el centro de control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud de cambios cuyo título sea parecido a **Añadir un modo de alto contraste al sitio web** o tenga un título más detallado.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud de cambios tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código que ha generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz de usuario para cambiar de modo;
   - el uso del almacenamiento local para conservar las preferencias del usuario;
   - los cambios de hojas de estilo en cascada (CSS) o de estilo para los modos de alto contraste y claro;
   - los atributos de Accessible Rich Internet Applications (ARIA, aplicaciones de Internet enriquecidas accesibles), la navegación con el teclado, etc.;
   - el código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en cola en la sección de comprobaciones de la solicitud de cambios. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. El proceso puede tardar varios minutos.
10. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente las funciones de accesibilidad. Ve al codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama que creó Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los selectores de los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que la selección se conserva al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas problemas de accesibilidad o aspectos que se puedan mejorar, puedes mencionar a **@copilot** en un comentario y pedirle que los resuelva, igual que con las solicitudes de cambios anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta útil para delegarle tareas reales de la lista de pendientes mientras tú te ocupas de otras cosas. Para adquirir este hábito, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario para mostrar interés de los patrocinadores en la página de detalles del juego.
- Implementar la paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas del repositorio** para comprobar cómo afectan más adelante al resultado del agente en la nube.
- **Has asignado incidencias al agente en la nube de Copilot** y has observado cómo configura su entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para la accesibilidad, con el que has añadido modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Has revisado las solicitudes de cambios del agente en la nube e iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los demás entornos complementan lo que has practicado aquí:

- **[Entorno de VS Code](../../vscode/)**: explora el modo Agente de Copilot Chat y la integración con MCP directamente desde el entorno de desarrollo integrado (IDE).
- **[Entorno de la interfaz de línea de comandos (CLI)](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de control de calidad (QA) personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
