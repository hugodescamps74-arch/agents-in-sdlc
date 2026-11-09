---
title: "Lección 3 - Modos del agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios más amplios necesitan un proceso más sólido. Por suerte, la interfaz de línea de comandos (CLI) de GitHub Copilot está diseñada para trabajar con los flujos existentes de una organización y ayudarnos a crear lo que hace falta de la forma adecuada. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, finalmente, la fusionarás correctamente en el proyecto.

> [!NOTE]
> Seguirás trabajando en la misma conversación y rama durante todo el flujo de desarrollo de la funcionalidad. Lo habitual sería usar ramas o solicitudes de cambios (pull requests, PR) distintas para los diferentes tipos de archivo, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección, vas a:

- iniciar una conversación nueva de Copilot a partir de una incidencia de GitHub.
- definir los requisitos en el modo Plan.
- implementar la nueva funcionalidad con el modo Autopilot.
- revisar el código.
- validar manualmente la funcionalidad en el navegador a través del puerto reenviado.

A medida que avances en el desarrollo de esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la skill existente `quality-checks`, añadirás validación con Model Context Protocol (MCP), crearás un agente de garantía de calidad (QA) y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys no deja de crecer y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar detalles como la combinación de categorías. Usarás el modo Plan para tomar esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con inteligencia artificial a tu proceso de desarrollo no cambia los fundamentos. De hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo pendiente.
2. Crear un plan de lo que se debe desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una PR.
7. Cuando se haya revisado el código y se haya completado correctamente el proceso de integración continua, fusionar el código.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos siguen una variante de los pasos anteriores.

Si sigues este proceso estándar, te aseguras de que el código generado por la inteligencia artificial cumpla los requisitos establecidos y pase por el mismo proceso de revisión que el código escrito a mano.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas y repite el proceso hasta que considera que la tarea está terminada.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están preparados.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha fusionado la PR de las valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia a la indicación y volver a la pestaña **Session**.

Observa que la indicación empieza ahora por `#7` (o por un número parecido). El carácter `#` permite añadir al contexto una incidencia o una solicitud de cambios de GitHub.

## Planifica la funcionalidad de filtrado

La planificación te permite definir el enfoque para implementar una funcionalidad o realizar una tarea antes de delegársela a Copilot. Para cualquier trabajo complejo, conviene dedicar un tiempo a planificar. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador situado debajo de la indicación muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, introduce esta indicación:

   ```plaintext
   Crea un plan para implementar esta funcionalidad.
   ```

Copilot empezará a elaborar el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Durante el proceso, es posible que Copilot te pregunte cómo deberían funcionar los filtros. Respóndele según tus preferencias. No hay respuestas incorrectas.
4. Cuando el plan esté completo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para desplegarlo.
5. Desplázate hacia arriba y hacia abajo para revisar el plan.
6. Pide a Copilot que revise cualquier parte del plan que no coincida con tus decisiones.

## Aprueba Autopilot

Ya tienes el plan escrito y revisado. Ahora toca implementarlo y dejar que Copilot se encargue del trabajo con Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción de nombre parecido que aparezca en la versión instalada.
2. Confirma que el indicador situado debajo de la indicación muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones.

## Revisa y verifica la implementación

Una vez generado el código, debes revisarlo antes de fusionarlo, igual que harías con cualquier otro código. Revisaremos el código y ejecutaremos el sitio para asegurarnos de que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador de modo ya no muestra **Plan** ni **Autopilot**.
2. Introduce `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los errores que haya.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar en el navegador la funcionalidad de filtrado. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones que acordaste en el plan.
4. Confirma que las opciones de restablecimiento y los resultados vacíos se comportan según la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has usado diferentes modos de conversación para crear y revisar una funcionalidad. En esta lección, has:

- iniciado una conversación nueva de Copilot a partir de una incidencia de GitHub.
- definido los requisitos en el modo Plan.
- implementado la nueva funcionalidad con el modo Autopilot.
- revisado el código.
- validado manualmente la funcionalidad en el navegador a través del puerto reenviado.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que sigue las prácticas documentadas [mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
