# Práctica 2 – Vídeo con FFmpeg

## 1. Instalación de FFmpeg en nuestra máquina

En nuestra máquina virtual introducimos los siguientes comandos:
```bash
sudo apt update
sudo apt install ffmpeg
```

Una vez instalado, hacemos una pequeña verificación con los comandos:
```bash
ffmpeg -version
ffprobe -version
```

## 2. Análisis de vídeo con ffprobe
Para realizar el análisis de un vídeo se ha utilizado el siguiente vídeo disponible en Aules:
```big-buck-bunny-1080p-30sec.mp4```

Una vez descargado, podemos hacer el análisis mediante el comando:
```bash
ffprobe -v error -show_streams big-buck-bunny-1080p-30sec.mp4
```

Esta la información que sea ha obtenido tras realizar el análisis:
- Información de vídeo:
  - Códec: H.264
  - Resolución: 1920x1080
  - Framerate: 24 fps
  - Bitrate: 5860 kbps
  - PixFmt: yuv420p
  - B-frames: 2
  - Duración: 30 segundos

- Información de audio:
  - Códec: AAC LC
  - Canales: 5.1
  - Frecuencia: 48 kHz
  - Bitrate: 192 kbps
  - Duración: 30 segundos

## 3. Remuxing
El cambio de contenedor de MP4 a MKV lo realizamos mediante el siguiente comando:
```bash
ffprobe -v error -show_streams big-buck-bunny-1080p-30sec.mp4
```

Tras realizar este cambio, una vez hecho de nuevo el análisis podemos notar que el tamaño no ha cambiado de forma muy significativa, ya que el contenido tanto de audio como de vídeo es idéntico y lo único que hemos cambiado ha sido el contenedor.

Resultados obtenidos:
- Tamaño antes del cambio de contenedor: 22.1 MB
- Tamaño tras el cambio de contenedor: 22.18 MB

Se nota que el cambio no es muy grande ya que esta diferencia de tamaño se limita única y exclusivamente a la sobrecarga del contenedor.

En este caso la carga de CPU ha sido muy baja porque no se ha recodificado nada sino que solo se han copiado flujos y, además, el proceso ha sido muy rápido, limitado casi solo por la velocidad de lectra/escritura del disco.

## 4. Cambio de códecs
Para crear el fichero en H.264 con un bitrate de 2 Mbps usamos el siguiente comando:
```bash
ffmpeg -i big-buck-bunny-1080p-30sec.mp4 -c:v libx264 -b:v 2M -c:a copy h264_2mbps.mp4
```

Datos:
- Tamaño: 8496 KB
- Bitrate real: 2320 kbps
- Velocidad 1.17x

Imagen

Para crear el fichero en H.265 con un bitrate de 2 Mbps usamos el siguiente comando:
```bash
ffmpeg -i big-buck-bunny-1080p-30sec.mp4 -c:v libx265 -b:v 2M -c:a copy h265_2mbps.mp4
```

Datos:
- Tamaño: 8622 KB
- Bitrate real: 2354 kbps
- Velocidad: 0.55x

Imagen

**¿Cuál presenta más artefactos (“cuadraditos”)?**
El formato que más artefactos a igual bitrate representa es H.264 ya que H.265 es mucho más eficiente y mantiene una mejor calidad con la misma cantidad de flujo de bits.

**Si ambos tienen el mismo bitrate (2 Mbps), ¿pesan lo mismo los archivos finales?**
En este caso los archivos finales no pesan lo mismo. Hemos obtenido los siguientes datos:
- Tamaño en H.264: 8496 KB
- Tamaño en H.265: 8622 KB

Esta pequeña diferencia normalemnete se debe al overhead del contenedor o las variaciones del propio codificador (VBR, cabeceras...)

## 5. Simulación de perfiles de Streaming
**Perfiles:**
- Low (móvil):
  - Resolución: 240 p
  - Bitrate: 400 kbps

Video

- High (fibra):
  - Resolución: 1080 p
  - Bitrate 2 Mbps

Video
