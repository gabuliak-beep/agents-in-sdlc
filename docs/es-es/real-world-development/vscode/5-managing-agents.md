---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo de agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat de VS Code muestra las llamadas a herramientas y las ediciones a medida que se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cambios y orientar la conversación con otros prompts sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- guiarás al agente durante la sesión para que añada un selector del modo claro además del modo de alto contraste.

## Situación

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para los usuarios a quienes les resulta demasiado oscuro el tema predeterminado. En vez de iniciar otra conversación, guiarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra los pasos del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat con sus diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar cada llamada a herramienta, lectura o edición de archivos y comando del terminal.
3. Cuando proponga una edición, aparecerá la diferencia en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y dejar que el agente vuelva a intentarlo.
4. Abre el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta requiere permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará **Allow**. Aprueba solo los comandos que consideres adecuados.

5. Espera a que el agente termine el modo de alto contraste. Verás un mensaje final que resume los cambios.

## Guía al agente para añadir el modo claro

Cuando el modo de alto contraste esté listo, amplía la conversación para añadir un selector del modo claro. Como continuarás en la misma sesión de chat, el agente conserva el contexto de su trabajo anterior: los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento desde el campo de Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente revisará los cambios anteriores y propondrá cambios para el modo claro.
3. Revisa cada edición nueva y selecciona **Keep** o **Undo**.
4. Si el agente toma un rumbo no deseado, por ejemplo, refactorizar el selector de una forma que no te guste, detenlo con el botón **Stop** junto al campo de chat y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de cero. Usa prompts de seguimiento para pedir ajustes («simplifica el CSS: usa variables en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer selector; integra el modo claro en el panel de ajustes existente») o ampliar el alcance («añade también atajos de teclado para ambos selectores»).

## Revisa los cambios

Antes de crear el commit, revisa rápidamente todo lo que haya modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre varios archivos y revisa las diferencias. Confirma que se siguen las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia con almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y próximos pasos

Has usado el modo de agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado las ediciones, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste y rechazaste ediciones individuales.
- guiaste una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad como solicitud de incorporación de cambios y revisarás los cambios del agente local personalizado.

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
