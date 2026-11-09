---
title: "Lección 7 - Crea y usa un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de la incidencia, la skill quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la skill `quality-checks` para ejecutar comprobaciones automatizadas y el servidor de Playwright, basado en Model Context Protocol (MCP), para observar la experiencia de filtrado en un navegador. Ahora combinarás esas capacidades en un agente personalizado con un proceso de garantía de calidad (QA) bien definido.

En esta lección, vas a:

- explorar cómo trabaja un agente personalizado con instrucciones, skills y herramientas MCP.
- crear e inspeccionar un perfil reutilizable de QA.
- seleccionar el agente de QA y revisar sus conclusiones según la incidencia de filtrado.

## Escenario

Tailspin Toys quiere que, antes de abrir una solicitud de cambios (pull request, PR), se revisen de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot que se define en un perfil de Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller, definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en la interfaz de línea de comandos (CLI) de Copilot.

Las personalizaciones que has usado cumplen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La skill `quality-checks` reúne comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de QA indica a Copilot cómo usar esas capacidades para evaluar los requisitos e informar de sus conclusiones. No sustituye a las demás personalizaciones ni requiere otra conversación.

## Crea el perfil de QA

Antes de abrir la PR de la funcionalidad, pedirás a Copilot que cree un perfil de QA reutilizable. El perfil definirá tanto las comprobaciones como los límites que debe respetar el agente.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las funcionalidades según sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la skill quality-checks, usar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Indícale que informe del estado de cada requisito como superado, no superado o bloqueado, con pruebas que respalden el resultado. Debe pedir permiso antes de modificar el código de implementación y no debe confirmar cambios ni abrir solicitudes de cambios.

   Por ahora, crea solo el perfil para que pueda revisarlo.
   ```

## Inspecciona el perfil

Antes de usar el agente nuevo, revisa su perfil para confirmar que Copilot ha capturado el flujo de QA y los límites previstos. Así evitarás que el agente esté incompleto o tenga demasiado margen para modificar la funcionalidad cuando solo quieres verificarla.

1. Introduce `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero incluirlo proporciona al agente un nombre claro en pantalla.
3. Lee las instrucciones del perfil y confirma que QA empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la skill `quality-checks` y usa Playwright MCP.
4. Confirma que QA aporta pruebas que respaldan sus conclusiones, pide permiso antes de modificar el código de implementación y no confirma cambios ni abre solicitudes de cambios.
5. Si al perfil generado le falta alguna de estas responsabilidades o límites, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta QA según la incidencia

Copilot CLI carga los agentes del proyecto cuando se inicia una conversación. Después de crear el perfil, reanuda la misma conversación de filtrado y selecciona QA para que pueda usar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con esta indicación:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que no aparezca en la lista de agentes disponibles. Aun así, el comando anterior lo activará.

2. Usa esta indicación para pedir al agente de QA que revise la funcionalidad:

   ```plaintext
   Revisa la funcionalidad de filtrado según la incidencia y las decisiones de nuestro plan. ¿Está lista para una PR?
   ```

3. El agente de QA empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado su trabajo. En esta lección, has:

- explorado cómo trabaja un agente personalizado con instrucciones, skills y herramientas MCP.
- creado e inspeccionado un perfil reutilizable de garantía de calidad (QA).
- seleccionado el agente de QA y revisado sus conclusiones según la incidencia de filtrado.

Ya tienes la implementación, la actualización de la skill, el perfil de QA, las pruebas y el informe de verificación listos para revisión. A continuación, [reunirás todo en una PR de la funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crea agentes personalizados para la interfaz de línea de comandos (CLI) de Copilot][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
