---
title: "Lección 8 - Crea y fusiona la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la skill, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la skill, el perfil de garantía de calidad (QA) y las pruebas están guardados en una misma rama. Es hora de revisarlos juntos y abrir una solicitud de cambios (pull request, PR). Tú mismo fusionaste la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería dividir la funcionalidad, las actualizaciones de instrucciones y de la skill, y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el trabajo del filtrado y del flujo de calidad en una sola conversación y rama, y lo incluirás en esta PR.

En esta lección, vas a:

- aprender qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionar todos los cambios de la funcionalidad y las pruebas de verificación.
- crear la PR del filtrado.
- activar Agent Merge solo después de la revisión y confirmar que se ha fusionado la PR.

## Escenario

Durante el trabajo en la funcionalidad de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar las tareas restantes de la PR, pero mantener bajo el control del desarrollador la autorización para fusionarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios. Cuando lo activas, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando hace falta. Después, activa la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora, seleccionabas tú mismo **Merge pull request**. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y confirma que está listo antes de activarlo.

## Deja que Agent Merge gestione la PR

Con todo el código creado, revisémoslo, creemos la PR y dejemos que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente introduciendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todas las comprobaciones se completen correctamente, realizará la fusión.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully.».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, lee el motivo y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de la solicitud de cambios. En esta lección, has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación.
- creado la PR del filtrado.
- activado Agent Merge solo después de la revisión y confirmado que se ha fusionado la PR.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestiona solicitudes de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
