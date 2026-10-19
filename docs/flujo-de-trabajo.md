# Flujo de trabajo

Tres participantes, cada uno con un papel fijo.

| Quién | Qué hace |
|---|---|
| Chat web del proyecto (claude.ai) | Planea los sprints, convierte tareas en issues o prompts, revisa PR, diagnostica errores y lleva el estado del proyecto |
| Claude Code (VS Code o GitHub Action) | Implementa, prueba y abre PR siguiendo CLAUDE.md |
| Artur | Decide, prueba en el emulador, fusiona y hace las tareas manuales |

## Una tarea, de principio a fin

1. **Elegir.** La siguiente tarea es el primer issue abierto del milestone del sprint actual. Las de etiqueta `claude` las puede hacer Claude solo; las de `manual` las haces tú.
2. **Lanzar.**
   - Modo autónomo: comenta en el issue `@claude implementa este issue siguiendo CLAUDE.md`. Solo funciona con comentarios tuyos, y corre uno a la vez.
   - Modo VS Code: abre el repo, en Claude Code escribe `Implementa el issue #N siguiendo CLAUDE.md`. Claude propone un plan y te pide confirmación antes de cada commit y cada push.
3. **Revisar.** Cuando haya PR: mira que el CI esté en verde, pruébalo en el emulador con la configuración "EnSeñas (dev)" y pega el enlace o el Resumen de sesión en el chat web para la revisión. Para pedir cambios, comenta `@claude <cambio>` en el PR.
4. **Fusionar.** Botón "Squash and merge". El título del PR queda como el commit en main y el issue se cierra solo con `Closes #N`. La rama se borra automáticamente.
5. **Cerrar.** Pega el Resumen de sesión en el chat web para actualizar el estado y recibir la siguiente tarea.

## Reglas del repositorio

- `main` está protegida: solo entra por PR con el check `verificar` del CI en verde, sin push directo ni force push, ni siquiera tuyo.
- Fusión solo por squash: un commit por tarea en main, con título en Conventional Commits.
- Dependabot abre PR semanales de dependencias; se revisan como cualquier otro.
- Al cerrar cada sprint: etiqueta `v0.N.0` y mueve la sección "Sin publicar" del CHANGELOG a esa versión.

## Ajustes personales

Lo que es solo tuyo no se sube: `.claude/settings.local.json` (permisos extra de Claude Code en tu computador) y `CLAUDE.local.md` (notas para Claude que no aplican al proyecto) están en .gitignore. La configuración compartida vive en `.claude/settings.json` y `CLAUDE.md`.

## Evidencia para la tesis

Cada PR fusionado es evidencia del capítulo tecnológico: qué se hizo, con qué pruebas y qué decisión lo justificó (`docs/decisiones/`). Revisar cada PR no es opcional: en la sustentación tienes que poder explicar el código como tuyo.
