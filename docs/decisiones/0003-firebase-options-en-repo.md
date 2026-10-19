# 0003 · Versionar firebase_options y google-services.json

Fecha: 2026-10-19 · Estado: aceptada

## Contexto
El repo es público. El plan inicial dejaba `firebase_options.dart` fuera del repo, pero el código lo importa en `main`, así que el CI y el GitHub Action no podrían compilar ni probar sin él.

## Decisión
Se versionan `lib/firebase_options_dev.dart`, `lib/firebase_options_prod.dart` y los `google-services.json` de cada flavor.

## Alternativas
Generarlos en el CI desde secretos: más configuración y el agente autónomo tampoco tendría los archivos al trabajar. Dejarlos fuera y usar stubs: dos versiones del mismo archivo que se desincronizan.

## Consecuencias
Estos archivos contienen identificadores públicos del proyecto (la API key de Firebase identifica, no autoriza). La protección real son las reglas de `firebase/` y, antes de la prueba con usuarios, restringir la API key a la app Android en Google Cloud y evaluar App Check. Cuentas de servicio, keystores y `.env` siguen fuera del repo.
