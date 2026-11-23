---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque trabaja de forma asíncrona y autónoma, es importante que puedas supervisar las tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de operaciones puedes ver las tareas de los agentes con solicitudes de incorporación de cambios abiertas, revisar lo que han hecho y orientar las sesiones en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se ha dado cuenta de que también convendría añadir un modo claro. Como ya está actualizando los estilos y añadiendo un control para cambiar de modo, quieres orientar el trabajo para que incluya tanto el modo claro como el modo de alto contraste.

## Revisar las tareas del agente en la nube de Copilot

Vamos a consultar el estado de las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Examina la lista de tareas, tanto en el panel principal como en el panel de la izquierda. Deberías encontrar las tareas que has asignado, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa los pasos que ha completado Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar varias operaciones para completar la tarea.

> [!NOTE]
> Los pasos exactos dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Fíjate también en el panel de solicitudes de incorporación de cambios de la derecha. Desde allí puedes consultar la PR y los archivos modificados.

## Orientar el trabajo del agente en la nube

Ahora que has visto las tareas activas, pidamos a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya usado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que el agente termine la configuración y empiece a trabajar. Lo sabrás cuando empiece a mostrar mensajes como los del ejemplo.
3. En el cuadro **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    Mientras trabajamos en el modo de alto contraste, añade también un modo claro. Debe haber un control para este modo que permita a los usuarios seleccionar el modo de visualización que prefieran.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro para orientar la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo reconoce y lo incorpora al trabajo.

## Dejar que Copilot trabaje

Copilot incorporará la nueva solicitud al trabajo cuando termine el paso que estaba realizando al recibir el mensaje.

Como el proceso puede tardar varios minutos, puedes aprovechar el tiempo para reflexionar sobre lo que has aprendido.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y dirigir las tareas del agente en la nube de GitHub Copilot. Desde allí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para ajustar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- usar mission control y la página de agentes para supervisar las tareas.
- redirigir una sesión en curso para pedir una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
