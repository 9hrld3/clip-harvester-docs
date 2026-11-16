# Guía de uso

**Español** · [English](usage-guide.md)

Esta guía describe la versión 0.2.0, que se usa desde la consola. Antes de empezar, lee el [aviso de uso responsable](../USO_RESPONSABLE.md).

## El comando

Abre una terminal en la carpeta que contiene `clip_harvester/` y ejecuta:

```bash
python -m clip_harvester URL INICIO FIN --tipo TIPO
```

Mientras trabaja, el programa informa de cada paso y termina mostrando la carpeta donde quedó el clip:

```text
Leyendo datos del video...
Descargando el tramo...
Escribiendo créditos...
Listo: clips\Referencia\2026-11-16_canal-ejemplo-titulo-del-video_01m20-01m32_NO-PUBLICAR
```

## Plataformas

Funciona con enlaces de YouTube, Instagram y TikTok. Usa el enlace directo al video, no el de un perfil ni el de una lista.

Instagram y TikTok piden con frecuencia haber iniciado sesión. En ese caso, inicia sesión en tu navegador y añade `--cookies` con el nombre del navegador.

## Cómo escribir los tiempos

`INICIO` y `FIN` aceptan tres formas:

| Forma | Ejemplo | Significa |
|---|---|---|
| Segundos | `80` | 80 segundos |
| Minutos:segundos | `1:20` | 1 min 20 s |
| Horas:minutos:segundos | `0:01:20.5` | 1 min 20,5 s |

Los decimales pueden escribirse con punto o con coma.

Reglas:

- El fin debe ser mayor que el inicio.
- Si el fin supera la duración del video, el tramo se recorta al final del video.
- Si el inicio queda más allá del final del video, el programa se detiene con un error y no descarga nada.

## Tipos de uso

`--tipo` es obligatorio. Lo declaras tú, de buena fe, y queda escrito en los créditos del clip.

| Tipo | Publicable | Para qué |
|---|---|---|
| `Propio` | sí | Material tuyo o sobre el que tienes los derechos. |
| `Edu` | sí | Segunda categoría de material publicable. |
| `Referencia` | **no** | Material ajeno que guardas solo para consulta o inspiración. |

Los clips de `Referencia` llevan el sufijo `_NO-PUBLICAR` en la carpeta y la línea `Publicable: NO (solo referencia)` en los créditos.

El programa no comprueba si tienes permiso para usar el material: solo registra lo que declaras.

## Opciones

### `--notas "texto"`

Guarda un texto libre en los créditos y en el índice. Sirve para anotar para qué quieres el clip o qué permiso tienes.

### `--cookies NAVEGADOR`

Usa la sesión iniciada en un navegador (`chrome`, `edge` o `firefox`) para videos que requieren haber iniciado sesión. Las cookies se leen directamente del navegador de tu equipo y no se guardan ni se envían a otro sitio que la plataforma de origen.

### `--salida CARPETA`

Cambia la carpeta de salida. Por defecto es `clips`, dentro de la carpeta desde la que ejecutas el comando.

### `--transcribir`

Convierte en texto lo que se dice en el clip y lo guarda junto a él como `transcripcion_original.txt` y `transcripcion_original.srt`.

- El idioma se detecta automáticamente.
- La primera vez se descarga el modelo de transcripción (`large-v3-turbo`), que ocupa más de 1 GB, en una carpeta `modelos/` junto al programa. Después funciona sin internet.
- Si el equipo tiene una tarjeta NVIDIA configurada, se usa para ir más rápido. Si no, o si falla, se usa el procesador, que es más lento.
- Si la voz no se distingue bien (por ejemplo, un clip que solo tiene música), el programa avisa con «Voz poco clara» y deja la transcripción vacía, en lugar de inventar texto.

### `--traducir IDIOMAS`

Traduce la transcripción a uno o varios idiomas, separados por comas y sin espacios: `es` (español), `en` (inglés), `it` (italiano). Por ejemplo, `--traducir es,en,it`.

- No hace falta añadir `--transcribir`: la traducción ya la incluye.
- Por cada idioma se guardan `traduccion_<idioma>.txt` y `traduccion_<idioma>.srt`. Los subtítulos conservan los tiempos del original.
- El idioma original del clip se omite: si el clip está en inglés y pides `es,en,it`, obtienes español e italiano.
- Solo se traduce si el idioma original es español, inglés o italiano. Con otro idioma, el programa avisa y deja solo la transcripción.
- La primera vez se descargan los paquetes de idioma. Después funciona sin internet.
- Es traducción automática: revísala antes de publicarla.

Con transcripción y traducción, la salida de la consola se ve así:

```text
Leyendo datos del video...
Descargando el tramo...
Escribiendo créditos...
Cargando modelo large-v3-turbo (GPU); la primera vez se descarga.
Transcribiendo...
Idioma detectado: en (98%).
Traduciendo a español...
Traduciendo a italiano...
Listo: clips\Propio\2026-11-16_canal-ejemplo-titulo-del-video_00m10-00m45
```

## Qué se guarda

### Nombre de la carpeta

```text
<fecha>_<autor>-<título>_<tramo>[_NO-PUBLICAR]
```

- **fecha**: día de la descarga, `AAAA-MM-DD`.
- **autor** y **título**: en minúsculas, sin tildes ni símbolos, y recortados (20 y 40 caracteres).
- **tramo**: por ejemplo `01m20-01m32`; si hay horas, `1h01m20`.

### `clip.mp4`

El tramo en MP4, con la mejor calidad disponible hasta 1080p. Los bordes del corte se recodifican para que el clip empiece y termine en el instante exacto.

### `CREDITOS.txt`

```text
CRÉDITOS DEL CLIP
=================
Título:            Título del video
Autor / canal:     Canal de ejemplo
Enlace del autor:  https://www.youtube.com/@canal-ejemplo
Enlace del video:  https://www.youtube.com/watch?v=ID_DEL_VIDEO
Plataforma:        Youtube
Publicado:         20250101
Tramo usado:       1:20 a 1:32 (12.0 s)
Descargado:        2026-11-16 10:27
Tipo de uso:       Referencia
Publicable:        NO (solo referencia)
Licencia indicada: no indicada
Notas:             Idea de transición

Dar el crédito no equivale a tener permiso de uso.
```

«Licencia indicada» es lo que declara la plataforma para ese video. Si la plataforma no lo informa, aparece «no indicada», lo cual **no** significa que el video sea de uso libre.

### `fuente.json`

Los mismos datos en formato estructurado, más algunos adicionales: identificador del video, duración original y los tiempos del tramo en segundos.

| Campo | Contenido |
|---|---|
| `titulo`, `autor`, `url_autor`, `url` | Datos y enlaces de la fuente. |
| `plataforma`, `id_video`, `fecha_publicacion` | Origen del video. |
| `duracion_original_s` | Duración del video completo, en segundos. |
| `licencia_declarada_por_la_plataforma` | Licencia que informa la plataforma, si la hay. |
| `inicio`, `fin`, `inicio_s`, `fin_s`, `duracion_s` | Tramo descargado. |
| `fecha_descarga` | Fecha y hora de la descarga. |
| `tipo_uso`, `publicable`, `notas` | Lo que declaraste al descargar. |
| `idioma_detectado`, `confianza_idioma`, `modelo_transcripcion` | Solo si transcribiste: idioma detectado, seguridad de la detección (de 0 a 1) y modelo usado. |

### Transcripción y traducciones

| Archivo | Contenido |
|---|---|
| `transcripcion_original.txt` | El texto, una frase por línea. |
| `transcripcion_original.srt` | Subtítulos con tiempos, listos para un editor de video. |
| `traduccion_<idioma>.txt` | El texto traducido. |
| `traduccion_<idioma>.srt` | Subtítulos traducidos, con los mismos tiempos. |

Los tiempos de los subtítulos son relativos al inicio del clip, no al video original.

### `_indice.csv`

Está en la raíz de la carpeta de salida y añade una fila por cada descarga. Columnas:

```text
fecha_descarga, tipo_uso, publicable, plataforma, autor, titulo, url, inicio, fin, duracion_s, carpeta, notas
```

Se abre directamente en Excel con las tildes correctas.

## Limitaciones conocidas

- Instagram y TikTok cambian a menudo: si una descarga deja de funcionar, actualiza yt-dlp.
- La traducción solo funciona entre español, inglés e italiano.
- La transcripción puede fallar o quedar vacía con música alta, ruido o varias personas hablando a la vez.
- Se descarga un tramo por comando.
- Si repites el mismo tramo del mismo video el mismo día, el clip se sobrescribe y el índice recibe una fila más.
- La resolución máxima es 1080p.

## Problemas frecuentes

| Mensaje o síntoma | Qué hacer |
|---|---|
| `Tiempo no válido` | Revisa el formato de `INICIO` y `FIN`. |
| `El fin del tramo debe ser mayor que el inicio.` | Intercambia o corrige los tiempos. |
| `El tramo empieza en … y el video dura …` | El inicio está más allá del final del video. |
| `ffmpeg` no encontrado, o el corte falla | Instala ffmpeg y comprueba que está en el `PATH`. |
| `No module named yt_dlp` | Ejecuta `pip install -U yt-dlp`. |
| La descarga falla en un video que antes funcionaba | Actualiza yt-dlp: las plataformas cambian con frecuencia. |
| `… pide iniciar sesión para este video` | Inicia sesión en tu navegador y usa `--cookies`. Si ya lo usabas, comprueba que la sesión sigue abierta y que el video no es privado. |
| `El enlace no es de un video compatible` | Copia el enlace directo al video. |
| `El video no está disponible` | Fue borrado, es privado o está bloqueado en tu región. |
| `No hay conexión a internet para descargar el video` | Revisa tu conexión. |
| `Voz poco clara … no se genera transcripción` | El clip no tiene voz reconocible; la transcripción queda vacía y no se traduce. |
| `El idioma original (…) no tiene traducción local` | El clip no está en español, inglés ni italiano. |
| `La GPU no respondió; se usa la CPU` | No es un error: la transcripción continúa, más lenta. |
| `No module named faster_whisper` o `argostranslate` | Instala el paquete que falta con `pip install`. |

Cuando ocurre un error, el programa lo muestra con el prefijo `Error:` y termina. Si falla la descarga, no se escriben créditos ni se toca el índice. Si falla la transcripción o la traducción, el clip y sus créditos ya están guardados.
