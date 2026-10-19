# Modelo de datos

Cloud Firestore en dos proyectos: `ensenas-dev` y `ensenas-prod`. Las reglas viven en `firebase/firestore.rules` y `firebase/storage.rules`.

## Colecciones de contenido (solo lectura para la app)

**units/{unitId}**

| Campo | Tipo | Nota |
|---|---|---|
| id | string | igual al id del documento |
| fase | int | fase del programa (el prototipo usa la 1) |
| orden | int | orden en el mapa |
| titulo | string | p. ej. "Saludos y cortesía" |
| descripcion | string | |
| icono | string | nombre del ícono |

**lessons/{lessonId}**

| Campo | Tipo | Nota |
|---|---|---|
| id | string | |
| unitId | string | referencia a units |
| orden | int | orden dentro de la unidad |
| signIds | string[] | señas que practica |
| tiposEjercicio | string[] | `verYElegir`, `palabraASena`, `parejas` |

**signs/{signId}**

| Campo | Tipo | Nota |
|---|---|---|
| id | string | |
| palabra | string | palabra en español |
| categoria | string | |
| videoUrl | string | ruta en Storage `signs/<unidad>/<id>.mp4` |
| autor | string | modelo lingüístico que grabó la seña |
| validadoPor | string \| null | quien validó; null = no se muestra fuera de dev |
| fechaValidacion | timestamp \| null | |

## Datos de cada persona

**users/{uid}**

| Campo | Tipo | Nota |
|---|---|---|
| nombre | string | |
| xp | int | empieza en 0 |
| racha | int | días seguidos con al menos una lección |
| rachaMaxima | int | para el perfil |
| ultimaActividad | timestamp | base del cálculo de racha (America/Bogota) |
| vidas | int | 0 a 5, empieza en 5 |
| vidasActualizadasEn | timestamp | base de la recuperación de una vida cada 4 horas |
| unidadActual | string | |

**users/{uid}/progress/{lessonId}**

| Campo | Tipo | Nota |
|---|---|---|
| estrellas | int | 0 a 3 |
| errores | string[] | signIds fallados, alimentan la lección de repaso |
| completadaEn | timestamp | |

## Reglas de negocio

- 10 XP por lección completada y 5 extra si no hubo errores.
- La racha sube con al menos una lección por día calendario en America/Bogota y vuelve a 0 si se salta un día.
- 5 vidas máximo; se pierde una por error y se recupera una cada 4 horas.

## Seguridad

- Contenido (units, lessons, signs y los videos): lectura para usuarios autenticados, escritura cerrada. Se carga con el script de seed y una cuenta de servicio que nunca entra al repo.
- users/{uid} y su progreso: solo el dueño lee y escribe. Al crear el documento, xp y racha deben ser 0 y vidas 5.
- En el prototipo la app escribe su propio XP; eso permite trampa, pero es aceptable para la prueba con usuarios. Moverlo a Cloud Functions queda fuera de alcance.

## Contenido real

Las señas reales llegan en `content/manifest.csv` (fuera del repo) con el formato de `docs/contenido/manifest.example.csv`.
