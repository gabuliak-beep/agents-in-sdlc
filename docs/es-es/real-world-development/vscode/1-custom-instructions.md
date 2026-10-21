---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta —o Copilot necesita conocer información previa—, asegúrate de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] permiten compartir indicaciones sobre la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos limitados a rutas.
- generarás una primera parte de la capa de datos para el filtrado (un helper de editoriales) con las instrucciones actuales.
- añadirás un nuevo estándar global a `.github/copilot-instructions.md`.
- repetirás el mismo prompt y observarás cómo el código generado adopta el nuevo estándar.
- confirmarás en `main` la actualización de instrucciones y la parte del filtrado, para que Copilot pueda usar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares definidos porque Copilot es probabilístico. El objetivo es observar la *tendencia* del comportamiento después de actualizar las instrucciones, no obtener un resultado idéntico carácter por carácter.

## Archivos de instrucciones

### Situación

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones te asegurarás de que Copilot disponga del contexto adecuado para realizar las tareas según estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Permiten orientar las sugerencias y los fragmentos de código, especificar convenciones de programación y bibliotecas, y definir qué tipo de comentarios prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivo o tareas concretas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información global del proyecto, como la pila tecnológica, una descripción del trabajo, las prácticas recomendadas y otras directrices generales.
- `.github/instructions/*.instructions.md`: archivos para tareas o tipos de archivo concretos. Puedes usarlos para establecer directrices para lenguajes como TypeScript o Astro y para tareas como crear componentes de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni sugerir ediciones siguientes.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo`.
>
> Copilot también [admite archivos de instrucciones de otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

No podemos abordar en este taller todos los detalles sobre cómo crear archivos de instrucciones. Los ejemplos del proyecto muestran un enfoque representativo:

- Limita `copilot-instructions.md` a directrices globales, como una descripción del proyecto, su estructura y los estándares generales de código.
- Usa archivos `*.instructions.md` para proporcionar instrucciones específicas de tipos de archivo —pruebas unitarias, componentes Astro o la capa de datos— o de tareas concretas.
- Usa un lenguaje natural y claro. Incluye ejemplos de cómo debe y no debe ser el código.

No hay una única forma correcta de crear archivos de instrucciones, igual que no hay una única forma de usar IA. Experimenta para descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todos los proyectos que usen GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. En este proyecto hay archivos para varios tipos de tareas, como las [actualizaciones de interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada interfaz lo hace de forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente te indicará cómo usar esta función.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Revisa la breve descripción del proyecto y las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a todas las interacciones con Copilot.
3. Explora `.github/instructions`. Hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` establece un patrón glob relativo a la raíz del repositorio. Por ejemplo, coincide con archivos de pruebas TypeScript que cumplen el patrón `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos del proyecto. Así puedes dividir conjuntos grandes de directrices en archivos pequeños y reutilizables y señalar ejemplos que Copilot puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación, pero todavía no exige documentar el código. En los siguientes pasos añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo pueden variar según el ejercicio; cada paso indica cuál debes usar.

Primero envía un prompt con el archivo de instrucciones actual. Después de actualizarlo, volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas de los ejercicios anteriores para que Copilot solo use el contexto necesario.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe estar el helper.
3. Selecciona **Agent** en el selector de agentes de la vista Chat para que Copilot pueda aplicar cambios a archivos.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios de código, a menudo en el archivo helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto sigue las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios aunque no se le hayan pedido. No pasa nada: el objetivo es observar la mejora de uniformidad al actualizar las instrucciones.

## Añade un estándar para todo el repositorio

`.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade una directriz para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Documenta los estándares de programación, pero todavía no incluye directrices para documentar el código; por eso el helper generado no tiene comentarios.
3. Añade estas líneas debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda el archivo.

> [!TIP]
> Como has visto en la lección anterior, puedes crear instrucciones globales en `.github/copilot-instructions.md` o usar archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para una norma global como la de comentarios de documentación.

## Repite el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el **mismo prompt**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto empieza ahora con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar y lo aplique a código real sobre el que trabajarás en el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones se gestionan como cualquier otro archivo del repositorio. Confirma los cambios y publica la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que las instrucciones y el helper están modificados:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot para crear el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Una vez publicada la rama, crea una solicitud de incorporación de cambios y vincúlala a la incidencia del backlog sobre los estándares de documentación. Copilot puede buscar la incidencia, crear la PR para cerrarla y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub al espacio de trabajo, así que no tienes que configurar nada. VS Code quizá te pida iniciar sesión la primera vez que Copilot las use. Sigue los pasos para permitirlo. En un ejercicio posterior verás cómo funciona esta conexión con Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia relacionada con las instrucciones y cree una PR desde la rama actual, describiendo tanto las instrucciones como el helper de editoriales:

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot solicite permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se cree la PR, pídele que la combine en `main` y vuelva a actualizar la rama local:

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot pide permiso para realizar acciones de GitHub o ejecutar comandos de shell, revisa el comando y selecciona **Allow** si corresponde.

¡Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un estándar global a `.github/copilot-instructions.md`.
- repetir el prompt y observar cómo el código propuesto adopta el nuevo estándar.
- confirmar las instrucciones y el helper en `main` para continuar el trabajo en el ejercicio siguiente.

A continuación, usarás las instrucciones en el [modo de agente][next-lesson] para añadir una nueva función al código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
