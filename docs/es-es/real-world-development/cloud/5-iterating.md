---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades: documentar el código, crear una sección de juegos relacionados y añadir opciones de accesibilidad, como los modos de alto contraste y claro. Vamos a revisar los cambios y, si hace falta, enviaremos comentarios a Copilot para que mejore el resultado.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de activar la revisión para el resto del equipo, revisaremos las propuestas de Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se han establecido medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para su código.
- El agente se ejecuta en GitHub Actions, en un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- De forma predeterminada, el [acceso a recursos externos está limitado][agent-firewall], incluidos los servidores MCP.

## Revisar la documentación generada

Empezaremos con la primera solicitud de incorporación de cambios (PR) creada por el agente en la nube de GitHub Copilot: la que añade documentación al código. Usarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que en la PR aparezca un aviso de que Copilot está bloqueado por un firewall. Es **normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot todavía está trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá comentarios TSDoc y otra documentación. Los cambios concretos pueden variar. Busca los comentarios TSDoc, las convenciones de TypeScript y los encabezados de archivo que proceden de las instrucciones personalizadas que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar varios minutos.

## Solicitar cambios a GitHub Copilot

Trabajar con Copilot en una PR no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios en línea del código, como harías con otra persona del equipo. Copilot leerá los comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no hay un prompt único que debas usar. Por ejemplo, puedes pedirle que:

- añada encabezados breves que describan la función de cada archivo de código.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario en la PR y menciona a **@copilot** para pedirle un cambio en la documentación generada. Puedes usar una de las ideas anteriores u otra sugerencia.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para aplicar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás una confirmación nueva en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y fusionarla en la rama principal.

![Marcar una PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Volvamos a la PR que Copilot generó para mostrar juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código.
5. Cuando hayas terminado la revisión, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones de la aplicación Astro se superarán; el proceso puede tardar varios minutos.
9. **Opcional:** puedes cambiar a esta rama en Codespaces para probar manualmente la funcionalidad de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ya ha creado la funcionalidad. Puedes pedirle cambios adicionales mediante un comentario nuevo en la pestaña **Conversation**. Por ejemplo, puedes cambiar el número de juegos relacionados que se muestran o pedirle que añada encabezados y comentarios TSDoc (recuerda que asignaste esta tarea **antes** de actualizar las instrucciones personalizadas).

## Revisar las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas por el agente personalizado. Esta PR debe incluir tanto el modo de alto contraste asignado en el ejercicio 3 como el modo claro solicitado desde mission control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación por teclado, etc.).
   - el código JavaScript/TypeScript que gestiona el cambio de modo.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que se han puesto en cola las comprobaciones y que las pruebas del proyecto se superan.
10. **Opcional:** cambia a esta rama en Codespaces y prueba manualmente las funciones de accesibilidad:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Comprueba que funcionan y que la configuración se conserva al volver a cargar la página.

El agente personalizado de accesibilidad ha ayudado a Copilot a implementar las funciones siguiendo buenas prácticas. Si detectas problemas o mejoras posibles, puedes mencionarlo en un comentario, como hiciste con las otras PR.

## Ejercicio opcional: seguir delegando

El agente en la nube funciona mejor cuando le asignas tareas reales del backlog y puedes ocuparte de otra cosa. Para practicar, crea algunas incidencias más y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para indicar interés en un juego.
- añadir paginación a la lista de juegos.
- añadir validación de entradas y gestión de errores a los métodos de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. A lo largo de estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para observar cómo influyen en los resultados del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** para implementar funciones de accesibilidad, como los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de operaciones** para supervisar la sesión de accesibilidad y orientarla mientras estaba en curso.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Repaso y próximos pasos

Has completado el itinerario del agente en la nube. Los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y una integración opcional con el agente en la nube mediante `/delegate`.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
