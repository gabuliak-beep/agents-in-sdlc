---
title: "Lección 4 - Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta, conviene que Copilot disponga de esas indicaciones. Los [archivos de instrucciones][instruction-files] describen tanto el código que quieres como la forma en que debe estructurarse. Ya has creado el filtrado; ahora explorarás las instrucciones que ha usado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones limitadas a rutas.
- actualizarás el archivo de instrucciones para asegurar que se siguen los estándares de código.
- observarás el impacto de los archivos de instrucciones en el código.

## Situación

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no resulten obvias, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos con TSDoc/JSDoc, incluido un argumento `db` inyectable cuando corresponda.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices existentes de formato y lint.

Con los archivos de instrucciones te asegurarás de que Copilot disponga del contexto adecuado para llevar a cabo las tareas según estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de código y tus requisitos. Te permiten orientar las sugerencias y los fragmentos de código. Puedes crear un archivo de instrucciones para todo el repositorio o archivos para tipos de archivos o tareas específicas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto que sea relevante para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`: archivos con directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia sobre compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de basarte en un formato específico.

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Antes de modificar nada, explóralos para observar su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Revisa la breve descripción del proyecto y las directrices de código. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros temas.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: establece un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa los enlaces a otros archivos de instrucciones y a los archivos existentes del proyecto. Así puedes dividir conjuntos grandes de instrucciones en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot puede seguir.

## Actualiza las instrucciones para reflejar las directrices del equipo

Los archivos existentes son un buen comienzo, pero falta una directriz. Modifica el archivo principal `copilot-instructions.md` para exigir comentarios TSDoc en el TypeScript que se genere.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar alrededor de la línea 35.
2. Añade este elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio cuando comienza una conversación. Después de guardar las instrucciones, sal de la conversación de filtrado y reanúdala para que Copilot use las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que esté seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Comprueba que los nuevos comentarios TSDoc explican el código correctamente.

## Resumen y próximos pasos

Has explorado cómo Copilot CLI recoge el contexto de los archivos de instrucciones y has aplicado una nueva norma a la funcionalidad. En concreto:

- exploraste cómo llegan al agente las instrucciones del repositorio y las instrucciones limitadas a rutas.
- actualizaste el archivo de instrucciones para asegurar que se siguen los estándares de código.
- observaste el impacto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para que las comprobaciones de lint y las pruebas se ejecuten de forma uniforme.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
