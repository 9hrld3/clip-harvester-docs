# Clip Harvester

**Español** · [English](README.en.md)

Clip Harvester es una herramienta de escritorio para Windows que descarga **solo el tramo que necesitas** de un video en línea y lo guarda junto con sus **créditos**: título, autor, enlaces, tramo, fecha, tipo de uso y licencia indicada por la plataforma.

Funciona **100 % en tu equipo**: sin cuentas, sin claves de API y sin servicios en la nube. Solo usa internet para bajar el video de origen.

> **Antes de usarla, lee el [aviso de uso responsable](USO_RESPONSABLE.md).** Dar el crédito no equivale a tener permiso.

> Este repositorio contiene únicamente la documentación. El código fuente se mantiene en un repositorio privado.

## Estado

Versión 0.1.0: uso por consola, probado con YouTube. Consulta el [registro de cambios](CHANGELOG.md) y la [hoja de ruta](#hoja-de-ruta).

## Qué hace

- Descarga únicamente el tramo indicado (inicio y fin), no el video completo.
- Corta con exactitud en los tiempos pedidos, en MP4 y hasta 1080p.
- Crea una carpeta por clip con el video, un archivo de créditos legible y los mismos datos en JSON.
- Clasifica cada clip por tipo de uso: `Propio`, `Edu` o `Referencia`.
- Marca los clips de `Referencia` como **NO-PUBLICAR**, en el nombre de la carpeta y en los créditos.
- Mantiene un índice general en CSV que se abre directamente en Excel.

## Requisitos

- Windows 10 u 11
- Python 3.11 o superior
- [ffmpeg](https://ffmpeg.org/) disponible en el `PATH`
- [yt-dlp](https://github.com/yt-dlp/yt-dlp) (paquete de Python)

## Instalación

1. Instala Python 3.11 o superior y ffmpeg. Comprueba que responden:

   ```bash
   python --version
   ```

   ```bash
   ffmpeg -version
   ```

2. Instala yt-dlp:

   ```bash
   pip install -U yt-dlp
   ```

3. Coloca la carpeta `clip_harvester/` dentro de tu carpeta de trabajo y abre una terminal en esa carpeta de trabajo.

## Uso

```bash
python -m clip_harvester URL INICIO FIN --tipo Propio|Edu|Referencia
```

Ejemplo: descargar del minuto 1:20 al 1:32 como material de referencia.

```bash
python -m clip_harvester "https://www.youtube.com/watch?v=ID_DEL_VIDEO" 1:20 1:32 --tipo Referencia --notas "Idea de transición"
```

| Argumento | Obligatorio | Descripción |
|---|---|---|
| `URL` | sí | Enlace del video. |
| `INICIO`, `FIN` | sí | Tiempos del tramo: `80`, `1:20` o `0:01:20.5`. |
| `--tipo` | sí | `Propio`, `Edu` o `Referencia`. |
| `--notas` | no | Texto libre que se guarda en los créditos. |
| `--cookies NAVEGADOR` | no | Usa la sesión de un navegador (`chrome`, `edge`, `firefox`). |
| `--salida CARPETA` | no | Carpeta de salida. Por defecto, `clips`. |

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
        └── fuente.json
```

- **`clip.mp4`**: el tramo descargado.
- **`CREDITOS.txt`**: créditos en texto legible.
- **`fuente.json`**: los mismos datos, en formato estructurado.
- **`_indice.csv`**: una fila por cada clip descargado.

Cada carpeta se nombra `<fecha>_<autor>-<título>_<tramo>`. Los clips de `Referencia` añaden el sufijo `_NO-PUBLICAR`.

## Privacidad

Clip Harvester no envía datos a ningún servicio propio ni de terceros. La única conexión que realiza es con la plataforma de origen para obtener el video.

## Hoja de ruta

- [x] Descarga de un tramo con créditos e índice (consola, YouTube)
- [ ] Instagram y TikTok
- [ ] Transcripción local (`.txt` y `.srt`)
- [ ] Traducción local a español, inglés e italiano
- [ ] Ventana gráfica, cola de descargas, varios tramos por video y ejecutable `.exe`

## Documentación

- [Guía de uso](docs/guia-de-uso.md)
- [Aviso de uso responsable](USO_RESPONSABLE.md)
- [Registro de cambios](CHANGELOG.md)

## Licencia

Documentación © 2026 HRLD CORP, publicada bajo [Creative Commons Atribución 4.0 Internacional (CC BY 4.0)](LICENSE). Esta licencia cubre la documentación de este repositorio, no el código de la aplicación ni los videos que se descarguen con ella.
