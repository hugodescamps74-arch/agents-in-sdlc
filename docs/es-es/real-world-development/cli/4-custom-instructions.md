---
title: "Lección 4 - Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando trabajas con inteligencia artificial generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot tenga esas indicaciones disponibles. Los [archivos de instrucciones][instruction-files] no solo describen qué código quieres, sino también cómo debe estructurarse. Ahora que ya has creado el filtrado, explorarás las instrucciones que ha usado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección, vas a:

- explorar cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de determinadas rutas.
- actualizar el archivo de instrucciones para asegurar que se siguen los estándares de programación.
- observar el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys cuenta con directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no limitarse a repetir el código.
- Las funciones exportadas en `db/` y `src/lib/` deben documentar su propósito, sus parámetros y sus valores devueltos mediante TSDoc (TypeScript Documentation)/JSDoc (JavaScript Documentation), incluido el argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices de formato y lint existentes.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información necesaria para realizar las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que pueda entender mejor tu estilo y tus requisitos de programación. Te permiten orientar sus sugerencias y fragmentos de código. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información del proyecto que sea pertinente para la mayoría de las solicitudes.
- Archivos `.github/instructions/*.instructions.md`, que proporcionan directrices para determinados lenguajes, tipos de archivos o tareas.

> [!NOTE]
> Los demás formatos de instrucciones y su compatibilidad varían según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Explóralos antes de hacer cambios para observar el efecto que tienen.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Examina el archivo y fíjate en la breve descripción del proyecto y en las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Hay instrucciones para archivos de Astro, la capa de datos de Drizzle, las pruebas y otros elementos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos extensos de instrucciones en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot puede seguir.

## Actualiza los archivos de instrucciones para que reflejen las directrices del equipo

Los archivos actuales son un buen punto de partida, pero falta una directriz. Modifica el archivo principal `copilot-instructions.md` para asegurarte de que se añaden comentarios TSDoc al nuevo código TypeScript.

1. En `.github/copilot-instructions.md`, localiza la sección titulada **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade este texto como último elemento de la lista:

   ```markdown
   - Todo el código TypeScript nuevo debe incluir comentarios TSDoc con fines de documentación.
   ```

El archivo se guardará automáticamente.

## Aplica las directrices actualizadas

La interfaz de línea de comandos (CLI) de Copilot carga las instrucciones del repositorio cuando se inicia una conversación. Después de guardar las instrucciones, sal de la conversación de filtrado y reanúdala para que Copilot cargue las directrices nuevas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, introduce `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   Acabamos de actualizar las instrucciones y las directrices de código. ¿Puedes actualizar el código que has generado para que las cumpla?
   ```

4. Introduce `/diff` y revisa los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc y confirma que explican el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI obtiene el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a tu funcionalidad. En concreto, has:

- explorado cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de determinadas rutas.
- actualizado el archivo de instrucciones para asegurar que se siguen los estándares de programación.
- observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la skill reutilizable `quality-checks`][next-lesson] para garantizar que se ejecutan las pruebas y el lint de forma coherente.

## Recursos

- [Añade instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
