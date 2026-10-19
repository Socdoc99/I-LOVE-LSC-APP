# Arranque (sprint 0)

Pasos que se hacen una sola vez en tu computador. Pega los prompts en Claude Code dentro de VS Code, uno a la vez, y revisa cada resultado antes del siguiente.

## 0. Requisitos

Flutter estable, VS Code con las extensiones recomendadas del repo (VS Code las ofrece al abrir la carpeta), Android Studio con un emulador, Firebase CLI (`npm i -g firebase-tools`) y FlutterFire CLI (`dart pub global activate flutterfire_cli`). Configura git con tu cuenta de la universidad:

```
git config --global user.name "Socdoc99"
git config --global user.email "santiago.ospina1@utp.edu.co"
```

Clona el repo y ábrelo en VS Code:

```
git clone https://github.com/Socdoc99/I-LOVE-LSC-APP.git
```

## 1. Diagnóstico del entorno

```
Lee CLAUDE.md completo. Luego ejecuta flutter doctor -v, firebase --version, flutterfire --version y git config user.email. Dime qué falta o qué está mal en mi entorno para este proyecto y cómo corregirlo en Windows. No crees ni modifiques archivos todavía.
```

## 2. Crear el proyecto Flutter (issue "S0 · Crear el proyecto Flutter")

```
Implementa el issue "S0 · Crear el proyecto Flutter" siguiendo CLAUDE.md. Crea la rama chore/proyecto-flutter y en la raíz ejecuta: flutter create --org co.ensenas --project-name ensenas --platforms android,ios .
Verifica con git status que CLAUDE.md, README.md, LICENSE, .gitignore, .gitattributes, .claude/, .github/, .vscode/, docs/ y firebase/ quedaron intactos; si flutter create modificó alguno, restáuralo y dime qué pasó.
Después propone (sin ejecutar aún) las dependencias de pubspec.yaml según el stack de CLAUDE.md, con versiones estables actuales, y explica en una línea para qué sirve cada una. Espera mi aprobación.
```

Antes de aprobar, revisa que la lista tenga: flutter_riverpod, riverpod_annotation, riverpod_generator, go_router, freezed, freezed_annotation, json_annotation, json_serializable, build_runner, very_good_analysis, mocktail, video_player, chewie, flutter_localizations e intl. Las de Firebase entran en el paso 4.

```
Con las dependencias aprobadas, arma el esqueleto según CLAUDE.md y docs/arquitectura.md: lib/main_dev.dart y lib/main_prod.dart con un AppConfig de entorno (todavía sin Firebase), app/ (tema oscuro con acento morado #9B5DE5, router con go_router y rutas vacías), core/, features/ (auth, onboarding, map, lesson, progress, profile) con data/domain/presentation, l10n en español con un ARB inicial y analysis_options.yaml con very_good_analysis. Configura los flavors de Android dev (applicationIdSuffix .dev) y prod.
Agrega un test de humo que arranque la app. Corre dart format ., flutter analyze y flutter test y muéstrame el resultado. Usa el subagente revisor, corrige lo de severidad alta, muéstrame qué vas a commitear y espera mi confirmación. Luego sube la rama y abre el PR con Closes y el Resumen de sesión.
```

Este PR es el primero que el CI verifica de verdad. Si sale en verde, la cadena completa funciona.

## 3. Conectar Claude a GitHub (issue "MANUAL · Conectar Claude Code a GitHub")

En Claude Code dentro del repo ejecuta `/install-github-app` y elige el token de tu suscripción. Instala la app de Claude en el repositorio y guarda el secreto `CLAUDE_CODE_OAUTH_TOKEN`. Desde ese momento puedes comentar `@claude` en los issues.

## 4. Firebase (issue "MANUAL · Crear y conectar Firebase dev y prod")

En la consola de Firebase crea dos proyectos. Si los ids `ensenas-dev` o `ensenas-prod` ya están tomados, Firebase te propone otros; anótalos.

```
Implementa el issue "MANUAL · Crear y conectar Firebase dev y prod" siguiendo CLAUDE.md, en la rama chore/firebase. Mis ids de proyecto son: dev = <id-dev>, prod = <id-prod>. Si difieren de .firebaserc, actualízalo.
Guíame paso a paso: dime qué hago yo en la consola (Auth con correo, Firestore en modo producción en southamerica-east1, Storage) y qué ejecutas tú. Usa flutterfire configure una vez por entorno con --out lib/firebase_options_<entorno>.dart, --android-package-name según el flavor y --android-out android/app/src/<flavor>/google-services.json. Revisa con git diff que firebase.json conserve las rutas de reglas.
Agrega las dependencias de Firebase, inicializa Firebase en main_dev.dart y main_prod.dart y publica las reglas con firebase deploy --only firestore:rules,storage --project dev. Verifica que la app arranque en el emulador con el flavor dev. Muéstrame qué vas a commitear antes de hacerlo.
```

## 5. Al cerrar cada sesión

```
Entrega el Resumen de sesión de CLAUDE.md para pegarlo en el chat web.
```
