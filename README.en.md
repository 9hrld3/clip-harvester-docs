# Clip Harvester

[Español](README.md) · **English**

Clip Harvester is a Windows desktop tool that downloads **only the segment you need** from an online video and saves it together with its **credits**: title, author, links, segment, date, type of use and the license stated by the platform. On request, it also transcribes what is said in the clip and translates it into Spanish, English and Italian.

It runs **100% on your machine**: no accounts, no API keys and no cloud services. It only uses the internet to fetch the source video and, one single time, the transcription and translation models.

> **Read the [responsible use notice](RESPONSIBLE_USE.md) before using it.** Giving credit is not the same as having permission.

> This repository contains documentation only. The source code is kept in a private repository.

## Status

Version 0.2.0: command-line use, with YouTube, Instagram and TikTok, and with local transcription and translation. See the [changelog](CHANGELOG.en.md) and the [roadmap](#roadmap).

## What it does

- Downloads only the requested segment (start and end), not the whole video.
- Cuts exactly at the requested times, as MP4 and up to 1080p.
- Creates one folder per clip with the video, a readable credits file and the same data as JSON.
- Classifies each clip by type of use: `Propio` (own), `Edu` or `Referencia` (reference).
- Marks `Referencia` clips as **NO-PUBLICAR** (do not publish), both in the folder name and in the credits.
- Keeps a general CSV index that opens directly in Excel.
- Works with YouTube, Instagram and TikTok links.
- Optionally transcribes the clip's audio to text (`.txt`) and subtitles (`.srt`), without sending it to any service.
- Optionally translates the transcription into Spanish, English and Italian, keeping the subtitle timings.

The command-line options, file names and credit fields are in Spanish; this documentation shows them as they are.

## Requirements

- Windows 10 or 11
- Python 3.11 or later
- [ffmpeg](https://ffmpeg.org/) available on the `PATH`
- [yt-dlp](https://github.com/yt-dlp/yt-dlp) (Python package)
- To transcribe: [faster-whisper](https://github.com/SYSTRAN/faster-whisper) and several GB of free disk space for the model
- To translate: [Argos Translate](https://github.com/argosopentech/argos-translate)
- Optional: an NVIDIA graphics card to speed up transcription. Without one, the processor is used.

## Installation

1. Install Python 3.11 or later and ffmpeg. Check that both respond:

   ```bash
   python --version
   ```

   ```bash
   ffmpeg -version
   ```

2. Install the Python packages:

   ```bash
   pip install -U yt-dlp faster-whisper argostranslate
   ```

   If you only want to download clips, `yt-dlp` is enough.

3. Optional, only with an NVIDIA card, to speed up transcription:

   ```bash
   pip install nvidia-cublas-cu12 nvidia-cudnn-cu12
   ```

4. Place the `clip_harvester/` folder inside your working folder and open a terminal in that working folder.

## Usage

```bash
python -m clip_harvester URL START END --tipo Propio|Edu|Referencia
```

Example: download from 1:20 to 1:32 as reference material.

```bash
python -m clip_harvester "https://www.youtube.com/watch?v=VIDEO_ID" 1:20 1:32 --tipo Referencia --notas "Transition idea"
```

Example: download a clip of your own, transcribe it and translate it into English and Italian.

```bash
python -m clip_harvester "https://www.youtube.com/watch?v=VIDEO_ID" 0:10 0:45 --tipo Propio --traducir en,it
```

| Argument | Required | Description |
|---|---|---|
| `URL` | yes | Link to the video. |
| `START`, `END` | yes | Segment times: `80`, `1:20` or `0:01:20.5`. |
| `--tipo` | yes | Type of use: `Propio`, `Edu` or `Referencia`. |
| `--notas` | no | Free-text notes saved in the credits. |
| `--cookies BROWSER` | no | Use a browser's session (`chrome`, `edge`, `firefox`). |
| `--salida FOLDER` | no | Output folder. Defaults to `clips`. |
| `--transcribir` | no | Generates the clip's transcription as `.txt` and `.srt`. |
| `--traducir LANGUAGES` | no | Translates the transcription: `es`, `en`, `it`, comma-separated. Includes the transcription. |

Full details are in the [usage guide](docs/usage-guide.md).

## Output folder structure

```text
clips/
├── _indice.csv
├── Propio/
├── Edu/
└── Referencia/
    └── 2026-11-16_example-channel-video-title_01m20-01m32_NO-PUBLICAR/
        ├── clip.mp4
        ├── CREDITOS.txt
        ├── fuente.json
        ├── transcripcion_original.txt   (with --transcribir)
        ├── transcripcion_original.srt
        ├── traduccion_en.txt            (with --traducir)
        └── traduccion_en.srt
```

- **`clip.mp4`**: the downloaded segment.
- **`CREDITOS.txt`**: credits as readable text.
- **`fuente.json`**: the same data in structured form.
- **`transcripcion_original.*`**: what is said in the clip, in its original language, as text and as subtitles.
- **`traduccion_<language>.*`**: the translated transcription, one pair of files per language.
- **`_indice.csv`**: one row per downloaded clip.

Each folder is named `<date>_<author>-<title>_<segment>`. `Referencia` clips get the `_NO-PUBLICAR` suffix.

## Privacy

Clip Harvester does not send data to any service of its own or of third parties. Transcription and translation run on your machine: neither the video, nor the audio, nor the text leave it.

The only connections it makes are to the source platform, to fetch the video, and to the public model repositories, the first time you transcribe or translate.

## Roadmap

- [x] Segment download with credits and index (command line, YouTube)
- [x] Instagram and TikTok
- [x] Local transcription (`.txt` and `.srt`)
- [x] Local translation into Spanish, English and Italian
- [ ] Graphical window, download queue, several segments per video and `.exe` build

## Documentation

- [Usage guide](docs/usage-guide.md)
- [Responsible use notice](RESPONSIBLE_USE.md)
- [Changelog](CHANGELOG.en.md)

## License

Documentation © 2026 HRLD CORP, released under [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE). This license covers the documentation in this repository, not the application code or any video downloaded with it.
