# EnSeñas

App móvil Flutter con dinámicas de juego para que personas oyentes aprendan Lengua de Señas Colombiana (LSC) básica. Es el prototipo de una tesis: debe poder validarse con usuarios reales en 10 semanas. Todo el contenido de LSC es co-creado y validado por personas sordas.

Documentación de apoyo (léela cuando la tarea la toque): `docs/arquitectura.md`, `docs/modelo-datos.md`, `docs/flujo-de-trabajo.md`, `docs/decisiones/`.

## Alcance del prototipo
- Dentro: registro/inicio de sesión, onboarding, mapa de unidades tipo Duolingo, 3 unidades (~25 señas en video), 3 ejercicios (Ver y elegir, Palabra a seña, Parejas), XP, racha diaria, vidas, lección de repaso, perfil con créditos, recordatorio diario.
- Fuera (no implementar sin que se pida): diccionario, logros, dactilología, imitación con cámara, reconocimiento por IA, pagos, tutorías, certificación.
- Si una tarea toca algo fuera de alcance, detente y pregunta.

## Stack
- Flutter estable + Dart, Android primero (iOS se compila pero no se prueba en el prototipo).
- Estado: Riverpod (con riverpod_generator). Navegación: go_router.
- Firebase: Auth (correo y Google con google_sign_in), Cloud Firestore, Storage, Analytics. Sin Cloud Messaging ni Cloud Functions.
- Recordatorio diario: flutter_local_notifications (programado en el teléfono; ver `docs/decisiones/0005-recordatorio-local.md`).
- Animaciones: las nativas de Flutter; sin Lottie ni Rive.
- Video: video_player + chewie, con velocidad 0.5x / 0.75x / 1x y bucle.
- Modelos inmutables con freezed + json_serializable.
- Textos: flutter_localizations + intl (ARB).
- Lint: very_good_analysis. Pruebas: flutter_test + mocktail + integration_test.
- Ningún paquete fuera de esta lista entra sin registrar la decisión en `docs/decisiones/`.

## Arquitectura
Feature-first, tres capas por feature (detalle en `docs/arquitectura.md`):

```
lib/
  main_dev.dart   arranque contra ensenas-dev
  main_prod.dart  arranque contra ensenas-prod
  app/        tema, router, configuración de entorno
  core/       widgets comunes, utilidades, errores
  features/<feature>/
    data/          repositorios, DTOs, fuentes Firebase
    domain/        entidades y reglas puras (sin Flutter ni Firebase)
    presentation/  pantallas, widgets, providers
  l10n/       textos en español (ARB)
firebase/     reglas de Firestore y Storage, índices
tool/         scripts de desarrollo (seed de datos de prueba)
```
Features: auth, onboarding, map, lesson, progress, profile.
- Las reglas de negocio (XP, racha, vidas) viven en domain/ y tienen pruebas unitarias. El reloj se inyecta para poder probar fechas; la zona horaria del negocio es America/Bogota.
- La UI nunca habla con Firebase directo: siempre a través de un repositorio expuesto por un provider.
- Errores de data se traducen a fallos de dominio en el borde; la UI muestra mensajes desde l10n.

## Entornos
- Dos proyectos Firebase: `ensenas-dev` (desarrollo y datos de prueba) y `ensenas-prod` (prueba con usuarios).
- `lib/firebase_options_dev.dart` y `lib/firebase_options_prod.dart` se versionan: son identificadores públicos, no secretos (ver `docs/decisiones/0003-firebase-options-en-repo.md`). La seguridad está en las reglas de `firebase/`.
- Android usa los flavors `dev` (id `co.ensenas.ensenas.dev`, se instala junto a prod) y `prod` (`co.ensenas.ensenas`), cada uno con su `google-services.json` en `android/app/src/<flavor>/`.
- Se ejecuta con `flutter run --flavor dev -t lib/main_dev.dart` o `--flavor prod -t lib/main_prod.dart` (en VS Code, con las configuraciones de `.vscode/launch.json`).

## Firestore
Esquema completo y reglas en `docs/modelo-datos.md`.
- units: id, fase, orden, titulo, descripcion, icono
- lessons: id, unitId, orden, signIds[], tiposEjercicio[]
- signs: id, palabra, categoria, videoUrl, autor, validadoPor, fechaValidacion
- users/{uid}: nombre, xp, racha, rachaMaxima, ultimaActividad, vidas, vidasActualizadasEn, unidadActual
- users/{uid}/progress/{lessonId}: estrellas, errores[], completadaEn
- Una seña sin validadoPor nunca se muestra al usuario final (solo en main_dev con el modo desarrollo activo).
- Si un cambio toca el modelo, actualiza en el mismo PR `docs/modelo-datos.md` y, si aplica, `firebase/firestore.rules`.

## Convenciones
- Código, nombres de archivos y clases en inglés; todo texto visible en español, vía l10n (nunca strings sueltos en widgets).
- Lenguaje respetuoso: "comunidad sorda", "persona sorda", "modelo lingüístico". Nunca "sordomudo" ni "discapacitado auditivo".
- Tema oscuro con acento morado. Contraste WCAG 2.1 AA, toque mínimo 48 dp, Semantics en elementos interactivos, feedback que no dependa solo del color.
- Commits: Conventional Commits en español, p. ej. `feat(lesson): agrega ejercicio Parejas`. El título del PR sigue el mismo formato (se fusiona con squash).
- Una rama por tarea: feat/..., fix/..., chore/..., docs/..., test/...
- Finales de línea LF (lo fija `.gitattributes`); no cambies eso aunque el equipo use Windows.

## Comandos
- `flutter pub get`
- `dart run build_runner build --delete-conflicting-outputs`
- `dart format .`
- `flutter analyze`
- `flutter test` (con cobertura: `flutter test --coverage`)
- `flutter run --flavor dev -t lib/main_dev.dart`

## Cómo trabajar
1. Sigue "Roles y flujo de trabajo" para saber cuándo esperar aprobación y cuándo continuar solo.
2. Implementa en pasos pequeños; corre formato, `flutter analyze` y `flutter test` antes de declarar algo terminado.
3. Agrega o actualiza pruebas en cada cambio de lógica.
4. Registra decisiones técnicas relevantes en `docs/decisiones/` copiando `0000-plantilla.md` (un archivo corto por decisión).
5. Al terminar, entrega el "Resumen de sesión" (ver abajo) y agrega una línea a `CHANGELOG.md` en la sección "Sin publicar".

## Roles y flujo de trabajo
- Tú (Claude Code) implementas, pruebas y abres PR. El chat web del proyecto planea, especifica y revisa. Artur decide y fusiona.
- Las tareas llegan como issue de GitHub o como prompt pegado en VS Code. Si no traen criterios de aceptación claros o chocan con este archivo, pregunta antes de programar (VS Code) o explica el bloqueo en un comentario del issue y detente (modo autónomo).
- Modo VS Code: plan corto y espera aprobación si la tarea toca más de dos archivos (skill flutter-feature). Muestra qué vas a commitear y espera confirmación antes de cada commit.
- Modo autónomo (GitHub Action): no hay nadie para aprobar. Escribe el plan al inicio de la descripción del PR y continúa. Detente solo si la tarea sale del alcance, exige consola de Firebase, credenciales, un dispositivo físico o contenido LSC real.
- Antes de abrir un PR o declarar algo terminado, usa el subagente revisor y corrige los hallazgos de severidad alta.
- Nunca fusiones PR ni hagas push a main (está protegida). No modifiques CLAUDE.md, `.github/workflows/` ni `.claude/` salvo que la tarea lo pida explícitamente.
- Una tarea por rama y por PR; si descubres trabajo extra, anótalo en "Pendientes" en vez de hacerlo. El PR cierra su issue con `Closes #<n>`.

## Resumen de sesión
Al cerrar una sesión en VS Code o al terminar un PR, entrega este bloque (en el chat o al final de la descripción del PR) para que Artur lo pegue en el chat web:

```
Resumen de sesión — AAAA-MM-DD
Tarea: issue o prompt atendido
Rama / PR:
Qué cambió: 2 a 4 líneas
Archivos: lista corta
Pruebas: qué se agregó; resultado de flutter analyze y flutter test
Decisiones: nuevas (con su archivo en docs/decisiones/) o "ninguna"
Pendientes y riesgos:
Preguntas para el chat web:
```

## Repositorio público
- Este repo es público: nunca subas videos de personas, datos personales, llaves de servicio, keystores ni archivos .env.
- Los videos reales viven solo en Firebase Storage; `content/` está en .gitignore. El formato del manifiesto está en `docs/contenido/manifest.example.csv`.
- El código es MIT; el contenido de LSC tiene derechos reservados (CONTENT-LICENSE.md).

## Reglas sobre LSC (no negociables)
- No inventes, describas ni corrijas señas de LSC. El contenido lingüístico viene solo de la comunidad sorda.
- Para desarrollar usa datos de prueba marcados `validadoPor: null`, nombres tipo "Seña de prueba 01" y videos de relleno.
- La Fase 04 se llama "constancia" o "insignia de logro", nunca "certificado" (Decreto 1075 de 2015, art. 2.6.6.8).
