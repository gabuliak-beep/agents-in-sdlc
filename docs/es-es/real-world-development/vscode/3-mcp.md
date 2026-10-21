---
title: "Ejercicio 3 - Prueba la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la función de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, confirma que funciona en el navegador. En vez de probar la aplicación manualmente, conecta el **servidor MCP de Playwright** y deja que Copilot controle un navegador real. Después publicarás la rama y abrirás una PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- añadirás el servidor MCP de Playwright al área de trabajo.
- pedirás a Copilot que lo use para probar manualmente el filtrado en el navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios para el filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es más potente cuando puede acceder a recursos fuera del editor. Model Context Protocol (MCP) permite que Copilot se comunique con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo de agente y de su interacción con el contexto, el LLM y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Los servidores MCP actúan como puente entre el agente de IA y las herramientas y servicios externos, como API existentes o herramientas locales de paquetes npm. Cada servidor proporciona un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona API para gestionar repositorios de GitHub, incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ofrece automatización del navegador con Playwright, como navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP. GitHub mantiene un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Revisa detenidamente el código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Usa solo servidores en los que confíes y ten cuidado al conceder acceso a información u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Ya debería estar configurado un servidor `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Esta entrada viene con la plantilla del proyecto y permitió a Copilot leer el backlog de incidencias en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server]. No tienes que instalarlo en local: VS Code se conecta por HTTP y te pide que te autentiques la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor MCP de Playwright

Añade un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**. VS Code lo inicia en tu equipo mediante `npx @playwright/mcp@latest`. El indicador `--headless` hace que el navegador se ejecute sin una ventana visible; es necesario en un codespace, donde no hay escritorio.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, por lo que normalmente ya está instalado. Si Copilot indica que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot usa sus herramientas, inicia el servidor y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, selecciona **Start**, que aparece encima de la entrada `playwright`, para iniciar el servidor y comprobar la configuración.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code lo iniciará y te pedirá que confirmes que confías en él cuando Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] permite a Copilot controlar un navegador real. El agente puede abrir una página, navegar, aplicar filtros y leer los resultados; después, te informa de lo que ha observado.

Internamente, el servidor MCP de Playwright trabaja con el [árbol de accesibilidad][playwright-mcp-server] de la página, no con capturas de pantalla. El agente interpreta elementos estructurados y etiquetados, como botones, enlaces y elementos de listas, de forma parecida a las tecnologías de asistencia. Así, una comprobación funcional rápida también sirve como comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador con el servidor MCP de Playwright, seguirá los pasos y te comunicará lo que ha observado. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona como esperas, haz preguntas o pide que corrija el código antes de abrir una PR.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use el servidor MCP de Playwright quizá tenga que descargar un navegador; si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Una vez que hayas confirmado que la función funciona, abre una solicitud de incorporación de cambios (PR) para que el equipo la revise. Primero publicarás la rama `filtering-vscode`.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de commit con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios, como usar github.com o la interfaz de línea de comandos (CLI) de GitHub. También puedes pedir a Copilot que la cree: puede encontrar la incidencia correspondiente y vincularla a la PR.

1. Abre Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree la PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice operaciones. La primera vez que use una herramienta de GitHub, quizá tengas que autenticarte.
5. Observa cómo Copilot busca la incidencia correcta y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la PR, pero **no la combines todavía**.

## Resumen y próximos pasos

En este ejercicio:

- aprendiste qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- añadiste el servidor MCP de Playwright al espacio de trabajo.
- lo usaste para probar el filtrado en el navegador antes de publicarlo.
- publicaste la rama y abriste una solicitud de incorporación de cambios.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué se habla tanto de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
