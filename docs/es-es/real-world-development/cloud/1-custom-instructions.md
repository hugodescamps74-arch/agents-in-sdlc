---
title: "Ejercicio 1: Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es clave cuando se trabaja con inteligencia artificial generativa. Si una tarea debe realizarse de una manera determinada —o Copilot necesita conocer información de contexto—, conviene asegurarse de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo qué quieres que haga, sino también cómo quieres que lo haga.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las pautas de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones asociados a rutas;
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y de la interfaz de línea de comandos (CLI), aquí no ejecutarás una indicación antes y después para comparar resultados. El agente en la nube de Copilot trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar su efecto en tiempo real. Más adelante, al revisar las solicitudes de cambios que genere, verás la influencia del archivo de instrucciones.

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

- `.github/copilot-instructions.md` es un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes relacionadas con el repositorio. Debe contener información de contexto aplicable a la mayoría de las solicitudes de chat o CLI que envíes a Copilot, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras pautas generales.
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

## Añadir un nuevo estándar para el repositorio

Este será el único cambio que realizarás en este ejercicio: añadirás una regla general para que la documentación se incluya en el código mediante comentarios TSDoc y encabezados de comentarios a nivel de archivo. El agente en la nube la tendrá en cuenta al trabajar en las incidencias que le asignes más adelante.

Antes de editar, crea una rama de trabajo. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero también confirmarás los cambios con una confirmación limpia.

1. En el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## Confirmar, publicar y combinar la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. Cuando le asignes una incidencia a Copilot en el siguiente ejercicio, Copilot creará una rama a partir de `main`. Por eso, los cambios de instrucciones deben llegar a `main` para que el agente en la nube pueda utilizarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de cambios desde `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube podrá leer estas instrucciones al trabajar en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios directamente en `main`. El paso de la rama permite practicar un flujo de trabajo habitual en proyectos reales.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza el contexto de los archivos de instrucciones de este proyecto y has añadido un nuevo estándar general a `.github/copilot-instructions.md`. Durante el resto de este entorno, verás cómo se aplica ese estándar en las solicitudes de cambios que genere el agente en la nube.

A continuación, [asigna tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
