---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta, conviene que Copilot tenga a mano esas indicaciones. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha seguido Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- actualizarás el archivo de instrucciones para que se sigan los estándares de programación.
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier empresa de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos con TSDoc/JSDoc, incluidos los argumentos `db` inyectables cuando existan.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben mantenerse al día cuando cambie el código relacionado.
- Se deben conservar las directrices de formato y lint existentes.

Los archivos de instrucciones permiten proporcionar a Copilot la información adecuada para que trabaje de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que entienda mejor el estilo de programación y los requisitos. Permiten orientar sus sugerencias y fragmentos de código con las convenciones, bibliotecas y comentarios que prefieras. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información de nivel de proyecto que sea pertinente para la mayoría de las solicitudes de chat o CLI.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivos o tareas concretos.

> [!NOTE]
> La disponibilidad de otros formatos de instrucciones depende del entorno. Consulta la [referencia sobre la compatibilidad con las instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones personalizadas de este proyecto

El proyecto de inicio ya incluye algunos archivos de instrucciones. Vamos a explorarlos antes de hacer cambios para observar el efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Verás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros ámbitos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables y enlazar ejemplos.

## Actualizar los archivos de instrucciones para reflejar las directrices del equipo

Aunque los archivos existentes son un buen punto de partida, falta algo. Modificaremos el archivo principal `copilot-instructions.md` para asegurarnos de que se añadan comentarios TSDoc al TypeScript nuevo.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade el siguiente punto al final de la sección:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Usar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio cuando empieza una conversación. Después de guardar las instrucciones actualizadas, sal de la conversación de filtrado y reanúdala para que Copilot cargue las directrices nuevas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda automáticamente las conversaciones.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   Acabamos de actualizar nuestras instrucciones y directrices de programación. Actualiza el código que has generado para que las cumpla.
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc nuevos y confirma que describen el código con precisión.

## Resumen y próximos pasos

Has explorado cómo Copilot CLI utiliza las instrucciones del repositorio y has aplicado un estándar nuevo a la funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- has actualizado el archivo de instrucciones para que se sigan los estándares de programación.
- has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para que el lint y las pruebas se ejecuten de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
