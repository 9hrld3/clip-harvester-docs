# Clip Harvester

**Español** · [English](README.en.md)

Clip Harvester es una herramienta de escritorio para Windows que descarga **solo el tramo que necesitas** de un video en línea y lo guarda junto con sus **créditos**: título, autor, enlaces, tramo, fecha, tipo de uso y licencia indicada por la plataforma. Si lo pides, también transcribe lo que se dice en el clip y lo traduce a español, inglés e italiano.

Funciona **100 % en tu equipo**: sin cuentas, sin claves de API y sin servicios en la nube. Solo usa internet para bajar el video de origen y, una única vez, los modelos de transcripción y traducción.

> **Antes de usarla, lee el [aviso de uso responsable](USO_RESPONSABLE.md).** Dar el crédito no equivale a tener permiso.

> Este repositorio contiene únicamente la documentación. El código fuente se mantiene en un repositorio privado.

## Estado

Versión 0.2.0: uso por consola, con YouTube, Instagram y TikTok, y con transcripción y traducción locales. Consulta el [registro de cambios](CHANGELOG.md) y la [hoja de ruta](#hoja-de-ruta).

## Qué hace

- Descarga únicamente el tramo indicado (inicio y fin), no el video completo.
- Corta con exactitud en los tiempos pedidos, en MP4 y hasta 1080p.
- Crea una carpeta por clip con el video, un archivo de créditos legible y los mismos datos en JSON.
- Clasifica cada clip por tipo de uso: `Propio`, `Edu` o `Referencia`.
- Marca los clips de `Referencia` como **NO-PUBLICAR**, en el nombre de la carpeta y en los créditos.
- Mantiene un índice general en CSV que se abre directamente en Excel.
- Funciona con enlaces de YouTube, Instagram y TikTok.
- Opcionalmente, transcribe el audio del clip a texto (`.txt`) y subtítulos (`.srt`), sin enviarlo a ningún servicio.
- Opcionalmente, traduce la transcripción a español, inglés e italiano, conservando los tiempos de los subtítulos.

## Requisitos

- Windows 10 u 11
- Python 3.11 o superior
- [ffmpeg](https://ffmpeg.org/) disponible en el `PATH`
- [yt-dlp](https://github.com/yt-dlp/yt-dlp) (paquete de Python)
- Para transcribir: [faster-whisper](https://github.com/SYSTRAN/faster-whisper) y varios GB de espacio libre para el modelo
- Para traducir: [Argos Translate](https://github.com/argosopentech/argos-translate)
- Opcional: tarjeta gráfica NVIDIA para acelerar la transcripción. Sin ella, se usa el procesador.

## Instalación

1. Instala Python 3.11 o superior y ffmpeg. Comprueba que responden:

   ```bash
   python --version
   ```

   ```bash
   ffmpeg -version
   ```

2. Instala los paquetes de Python:

   ```bash
   pip install -U yt-dlp faster-whisper argostranslate
   ```

   Si solo vas a descargar clips, basta con `yt-dlp`.

3. Opcional, solo con tarjeta NVIDIA, para acelerar la transcripción:

   ```bash
   pip install nvidia-cublas-cu12 nvidia-cudnn-cu12
   ```

4. Coloca la carpeta `clip_harvester/` dentro de tu carpeta de trabajo y abre una terminal en esa carpeta de trabajo.

## Uso

```bash
python -m clip_harvester URL INICIO FIN --tipo Propio|Edu|Referencia
```

Ejemplo: descargar del minuto 1:20 al 1:32 como material de referencia.

```bash
python -m clip_harvester "https://www.youtube.com/watch?v=ID_DEL_VIDEO" 1:20 1:32 --tipo Referencia --notas "Idea de transición"
```

Ejemplo: descargar un clip propio, transcribirlo y traducirlo a inglés e italiano.

```bash
python -m clip_harvester "https://www.youtube.com/watch?v=ID_DEL_VIDEO" 0:10 0:45 --tipo Propio --traducir en,it
```

| Argumento | Obligatorio | Descripción |
|---|---|---|
| `URL` | sí | Enlace del video. |
| `INICIO`, `FIN` | sí | Tiempos del tramo: `80`, `1:20` o `0:01:20.5`. |
| `--tipo` | sí | `Propio`, `Edu` o `Referencia`. |
| `--notas` | no | Texto libre que se guarda en los créditos. |
| `--cookies NAVEGADOR` | no | Usa la sesión de un navegador (`chrome`, `edge`, `firefox`). |
| `--salida CARPETA` | no | Carpeta de salida. Por defecto, `clips`. |
| `--transcribir` | no | Genera la transcripción del clip en `.txt` y `.srt`. |
| `--traducir IDIOMAS` | no | Traduce la transcripción: `es`, `en`, `it`, separados por comas. Incluye la transcripción. |

Todos los detalles están en la [guía de uso](docs/guia-de-uso.md).

## Estructura de la carpeta de salida

```text
clips/
├── _indice.csv
├── Propio/
├── Edu/
└── Referencia/
    └── 2026-11-16_canal-ejemplo-titulo-del-video_01m20-01m32_NO-PUBLICAR/
        ├── clip.mp4
        ├── CREDITOS.txt
        ├── fuente.json
        ├── transcripcion_original.txt   (con --transcribir)
        ├── transcripcion_original.srt
        ├── traduccion_en.txt            (con --traducir)
        └── traduccion_en.srt
```

- **`clip.mp4`**: el tramo descargado.
- **`CREDITOS.txt`**: créditos en texto legible.
- **`fuente.json`**: los mismos datos, en formato estructurado.
- **`transcripcion_original.*`**: lo que se dice en el clip, en su idioma original, como texto y como subtítulos.
- **`traduccion_<idioma>.*`**: la transcripción traducida, un par de archivos por idioma.
- **`_indice.csv`**: una fila por cada clip descargado.

Cada carpeta se nombra `<fecha>_<autor>-<título>_<tramo>`. Los clips de `Referencia` añaden el sufijo `_NO-PUBLICAR`.

## Privacidad

Clip Harvester no envía datos a ningún servicio propio ni de terceros. La transcripción y la traducción se hacen en tu equipo: ni el video, ni el audio, ni el texto salen de él.

Las únicas conexiones que realiza son con la plataforma de origen, para obtener el video, y con los repositorios públicos de los modelos, la primera vez que transcribes o traduces.

## Hoja de ruta

- [x] Descarga de un tramo con créditos e índice (consola, YouTube)
- [x] Instagram y TikTok
- [x] Transcripción local (`.txt` y `.srt`)
- [x] Traducción local a español, inglés e italiano
- [ ] Ventana gráfica, cola de descargas, varios tramos por video y ejecutable `.exe`

## Documentación

- [Guía de uso](docs/guia-de-uso.md)
- [Aviso de uso responsable](USO_RESPONSABLE.md)
- [Registro de cambios](CHANGELOG.md)

## Licencia

Documentación © 2026 HRLD CORP, publicada bajo [Creative Commons Atribución 4.0 Internacional (CC BY 4.0)](LICENSE). Esta licencia cubre la documentación de este repositorio, no el código de la aplicación ni los videos que se descarguen con ella.
