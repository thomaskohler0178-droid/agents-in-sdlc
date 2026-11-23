---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, conviene que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan directrices para que Copilot entienda no solo *qué* quieres, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones específicas de rutas.
- añadirás un estándar global al archivo `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no compararás prompts antes y después. El agente en la nube de Copilot trabaja de forma asíncrona con incidencias de GitHub, por lo que su efecto es más difícil de mostrar en tiempo real. Verás cómo influyen tus instrucciones cuando revises las solicitudes de incorporación de cambios que genere en ejercicios posteriores.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones, proporcionarás a Copilot la información que necesita para realizar estas tareas de acuerdo con las prácticas del equipo.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda mejor el estilo de programación y los requisitos. Puedes especificar las convenciones, bibliotecas y comentarios que prefieras, y crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

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

La aplicación de ejemplo incluye un enfoque representativo. En general:

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

Dedica un momento a leer los archivos de instrucciones incluidos en el repositorio: hay un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para tareas distintas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, presta atención a las directrices anidadas sobre **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora sus archivos. Verás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo`: define un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos de prueba TypeScript, como los que siguen el patrón `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en otros más pequeños y reutilizables, y enlazar ejemplos. En este archivo, las rutas son relativas a la ubicación del propio archivo, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación, pero todavía no exige incluir documentación en el código. En los pasos siguientes añadirás directrices sobre comentarios TSDoc y encabezados de archivo.

## Añadir un estándar para todo el repositorio

Primero, crea una rama para trabajar:

1. Vuelve al terminal de tu Codespace.
2. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

El archivo `.github/copilot-instructions.md` proporciona información de nivel de proyecto a Copilot. Añadiremos directrices para documentar el código:

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Aunque documenta los estándares de programación, todavía no indica que haya que documentar el código.
3. Debajo de los estándares existentes, añade estas líneas Markdown para indicar a Copilot que incluya encabezados de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en `.github/copilot-instructions.md` para las directrices globales o como `*.instructions.md` para lenguajes, tipos de archivos y tareas específicos. El archivo del repositorio es el lugar adecuado para estándares generales, como el requisito de incluir comentarios de documentación.

## Confirmar, subir y fusionar la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio, partirá de `main`; por eso, los cambios deben llegar a `main` para que pueda usarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Sube la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y fusiónala. Así, el agente en la nube leerá estas instrucciones cuando le asignes incidencias en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios directamente en `main`. Usar una rama permite practicar un flujo de trabajo habitual.

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones de este proyecto y has añadido un estándar para todo el repositorio en `.github/copilot-instructions.md`. Verás cómo se aplica ese estándar en las solicitudes de incorporación de cambios que genere el agente en la nube durante el resto del itinerario.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
