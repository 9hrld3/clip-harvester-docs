# Registro de cambios

**Español** · [English](CHANGELOG.en.md)

Este archivo sigue el formato de [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/) y el proyecto usa [versionado semántico](https://semver.org/lang/es/).

## [Sin publicar]

### Previsto

- Ventana gráfica, cola de descargas, varios tramos por video y ejecutable `.exe`.

## [0.2.0] - 2026-11-16

### Añadido

- Compatibilidad con Instagram y TikTok.
- Opción `--transcribir`: transcripción local del clip a `transcripcion_original.txt` y `.srt`, con detección automática del idioma y uso de tarjeta NVIDIA si está disponible.
- Opción `--traducir`: traducción local de la transcripción a español, inglés e italiano, en `.txt` y `.srt` con los mismos tiempos.
- `fuente.json` registra el idioma detectado, la confianza de la detección y el modelo de transcripción.

### Cambiado

- Los errores de descarga se explican en lenguaje claro: inicio de sesión requerido, enlace no compatible, video no disponible o falta de conexión.

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
