---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo de agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramienta y cada edición. Puedes revisar cada diff, aceptar o rechazar cambios concretos y orientar la conversación con prompts adicionales sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos.
- orientarás al agente durante la sesión para añadir un control del modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has usado en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado añadir también un modo claro para quienes encuentran demasiado oscuro el tema predeterminado. En vez de empezar de cero, orientarás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando proponga una edición, el diff aparecerá en el chat:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed**, en la parte superior del chat, para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta requiere permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará la opción **Allow**. Aprueba únicamente los comandos que consideres adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Orientar al agente para añadir el modo claro

Cuando esté listo el modo de alto contraste, amplía la misma conversación para añadir un control del modo claro. Como continuarás en la misma sesión, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, los estilos y el uso de almacenamiento local.

1. Envía este prompt adicional en el cuadro de Copilot Chat:

    ```plaintext
    ¡Buen trabajo! Ahora añade un control del modo claro junto al del modo de alto contraste. El modo claro debe seguir el mismo patrón: conservar la selección en el almacenamiento local y estar disponible en la misma zona de la IU. El usuario debe poder activar el modo de alto contraste y el modo claro de forma independiente.
    ```

2. El agente explorará los cambios existentes y propondrá los cambios necesarios para añadir el modo claro.
3. Revisa las nuevas ediciones a medida que se apliquen y selecciona **Keep** o **Undo** en cada diff.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta reorganizar los controles—, detenlo con el botón **Stop** junto al cuadro de chat y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa prompts adicionales para pedir ajustes, corregir el enfoque o ampliar el alcance; por ejemplo, simplificar el CSS, integrar el modo claro en el panel existente o añadir métodos abreviados de teclado.

## Revisar los archivos modificados

Antes de confirmar los cambios, revisa todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos; debería incluir componentes Astro, estilos y las pruebas pertinentes.
3. Abre algunos archivos y examina los diffs. Confirma que el trabajo sigue las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios en el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y próximos pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts adicionales para ampliar el trabajo sin perder el contexto.

En esta lección has explorado:

- supervisar el trabajo de un agente en tiempo real.
- aceptar y rechazar ediciones concretas.
- orientar una sesión en curso con prompts adicionales.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios propuestos por el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
