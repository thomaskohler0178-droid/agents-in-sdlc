---
title: "Lección 5 - Personalizar y usar una habilidad quality-checks"
description: "Explora la habilidad quality-checks, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Para desarrollar código hay que hacer más que escribir código. Ya has comprobado manualmente que funciona y has usado archivos de instrucciones para que siga los estándares. ¿Qué ocurre con las pruebas, el lint y las demás partes de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para estas tareas. Ayudan a Copilot a entender cómo realizar operaciones como estas.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de sus resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones de tareas reutilizables, scripts ejecutables y recursos de apoyo que un agente carga cuando los necesita. Normalmente son una carpeta con el nombre de la habilidad y un archivo Markdown `SKILL.md`. El archivo contiene un frontmatter con `name` y `description`, una descripción general y directrices sobre cuándo usar la habilidad. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> No es obligatorio incluir otros archivos o carpetas. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque usa los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para compartirlas y reutilizarlas en el equipo, o en la carpeta personal de habilidades `~/.copilot/skills`.

## Explorar la habilidad

Vamos a explorar la habilidad `quality-checks` que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve al Codespace y abre `.github/skills/quality-checks/SKILL.md` en el editor.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo usar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Fíjate en que todavía no hay una sección **Results output formatting**.

## Ejecutar la habilidad antes de cambiarla

Puedes llamar a las habilidades directamente desde Copilot CLI o pedirlas en lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe que aparece al final.

## Personalizar el informe

Vamos a mejorar el informe para que indique qué se ha ejecutado, si ha terminado correctamente y qué resultados han devuelto las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades modificadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe y compáralo con el primero.

## Resumen y próximos pasos

Has personalizado y usado una habilidad existente. En esta lección:

- has explorado la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has vuelto a cargar y ejecutado la habilidad.

El cambio se incluirá con el filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el [servidor Playwright MCP][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
