---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica: incidencias de seguridad pendientes, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en el backlog. El agente en la nube de GitHub Copilot está diseñado para tareas como actualizar código y añadir funciones de forma autónoma. Cuando termina, genera un borrador de solicitud de incorporación de cambios (PR) para que lo revise una persona. Así puedes delegar tareas tediosas y dedicar más tiempo a los retos importantes.

Con el agente en la nube de Copilot explorarás cómo:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realizan de forma segura.
- crear incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que falta por completo. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para explorar la experiencia y probarla con usuarios. No hace falta una implementación definitiva, pero el trabajo está bloqueado y hay otras incidencias de mayor prioridad.

Estas tareas pueden quedar relegadas en el backlog, por lo que son buenas candidatas para el agente en la nube de Copilot. El agente puede trabajar en segundo plano mientras tú te ocupas de otras tareas y, cuando termine, podrás revisar su propuesta.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot][copilot-agents] puede realizar tareas en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignarle una incidencia de GitHub][assign-issue]. Cuando se la asignas, Copilot crea una PR preliminar para hacer seguimiento del trabajo, configura el entorno y empieza a trabajar. Puedes inspeccionar la sesión mientras está en curso o una vez terminada. Cuando la solución esté lista para revisarla, Copilot te mencionará en la PR.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no puedes saltarte el análisis. Incluso las tareas aparentemente sencillas pueden ser complejas. Por eso, sigue las [prácticas recomendadas para definir el alcance de las tareas del agente en la nube][cloud-agent-best-practices]. Piensa en él como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md`. Si no, este es un buen momento para revisar los archivos de instrucciones del repositorio antes de asignar trabajo al agente.

Abre estos archivos en la interfaz web de GitHub o en un Codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de archivo de la PR resultante. Estas pautas proceden de los archivos de instrucciones. Volverás a verlas cuando revises las PR en una lección posterior.

## Configurar el entorno de desarrollo del agente en la nube

Para crear código es necesario configurar el entorno y ejecutar los scripts necesarios. Lo mismo ocurre cuando asignas tareas a Copilot: trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions]. Puedes personalizar su entorno con un [flujo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. El flujo prepara Node.js, las dependencias, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no clonas el repositorio en los pasos de configuración, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y de la aplicación - Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero hay un par de aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

La mayoría de los proyectos tienen información desactualizada o carecen de documentación. Esta deuda técnica ralentiza el trabajo y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot puede ayudar a crear documentación, así que esta es una buena incidencia para asignarle. Trabajará en segundo plano y revisarás el resultado en un ejercicio futuro.

1. Abre el repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de nueva incidencia.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y vuelve a cargar la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> Es posible que la lista tarde varios minutos en aparecer. Copilot está preparando el entorno, ejecutando el flujo de trabajo anterior, analizando el proyecto y determinando cómo resolver la incidencia.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la PR hasta ver que Copilot ha empezado a trabajar.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que debas volver a cargar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot explora el código, analiza el estado, elabora un plan y crea código.

El proceso puede tardar varios minutos. Una ventaja importante del agente en la nube es que puede trabajar de forma asíncrona, así que aprovecharemos esa función para asignarle otra tarea mientras añadimos funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una ventaja del agente en la nube es que puedes repartir el trabajo: tú te ocupas de una tarea mientras él trabaja en otra. Aunque añadir una sección de juegos relacionados no debería llevar mucho tiempo, puedes aprovechar ese tiempo para otras tareas. Vamos a asignársela al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot**.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después verás unos 👀 en el primer comentario de la incidencia. Indican que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube funciona de manera parecida a un ingeniero de software: no hace falta supervisarlo continuamente, puedes revisar el resultado cuando termine. Mientras tanto, vamos a crear y usar agentes personalizados.

## Resumen y próximos pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y ayudarte con deuda técnica, nuevas funcionalidades o migraciones de código.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realizan de forma segura.
- crear incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes pasar a crear y usar agentes personalizados. El [agente en la nube también puede usar servidores MCP][cloud-agent-mcp] y las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
