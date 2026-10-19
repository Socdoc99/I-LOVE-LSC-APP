# 0001 · Flutter + Firebase + Riverpod

Fecha: 2026-10-15 · Estado: aceptada

## Contexto
El prototipo debe validarse con usuarios en 10 semanas, desarrollado por una persona con apoyo de Claude Code. El documento original de I LOVE LSC proponía Ionic, React y Django.

## Decisión
Flutter (Android primero) con Riverpod y go_router, y Firebase (Auth, Firestore, Storage, Cloud Messaging, Analytics) como backend.

## Alternativas
Ionic + React + Django: dos bases de código (app y API) y un servidor que mantener; reproducción de video y animaciones menos fluidas en gama media. Kotlin con Jetpack Compose: solo Android y sin reutilización hacia iOS.

## Consecuencias
Un solo código para la app y backend administrado sin servidor propio. Riverpod con generación de código da providers verificados en compilación y fáciles de reemplazar en pruebas. El capítulo tecnológico de la tesis debe actualizarse para describir este stack.
