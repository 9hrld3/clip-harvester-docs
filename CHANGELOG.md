# Registro de cambios

**Español** · [English](CHANGELOG.en.md)

Este archivo sigue el formato de [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/) y el proyecto usa [versionado semántico](https://semver.org/lang/es/).

## [Sin publicar]

### Previsto

- Compatibilidad con Instagram y TikTok.
- Transcripción local a `.txt` y `.srt`.
- Traducción local a español, inglés e italiano.
- Ventana gráfica, cola de descargas, varios tramos por video y ejecutable `.exe`.

## [0.1.0] - 2026-11-16

### Añadido

- Descarga de un tramo (inicio y fin) de un video, en MP4 y hasta 1080p, con corte exacto en los tiempos pedidos.
- Uso por consola: `python -m clip_harvester URL INICIO FIN --tipo Propio|Edu|Referencia`.
- Opciones `--notas`, `--cookies` y `--salida`.
- Una carpeta por clip con `clip.mp4`, `CREDITOS.txt` y `fuente.json`.
- Clasificación por tipo de uso y marca `NO-PUBLICAR` para los clips de `Referencia`.
- Índice general `_indice.csv`, compatible con Excel.
- Validación del tramo frente a la duración del video.
- Probado con YouTube.
