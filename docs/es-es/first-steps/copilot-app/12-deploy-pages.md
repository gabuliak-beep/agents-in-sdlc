---
title: "Lección 12 - Desplegar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y verificar que Space Quiz está publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dale a Space Quiz un sitio público desplegándolo con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiar el despliegue, pero debes revisar los cambios y confirmar que el sitio está publicado.

En esta lección:

- pedirás a Copilot que configure el despliegue de GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- seguirás el despliegue y verificarás el sitio publicado.

## Situación

Space Quiz ya está en un repositorio de GitHub, pero la gente todavía necesita una forma sencilla de probarlo. Al publicar el sitio de un solo archivo con GitHub Pages, estará disponible en una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que despliegue el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres etapas: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de GitHub Actions «Deploy website» con los pasos «Build website», «Upload artifact» y «Deploy to Pages»; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y combine una solicitud de incorporación de cambios y cambie la configuración de Pages del repositorio. Revisa las diferencias, los permisos de despliegue y las solicitudes de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y verifica el despliegue

1. Confirma que el archivo README solo se añade si el repositorio todavía no tiene uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos de compilación y despliegue del flujo de trabajo.
3. Combina los cambios solo después de que se superen las comprobaciones obligatorias de la solicitud.
4. Sigue la ejecución del despliegue de GitHub Actions hasta que termine correctamente.
5. Abre la URL indicada y confirma que Space Quiz se carga.

Si el agente no puede habilitar Pages o combinar la solicitud de incorporación de cambios, sigue las instrucciones específicas que te proporcione. No des por terminada la tarea hasta que el despliegue haya finalizado y el sitio publicado se cargue correctamente.

## Resumen y próximos pasos

Space Quiz ya está disponible como sitio publicado y has verificado el despliegue en vez de confiar únicamente en el informe del agente. Continúa con la [lección 13: Revisión y próximos pasos][next-lesson].

[next-lesson]: ../13-review/
