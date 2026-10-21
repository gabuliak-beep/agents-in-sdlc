---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen que ocuparse de deuda técnica: incidencias de seguridad sin resolver, código heredado que hay que actualizar o funcionalidades del backlog que se han pospuesto por falta de tiempo. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidad de forma autónoma. Cuando termina, crea un borrador de solicitud de incorporación de cambios (PR) para que un desarrollador lo revise. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

Con el agente en la nube de Copilot explorarás cómo:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realizan de forma segura.
- delimitar claramente las incidencias.
- asignar incidencias a Copilot.

## Situaciones

Tailspin Toys quiere abordar parte de su deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado; de hecho, no hay documentación. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego, para que los usuarios puedan seguir explorando. Por ahora no hace falta una implementación pulida: basta con una versión que permita probar la experiencia de usuario. Este trabajo no es prioritario, pero hay otras incidencias más urgentes.

Son ejemplos de tareas que suelen posponerse y que puedes asignar al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras tú te ocupas de otras tareas, y después puedes revisar el resultado.

## Presentación del agente en la nube de GitHub Copilot

[El agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano, de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Al asignarla, Copilot crea un borrador de PR para registrar el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o cuando haya terminado. Cuando la solución esté lista para que la revises, te mencionará en la PR.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte la fase de reflexión. Incluso las tareas aparentemente sencillas pueden ser complejas, así que [delimita cuidadosamente las tareas que asignas al agente en la nube][cloud-agent-best-practices]. Trátalo como a un programador que trabaja contigo: avanza por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya completaste el ejercicio práctico de instrucciones personalizadas, has visto `.github/copilot-instructions.md` en acción. Si no, puedes echarle un vistazo. Antes de asignar trabajo al agente en la nube, revisa los archivos de instrucciones de este repositorio para poder observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa los metadatos `applyTo`, que limitan estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario en la PR resultante. Estas directrices proceden de los archivos de instrucciones; volveremos a revisarlas al examinar las PR más adelante.

## Configura el entorno de desarrollo del agente en la nube de Copilot

Crear código suele requerir un entorno específico y ejecutar scripts de configuración para dejarlo listo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para preparar el entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow], guardado en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. En este laboratorio ya está configurado: proporciona Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

Como otros archivos de flujo de trabajo de GitHub, tiene algunas particularidades:

- Contiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todos reconocen la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica puede ralentizar el trabajo y dificultar el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación, así que es una tarea adecuada para asignar al agente en la nube. Trabajará en segundo plano; más adelante revisarás el resultado.

1. Abre una pestaña del navegador y ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. En **Description**, escribe:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create**.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de incorporación de cambios (PR), cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno con el flujo de trabajo anterior, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista de tareas que va a realizar.
14. Desplázate por la cronología de la PR. Verás una actualización cuando Copilot empiece a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la página para ver el indicador actualizado.

16. Recorre la sesión en directo para observar cómo Copilot explora el código, comprende el estado del proyecto, planifica y crea código.

La tarea puede tardar varios minutos. Una de las ventajas principales del agente en la nube es que trabaja de forma asíncrona, lo que te permite ocuparte de otras tareas. Aprovecharemos esta función para asignar otra tarea y, después, crear una funcionalidad en la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube es que permite repartir el trabajo: tú puedes ocuparte de una tarea mientras él trabaja en otra. Aunque añadir una sección de juegos relacionados no lleve mucho tiempo, podrías dedicar ese tiempo a otras tareas. Asígnasela al agente en la nube.

1. Vuelve al repositorio en github.com.
2. Selecciona **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. Escribe `Show related games on the game details page` en **Title**.
5. En **Description**, escribe:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. Selecciona **Create**.
7. A la derecha, selecciona **Assign to Copilot** y, después, **Assign**.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

Al poco tiempo, verás un emoji 👀 en el primer comentario de la incidencia, que indica que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda petición. El agente en la nube trabaja de forma parecida a un ingeniero de software: no hace falta supervisarlo activamente, sino revisar el resultado cuando termine. Ahora veremos cómo crear y usar agentes personalizados.

## Resumen y próximos pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. También puedes usarlo para abordar deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has aprendido a:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realizan de forma segura.
- delimitar claramente las incidencias.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y usar agentes personalizados. El [agente en la nube de Copilot también puede usar servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que ya exploraste en módulos anteriores.

## Recursos

- [Información sobre el agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
