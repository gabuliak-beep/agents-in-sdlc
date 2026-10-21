---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta —o Copilot necesita conocer información previa—, asegúrate de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esas directrices para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones limitados a rutas.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no harás una comparación de prompts «antes y después». El agente en la nube de Copilot trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar su impacto en tiempo real. Verás cómo influyen las instrucciones más adelante, al revisar las solicitudes de incorporación de cambios que genere el agente.

## Archivos de instrucciones

### Situación

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones te asegurarás de que Copilot disponga del contexto adecuado para realizar las tareas según estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Te permiten orientar sus sugerencias y fragmentos de código, especificar convenciones, bibliotecas y hasta el tipo de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos y tareas específicas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información global del proyecto, como la pila tecnológica, una descripción de lo que se está desarrollando, prácticas recomendadas y otras directrices generales.
- `.github/instructions/*.instructions.md`: archivos que puedes crear para tareas o tipos de archivo concretos. Permiten especificar directrices para lenguajes como TypeScript o Astro, así como para tareas como crear componentes de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni sugerir ediciones siguientes.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo`.
>
> Copilot también [admite archivos de instrucciones de otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

No podemos abordar en este taller todos los detalles sobre cómo crear archivos de instrucciones. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo:

- Limita `copilot-instructions.md` a directrices generales del proyecto, como su descripción, estructura y estándares de programación globales.
- Usa archivos `*.instructions.md` para proporcionar instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro y capa de datos) o tareas concretas.
- Usa un lenguaje natural y claro. Incluye ejemplos de cómo debe y no debe ser el código.

No hay una única forma correcta de crear archivos de instrucciones, igual que no hay una única forma de usar IA. Con la experimentación descubrirás qué funciona mejor para cada proyecto.

> [!TIP]
> Todos los proyectos que usen GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, quizá veas que cubren muchos tipos de tareas, como las [actualizaciones de interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada interfaz ofrece esta función de forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente te indicará cómo usarla.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Revisa la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a todas las interacciones con Copilot.
3. Explora la carpeta `.github/instructions`. Incluye instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo`: especifica un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos de prueba TypeScript, como los que siguen el patrón `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes de instrucciones en archivos más pequeños y reutilizables y señalar ejemplos que Copilot puede seguir. Las rutas de esos enlaces son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentar el código. En los siguientes pasos añadirás reglas para incluir comentarios TSDoc y encabezados de archivo.

## Añade un estándar para todo el repositorio

El siguiente paso es la única edición que harás en este ejercicio: añadir una regla para documentar el código con comentarios TSDoc y un encabezado de archivo. El agente en la nube la aplicará al trabajar en las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero así podrás confirmar los cambios de forma ordenada:

1. Desde el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como ya hemos explicado, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade el estándar de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares actuales, pero todavía no exige documentar el código, por eso el helper generado no tiene comentarios de documentación.
3. Añade estas líneas de Markdown debajo de los estándares existentes para pedir a Copilot que incluya encabezados de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en `.github/copilot-instructions.md` para proporcionar directrices globales, o usar archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para normas generales, como la regla de comentarios de documentación que acabas de añadir.

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama a la que apunta la incidencia. Cuando le asignes una incidencia en el siguiente ejercicio, Copilot creará una rama a partir de `main`; por eso, la actualización debe llegar a `main` para que el agente pueda usarla.

1. Prepara los cambios y crea un commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. El agente en la nube usará estas instrucciones en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir la rama y crear el commit directamente en `main`. Este paso con una rama permite practicar el flujo que seguirías en un proyecto real.

## Resumen y próximos pasos

Has explorado cómo obtiene Copilot el contexto de los archivos de instrucciones del proyecto y has añadido un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`. Verás cómo se aplica cuando el agente en la nube genere solicitudes de incorporación de cambios en los siguientes ejercicios.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
