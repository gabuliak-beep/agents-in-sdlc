---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara el entorno. Crearás una copia del repositorio Tailspin Toys, iniciarás un [codespace][codespaces] y confirmarás que GitHub Copilot Chat funciona en el editor.

## Configura el repositorio del laboratorio

Para crear una copia del repositorio donde trabajarás, crearás una instancia a partir de la [plantilla][template-repository]. Incluirá todos los archivos necesarios para el laboratorio.

1. Abre una ventana nueva del navegador y ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea tu propia copia del repositorio seleccionando **Use this template** en la página del repositorio del laboratorio. Después, selecciona **Create a new repository**.

   ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

   ![Introducir la configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**); la necesitarás más adelante.

> [!NOTE]
> **El backlog ya está listo**
>
> Al crear el repositorio desde la plantilla, también se genera automáticamente un backlog de incidencias de GitHub. Trabajarás con ellas durante el taller; no hace falta que crees ninguna.

## Crea un codespace

A continuación, usarás un codespace para completar los ejercicios.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un IDE completo con compatibilidad para varios lenguajes, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

   ![Seleccionar el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y pulsa **+** para crear un codespace.

   ![Crear un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará varios minutos, aunque sigue siendo mucho más rápida que instalar manualmente todos los servicios. Mientras tanto, puedes explorar otras funciones de GitHub Copilot que veremos a continuación.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos entornos incluyen los requisitos previos necesarios. Si prefieres trabajar en local, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor que usa el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usa Copilot Chat y el modo de agente

Para acceder al modo de agente de GitHub Copilot Chat necesitas tener instalada la extensión GitHub Copilot Chat en el IDE. Si usas GitHub Codespaces, debería estar instalada.

> [!TIP]
> Si no tienes instalada la extensión, puedes [instalarla desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista Extensions de Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Después de instalar la extensión, quizá tengas que autenticarte con tu cuenta de GitHub.

1. Vuelve a tu codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del codespace.
3. Escribe un mensaje como «Hello world» en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>.
4. Si todavía no te has autenticado, se te pedirá que inicies sesión en GitHub. Sigue las instrucciones.

   ![Ejemplo del mensaje de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, aparecerá la ventana de Copilot Chat.

## Resumen

¡Enhorabuena! Has creado una copia del repositorio del laboratorio y has iniciado la creación del codespace que usarás para escribir código.

## Siguiente paso

Empecemos a trabajar con Copilot. Continúa con el [ejercicio 1: Instrucciones personalizadas][next-lesson], donde le enseñarás las convenciones del proyecto.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Introducción a Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
