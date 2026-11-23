---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes en archivos Markdown de la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos para orientar tareas concretas. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown dentro de `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML con, al menos, los campos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se parecen en algunos aspectos: ambos se definen principalmente con archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La diferencia más sencilla es que un **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisará el sitio según las pautas de accesibilidad y lo actualizará. Como parte de su trabajo, podría utilizar habilidades como una que gestione el flujo de trabajo de las solicitudes de incorporación de cambios o ejecute pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la que mejor se adapte al entorno y a las tareas.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio:

- revisarás el agente personalizado de accesibilidad existente.
- asignarás una tarea al agente personalizado.

## Escenario

Tailspin Toys quiere que su plataforma de micromecenazgo sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Algunos usuarios han señalado que cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante y quieres implementar el cambio cuanto antes. Para ello, usarás un agente personalizado.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Vamos a revisar su contenido para entender cómo guiará a Copilot.

Vuelve a tu Codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - problemas frecuentes y errores que hay que evitar.

## Crear y asignar una incidencia

Mission control es el centro de operaciones de los agentes de tu entorno. Desde allí puedes asignar tareas al agente en la nube de Copilot, supervisarlas y orientar una sesión en curso. Vamos a asignarle una tarea para añadir el modo de alto contraste.

1. Ve al repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de nueva incidencia.
4. Selecciona **Blank issue**.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Necesitamos un modo de alto contraste para el sitio. Debe haber un control que permita al usuario activar el modo y la selección debe guardarse en el almacenamiento local del navegador.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Captura del cuadro de asignación del agente en la nube, con los campos del agente personalizado y de accesibilidad resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y próximos pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde mission control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para usar agentes personalizados][org-custom-agents]
- [Preparar la empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
