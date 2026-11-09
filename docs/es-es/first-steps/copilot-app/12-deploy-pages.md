---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar la solicitud de cambios y comprobar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz en una URL pública con GitHub Pages para que cualquiera pueda probarlo. Copilot puede preparar el flujo de trabajo y guiarte durante la publicación, pero debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure una publicación en GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de cambios antes de fusionarla.
- seguirás la publicación y comprobarás el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero quienes lo visitan aún necesitan una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages lo hace accesible mediante una URL pública, sin añadir un servidor ni dependencias de runtime.

## Pide a Copilot que publique el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este mensaje:

```plaintext
Habilita GitHub Pages en este repositorio. Crea un flujo de trabajo de GitHub Actions que compile el sitio web si es necesario y lo publique en GitHub Pages. Si todavía no hay un README, añade uno que describa la aplicación y explique cómo ejecutarla localmente. Crea una solicitud de cambios con estos cambios y fusiónala cuando pasen las comprobaciones requeridas. Supervisa la publicación hasta que termine correctamente y, después, indica la URL final del sitio. Si no tienes permiso para habilitar Pages o fusionar la solicitud, dime exactamente qué debo aprobar o hacer; no indiques que ha terminado correctamente hasta que el sitio esté publicado.
```

![Diagrama de tres etapas: una solicitud de cambios de space-quiz fusionada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz disponible en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este mensaje pide al agente que cree y fusione una solicitud de cambios y modifique la configuración de Pages del repositorio. Revisa la diferencia, los permisos de publicación y las solicitudes de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y comprueba la publicación

1. Confirma que el archivo README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la solicitud de cambios, incluidos los pasos de compilación y publicación del flujo de trabajo.
3. Fusiona los cambios solo después de que pasen las comprobaciones requeridas de la solicitud de cambios.
4. Sigue la ejecución de publicación de GitHub Actions hasta que termine correctamente.
5. Abre la URL indicada y confirma que Space Quiz se carga.

Si el agente no puede habilitar Pages o fusionar la solicitud de cambios, sigue las instrucciones concretas que te dé. No des la tarea por terminada hasta que la publicación se complete y el sitio esté disponible.

## Resumen y pasos siguientes

Space Quiz ya está disponible como sitio publicado y has comprobado la publicación en lugar de confiar en el informe del agente. Continúa con la [lección 13: Repaso y próximos pasos][next-lesson].

[next-lesson]: ../13-review/
