# Changelog

[Español](CHANGELOG.md) · **English**

This file follows the [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format and the project uses [semantic versioning](https://semver.org/).

## [Unreleased]

### Planned

- Graphical window, download queue, several segments per video and `.exe` build.

## [0.2.0] - 2026-11-16

### Added

- Instagram and TikTok support.
- `--transcribir` option: local transcription of the clip to `transcripcion_original.txt` and `.srt`, with automatic language detection and use of an NVIDIA card when available.
- `--traducir` option: local translation of the transcription into Spanish, English and Italian, as `.txt` and `.srt` with the same timings.
- `fuente.json` records the detected language, the detection confidence and the transcription model.

### Changed

- Download errors are explained in plain language: sign-in required, unsupported link, video unavailable or no connection.

## [0.1.0] - 2026-11-16

### Added

- Download of a segment (start and end) of a video, as MP4 and up to 1080p, cut exactly at the requested times.
- Command-line use: `python -m clip_harvester URL START END --tipo Propio|Edu|Referencia`.
- `--notas`, `--cookies` and `--salida` options.
- One folder per clip with `clip.mp4`, `CREDITOS.txt` and `fuente.json`.
- Classification by type of use and `NO-PUBLICAR` mark for `Referencia` clips.
- General `_indice.csv` index, compatible with Excel.
- Segment validation against the video's duration.
- Tested with YouTube.
