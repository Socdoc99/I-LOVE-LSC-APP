---
name: contenido-lsc
description: Reglas para cargar o modificar contenido de LSC en EnSeñas (señas, videos, unidades, lecciones, textos sobre la comunidad sorda). Úsalo siempre que una tarea toque la colección signs, units o lessons, scripts de carga, videos o textos culturales.
---

# Contenido de LSC en EnSeñas

El contenido lingüístico es de la comunidad sorda. El agente solo lo transporta.

1. **Nunca generes señas.** No describas cómo se hace una seña, no inventes palabras del vocabulario ni corrijas un video. Si falta contenido, deja un marcador y avisa.
2. **Fuente única.** Las señas reales llegan en `content/manifest.csv` (formato en `docs/contenido/manifest.example.csv`: id, palabra, categoria, unidad, archivo_video, autor, validado_por, fecha_validacion) junto con los videos. Cárgalas con el script de seed; no las escribas a mano en el código.
3. **Datos de prueba.** En desarrollo usa nombres tipo "Seña de prueba 01", `validadoPor: null` y videos de relleno sin personas. Solo en el proyecto `ensenas-dev`.
4. **Validación obligatoria.** Una seña sin `validado_por` se guarda con `validadoPor: null` y la app no la muestra fuera del modo desarrollo.
5. **Crédito visible.** Todo autor y validador aparece en la pantalla de créditos del perfil y en la línea de créditos al terminar una lección.
6. **Videos.** MP4 H.264, vertical 9:16, 2 a 4 segundos, sin audio, máximo ~600 KB. Ruta en Storage: `signs/<unidad>/<id>.mp4`.
7. **Lenguaje.** Usa "comunidad sorda", "persona sorda", "modelo lingüístico". Nunca "sordomudo" ni "discapacitado auditivo".
8. **Datos personales.** Las personas grabadas firmaron autorización de imagen (Ley 1581 de 2012). No subas videos que no estén en el manifiesto ni ningún video al repositorio.
9. **Credenciales.** El seed usa una cuenta de servicio que vive fuera del repo (ruta por variable de entorno). Nunca la leas, copies ni imprimas.
