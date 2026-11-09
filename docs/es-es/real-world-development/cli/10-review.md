---
title: "Lección 10 - Repaso y siguientes pasos"
description: "Recapitula el flujo de trabajo con Copilot CLI, las dos solicitudes de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado la interfaz de línea de comandos (CLI) de GitHub Copilot a lo largo de un proceso continuo de desarrollo en Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado incluida en el repositorio.
- añadido las valoraciones con estrellas, revisado el resultado en el navegador a través del puerto reenviado y fusionado manualmente tu primera solicitud de cambios (pull request, PR).
- partido de la incidencia de filtrado, definido el enfoque en el modo Plan, implementado la funcionalidad en el modo Autopilot y revisado el resultado en el modo Interactive.
- guiado al agente con instrucciones personalizadas, personalizado la skill existente `quality-checks` y usado esa skill para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Playwright de Model Context Protocol (MCP) y usado el servidor para explorar el filtrado en un navegador real.
- creado y seleccionado un agente personalizado de garantía de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la skill y las pruebas obtenidas en el navegador.
- revisado todos los cambios del filtrado y autorizado Agent Merge para la PR correspondiente.
- explorado comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Lo que has entregado

El taller incluye dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizada:

1. **Valoraciones con estrellas:** mostrar el valor existente de `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y la misma rama. Hemos reunido todo el trabajo en una PR para simplificar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, examinándolo tú mismo en el navegador y pidiendo a Copilot que lo explorara mediante MCP. La skill `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos, y ha incluido los resultados en el nuevo formato. El agente de QA ha reunido esos resultados con la revisión de los requisitos y de la cobertura de las pruebas antes de la PR.

Las pruebas nuevas deben cubrir carencias reales; si una ejecución de QA determina que no hace falta añadir pruebas, también puede ser un resultado correcto. Revisa el código y las pruebas que respaldan las conclusiones antes de autorizar la fusión. Si haces cambios, actualiza las pruebas y demás evidencias afectadas.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller, has actualizado las instrucciones, personalizado una skill, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las skills describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para pensar en un enfoque antes de empezar a programar, **Interactive** para seguir interviniendo en los cambios acotados y **Autopilot** para las tareas bien delimitadas. Elige un modelo más rápido para los cambios rutinarios y otro más potente para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad qué quieres crear, por qué lo necesitas y cómo debe funcionar influye de forma significativa en el resultado.

## Más posibilidades

Ya has cubierto el flujo de trabajo principal. Estas son algunas funciones más de la CLI que puedes explorar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que tienes en mente desde hace años y aún no has creado. Comparte lo que aprendas con el equipo y aprovecha lo que aprendan los demás. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller de Copilot cloud agent][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personaliza Copilot CLI][customize-cli]
- [Gestiona solicitudes de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
