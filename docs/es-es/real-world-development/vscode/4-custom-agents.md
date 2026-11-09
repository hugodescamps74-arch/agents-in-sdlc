---
title: "Ejercicio 4: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de inteligencia artificial especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionarles instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio para que realicen determinados tipos de trabajo con mayor eficacia. Los equipos pueden convertir sus conocimientos en agentes reutilizables: un agente de accesibilidad que aplique las Pautas de Accesibilidad para el Contenido Web (WCAG, por sus siglas en inglés), uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter en YAML (YAML Ain't Markup Language) que incluye como mínimo `name` y `description`, seguido de una indicación en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre los agentes personalizados y las habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común. Ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar operaciones. La forma más sencilla de distinguirlos es esta: un **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes mientras trabajan. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio según las pautas de accesibilidad. Para ello, podría recurrir a habilidades, como una para el flujo de trabajo de solicitudes de cambios o una para ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Como con cualquier otra función de inteligencia artificial, prueba distintos enfoques e itera para encontrar los adecuados para tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes de los usuarios indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, queremos implementar esta funcionalidad lo antes posible. Para generar la funcionalidad, utilizarás un agente personalizado.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente;
- utilizarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve al codespace y abre un terminal para crear una rama nueva a partir de `main`. Así mantendrás separado el trabajo de accesibilidad y la solicitud de cambios (PR) de filtrado del ejercicio 3.

1. Ejecuta estos comandos para cambiar a `main`, actualizarla y crear la rama de accesibilidad:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abre `.github/agents/accessibility.md`.
3. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

4. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las prácticas recomendadas de accesibilidad;
   - ejemplos de código en lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript;
   - errores y problemas habituales.

## Utilizar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el menú desplegable de agentes, en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para limitar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** si todavía no está seleccionado.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú desplegable de agentes situado en la parte inferior de la vista Chat. Muestra el agente activo; de forma predeterminada, es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía esta indicación al agente de accesibilidad:

    ```plaintext
    Añade un modo de alto contraste al sitio. Debe haber una opción para activar y desactivar el alto contraste. La preferencia debe conservarse al volver a cargar la página mediante el almacenamiento local del navegador.
    ```

5. Copilot Chat empezará a trabajar: explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en línea en el chat con la ruta del archivo y un diff que podrás revisar.
6. A medida que se apliquen los cambios, se actualizará el indicador **Files changed** para mostrar los archivos que el agente ha modificado durante la sesión.

> [!NOTE]
> Es probable que el proceso tarde unos minutos. Copilot está realizando cambios reales en el repositorio: mientras trabaja, puede editar componentes Astro, archivos CSS y pruebas relacionadas.

Revisarás y dirigirás este trabajo en curso en el siguiente ejercicio.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Los agentes personalizados permiten convertir los conocimientos y estándares del equipo en agentes reutilizables que orientan a Copilot para que realice determinados tipos de trabajo con mayor eficacia.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo utilizar un agente personalizado en el modo Agente de Copilot Chat.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisarás los cambios mientras se producen y añadirás un selector del modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el entorno de desarrollo integrado (IDE)][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la función con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
