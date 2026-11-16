# Usage guide

[Español](guia-de-uso.md) · **English**

This guide describes version 0.1.0, which is used from the command line. Read the [responsible use notice](../RESPONSIBLE_USE.md) before you start.

The program's options, messages, file names and credit fields are in Spanish. They are shown here as they are, with a translation where it helps.

## The command

Open a terminal in the folder that contains `clip_harvester/` and run:

```bash
python -m clip_harvester URL START END --tipo TYPE
```

While it works, the program reports each step and ends by showing the folder where the clip was saved:

```text
Leyendo datos del video...
Descargando el tramo...
Escribiendo créditos...
Listo: clips\Referencia\2026-11-16_example-channel-video-title_01m20-01m32_NO-PUBLICAR
```

(Reading video data, downloading the segment, writing credits, done.)

## How to write times

`START` and `END` accept three forms:

| Form | Example | Means |
|---|---|---|
| Seconds | `80` | 80 seconds |
| Minutes:seconds | `1:20` | 1 min 20 s |
| Hours:minutes:seconds | `0:01:20.5` | 1 min 20.5 s |

Decimals can be written with a point or a comma.

Rules:

- The end must be greater than the start.
- If the end is past the video's duration, the segment is trimmed to the end of the video.
- If the start is past the end of the video, the program stops with an error and downloads nothing.

## Types of use

`--tipo` is required. You declare it yourself, in good faith, and it is written into the clip's credits.

| Type | Publishable | Purpose |
|---|---|---|
| `Propio` (own) | yes | Your own material, or material you hold the rights to. |
| `Edu` | yes | Second category of publishable material. |
| `Referencia` (reference) | **no** | Other people's material that you keep only for reference or inspiration. |

`Referencia` clips get the `_NO-PUBLICAR` (do not publish) suffix on the folder and the line `Publicable: NO (solo referencia)` in the credits.

The program does not check whether you have permission to use the material: it only records what you declare.

## Options

### `--notas "text"`

Saves free-text notes in the credits and in the index. Use it to record what you want the clip for or what permission you have.

### `--cookies BROWSER`

Uses the session of a browser (`chrome`, `edge` or `firefox`) for videos that require being signed in. Cookies are read directly from the browser on your machine and are neither stored nor sent anywhere other than the source platform.

### `--salida FOLDER`

Changes the output folder. The default is `clips`, inside the folder you run the command from.

## What gets saved

### Folder name

```text
<date>_<author>-<title>_<segment>[_NO-PUBLICAR]
```

- **date**: download day, `YYYY-MM-DD`.
- **author** and **title**: lower case, without accents or symbols, and shortened (20 and 40 characters).
- **segment**: for example `01m20-01m32`; with hours, `1h01m20`.

### `clip.mp4`

The segment as MP4, at the best available quality up to 1080p. The cut edges are re-encoded so that the clip starts and ends at the exact instant.

### `CREDITOS.txt`

```text
CRÉDITOS DEL CLIP
=================
Título:            Video title
Autor / canal:     Example channel
Enlace del autor:  https://www.youtube.com/@example-channel
Enlace del video:  https://www.youtube.com/watch?v=VIDEO_ID
Plataforma:        Youtube
Publicado:         20250101
Tramo usado:       1:20 a 1:32 (12.0 s)
Descargado:        2026-11-16 10:27
Tipo de uso:       Referencia
Publicable:        NO (solo referencia)
Licencia indicada: no indicada
Notas:             Transition idea

Dar el crédito no equivale a tener permiso de uso.
```

The fields are, in order: title, author/channel, author link, video link, platform, publication date, segment used, download date, type of use, publishable, stated license and notes. The last line reads "Giving credit is not the same as having permission to use".

"Licencia indicada" is what the platform states for that video. If the platform does not report it, the file says "no indicada" (not stated), which does **not** mean the video is free to use.

### `fuente.json`

The same data in structured form, plus a few extras: video identifier, original duration and the segment times in seconds.

| Field | Content |
|---|---|
| `titulo`, `autor`, `url_autor`, `url` | Source details and links. |
| `plataforma`, `id_video`, `fecha_publicacion` | Where the video comes from. |
| `duracion_original_s` | Duration of the full video, in seconds. |
| `licencia_declarada_por_la_plataforma` | License reported by the platform, if any. |
| `inicio`, `fin`, `inicio_s`, `fin_s`, `duracion_s` | Downloaded segment. |
| `fecha_descarga` | Download date and time. |
| `tipo_uso`, `publicable`, `notas` | What you declared when downloading. |

### `_indice.csv`

It sits at the root of the output folder and gets one row per download. Columns:

```text
fecha_descarga, tipo_uso, publicable, plataforma, autor, titulo, url, inicio, fin, duracion_s, carpeta, notas
```

It opens directly in Excel with accented characters shown correctly.

## Known limitations

- Only tested with YouTube. Instagram and TikTok are on the roadmap.
- One segment per command.
- If you repeat the same segment of the same video on the same day, the clip is overwritten and the index gets one more row.
- Maximum resolution is 1080p.

## Troubleshooting

| Message or symptom | What to do |
|---|---|
| `Tiempo no válido` (invalid time) | Check the format of `START` and `END`. |
| `El fin del tramo debe ser mayor que el inicio.` | The end must be greater than the start: swap or fix the times. |
| `El tramo empieza en … y el video dura …` | The start is past the end of the video. |
| `ffmpeg` not found, or the cut fails | Install ffmpeg and check that it is on the `PATH`. |
| `No module named yt_dlp` | Run `pip install -U yt-dlp`. |
| A download fails on a video that used to work | Update yt-dlp: platforms change often. |
| The video asks you to sign in | Sign in with your browser and use `--cookies`. |

When an error occurs, the program shows it with the `Error:` prefix and exits without writing credits or touching the index.
