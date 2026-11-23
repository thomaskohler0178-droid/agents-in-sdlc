---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, autoriza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya combinaste manualmente la PR de las valoraciones con estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y un worktree, con todo ese trabajo en una sola PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- examinarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR se ha fusionado.

## Escenario

A lo largo del flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener la autorización de fusión bajo el control del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión lee la PR y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y reorganiza la base cuando hace falta. Después, la fusiona en cuanto GitHub lo permite. El proceso se ejecuta en segundo plano, continúa aunque reinicies la aplicación y se desactiva cuando se fusiona la PR.

Hasta ahora has seleccionado **Merge pull request** tú mismo. Agent Merge puede encargarse de esa tarea, pero todavía necesita tu autorización explícita para editar código y fusionar. Revisa las acciones que se le permiten y el trabajo antes de darle permiso para fusionar.

## Usar Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Vamos a permitir que Agent Merge gestione el proceso de la PR.

1. Usa el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

El proceso de Agent Merge empezará. Se encargará de:

- crear la PR con un título y una descripción.
- incluir una referencia a la incidencia relacionada en la descripción si iniciaste la sesión desde una incidencia.
- reorganizar la base o resolver posibles conflictos con la rama de destino.
- supervisar CI para confirmar que se superan todas las comprobaciones.
- supervisar la PR para responder a los comentarios de otros desarrolladores o de la revisión de código de Copilot.
- fusionar la PR automáticamente cuando todo se haya completado correctamente, si así lo autorizas.

Vamos a permitir que Agent Merge fusione la PR cuando se superen todas las comprobaciones:

5. Revisa el diff de la PR y las pruebas de verificación antes de conceder permiso para fusionarla.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que está seleccionada la opción **Merge pull request**.
8. Pide explícitamente al agente que fusione:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Supervisa el progreso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR es **Merged**. Tener habilitado el permiso de fusión o un agente en ejecución no confirma que la fusión haya terminado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y gestionar las solicitudes de incorporación de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- has examinado la PR completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge solo después de la revisión y confirmado que la PR se ha fusionado.

A continuación, [usarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
