---
title: "Lección 7 - Crear y usar un agente de QA"
description: "Crea un agente personalizado de QA que reúna los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en el navegador. Ahora reunirás estas funciones en un agente personalizado con un proceso de QA claramente definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crearás e inspeccionarás un perfil reutilizable de control de calidad.
- seleccionarás el agente de QA y revisarás sus conclusiones según la incidencia de filtrado.

## Situación

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma uniforme los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar este proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot, definida en un perfil Markdown. El perfil describe el propósito del agente, sus instrucciones y las herramientas disponibles. En este taller definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has usado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` reúne las comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de QA indica a Copilot cómo usar esas funciones para evaluar los requisitos y comunicar las conclusiones. No sustituye las herramientas ni requiere otra conversación.

## Crea el perfil de QA

Antes de abrir la PR de la funcionalidad, pedirás a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones y los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en el modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de usar el agente, revisa su perfil para confirmar que Copilot ha recogido el flujo de QA y los límites deseados. Así evitarás que un agente incompleto o demasiado general modifique la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee los metadatos de la parte superior. `description` es obligatorio; `name` es opcional, pero permite mostrar un nombre claro al agente.
3. Lee las instrucciones del perfil y confirma que QA parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y usa Playwright MCP.
4. Confirma que QA aporta pruebas para sus conclusiones, pide permiso antes de cambiar el código de implementación y no crea commits ni abre solicitudes de incorporación de cambios.
5. Si al perfil le falta alguna responsabilidad o límite, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta QA en la incidencia

Copilot CLI carga los agentes del proyecto cuando empieza una conversación. Reanuda la conversación de filtrado después de crear el perfil y selecciona QA para que pueda usar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, puede que no aparezca en la lista. Aun así, existe y puedes activarlo con el comando anterior.

2. Pide al agente de QA que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de QA empezará a trabajar.
4. Lee el informe cuando termine.

## Resumen y próximos pasos

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado sus resultados. En esta lección:

- exploraste cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- creaste e inspeccionaste un perfil reutilizable de control de calidad.
- seleccionaste el agente de QA y revisaste sus conclusiones según la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación listos para revisar. A continuación, [reunirás todo en una PR de funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
