# EnSeñas

Aplicación móvil con dinámicas de juego para que personas oyentes aprendan Lengua de Señas Colombiana (LSC) básica, co-creada y validada con la comunidad sorda. Prototipo de tesis del proyecto I LOVE LSC.

**Estado:** sprint 0, configuración del repositorio. El proyecto Flutter se crea en el issue "S0 · Crear el proyecto Flutter".

## Stack

Flutter · Riverpod · go_router · Firebase (Auth, Firestore, Storage, Analytics) · video_player + chewie · freezed · flutter_local_notifications

## Empezar

Sigue [docs/arranque.md](docs/arranque.md). Para ejecutar, una vez creado el proyecto:

```
flutter pub get
dart run build_runner build --delete-conflicting-outputs
flutter run --flavor dev -t lib/main_dev.dart
```

## Documentación

| Documento | Contenido |
|---|---|
| [CLAUDE.md](CLAUDE.md) | Alcance, reglas y convenciones; lo lee Claude Code en cada sesión |
| [docs/plan.md](docs/plan.md) | Sprints y fechas |
| [docs/flujo-de-trabajo.md](docs/flujo-de-trabajo.md) | Cómo pasa una tarea de issue a main |
| [docs/arquitectura.md](docs/arquitectura.md) | Capas, features, navegación, entornos y pruebas |
| [docs/modelo-datos.md](docs/modelo-datos.md) | Firestore, reglas de negocio y seguridad |
| [docs/decisiones/](docs/decisiones/) | Registro de decisiones técnicas |
| [CHANGELOG.md](CHANGELOG.md) | Cambios por versión |

## Cómo se trabaja

Cada tarea es un issue dentro del milestone de su sprint. Claude Code la implementa en una rama propia (en VS Code o comentando `@claude` en el issue), el CI verifica formato, análisis y pruebas, y el PR se fusiona con squash. `main` está protegida.

## Licencia y aviso

El código es MIT ([LICENSE](LICENSE)). Los videos, las señas y la identidad de EnSeñas e I LOVE LSC tienen todos los derechos reservados ([CONTENT-LICENSE.md](CONTENT-LICENSE.md)).

Contenido de educación informal; no conduce a título ni certificado de aptitud ocupacional (Decreto 1075 de 2015, art. 2.6.6.8).
