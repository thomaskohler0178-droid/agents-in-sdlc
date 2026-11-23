---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definirlos en archivos Markdown dentro de `.github/agents` del repositorio, puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML con, al menos, los campos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se parecen: ambos se definen principalmente con archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La diferencia más sencilla es que un **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará el sitio según las pautas de accesibilidad y lo actualizará. Como parte de ese trabajo, podría utilizar habilidades para gestionar solicitudes de incorporación de cambios o ejecutar pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la que mejor se adapte al entorno y a las tareas.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de micromecenazgo sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Algunos usuarios han señalado que cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

En este ejercicio:

- revisarás el agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar el modo de alto contraste.

La accesibilidad es importante y quieres implementar el cambio cuanto antes. Para ello, usarás un agente personalizado.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Vamos a revisar su contenido para entender cómo guiará a Copilot.

Vuelve a tu Codespace y abre un terminal para crear una rama nueva a partir de `main`. Mantendrás la PR de filtrado del ejercicio 3 separada:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - problemas frecuentes y errores que hay que evitar.

## Usar el agente personalizado en Copilot Chat

VS Code muestra los agentes personalizados definidos en `.github/agents` en el menú desplegable de agentes, en la parte inferior de la vista Copilot Chat. Puedes seleccionar uno para limitar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea importante.

1. Si todavía no está seleccionado, elige **Agent** en el menú desplegable de agentes.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el menú de agentes situado en la parte inferior de la vista Chat. De forma predeterminada muestra el agente activo, **default**.
3. Selecciona **Accessibility agent** en la lista de agentes.
4. Envía este prompt:

    ```plaintext
    Añade un modo de alto contraste al sitio. Debe haber un control para que el usuario active el modo y la selección debe persistir al volver a cargar la página mediante el almacenamiento local del navegador.
    ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff para que puedas revisarla.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot hará cambios reales en el repositorio: editará componentes Astro, CSS y las pruebas relacionadas.

Revisarás y orientarás este trabajo en curso en el siguiente ejercicio.

## Resumen y próximos pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios a medida que se aplican y añadirás un control para el modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
