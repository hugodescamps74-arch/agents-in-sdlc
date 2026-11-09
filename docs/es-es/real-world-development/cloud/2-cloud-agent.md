---
title: "Ejercicio 2: Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en la lista de tareas pendientes porque no ha habido tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de cambios (PR) en borrador para que la revise un desarrollador. Así, puede encargarse de tareas tediosas, acelerar el desarrollo y permitir que los desarrolladores se centren en asuntos de mayor alcance.

Con el agente en la nube de Copilot explorarás cómo:

- personalizar el entorno en el que se genera el código;
- garantizar que las operaciones se realicen de forma segura;
- formular incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco satisfactorio: comprobarás que no hay ninguna. Como primer paso, quieren añadir comentarios de documentación de TypeScript (TSDoc) a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere que la página de detalles de cada juego muestre juegos relacionados —otros títulos de la misma categoría— para que la gente pueda seguir explorando. Por ahora no necesita una implementación pulida; solo algo que pueda utilizar para hacer pruebas de aceptación de la experiencia de usuario. Este trabajo está bloqueado, pero hay otras incidencias con mayor prioridad.

Estas son tareas que suelen perder prioridad rápidamente y que conviene asignar al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, puede revisar el trabajo y comprobar que el resultado es el esperado.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que cuando trabajas con otra persona, puedes asignarle trabajo de varias maneras, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot creará una solicitud de cambios en borrador para registrar el progreso, configurará un entorno y empezará a trabajar. Puedes consultar la sesión mientras sigue activa o después de que termine. Cuando la solución propuesta esté lista para revisarse, Copilot te mencionará en la solicitud de cambios.

## La importancia de definir bien el alcance de las instrucciones

GitHub Copilot no hace magia: no puedes dejar de pensar. Incluso las operaciones que parecen sencillas pueden volverse complejas al examinarlas con detalle, así que [define cuidadosamente el alcance de las tareas que asignas al agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación basado en IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar inteligencia artificial generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube, revisa los archivos de instrucciones que ya incluye este repositorio, sin modificarlos, para que después puedas reconocer su efecto.

Abre los siguientes archivos en la interfaz web de GitHub para tu repositorio o en un codespace que ya tengas en ejecución:

1. Abre `.github/copilot-instructions.md` y revisa la sección **Code standards**, especialmente las expectativas sobre las convenciones de TypeScript y los comentarios TSDoc.
2. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `El código carece de documentación` en la sección siguiente, observa si la solicitud de cambios resultante incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas pautas proceden de los archivos de instrucciones. Volverás a prestarles atención al revisar las solicitudes de cambios en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube de Copilot

Normalmente, para crear código hay que utilizar un entorno específico y ejecutar algunos scripts de configuración para que todo esté en buen estado. Lo mismo se aplica al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Así puede acceder a las herramientas y dependencias de desarrollo necesarias. Este flujo ya está configurado para facilitar el laboratorio y ofrecerte la oportunidad de conocer esta función. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en esos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y de la aplicación: Node.js (toda la aplicación utiliza ahora Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en SQLite local para que la compilación y las pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

Como cualquier otro archivo de flujo de trabajo de GitHub, contiene algunos elementos clave:

- Incluye un único trabajo llamado `copilot-setup-steps`. Este trabajo se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de cambios.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Esto resulta útil para comprobar que el flujo se ejecuta correctamente, sin esperar a que Copilot lo inicie.

## Añadir documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica suele quedar sin resolver, lo que reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot destaca en la creación de documentación, por lo que este es un buen ejemplo de incidencia para asignar al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior revisarás el resultado.

1. Abre una pestaña nueva del navegador y ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación de incidencias.
4. Selecciona **Blank issue** para crear la incidencia.
5. Escribe `El código carece de documentación` en el campo **Title**.
6. Escribe lo siguiente en el campo **Description**:

    ```plaintext
    Nuestra organización exige que las funciones y los métodos incluyan comentarios TSDoc cuando sean útiles. Por desgracia, las actualizaciones recientes no han seguido este estándar. Tenemos que actualizar el código existente para incluir comentarios de documentación donde ayuden a aclarar el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de cambios (PR) que se acaba de generar. Su título será parecido a `[WIP]: El código carece de documentación`. Si todavía no aparece ninguna solicitud, espera uno o dos minutos y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> Es posible que la lista de tareas de Copilot tarde varios minutos en aparecer en la solicitud de cambios. Copilot está creando su entorno (ejecutando el flujo de trabajo que se ha descrito antes), analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la solicitud de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene a pensar y decidir el plan adecuado, y crea código.

El proceso probablemente tardará varios minutos. Uno de los objetivos principales del agente en la nube de Copilot es realizar tareas de forma asíncrona para que puedas centrarte en otras cosas. Aprovecharemos precisamente esa función: asignaremos otra tarea al agente en la nube y, mientras trabaja, nos ocuparemos de escribir código para añadir funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube de GitHub Copilot es que permite repartir el trabajo para que puedas centrarte en una tarea mientras él se ocupa de otra. Aunque añadir una sección de juegos relacionados para el equipo de diseño no necesariamente llevaría mucho tiempo, ese tiempo podría dedicarse a otras tareas. Vamos a asignársela al agente en la nube de Copilot.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación de incidencias.
4. Selecciona **Blank issue** para utilizar la plantilla en blanco.
5. Escribe `Mostrar juegos relacionados en la página de detalles del juego` en el campo **Title**.
6. Escribe lo siguiente en el campo **Description**:

    ```markdown
    Queremos ayudar a la gente a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y realizar pruebas de aceptación. Estos son los requisitos:

   - Añadir una función auxiliar de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego determinado, sin incluir ese juego
   - Mostrar una sección «Juegos relacionados» en la página de detalles del juego que utilice la función auxiliar
   - Gestionar el caso en que un juego no tenga juegos relacionados
   - Crear pruebas unitarias para la nueva función auxiliar
   - Comprobar que todas las pruebas se superan antes de crear la solicitud de cambios
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, deberían aparecer unos ojos 👀 en el primer comentario de la incidencia para indicar que Copilot ya está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube de Copilot funciona de forma parecida a un ingeniero de software. No necesitas supervisarlo continuamente: puedes revisar el resultado cuando haya terminado. Ahora, vamos a crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje en ellas de forma asíncrona y se ocupe de la deuda técnica, cree funcionalidades o ayude a migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno en el que se genera el código;
- garantizar que las operaciones se realicen de forma segura;
- formular incidencias con un alcance claro;
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes centrarte en crear y utilizar agentes personalizados. El agente en la nube de Copilot también puede utilizar servidores del Protocolo de contexto de modelo (Model Context Protocol, MCP) y acceder a instrucciones personalizadas, que ya has explorado en ejercicios anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
