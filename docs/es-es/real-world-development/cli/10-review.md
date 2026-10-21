---
title: "Lección 10 - Resumen y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo con Tailspin Toys. En concreto:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y encontraste la incidencia de filtrado inicial.
- añadiste las valoraciones por estrellas, revisaste el resultado en un navegador reenviado y combinaste manualmente tu primera solicitud de incorporación de cambios (PR).
- partiste de la incidencia de filtrado, definiste el enfoque en el modo Plan, implementaste el cambio en el modo Autopilot y lo revisaste en el modo Interactive.
- guiaste al agente con instrucciones personalizadas; después, personalizaste la habilidad existente `quality-checks` y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadiste el servidor Model Context Protocol (MCP) de Playwright y lo usaste para explorar el filtrado en un navegador real.
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- revisaste todos los cambios del filtrado y autorizaste Agent Merge para la PR.
- exploraste comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Lo que has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizado:

1. **Valoraciones por estrellas**: muestra `starRating` y un estado explícito para los juegos sin valoración en las tarjetas.
2. **Filtrado y flujo de trabajo de calidad**: implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, usaste la misma conversación y la misma rama. Para agilizar el taller, reunimos todo ese trabajo en una sola PR.

## Diferentes tipos de verificación

Comprobaste el código de varias formas: con pruebas automatizadas, mediante tu propia inspección en el navegador y con la exploración del navegador mediante MCP de Copilot. La habilidad `quality-checks` ejecutó las pruebas unitarias, lint y las comprobaciones de tipos, y las comunicó con el nuevo formato. Antes de la PR, QA reunió esos resultados con una revisión de los requisitos y la cobertura de pruebas.

Las pruebas añadidas deben cubrir carencias reales; una ejecución de QA que no requiera pruebas nuevas también puede ser correcta. Revisa el código y las pruebas antes de autorizar la combinación y actualiza las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones fijan los estándares; las habilidades describen tareas repetibles; los agentes personalizados definen roles especializados; y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para definir el enfoque antes de empezar, **Interactive** para participar en cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo rápido para cambios habituales y otro más potente para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más opciones para explorar

Ya has visto el flujo de trabajo principal. Estas son algunas funciones más de CLI que puedes probar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Próximos pasos

La mejor forma de aprender a usar una herramienta es seguir practicando. Úsala en código de producción, en tus proyectos personales o para crear esa pequeña aplicación que llevas años imaginando. Comparte lo aprendido con el equipo y aprende de los demás. Y, como siempre, consulta la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para explorar otros entornos, consulta el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Información sobre GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
