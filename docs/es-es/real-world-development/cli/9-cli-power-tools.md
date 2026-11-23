---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente a la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier herramienta de CLI, GitHub Copilot CLI incluye numerosos comandos de barra para interactuar con ella. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Vamos a explorar otros comandos útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usarás `/model` para explorar los modelos disponibles.
- aprenderás cómo `/share` puede exportar o compartir una sesión.
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar en el agente en la nube.

## Escenario

Has terminado el flujo de trabajo principal de CLI. Ahora veremos otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y, de forma opcional, delegar tareas en [Copilot cloud agent][about-cloud-agent].

## Explorar el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puede que alcances el límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario. También puedes inspeccionar o compactar el contexto con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Fíjate en el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es pequeña, quizá apenas haya cambiado.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena la ventana. Usa `/compact` si quieres elegir el momento. Usa `/clear` o `/new` cuando cambies a una tarea que no esté relacionada y quieras empezar una conversación nueva.

## Elegir un modelo

Los modelos tienen distintas ventajas y cada desarrollador puede tener sus preferencias. Copilot CLI permite mostrar la lista de modelos y seleccionar uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Trabajar en equipo y compartir lo aprendido ayuda a todos a sacar más partido a las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en la versión instalada.
2. Si quieres compartir la sesión, elige el destino adecuado. Por ejemplo, `/share file` crea una exportación Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones de sesiones pueden incluir prompts, respuestas e información del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio ni de la conversación que el equipo no quiera compartir.

## Opcional: ampliar o delegar el trabajo

Has completado el taller principal. Copilot CLI también ofrece comandos para tareas de mayor alcance:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Empieza por una tarea nueva y bien delimitada, y revisa el resultado con tu flujo de trabajo habitual. Si quieres profundizar en el trabajo de agentes asíncronos, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y próximos pasos

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y consultar lo que ocurre en segundo plano. En esta lección:

- has usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- has usado `/model` para explorar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir una sesión.
- has explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Copilot CLI incluye muchos otros comandos de barra y funciones por descubrir. Para cerrar el recorrido, [repasaremos lo aprendido][next-lesson] y veremos los próximos pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
