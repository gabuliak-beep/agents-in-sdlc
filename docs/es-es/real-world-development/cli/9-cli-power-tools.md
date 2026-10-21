---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar trabajo opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como otras herramientas de CLI, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Estos comandos permiten acceder a funciones avanzadas, información interna y otras opciones de configuración. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás algunos más.

En esta lección:

- usarás `/context` y `/compact` para saber cómo gestiona Copilot el contexto de la conversación.
- usarás `/model` para explorar los modelos disponibles.
- aprenderás a usar `/share` para exportar o compartir una sesión.
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

## Situación

Has terminado el flujo de trabajo principal de CLI. Ahora explorarás otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar trabajo de forma opcional al [agente en la nube de Copilot][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puedes alcanzar el límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta, y puedes inspeccionar o compactar el contexto manualmente con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio, si todavía no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya era corta, es posible que no notes una gran diferencia.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente cuando la ventana se llena. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` para empezar una conversación nueva cuando pases a una tarea sin relación.

## Elige un modelo

Cada modelo tiene sus puntos fuertes, y cada desarrollador puede tener preferencias distintas. Copilot CLI permite consultar los modelos disponibles y elegir uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Colaborar en equipo y compartir lo aprendido ayuda a todos a sacar más partido a las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y consulta las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige el destino que te convenga; por ejemplo, `/share file` para exportarla a un archivo Markdown local.
3. Revisa el contenido exportado antes de enviárselo a alguien o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que tu equipo no quiera compartir.

## Opcional: ampliar o delegar el trabajo

El taller principal ha terminado. Copilot CLI también ofrece comandos para tareas más grandes:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o iniciar acciones remotas. Antes de probarlos, define una tarea nueva y bien delimitada y revisa el resultado con tu flujo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y próximos pasos

Los comandos de barra de Copilot CLI te permiten configurar la herramienta, compartir sesiones y consultar lo que ocurre internamente. En esta lección:

- usaste `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usaste `/model` para explorar los modelos disponibles.
- aprendiste a usar `/share` para exportar o compartir una sesión.
- exploraste comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Hay muchos más comandos de barra que puedes explorar. Vamos a cerrar el recorrido [repasando lo aprendido][next-lesson] y los próximos pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Información sobre el agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
