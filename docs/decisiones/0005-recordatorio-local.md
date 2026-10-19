# 0005 · Recordatorio diario con notificaciones locales

Fecha: 2026-10-19 · Estado: aceptada

## Contexto
El plan listaba Firebase Cloud Messaging para el recordatorio diario. FCM solo entrega notificaciones que un servidor envía, así que un aviso diario exigiría una Cloud Function programada, que a su vez requiere el plan de pago Blaze.

## Decisión
El recordatorio se programa en el teléfono con flutter_local_notifications, a la hora elegida en el perfil, y se omite si ese día ya hubo una lección. Cloud Messaging y Cloud Functions quedan fuera del prototipo. Las animaciones (racha, resultado) usan las APIs nativas de Flutter, sin Lottie ni Rive.

## Alternativas
FCM con Cloud Functions: costo, un despliegue más y lógica de servidor que no aporta a la validación. Sin recordatorio: es la primera función a recortar si el calendario aprieta, pero ayuda a sostener la racha durante la prueba.

## Consecuencias
Sin servidor ni costo. El aviso funciona aunque no haya conexión. Android 13 o superior pide permiso de notificaciones, así que la app lo solicita con una explicación previa.
