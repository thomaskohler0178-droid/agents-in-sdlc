---
title: "Lección 8 - Crear y fusionar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Tú mismo fusionaste la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionarás todo el cambio de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- activarás Agent Merge solo después de la revisión y confirmarás que la PR se ha fusionado.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar las tareas restantes de la PR sin quitar al desarrollador el control sobre la autorización de fusión.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al activarlo, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y vuelve a establecer la base cuando hace falta. Después, habilita la fusión automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** tú mismo. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y decide que está listo antes de activarlo.

## Usar Agent Merge para gestionar la PR

Ya has creado todo el código. Vamos a revisarlo, crear la PR y dejar que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro del agente con el comando `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo termine correctamente, realizará la fusión.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, lee el motivo y realiza manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y próximos pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y ahora gestionar la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- has inspeccionado todo el cambio de la funcionalidad y las pruebas de verificación.
- has creado la PR de filtrado.
- has activado Agent Merge solo después de la revisión y confirmado que la PR se ha fusionado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
