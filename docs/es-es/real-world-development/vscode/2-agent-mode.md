---
title: "Ejercicio 2 - Añadir funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos suelen afectar a varios archivos y requieren operaciones como ejecutar pruebas. Normalmente, el desarrollador localiza todos los archivos necesarios, hace los cambios, ejecuta las pruebas, depura el resultado, descubre qué archivo ha olvidado y vuelve a modificarlo. Y así sucesivamente.

El modo de agente de Copilot puede ayudar con este proceso.

El modo de agente de Copilot trabaja con más autonomía en el IDE. Primero explora la estructura del proyecto, realiza los cambios necesarios y ejecuta tareas como las pruebas. Si detecta errores, puede corregirlos automáticamente. Vamos a ver cómo usar el modo de agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube están diseñados para experiencias diferentes. El modo de agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción. El agente en la nube trabaja de forma asíncrona, como un compañero del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- usar el modo de agente de Copilot para explorar el proyecto, identificar los archivos relevantes y coordinar cambios.
- implementar funcionalidades en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas generadas por el modo de agente antes de fusionarlos.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior añadiste un método auxiliar para recuperar editores. Ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web Tailspin Toys

Antes de hacer cambios, explora el sitio para entender cómo funciona.

Tailspin Toys es una plataforma de micromecenazgo para juegos de mesa con un tema de desarrollo. Permite mostrar juegos y sus detalles. El sitio es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM desde su frontmatter; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los métodos reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con un pequeño `<script>` de Astro que usa las API DOM estándar.

### Iniciar el sitio web

El comando `npm run dev` inicia el único servidor de desarrollo de Astro.

1. Vuelve a tu Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de Astro esté listo, aparecerá un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost en el terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al Codespace, que ahora hospeda el servidor web.

### Explorar el sitio web

Cuando el sitio esté en marcha, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea importante.

El sitio ya funciona, pero queremos añadir capacidades. Como al crear el proyecto a partir de la plantilla se generaron automáticamente incidencias, pide a GitHub Copilot que te muestre el backlog.

1. En el menú desplegable de agentes de Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores no recomiendan un modelo concreto. Durante la elaboración del taller se usó Claude Sonnet 4.5 para que las sugerencias fueran relativamente coherentes y ofrecer una experiencia adecuada. Como los modelos de lenguaje son probabilísticos, es normal que las sugerencias difieran de los ejemplos.

> [!NOTE]
> Debido al comportamiento probabilístico de los modelos de lenguaje, Copilot podría usar un comando MCP diferente y completar igualmente la tarea.

3. Pide a Copilot que muestre las incidencias del backlog:

   ```plaintext
   Muéstrame el backlog del repositorio de GitHub. Ayúdame a priorizar sus elementos según los que resulten más útiles para el usuario.
   ```

4. Selecciona **Continue** para permitir que se ejecute el comando que enumera las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado las incidencias según el valor que considere que tienen para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporciona contexto. Usarás el archivo de [interfaz de usuario (IU)][ui-instructions], que explica cómo añadir funciones al sitio.

1. En el Codespace, ve a `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales, que incluyen:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos específicos, incluidos los de Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del propio archivo. Así se pueden reutilizar directrices, dividir conjuntos complejos en fragmentos manejables y proporcionar ejemplos y plantillas.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## Implementar el filtrado

Para completar el filtrado, la aplicación necesita al menos tres cambios:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Las pruebas también deben ejecutarse y superarse antes de fusionar los cambios. El modo de agente puede encargarse de estas tareas.

1. Continúa la conversación actual con Copilot o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para incluir el archivo de instrucciones.

   ![Captura de la selección del archivo de instrucciones de IU](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Actualiza el sitio para incluir filtros por editor y categoría según los requisitos de la incidencia relacionada del backlog. Ya existe un método auxiliar para los editores del ejercicio anterior, así que conserva y perfecciona ese trabajo cuando sea necesario mientras completas el resto de la capa de datos, la IU y las pruebas. Comprueba que se superen todas las pruebas antes de terminar. El servidor ya está en ejecución; no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes en la capa de datos, la interfaz y las pruebas. Después, modificará los archivos y ejecutará pruebas.

   ![Captura de Copilot explorando archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar varios minutos. Mientras trabaja, es posible que aparezcan los botones **Keep** y **Undo** en el editor. No tienes que seleccionarlos hasta que Copilot termine; entonces tendrás que aceptar o deshacer todos los cambios.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura del cuadro de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas o que fallen algunas pruebas durante el proceso. Copilot alternará entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completa con el modo de agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y próximos pasos

Has explorado cómo usar el modo de agente de GitHub Copilot para añadir funciones al sitio Tailspin Toys. Has aprendido a:

- implementar funciones en la interfaz y la capa de datos con el modo de agente.
- explorar el proyecto, identificar archivos y coordinar cambios.
- revisar los cambios y las pruebas antes de fusionarlos.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio extra: implementar la paginación

A medida que crezca la lista de juegos, necesitarás paginarla. Usa lo aprendido para pedir a Copilot que implemente la paginación. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas y usa los archivos de instrucciones existentes.
- decide cómo implementar la paginación y si el usuario podrá elegir el tamaño de página.
- proporciona a Copilot las instrucciones necesarias para obtener el comportamiento deseado.
- puede que tengas que iterar, pedir cambios y aportar más contexto. Así suele trabajarse con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: funciones y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
