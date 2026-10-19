---
name: flutter-feature
description: Crea o amplía un feature de EnSeñas (auth, onboarding, map, lesson, progress, profile) siguiendo la arquitectura feature-first con Riverpod, go_router y Firebase. Úsalo cuando se pida una pantalla, un ejercicio, un repositorio o una regla de negocio nueva.
---

# Crear un feature en EnSeñas

Sigue estos pasos en orden. No saltes las pruebas. La guía completa de capas está en `docs/arquitectura.md`.

1. **Entiende el pedido.** Identifica el feature, la capa afectada y el issue. Si toca algo fuera del alcance de CLAUDE.md, pregunta antes de seguir.
2. **Rama.** `feat/<feature>-<tema-corto>` desde main actualizada.
3. **Plan corto.** Lista los archivos a crear o cambiar. En VS Code espera aprobación si son más de dos; en modo autónomo escríbelo al inicio de la descripción del PR.
4. **Domain primero.** Entidades con freezed y reglas puras en `features/<f>/domain/`. Sin imports de Flutter ni Firebase. Interfaz del repositorio aquí. Reloj inyectado si hay fechas.
5. **Data.** Implementación Firebase en `data/`. DTOs con json_serializable; convierte DTO ↔ entidad en el borde y traduce excepciones de Firebase a fallos de dominio.
6. **Presentation.** Providers con riverpod_generator; pantallas pequeñas, widgets extraídos cuando pasen de ~80 líneas. Textos desde l10n. Ruta registrada en `app/router.dart`.
7. **Accesibilidad.** Semantics con etiqueta en cada control, toque mínimo 48 dp, contraste AA sobre el tema oscuro, feedback con ícono + texto (nunca solo color), texto que soporte escala al 200 %.
8. **Pruebas.**
   - Unitarias para cada regla de domain y para la conversión DTO ↔ entidad.
   - De widget para cada pantalla nueva, con el repositorio simulado (mocktail) mediante overrides de Riverpod.
9. **Verifica.** `dart run build_runner build --delete-conflicting-outputs`, `dart format .`, `flutter analyze` sin advertencias, `flutter test` en verde.
10. **Documenta.** Si cambió el modelo de datos, actualiza `docs/modelo-datos.md` y las reglas en `firebase/`. Si tomaste una decisión técnica, agrega un archivo en `docs/decisiones/`. Una línea en `CHANGELOG.md`.
11. **Cierre.** Subagente revisor; corrige severidad alta. Commit en Conventional Commits en español (en VS Code, confirma con Artur antes). PR con `Closes #<n>` y el Resumen de sesión.
