---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos destacado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, y permitir que Copilot acceda a más automatizaciones. Para eso sirven los servidores MCP: amplían las funciones integradas de Copilot CLI con más herramientas y servicios.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadirás el servidor MCP de Playwright si todavía no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Situación

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz de usuario hay que interactuar con ella. Quieres que Copilot pueda usar el sitio web como lo haría una persona para automatizar aún más las comprobaciones y tener más confianza en que los cambios funcionan como esperas.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Gracias a MCP, los agentes de IA pueden comunicarse con herramientas y servicios externos en tiempo real, acceder a información actualizada y realizar acciones en tu nombre.

Estas herramientas y recursos se exponen mediante un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP proporciona un conjunto distinto de herramientas y recursos al agente.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: ofrece funciones de automatización del navegador con Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si el servidor MCP de Playwright todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio web como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. Abre el cuadro para añadir un servidor MCP escribiendo este comando en Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega este comando en el cuadro **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Antes confirmaste manualmente que la funcionalidad se comportaba como esperabas. Ahora pide a Copilot que haga lo mismo con el servidor MCP de Playwright.

1. Usa este prompt para pedir a Copilot que valide la funcionalidad con el servidor MCP de Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No es necesario pedir explícitamente a Copilot que use el servidor MCP: normalmente lo detecta por su cuenta. Aun así, indicarle qué debe usar ayuda a obtener resultados más uniformes y a ahorrar algunos tokens.

2. Observa cómo Copilot enumera los pasos que realiza en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como esperas.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio web, detendrá el servidor y te proporcionará un informe.

## Resumen y próximos pasos

¡Enhorabuena! Has usado el servidor MCP de Playwright desde Copilot CLI para explorar la funcionalidad en un navegador real. En resumen:

- aprendiste qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadiste el servidor MCP de Playwright si todavía no estaba disponible.
- pediste al agente que controlara un navegador y explorara la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que reúne la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué es MCP y por qué se habla tanto de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
