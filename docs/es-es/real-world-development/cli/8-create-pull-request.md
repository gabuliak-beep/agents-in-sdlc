---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Tú combinaste la solicitud de incorporación de cambios (PR) de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería separar la funcionalidad, las actualizaciones de instrucciones, la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una misma conversación y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de incorporación de cambios.
- inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás la PR del filtrado.
- habilitarás Agent Merge solo después de revisar el cambio y confirmarás que la PR se ha combinado.

## Situación

Durante el flujo de trabajo del filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y, al mismo tiempo, mantener el control de la autorización para combinarla en manos del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot se ocupa de los bloqueos de la PR: corrige los errores en las comprobaciones de integración continua (CI), responde a los comentarios de revisión y actualiza la rama si es necesario. Después, habilita la combinación automática de GitHub cuando el repositorio lo permite.

Hasta ahora, tú mismo has seleccionado **Merge pull request**. Agent Merge puede hacerse cargo de ese paso. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Deja que Agent Merge gestione la PR

Con todo el código preparado, revísalo, crea la PR y deja que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR. Cuando todo esté correcto, la combinará.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, las protecciones de rama, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, revisa el motivo indicado y completa manualmente la combinación revisada si el repositorio lo permite.

## Resumen y próximos pasos

Has automatizado varias etapas del desarrollo: generar código, probarlo y validarlo, y ahora gestionar la solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de incorporación de cambios.
- inspeccionaste el cambio completo de la funcionalidad y las pruebas de verificación.
- creaste la PR del filtrado.
- habilitaste Agent Merge solo después de revisar el cambio y confirmaste que la PR se había combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a un agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
