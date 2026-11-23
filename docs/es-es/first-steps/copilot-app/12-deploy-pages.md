---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz en GitHub Pages para que todo el mundo pueda acceder a él. Copilot puede preparar el flujo de trabajo y guiarte durante la publicación, pero debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure la publicación en GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de fusionarla.
- seguirás el proceso de publicación y comprobarás el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pedir a Copilot que prepare la publicación

> [!WARNING]
> Crear una solicitud de incorporación de cambios puede confirmar y subir código. Revisa la configuración de aprobación y el repositorio de destino antes de enviar este prompt. Si se ha seleccionado **Approve all**, no esperes otro mensaje de confirmación.

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Prepara la publicación de este repositorio en GitHub Pages. Crea un flujo de trabajo de GitHub Actions que compile el sitio web si es necesario y lo publique en GitHub Pages cuando los cambios lleguen a la rama predeterminada. Si todavía no hay un archivo README, añade uno que describa la aplicación y explique cómo ejecutarla en local. Crea una solicitud de incorporación de cambios con estos cambios e indica su enlace y cualquier configuración o permiso necesario de Pages. No fusiones la solicitud, no habilites la fusión automática, no cambies la configuración de Pages ni publiques el sitio. Detente y espera a que revise los cambios.
```

## Revisar el flujo de trabajo y la solicitud de incorporación de cambios

1. Confirma que el archivo README solo se ha añadido si el repositorio todavía no tenía uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos para compilar y publicar el flujo de trabajo, sus desencadenadores y sus permisos.
3. Confirma que el flujo de trabajo publica desde la rama predeterminada, no desde una solicitud de incorporación de cambios que aún no se haya fusionado.
4. Pide las correcciones que hagan falta y revisa el diff actualizado. Espera a que se superen las comprobaciones obligatorias de la solicitud antes de autorizar la fusión.

## Autorizar la fusión y la publicación

> [!WARNING]
> El siguiente prompt autoriza al agente a fusionar la solicitud revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíalo solo después de terminar la revisión y confirmar el repositorio y la solicitud de incorporación de cambios. Según la configuración de aprobación, puede que estas acciones no requieran otra confirmación.

Sustituye `<PR_NUMBER>` por el número de la solicitud que has revisado y envía este prompt independiente:

```plaintext
He revisado la solicitud de incorporación de cambios <PR_NUMBER> de este repositorio y autorizo su fusión y publicación. Habilita GitHub Pages con GitHub Actions, fusiona la solicitud solo después de que se superen las comprobaciones necesarias y supervisa la publicación hasta que termine correctamente. Indica la URL final del sitio. Si no tienes permiso para habilitar Pages o fusionar la solicitud, dime exactamente qué debo aprobar o hacer; no informes de que la publicación se ha completado hasta que el sitio esté disponible.
```

![Diagrama de tres fases: una solicitud de incorporación de cambios de space-quiz fusionada; una ejecución correcta de GitHub Actions Deploy website con los pasos Build website, Upload artifact y Deploy to Pages; y el sitio Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verificar la publicación

1. Sigue la ejecución de publicación de GitHub Actions hasta que termine correctamente.
2. Abre la URL indicada y confirma que se carga Space Quiz.

Si el agente no puede habilitar Pages ni fusionar la solicitud, sigue las instrucciones concretas que te proporcione. No des por terminada la tarea hasta que la publicación se haya completado y el sitio esté disponible.

## Resumen y próximos pasos

Space Quiz ya está disponible como sitio web y has comprobado la publicación, en lugar de confiar únicamente en el informe del agente. Continúa con la [lección 13: Repaso y próximos pasos][next-lesson].

[next-lesson]: ../13-review/
