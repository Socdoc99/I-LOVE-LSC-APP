# 0002 · Roles del chat web, Claude Code y Artur

Fecha: 2026-10-18 · Estado: aceptada

## Contexto
El código lo escribe Claude Code, en VS Code o como GitHub Action, y la planeación ocurre en el chat web del proyecto. Sin reglas claras, el agente autónomo se queda esperando aprobaciones que nadie da, o toca cosas fuera de su tarea.

## Decisión
El chat web planea, especifica y revisa; Claude Code implementa, prueba y abre PR; Artur decide y fusiona. En VS Code Claude pide confirmación antes de cada commit; en modo autónomo escribe su plan en el PR y continúa. El puente entre ambos es el "Resumen de sesión" de CLAUDE.md. main está protegida y solo acepta PR con el CI en verde.

## Alternativas
Trabajar solo en VS Code: más lento y sin aprovechar las horas en que Artur no está en el computador. Trabajar solo con el Action: Firebase, dispositivos y la comunidad necesitan a Artur.

## Consecuencias
Cada tarea deja un PR con plan, pruebas y resumen, que sirve como evidencia para la tesis. El Action solo responde a comentarios del dueño del repo, para que nadie más gaste el plan de Claude.
