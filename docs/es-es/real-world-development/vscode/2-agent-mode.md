---
title: "Ejercicio 2 - Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos en una aplicación suelen requerir modificar varios archivos y ejecutar operaciones como las pruebas. Normalmente, el desarrollador localiza todos los archivos necesarios, hace los cambios, ejecuta las pruebas, depura los errores, descubre qué archivo falta y vuelve a modificarlo. Y así sucesivamente.

El modo de agente de Copilot ayuda con este trabajo.

El modo de agente puede trabajar de forma más autónoma en el IDE. Explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Veamos cómo usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE, con interacción y ciclos de respuesta rápidos. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- usar el modo de agente para explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- implementar funciones en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados antes de combinarlos en el código.

## Situación

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior añadiste un helper para las editoriales; ahora completarás el resto de la capa de datos, la interfaz y las pruebas con el modo de agente.

## Ejecuta el sitio web de Tailspin Toys

Antes de modificar nada, explora el sitio para conocer sus funciones actuales.

Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite consultar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan una base de datos SQLite local con Drizzle ORM en los metadatos de las páginas: no hay una API de backend separada ni un framework de interfaz ejecutado en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`; la interactividad usa pequeños elementos `<script>` de Astro con las API estándar del DOM.

### Inicia el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro. En el Codespace:

1. Vuelve a tu Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, verás un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir un navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace `localhost` en el terminal se te redirige a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en ejecución, prueba sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, verás una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios, vuelve al codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo pueden cambiar de un ejercicio a otro; cada paso indica cuál debes usar.

La primera versión del sitio funciona, pero queremos añadir funciones. Empieza por revisar el backlog, que se creó automáticamente a partir de la plantilla.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El modo **Agent** planifica e implementa cambios en varios archivos, ejecuta comandos del terminal e invoca herramientas.
2. Selecciona **Claude Sonnet 4.5** entre los modelos disponibles.

> [!CAUTION]
> Los autores no recomiendan un modelo por encima de otro. Usamos Claude Sonnet 4.5 para crear este taller con la esperanza de obtener sugerencias relativamente uniformes. Como los LLM son probabilísticos, los resultados pueden diferir de los ejemplos; es normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede usar un comando MCP distinto y completar igualmente la tarea.

3. Envía este prompt para consultar el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que muestra las incidencias.
5. Revisa la lista generada. Copilot también las habrá priorizado según su utilidad para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo que dará contexto a Copilot: `.github/instructions/ui.instructions.md`, que explica cómo añadir funciones al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices sobre la arquitectura, el diseño de componentes, las pruebas y la accesibilidad. También incluye enlaces a instrucciones para tipos de archivo como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones, lo que permite reutilizar contenido y enlazar ejemplos.

## Implementa el filtrado

Para completar el filtrado hay que hacer al menos tres cambios:

- añadir o ajustar la lógica de filtrado en `src/lib/`.
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de la lista de juegos para incorporar la interfaz de filtrado.

Las pruebas deben superarse antes de combinar los cambios. El modo de agente puede realizar estas tareas.

1. Continúa la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.
3. Confirma que **Agent** sigue seleccionado y que el modelo es **Claude Sonnet 4.5**.
4. Pide a Copilot que implemente el filtrado a partir de la incidencia:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. Observa cómo Copilot explora el proyecto, localiza los helpers de la capa de datos, la interfaz y las pruebas, modifica archivos y ejecuta las pruebas.

> [!NOTE]
> Es posible que Copilot tarde varios minutos en explorar, modificar archivos y ejecutar pruebas. Mientras trabaja, el editor puede mostrar las opciones **Keep** y **Undo**. No tienes que seleccionarlas hasta que termine.

6. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.
7. Puede haber pausas e incluso pruebas fallidas. Copilot itera entre la generación de código y las pruebas hasta que termina y no detecta errores.
8. Inspecciona el código generado para buscar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

9. Vuelve al navegador donde se ejecuta el sitio y prueba la nueva función.
10. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y próximos pasos

En este ejercicio has usado el modo de agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has aprendido a:

- implementar funciones en la interfaz y la capa de datos.
- explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- revisar los cambios y las pruebas generados antes de combinarlos.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración opcional: implementar paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Usa lo aprendido para pedir a Copilot que implemente esta función:

- sigue las prácticas recomendadas y usa los archivos de instrucciones existentes.
- decide si el usuario podrá elegir el tamaño de página o si será fijo.
- proporciona instrucciones claras para obtener la implementación deseada.
- itera con Copilot, pidiendo cambios y aportando contexto. Es normal trabajar así con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
