# 0004 · Entornos dev y prod con flavors

Fecha: 2026-10-19 · Estado: aceptada

## Contexto
El desarrollo usa datos de prueba con `validadoPor: null`; la prueba con usuarios debe ver solo contenido validado por la comunidad sorda y no mezclarse con esos datos.

## Decisión
Dos proyectos Firebase (`ensenas-dev`, `ensenas-prod`), dos puntos de entrada (`main_dev.dart`, `main_prod.dart`) y dos flavors de Android (`dev` con sufijo `.dev`, `prod`).

## Alternativas
Un solo proyecto con una bandera: los datos de prueba quedarían junto a los de los participantes. Solo `--dart-define` sin flavors: no permite tener las dos apps instaladas ni un `google-services.json` por entorno.

## Consecuencias
Se puede probar la versión de desarrollo y la de la prueba en el mismo teléfono. Cada comando de ejecución y compilación debe indicar el flavor y el punto de entrada.
