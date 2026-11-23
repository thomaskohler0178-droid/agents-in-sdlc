---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, conviene que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan directrices para la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones del repositorio y las instrucciones específicas de rutas.
- generarás la primera parte de la capa de datos para el filtrado (un método auxiliar para editores) con las instrucciones actuales.
- añadirás un estándar global al archivo `.github/copilot-instructions.md`.
- volverás a enviar el mismo prompt y observarás cómo se adapta el código generado al nuevo estándar.
- confirmarás los cambios de instrucciones y el trabajo de filtrado en `main` para que Copilot pueda usar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares: Copilot no es determinista. El objetivo es observar la tendencia del comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo código carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices para las prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Usarás archivos de instrucciones para proporcionar a Copilot el contexto que necesita para realizar estas tareas de acuerdo con las prácticas del equipo.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda mejor el estilo y los requisitos de programación. Puedes especificar las convenciones, bibliotecas y comentarios que prefieras, y crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información de nivel de proyecto pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general del proyecto y las prácticas recomendadas.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivos o tareas concretos, como TypeScript, componentes de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las sugerencias de finalización de código ni de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan las instrucciones del repositorio y los archivos `*.instructions.md` que tengan frontmatter `applyTo`.
>
> Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md, tal como se describe en la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en directrices de nivel de proyecto, como la descripción, la estructura y los estándares generales del proyecto.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivo o tareas, como pruebas unitarias, componentes Astro o la capa de datos.
- Escribe en lenguaje natural, con instrucciones claras y ejemplos de código correcto e incorrecto.

No hay una única manera correcta de usar IA o de escribir archivos de instrucciones. Experimenta para descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás archivos para tareas como [actualizar la IU][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El taller del entorno correspondiente lo explicará cuando sea pertinente.
>
> Para encontrar plantillas e ideas, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

El repositorio incluye un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para tareas distintas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Fíjate en la descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, presta atención a las directrices anidadas sobre **GitHub Actions Workflows**; se aplican a cualquier interacción con Copilot.
3. Abre `.github/instructions` y explora sus archivos. Verás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` define un patrón glob relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos de prueba TypeScript, como los que siguen `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en otros más pequeños y reutilizables, y enlazar ejemplos. Las rutas de este archivo son relativas a su propia ubicación.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación, pero todavía no exige incluir documentación en el código. A continuación, añadirás directrices sobre comentarios TSDoc y encabezados de archivo.

## Crear una rama para los cambios

Sigue las prácticas recomendadas y crea una rama:

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea importante.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con las instrucciones actuales. Más adelante, actualizarás el archivo y volverás a enviar el mismo prompt.

1. Cierra las pestañas abiertas del editor para que Copilot solo reciba el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el método auxiliar.
3. En el menú desplegable de agentes de Chat, selecciona **Agent** para que Copilot pueda aplicar cambios.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un método auxiliar de acceso a datos que devuelva una lista de todos los editores, con el nombre y el id de cada uno. Aplica los cambios al archivo.
   ```

5. Copilot explorará el proyecto y aplicará cambios, normalmente al archivo del método auxiliar y sus pruebas.
6. Observa que el método propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto sigue las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de archivo.

> [!CAUTION]
> Copilot es probabilístico, así que podría añadir comentarios incluso sin pedírselo. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añadir un estándar para todo el repositorio

El archivo `.github/copilot-instructions.md` proporciona información de nivel de proyecto. Añade un estándar para mejorar las sugerencias de código:

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Todavía no contiene ninguna regla sobre documentación en el código.
3. Debajo de los estándares existentes, añade estas líneas Markdown para pedir encabezados de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en `.github/copilot-instructions.md` para directrices globales o como `*.instructions.md` para lenguajes, tipos de archivos y tareas específicos. El archivo del repositorio es el lugar adecuado para estándares generales, como el requisito de incluir comentarios de documentación.

## Volver a enviar el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para vaciar el contexto de la conversación.
2. Vuelve a seleccionar `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía **el mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique al código que usarás en el ejercicio siguiente.

## Confirmar las instrucciones y subir la rama

Los archivos de instrucciones se gestionan con el mismo control de código fuente que los demás archivos del repositorio. Vamos a confirmar los cambios y subir la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Ejecuta `git status` para comprobar que están los cambios de instrucciones y del método auxiliar.
3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Sube la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y fusionar una solicitud de incorporación de cambios

Con la rama publicada, crea una solicitud de incorporación de cambios y asóciala a la incidencia del backlog sobre los estándares de documentación. Puedes pedir a Copilot que busque la incidencia, cree una PR para cerrarla y la fusione.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actuará en GitHub en tu nombre. La plantilla del proyecto ya configura las herramientas de GitHub en el entorno. La primera vez que Copilot use una de ellas, VS Code podría pedirte que inicies sesión en GitHub. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista de Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia y cree una PR desde la rama actual, indicando que incluye las instrucciones y el método auxiliar de editores:

   ```
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una solicitud de incorporación de cambios a partir de la rama actual e indica que cierra esa incidencia. Incluye también que hemos añadido la base del método auxiliar de editores para el trabajo de filtrado posterior.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se cree la PR, pide a Copilot que la fusione en `main` y vuelva a actualizar la rama local:

   ```
   Fusiona la solicitud de incorporación de cambios en main. Después, vuelve a main en local y descarga el código más reciente para que quede actualizado.
   ```

7. Cuando Copilot te pida permiso para realizar acciones en GitHub y ejecutar comandos de shell, revisa cada comando y selecciona **Allow** si corresponde.

¡Has creado y fusionado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo Copilot usa los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un estándar global a `.github/copilot-instructions.md`.
- volver a enviar el mismo prompt y observar cómo se adapta el código propuesto.
- confirmar las instrucciones y los cambios iniciales del método auxiliar en `main` para que el siguiente ejercicio pueda usarlos.

A continuación, pondrás en práctica estas instrucciones con el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
