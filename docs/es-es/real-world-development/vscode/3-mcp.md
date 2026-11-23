---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En lugar de recorrer la aplicación manualmente, conectarás el **servidor Playwright MCP** y dejarás que Copilot controle un navegador real. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es el Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas.
- añadirás el servidor Playwright MCP al entorno de trabajo.
- pedirás a Copilot que lo use para probar manualmente la funcionalidad de filtrado en un navegador.
- publicarás la rama y abrirás una PR para el trabajo de filtrado.

## ¿Qué es el Model Context Protocol (MCP)?

El modo de agente es más potente cuando puede acceder a recursos externos al editor. El Model Context Protocol (MCP) es un estándar que permite a Copilot comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

El [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos. Pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente y gestiona la comunicación con herramientas externas, como API existentes o herramientas locales (por ejemplo, paquetes de NPM). Cada servidor MCP representa un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona API para gestionar repositorios de GitHub, crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona automatización del navegador con Playwright para navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP. GitHub aloja un [registro de MCP](https://github.com/mcp) que facilita descubrir soluciones y contribuir al ecosistema.

> [!CAUTION]
> En materia de seguridad, trata los servidores MCP como cualquier otra dependencia. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y ten en cuenta las implicaciones de seguridad. Usa únicamente servidores de confianza y ten cuidado al conceder acceso a recursos o acciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Deberías ver que ya está configurado el servidor `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye esta entrada `github`, por lo que Copilot pudo leer el backlog en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server]; no tienes que instalarlo localmente. VS Code se conecta por HTTP y la primera vez que Copilot utiliza una herramienta te autenticas con GitHub.

## Añadir el servidor Playwright MCP

Añade un segundo servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es local: VS Code lo inicia en tu equipo con `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin ventana visible, algo necesario en un Codespace, que no tiene escritorio gráfico.

> [!NOTE]
> Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, por lo que normalmente ya está instalado el navegador. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Iniciar el servidor y confiar en él

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, selecciona la acción **Start** que aparece encima de la entrada `playwright` para iniciar el servidor y comprobar la configuración.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él.

El inicio manual es opcional. Si lo omites, VS Code iniciará el servidor y te pedirá que confirmes que confías en él la primera vez que Copilot necesite sus herramientas.

## Probar la funcionalidad de filtrado

El [servidor Playwright MCP][playwright-mcp-server] permite que Copilot controle un navegador real. El agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados, y después resumir lo que ha observado. Así se comprueba rápidamente una funcionalidad sin salir de la conversación.

El servidor Playwright MCP obtiene información del [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de hacerlo de capturas de pantalla. Por eso el agente trabaja con elementos estructurados y etiquetados (botones, enlaces y elementos de lista), como lo hace la tecnología de asistencia. Esta comprobación funcional también sirve como verificación básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe el filtrado:

```text
Usa el servidor Playwright MCP para abrir un navegador en la aplicación que se está ejecutando en http://localhost:4321 y comprobar la nueva funcionalidad de filtrado de juegos:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Indica lo que observes en cada paso y señala cualquier comportamiento inesperado.
```

Copilot abrirá un navegador mediante Playwright MCP, seguirá los pasos y te informará de lo que ha encontrado. Compara el resultado con los criterios de aceptación de la incidencia. Si algo no funciona, pide más información o solicita que corrija el código antes de abrir una PR.

> [!NOTE]
> La aplicación debe estar en marcha en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use Playwright MCP puede que tenga que descargar un navegador; si informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de incorporación de cambios

Cuando hayas confirmado que la funcionalidad funciona, publica la rama `filtering-vscode` para que el equipo pueda revisarla.

1. Abre el panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Selecciona el icono **+** para preparar los cambios.
3. Usa el botón **Sparkle** para generar un mensaje de confirmación.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para subir la rama al repositorio.

## Crear la solicitud de incorporación de cambios

Hay varias formas de crear una PR, incluida la interfaz de github.com y GitHub CLI. Como ya estás trabajando con Copilot, pídele que la cree y asocie la incidencia pertinente.

1. Ve al panel de Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editor. Crea una solicitud de incorporación de cambios para la rama actual y asóciala a la incidencia correcta.
   ```

4. Si se te solicita, selecciona **Continue** para permitir las operaciones necesarias. La primera vez que use una herramienta de GitHub, es posible que tengas que autenticarte.
5. Observa cómo Copilot busca la incidencia correspondiente y crea la PR.
6. Selecciona el enlace generado para revisar la PR, pero **no la fusiones todavía**.

## Resumen y próximos pasos

En este ejercicio:

- has aprendido qué es el Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot.
- has añadido el servidor Playwright MCP al espacio de trabajo.
- lo has usado para probar manualmente el filtrado en un navegador antes de publicarlo.
- has publicado la rama y abierto una PR para el trabajo de filtrado.

Has usado un servidor MCP para probar una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué habla todo el mundo de ello][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
