---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas distintas para mejorar la experiencia de usuario y añadir funciones. Aunque el agente en la nube está diseñado para trabajar de forma asíncrona y autónoma, también es importante poder supervisar estas tareas.

Dispones de varias herramientas para gestionar el trabajo asignado al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de agentes con solicitudes de incorporación de cambios (PR) abiertas. También puedes explorar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para pedir una función adicional.

## Situación

Después de asignar al agente la creación del modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo funciones de alternancia, tenía sentido incluir esta función. Guiarás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Consulta el estado actual de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las acciones que ha realizado Copilot, como:
   - descargar el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar distintas tareas para completar el trabajo asignado.

> [!NOTE]
> Los pasos exactos dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios que aparece a la derecha. Desde allí puedes ver la PR y los archivos modificados para supervisar mejor el trabajo.

## Guía al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada un selector de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que use Copilot y del estado actual del trabajo.

   ![Sesión de accesibilidad en Mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando aparezcan mensajes similares a los siguientes.
3. En el cuadro **Steer active session while Copilot is working**, añade este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro para guiar la sesión activa mientras trabaja Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora a su flujo de trabajo.

## Deja que Copilot complete la tarea

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva petición a su flujo después de terminar el paso en el que estaba trabajando cuando enviaste el mensaje.

La tarea tardará varios minutos. Aprovecha el tiempo para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y guiar las tareas del agente en la nube de GitHub Copilot. Puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para ajustar el alcance o proporcionar más indicaciones.

Has aprendido:

- a usar Mission control y la página de agentes para supervisar las tareas del agente en la nube.
- a redirigir una sesión en curso para solicitar una función adicional.

Mientras Copilot completa las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
