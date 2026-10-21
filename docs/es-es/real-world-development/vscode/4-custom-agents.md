---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes mediante archivos Markdown en `.github/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimientos del dominio. Así, los equipos pueden codificar su experiencia en agentes reutilizables: uno de accesibilidad que siga las directrices [WCAG][wcag], uno de seguridad o uno de pruebas.

Los agentes personalizados se definen en archivos Markdown dentro de `.github/agents` del proyecto o globalmente en `~/.copilot/agents`. Cada archivo incluye metadatos YAML con al menos `name` y `description`, seguidos de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se parecen: ambos se definen principalmente con Markdown e indican a la IA cómo realizar tareas. La distinción más clara es que el **agente personalizado** realiza el trabajo y las **habilidades** son herramientas.

Los agentes tienen su propia ventana de contexto y pueden coordinar habilidades y otros agentes. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad. Para ello, podría usar habilidades de flujo de trabajo de PR o de ejecución y gestión de pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba e itera hasta encontrar lo que funciona en tu entorno y situación.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Situación

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todos, independientemente de sus capacidades visuales o preferencias. Algunos usuarios tienen dificultades para leer el tema oscuro porque el contraste entre el texto y el fondo es insuficiente. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es esencial, así que usarás un agente personalizado para crear esta función cuanto antes.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Vuelve a tu codespace y cambia a una rama nueva desde `main` para mantener separado el trabajo de accesibilidad de la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` de los metadatos YAML.

> [!CAUTION]
> Los metadatos `name` y `description` son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que incluyen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas frecuentes.

## Usa el agente personalizado en Copilot Chat

VS Code muestra los agentes personalizados de `.github/agents` en el selector de agentes de Copilot Chat. Al seleccionar uno, la sesión queda limitada a las instrucciones y herramientas de ese agente.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo pueden variar según el ejercicio; sigue las indicaciones de cada paso.

1. Selecciona **Agent** en el selector de agentes de la vista Chat si todavía no está seleccionado.
2. Abre el selector de agentes situado en la parte inferior de la vista Chat. Muestra el agente activo, que por defecto es **default**.
3. Selecciona **Accessibility agent**.
4. Envía este prompt al agente:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot explorará el código, propondrá cambios y editará los archivos del proyecto. Verás las ediciones y sus diferencias en el chat.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos.

> [!NOTE]
> Este proceso puede tardar varios minutos. Copilot hará cambios reales en el repositorio, por ejemplo, en componentes Astro, CSS y pruebas relacionadas.

En el siguiente ejercicio revisarás y guiarás el trabajo mientras sigue en curso.

## Resumen y próximos pasos

En esta lección has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados en tareas y ámbitos concretos. Permiten convertir los conocimientos y estándares del equipo en agentes reutilizables que guían a Copilot.

Has aprendido:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un selector del modo claro a la misma sesión.

## Recursos

- [Información sobre los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
