---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos por añadir una funcionalidad pequeña al proyecto. Los cambios mayores requieren un proceso más sólido. GitHub Copilot CLI está diseñado para trabajar con los flujos de trabajo existentes de una organización y ayudarnos a crear lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual basado en agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que funciona como esperas y, finalmente, lo combinarás en el proyecto.

> [!NOTE]
> Usarás la misma conversación y la misma rama durante todo el flujo de trabajo de esta funcionalidad. Normalmente se usarían ramas o PR distintas para los diferentes tipos de archivos, pero seguiremos un camino más corto para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación nueva de Copilot a partir de una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad en el modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en un navegador reenviado.

Durante el desarrollo de esta funcionalidad actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la PR.

## Situación

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero hay que acordar detalles, como la posibilidad de combinar categorías, antes de escribir código. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia sus fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Parten de una incidencia registrada que describe el trabajo necesario.
2. Planifican lo que hay que desarrollar.
3. Crean y revisan el código.
4. Ejecutan las pruebas para validar el código.
5. Validan manualmente la nueva funcionalidad.
6. Crean una solicitud de incorporación de cambios (PR).
7. Cuando se ha revisado el código y se ha superado la integración continua, combinan el código.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría siguen alguna variante de este proceso.

Si sigues este enfoque estándar, te aseguras de que el código generado por IA cumple los requisitos y pasa por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina cuánta autonomía tiene el agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: tú y el agente trabajáis juntos. El agente propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta que considera que la tarea está terminada.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar el trabajo de filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que la PR de las valoraciones por estrellas se ha combinado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o un número parecido). El símbolo `#` permite añadir al contexto una incidencia o solicitud de incorporación de cambios (PR) de GitHub.

## Planifica la funcionalidad de filtrado

La planificación permite definir el enfoque para implementar una funcionalidad o realizar una tarea antes de asignársela a Copilot. Conviene dedicar tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador situado debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empieza a trabajar en el plan. Primero explora el proyecto y, a continuación, determina el mejor enfoque.

3. Es posible que Copilot te haga preguntas para saber cómo debe funcionar el filtrado. Respóndelas según tus preferencias: no hay una única respuesta correcta.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que revise cualquier parte que no coincida con tus decisiones.

## Aprueba Autopilot

Una vez redactado y revisado el plan, es el momento de implementarlo. Deja que Copilot trabaje con el modo Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)**, o una opción con un nombre similar en la versión instalada.
2. Confirma que el indicador situado debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot itera según el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa primero el plan. Si Copilot informa de dependencias que faltan o de un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisa y verifica la implementación

Una vez generado el código, hay que revisarlo antes de combinarlo, como cualquier otro código. Revisaremos el código y ejecutaremos el sitio para comprobar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador del modo ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y con las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los fallos.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio desde el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, el filtrado por editorial y las combinaciones que hayas acordado en el plan.
4. Confirma que el reinicio de los filtros y el comportamiento cuando no hay resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y próximos pasos

Has usado varios modos de conversación para crear y revisar una funcionalidad. En esta lección:

- iniciaste una conversación nueva de Copilot a partir de una incidencia de GitHub.
- definiste los requisitos en el modo Plan.
- implementaste la funcionalidad en el modo Autopilot.
- revisaste el código.
- validaste manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en la generación de código para asegurarte de que sigue las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
