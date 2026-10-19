# Arquitectura

EnSeñas es una app Flutter organizada por features. Cada feature tiene tres capas y las dependencias solo apuntan hacia adentro: presentation depende de domain, data implementa domain, y domain no depende de nadie.

```
presentation  ──usa──▶  domain  ◀──implementa──  data  ──habla con──▶  Firebase
(pantallas,             (entidades, reglas,       (repositorios,
 providers)              interfaces de repo)       DTOs, fuentes)
```

## Capas

**domain/** contiene entidades inmutables (freezed), reglas de negocio puras e interfaces de repositorio. No importa Flutter ni Firebase, así que se prueba con `flutter test` sin emulador. Las reglas que dependen de la fecha (racha, recuperación de vidas) reciben un reloj inyectado y trabajan en la zona America/Bogota.

**data/** implementa los repositorios contra Firebase. Los DTOs (json_serializable) se convierten a entidades en el borde y las excepciones de Firebase se traducen a fallos de dominio, para que la UI nunca vea un `FirebaseException`.

**presentation/** contiene pantallas, widgets y providers generados con riverpod_generator. Los providers exponen repositorios y casos de uso; las pruebas de widget los reemplazan con overrides y dobles de mocktail.

## Features

| Feature | Responsabilidad |
|---|---|
| auth | Registro, inicio y cierre de sesión (correo y Google), creación de `users/{uid}` |
| onboarding | Tres pantallas de bienvenida, se muestran una sola vez |
| map | Mapa vertical de unidades y lecciones con estado bloqueada / disponible / completada |
| lesson | Motor de lecciones, interfaz `Exercise`, los tres ejercicios, reproductor de señas, resultado |
| progress | Reglas de XP, racha y vidas; guardado de progreso; lección de repaso; recordatorio diario |
| profile | Perfil, ajustes, créditos de la comunidad sorda |

Lo compartido va en `core/` (widgets comunes, manejo de errores, utilidades) y en `app/` (tema, router, entorno).

## Navegación

go_router con redirección según la sesión: sin sesión va a onboarding o login; con sesión, al mapa. Rutas principales: `/onboarding`, `/login`, `/registro`, `/mapa`, `/leccion/:lessonId`, `/resultado/:lessonId`, `/repasar`, `/perfil`.

## Entornos

Dos puntos de entrada, `lib/main_dev.dart` y `lib/main_prod.dart`, que inicializan Firebase con `firebase_options_dev.dart` o `firebase_options_prod.dart` y un `AppConfig` con el entorno. En Android cada entorno es un flavor (`dev` con id `co.ensenas.ensenas.dev`, `prod` con `co.ensenas.ensenas`), así ambas apps conviven en el mismo teléfono. Solo en dev se muestra contenido con `validadoPor: null`. Ver `docs/decisiones/0004-entornos-dev-prod.md`.

## Pruebas

- Unitarias: reglas de domain y conversión DTO ↔ entidad.
- De widget: cada pantalla, con repositorios simulados.
- De integración (sprint 4): el flujo de una lección completa en el emulador.
- Reglas de Firebase: se prueban con el emulador cuando cambian (`firebase emulators:start`).

La meta no es un porcentaje de cobertura sino que toda regla de negocio tenga pruebas; el CI guarda el reporte de cobertura como artefacto.
