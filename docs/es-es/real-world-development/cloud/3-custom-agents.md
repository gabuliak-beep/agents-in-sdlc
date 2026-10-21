---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio para que Copilot realice determinados tipos de trabajo con más eficacia. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que compruebe el cumplimiento de las [WCAG][wcag], uno de seguridad que siga prácticas de programación seguras o uno de pruebas que mantenga patrones de prueba uniformes.

Los agentes personalizados se definen mediante archivos Markdown en `.github/agents` del proyecto o de forma global en `~/.copilot/agents`. Cada archivo incluye metadatos YAML con, al menos, los campos `name` y `description`, seguidos de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos puntos en común. Ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar tareas. La distinción más clara es que un **agente personalizado** realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad; para ello, podría usar habilidades como la de flujo de trabajo de solicitudes de incorporación de cambios o la de ejecución y gestión de pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones e itera hasta encontrar la que mejor funcione en tus entornos y situaciones.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados explorarás:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

## Situación

Tailspin Toys se compromete a que todos puedan usar su plataforma de financiación colectiva, independientemente de su capacidad visual o sus preferencias. Comentarios recientes de usuarios indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al poco contraste entre el texto y el fondo. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es esencial, así que quieres implementar esta función cuanto antes. Usarás un agente personalizado para generarla.

## Revisa el agente personalizado de accesibilidad

Ya tienes un agente personalizado para tareas de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

Vuelve a tu codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` de los metadatos YAML.

> [!CAUTION]
> Los metadatos `name` y `description` son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que destacan:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores frecuentes y problemas que debes evitar.

## Crea y asigna una incidencia

Mission control es el lugar central donde se trabaja con los agentes del entorno. Permite asignar tareas al agente en la nube de Copilot, supervisarlas y redirigirlas o proporcionar indicaciones adicionales. Vamos a asignar a Copilot la tarea de crear el modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de creación.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Add high contrast mode to website` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

   ![Captura del cuadro de asignación del agente en la nube, con el agente personalizado y Accessibility resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y próximos pasos

En esta lección has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados en tareas y ámbitos concretos de GitHub Copilot. Permiten convertir los conocimientos y estándares del equipo en agentes reutilizables que guían a Copilot para realizar determinados tipos de trabajo con más eficacia.

Has aprendido:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

Mientras Copilot trabaja en el modo de alto contraste, puedes [supervisar y guiar la sesión del agente][next-lesson] desde Mission control.

## Recursos

- [Información sobre los agentes personalizados][custom-agents]
- [Preparar el uso de agentes personalizados en tu organización][org-custom-agents]
- [Preparar el uso de agentes personalizados en tu empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
