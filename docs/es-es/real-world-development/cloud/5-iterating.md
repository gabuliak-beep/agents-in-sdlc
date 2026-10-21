---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Le pediste que documentara el código, creara una función de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera funciones de accesibilidad, incluidos los modos de alto contraste y claro. Ahora revisarás los cambios de código y, si hace falta, darás indicaciones a Copilot para mejorarlos.

### Situación

Como hemos repetido, los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de habilitar la revisión para el resto del equipo, revisaremos las propuestas de GitHub Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot trabaja de forma asíncrona y sin supervisión, existen medidas de seguridad para proteger el entorno:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama de trabajo.
- El agente se ejecuta en GitHub Actions y crea un entorno independiente y efímero.
- Un usuario debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- El acceso a [recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por explorar la primera solicitud de incorporación de cambios (PR) generada por el agente en la nube: la que añade documentación al código. Usarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que la PR muestre una advertencia de que un firewall ha bloqueado a GitHub Copilot. Es **lo esperado**, ya que el acceso a recursos externos, incluidas las llamadas a servidores MCP, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada algo parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, incluidos los comentarios TSDoc y otros cambios de documentación. El resultado exacto puede variar. Busca los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario que proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca el indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar los flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se superarán. Puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una PR no es unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios insertados en el código. Copilot verá el comentario e iniciará otra sesión para responder. Como los resultados son probabilísticos, no hay una instrucción única que debas usar. Por ejemplo, puedes pedirle que:

- añada encabezados de comentario al principio de cada archivo de código para describir su propósito.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario en el que menciones a **@copilot** y solicites un cambio en la documentación generada. Puedes usar una idea anterior u otra sugerencia.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para aplicar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya terminado, aparecerá un commit nuevo en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que el resultado te convenza. Después, convierte la PR de borrador en lista para revisión y combínala.

![Convertir la PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot generó para mostrar juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada algo parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Busca el indicador de que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar los flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que las comprobaciones del proyecto para la aplicación Astro se superan; puede tardar unos minutos.
9. **Opcional:** cambia a la rama de Copilot en tu Codespace para probar manualmente la función. En el terminal, ejecuta estos comandos y sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes seguir iterando con el agente en la nube para pedir cambios, por ejemplo, ajustar el número de juegos relacionados o añadir encabezados de comentario y comentarios TSDoc. Recuerda que esta incidencia se asignó **antes** de actualizar las instrucciones personalizadas. Para pedir cambios, añade otro comentario en la pestaña **Conversation**; Copilot iniciará una nueva sesión.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debe incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste en Mission control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada algo parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía está trabajando, la PR tendrá la etiqueta **[WIP]**. Espera a que termine.

4. Selecciona **Files changed** y revisa el código generado.
5. Revisa la implementación, prestando atención a:
   - los componentes de alternancia de los modos.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios CSS o de estilo para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como etiquetas ARIA y navegación con teclado.
   - el código JavaScript/TypeScript que gestiona los modos.
6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Busca el indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar los flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que las comprobaciones del proyecto para la aplicación Astro se superan; puede tardar unos minutos.
10. **Opcional:** cambia a la rama en tu Codespace para probar las funciones de accesibilidad. Ejecuta los comandos siguientes y sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicia la aplicación y prueba en el navegador los modos de alto contraste y claro. Confirma que funcionan y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad guió a Copilot para seguir las prácticas recomendadas. Si detectas problemas o mejoras de accesibilidad, menciona a **@copilot** en un comentario y solicita los cambios, como hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube es más útil cuando le asignas elementos reales del backlog y puedes ocuparte de otra cosa. Para adquirir esta costumbre, crea algunas incidencias en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- crea un formulario para que los usuarios expresen interés en la página de detalles del juego.
- implementa la paginación en la lista de juegos.
- añade validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizados incluidos en el repositorio** para observar su efecto en el trabajo del agente.
- **Asignaste incidencias al agente en la nube de Copilot** y observaste cómo preparaba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y usaste un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad.
- **Revisaste e iteraste sobre las PR del agente en la nube**, mencionando a `@copilot` para pedir cambios y aprobando flujos de trabajo.

## Revisión y próximos pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo aprendido:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
