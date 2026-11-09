---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo Agente →][next-lesson]

El contexto es clave cuando se trabaja con inteligencia artificial generativa. Si una tarea debe realizarse de una manera determinada —o Copilot necesita conocer información de contexto—, conviene asegurarse de que pueda acceder a ella. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan pautas para generar código.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las pautas de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones asociados a rutas;
- generarás la primera parte de la capa de datos para el filtrado (una función auxiliar para consultar editoriales) con las instrucciones actuales;
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`;
- volverás a enviar la misma indicación y observarás cómo el código generado incorpora el nuevo estándar;
- confirmarás los cambios de instrucciones y la función auxiliar de filtrado en `main` para que Copilot pueda utilizar las pautas actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que hayas definido. Copilot no es determinista. El objetivo del ejercicio es observar la tendencia del comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buena empresa de desarrollo, Tailspin Toys tiene pautas y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación de TypeScript (TSDoc).
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función te permite orientar a Copilot para obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas e incluso los tipos de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos y tareas específicas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes relacionadas con el repositorio. Debe contener información de contexto aplicable a la mayoría de las solicitudes de chat o de la interfaz de línea de comandos (CLI) que envíes a Copilot, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras pautas generales.
- Puedes crear archivos `.github/instructions/*.instructions.md` para tareas o tipos de archivo específicos. Por ejemplo, sirven para definir pautas para TypeScript o Astro, o para crear un componente de interfaz de usuario o un conjunto de pruebas unitarias.

> [!NOTE]
> En el entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las sugerencias de código ni las sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` (con metadatos `applyTo` en el frontmatter) al generar código.
>
> Copilot también [admite archivos de instrucciones de otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Describir por completo cómo crear archivos de instrucciones queda fuera del alcance de este taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `.github/copilot-instructions.md` centrado en las pautas del proyecto, como una descripción de lo que se está creando, la estructura del proyecto y los estándares generales de programación.
- Utiliza archivos `*.instructions.md` para tareas o tipos de archivo concretos, como las pruebas unitarias, los componentes Astro o la capa de datos.
- Escribe en lenguaje natural. Expón las pautas con claridad e incluye ejemplos de cómo debería ser el código y de cómo no debería serlo.

No hay una única manera correcta de crear archivos de instrucciones, igual que no hay una única manera de utilizar la inteligencia artificial. La experimentación te ayudará a encontrar el enfoque más adecuado para tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería contar con un conjunto sólido de archivos de instrucciones. Al explorar los de este proyecto, quizá encuentres archivos para distintos tipos de tareas, como las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta: por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección del entorno que estés utilizando lo indicará cuando corresponda.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las pautas anidadas para **GitHub Actions Workflows**. Estas pautas se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Encontrarás instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` del principio: define un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, uno que cumpla `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de los archivos.

## Crear una rama para los cambios

Siguiendo las prácticas recomendadas, crea una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utilizar Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

Para observar el efecto de las instrucciones personalizadas, empieza enviando una indicación con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar la misma indicación para comparar el resultado.

1. Cierra las pestañas abiertas del editor para que Copilot utilice solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe guardarse la función auxiliar.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía esta indicación:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios a los archivos.
   ```

5. Copilot explora el proyecto y aplica actualizaciones al código, a menudo tanto al archivo de la función auxiliar como a sus pruebas.
6. Observa que la función propuesta tiene tipos, recibe un cliente `db` como primer argumento y devuelve una matriz de editoriales con tipos. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentarios a nivel de archivo.

> [!CAUTION]
> Como Copilot funciona de forma probabilística, es posible que añada comentarios de documentación aunque no se lo hayas indicado. No pasa nada: el objetivo es observar cómo mejora la coherencia tras actualizar las instrucciones.

## Añadir un nuevo estándar para el repositorio

Como se ha indicado, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade el estándar de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa cómo documenta los estándares de programación del proyecto, pero todavía no incluye ninguna regla para la documentación en el código; por eso la función generada no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo debajo de los estándares existentes. Así indicarás a Copilot que añada encabezados de comentarios a los archivos y comentarios TSDoc:

   ```markdown
   - Todas las funciones exportadas deben incluir un comentario TSDoc que describa su propósito, sus parámetros y su valor de retorno.
   - Antes de las importaciones o de cualquier otro código, añade un bloque de comentarios que explique la finalidad del archivo.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el repositorio (`.github/copilot-instructions.md`) para ofrecer pautas generales o archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Volver a enviar la indicación y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contenido anterior.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía la **misma indicación** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios a los archivos.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Funciones auxiliares de acceso a datos de editoriales para la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar y lo aplique al código real sobre el que trabajarás en el siguiente ejercicio.

## Confirmar las instrucciones y publicar la rama

Los archivos de instrucciones se gestionan mediante el control de código fuente, igual que cualquier otro recurso del repositorio. Confirma los cambios y publica la rama.

1. Abre una ventana nueva del terminal pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. En el terminal, comprueba que estén presentes los cambios en las instrucciones y la función auxiliar ejecutando:

   ```bash
   git status
   ```

3. En el terminal, prepara y confirma la actualización de instrucciones y todos los archivos que Copilot haya modificado para crear la base de la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Desde el terminal, publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de cambios

Con la rama publicada, crea una solicitud de cambios y relaciónala con la incidencia sobre los estándares de documentación de la lista de tareas pendientes. En vez de hacerlo manualmente, puedes pedir a Copilot que busque la incidencia, cree la solicitud de cambios para resolverla y la combine.

> [!NOTE]
> En este ejercicio Copilot actuará por primera vez en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub con tu espacio de trabajo, así que no tienes que configurar nada. La primera vez que Copilot utilice una herramienta, VS Code puede pedirte que inicies sesión en GitHub. Sigue las indicaciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** esté seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y que cree una solicitud de cambios desde la rama actual. Indícale que describa tanto la actualización de las instrucciones como la nueva base de la función auxiliar de editoriales:

   ```
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una solicitud de cambios desde la rama actual, indica que resuelve esa incidencia e incluye que también hemos añadido la base de la función auxiliar de editoriales para el próximo trabajo de filtrado.
   ```

4. Copilot empezará a buscar la incidencia y a crear la solicitud de cambios.
5. Cuando Copilot te pida permiso para realizar acciones en GitHub en tu nombre, revisa el comando y selecciona **Allow** si procede.
6. Cuando se haya creado la solicitud de cambios, pídele a Copilot que la combine y que vuelva a `main`:

   ```
   Combina la solicitud de cambios en main. Después, vuelve localmente a main y descarga los cambios más recientes para tener el repositorio actualizado.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell en tu nombre, revisa el comando y selecciona **Allow** si procede.

¡Has creado y combinado una solicitud de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza el contexto de los archivos de instrucciones y, después, has utilizado Copilot Chat en VS Code para:

- enviar una indicación de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`;
- volver a enviar la misma indicación y observar cómo el código propuesto incorpora el nuevo estándar;
- confirmar los cambios de instrucciones y la base de la función auxiliar en `main` para que el siguiente ejercicio pueda utilizarlos.

A continuación, pondrás en práctica esas instrucciones en el [modo Agente][next-lesson] mientras Copilot añade una nueva función a varios archivos del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
