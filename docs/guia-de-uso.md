# Guía de uso

**Español** · [English](usage-guide.md)

Esta guía describe la versión 0.1.0, que se usa desde la consola. Antes de empezar, lee el [aviso de uso responsable](../USO_RESPONSABLE.md).

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

### `_indice.csv`

Está en la raíz de la carpeta de salida y añade una fila por cada descarga. Columnas:

```text
fecha_descarga, tipo_uso, publicable, plataforma, autor, titulo, url, inicio, fin, duracion_s, carpeta, notas
```

Se abre directamente en Excel con las tildes correctas.

## Limitaciones conocidas

- Solo se ha probado con YouTube. Instagram y TikTok están en la hoja de ruta.
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
| El video pide iniciar sesión | Inicia sesión en tu navegador y usa `--cookies`. |

Cuando ocurre un error, el programa lo muestra con el prefijo `Error:` y termina sin escribir créditos ni tocar el índice.
