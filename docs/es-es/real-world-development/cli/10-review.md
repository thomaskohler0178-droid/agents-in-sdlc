---
title: "Lección 10 - Repaso y próximos pasos"
description: "Recapitula el flujo de trabajo con Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo con Tailspin Toys. En este recorrido:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y localizado la incidencia de filtrado.
- has añadido las valoraciones con estrellas, revisado el resultado en un navegador reenviado y fusionado manualmente tu primera solicitud de incorporación de cambios (PR).
- has partido de la incidencia de filtrado, definido el enfoque en el modo Plan, creado la funcionalidad en Autopilot y revisado el resultado en Interactive.
- has guiado al agente con instrucciones personalizadas, personalizado la habilidad `quality-checks` y la has usado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has usado para explorar el filtrado en un navegador real.
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- has revisado el cambio completo de filtrado y autorizado Agent Merge para fusionar la PR.
- has explorado comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Lo que has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizado:

1. **Valoraciones con estrellas:** mostrar el valor existente de `starRating` y un estado explícito para las tarjetas de juegos sin valoración.
2. **Filtrado y flujo de trabajo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y rama. Para simplificar el taller, hemos incluido todo ese trabajo en una sola PR.

## Diferentes tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, examinándolo en el navegador y pidiendo a Copilot que lo explore mediante MCP. La habilidad `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos y ha informado de los resultados con el nuevo formato. El agente de QA ha reunido esos resultados y los ha completado con una revisión de los requisitos y de la cobertura de pruebas antes de abrir la PR.

Las pruebas nuevas deben cubrir lagunas reales; una ejecución de QA que no necesite ninguna prueba adicional puede ser correcta. Revisa el código y las pruebas antes de autorizar la fusión, y actualiza las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado las instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados establecen funciones especializadas y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para pensar en un enfoque antes de empezar, **Interactive** para mantenerte al tanto de los cambios acotados y **Autopilot** para las tareas bien definidas. Elige un modelo más rápido para los cambios rutinarios y otro más capaz para los trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye mucho en el resultado.

## Más cosas por explorar

Ya has recorrido el flujo de trabajo principal. Estas funciones adicionales de CLI también pueden resultarte útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Utilízala para código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo que aprendas con el equipo y aprovecha la experiencia de los demás. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller de Copilot cloud agent][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
