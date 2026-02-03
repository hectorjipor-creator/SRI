# Apuntes Servicios de Streaming

## 1. Descarga directa vs Streaming

### Descarga directa
- El usuario solicita un fichero completo (ej. 100 MB).
- El servidor envía todo el archivo, aunque el usuario no lo consuma entero.
- Se almacena localmente (buffer + disco).
- Ejemplo: descargas de Mega, Google Drive, etc.

**Problema:**
Si el usuario solo escucha 2 minutos de un audio de 10, el servidor igualmente ha enviado los 100 MB.

### Streaming
- El servidor envía datos en flujo continuo, no un archivo completo.
- No hay almacenamiento permanente.
- Solo se consume el ancho de banda equivalente al tiempo reproducido.
- Ejemplo: Spotify, radios online, YouTube en directo.

**Ventaja:**
Optimiza el ancho de banda: si escuchas 2 minutos, solo consumes 2 minutos.

## 2. Topologías de red aplicadas al Streaming

### Unicast
- Conexión 1 a 1 entre servidor y cliente.
- Si hay 100 oyentes, el servidor abre 100 sockets TCP y envía 100 veces el mismo flujo.
- Fórmula de ancho de banda: ```BW(total) = BW (stream) x N(usuarios)```

**Desventaja:**
Muy poco escalable

 ### Multicast
 - El servidor envía a una dirección multicast (224.0.0.0 – 239.255.255.255).
 - Los routers replican el tráfico solo si hay suscriptores.

**Desventaja:**
Internet público bloquea multicast → solo útil en redes internas.

### Broadcast
- Envío a toda la red local.
- No se usa para streaming profesional.

## 3.  Capa de transporte TCP vs UDP

### TCP
- Fiable: si un paquete se pierde, se retransmite.
- Usa ACK/NACK.
- Pasa bien por firewalls, NAT y proxys.
- **Desventaja:** mayor latencia.

Ideal para:
- Streaming no interactivo (radio, Netflix, Spotify).
- Descargas.
- HTTP.

### UDP
- No hay retransmisión.
- Baja latencia.
- Puede perder paquetes → artefactos, cortes.

Ideal para:
- Videollamadas.
- Juegos online.
- WebRTC.
- RTSP.

## 4. QoS: Jitter y Buffer

### Jitter
Variación en el tiempo de llegada de los paquetes.

**Ejemplo:**
- Paquete 1 → 20 ms
- Paquete 2 → 150 ms
- Paquete 3 → 20 ms

Si el jitter supera el tamaño del buffer -> **cortes de audio**

### Buffer
Memoria temporal para absorber jitter.
- A mayor buffer → más estabilidad.
- Pero también → **más latencia**.

### Burst-on-Connect (Icecast)
Característica específica de servidores como Icecast.

- Al conectarse un oyente, el servidor envía una ráfaga inicial (ej. 64 KB) a máxima velocidad.
- El buffer se llena casi instantáneamente.
- Reduce el *time-to-first-byte*.
- **Problema:** Al conectarse, el oyente tardaría varios segundos en llenar su buffer a velocidad normal (1x).
- **Solución (Burst):** El servidor envía los datos iniciales (ej. 64KB) a la máxima velocidad posible que permita la red (ej. 10x), llenando el buffer del cliente casi instantáneamente para que el audio empiece a sonar de inmediato (Time-to-first-byte reducido).

## 5. Protocolos de Streaming

### 5.1. Capa de transporte
- TCP → calidad, latencia alta.
- UDP → baja latencia, posible pérdida.

### 5.2 Capa de aplicación (3 modelos)

**HTTP Legacy (Icecast2)**
- Protocolo: ICY
- Flujo continuo por TCP (el servidor envía flujo de datos sin parar hasta que el cliente cierra la conexión).
- Puertos: 80, 443, 8000.
- Formatos: MP3, OGG, AAC (flujo continuo de bytes).

**HTTP Adaptativo (HTTP Live Streaming de Apple / MPEG-DASH)**
- No es flujo continuo. 
- El servidor trocea el fichero en pequeños trozos (chunks) de 2 a 10 segundos.
- Formatos: ```.ts```, ```.m4s```.
- El cliente elige calidad según su ancho de banda → calidad adaptativa.

Usado por:
- Netflix
- Youtube
- Disney+
- HBO

**Real-Time**
- RTMP
  - TCP.
  - Obsoleto para usuarios finales.
  - Se usa para ingesta (OBS → YouTube/Twitch).
- RTSP
  - Cámaras IP.
  - UDP para datos, TCP para control.
  - Problemas con NAT.
- WebRTC
  - Videollamadas.
  - P2P, cifrado.
  - UDP.
  - Ultra baja latencia (<0.5s).

### Cuadro resumen

| Protocolo     | Base      | Latencia   | Uso                | Firewall      | CDN       |
|---------------|-----------|------------|---------------------|---------------|-----------|
| Icecast (ICY) | TCP/HTTP  | 10–30 s    | Radio               | Muy fácil     | Difícil   |
| HLS/DASH      | TCP/HTTP  | 15–45 s    | Vídeo bajo demanda  | Muy fácil     | Excelente |
| RTMP          | TCP       | 2–5 s      | Ingesta             | Medio         | No        |
| WebRTC        | UDP/TCP   | —          | Videollamadas       | Complejo      | No        |
| RTSP          | UDP+TCP   | —          | Cámaras             | Problemas NAT | No        |

## 6. Icecast2
Icecast es un servidor de streaming de audio.
No genera contenido → necesita un source client como:
- Mixxx
- Butt

Características:
- Formatos: MP3, OGG.
- Puntos de montaje.
- Gestión de oyentes.
- Web admin.

### Instalación:
```bash
apt update
apt install icecast2
```

Configurar:
- source-password
- admin-password
- puerto 8000

## 7. Mixxx (emisor)

### Instalación:
```bash
add-apt-repository ppa:mixxx/mixxx
apt update
apt install mixxx
```

Configuración:
- Tipo: Icecast2
- Montaje: /hectorsri
- Servidor: 172.30.16.101
- Puerto: 8000
- Usuario: source
- Contraseña: la configurada en Icecast

## 8. Códecs de Audio

Son algoritmos que permiten la compresión de los ficheros de audio/vídeo. También sirven para la descompresión y para reducir el trasiego de información sin perder calidad. 

### Códecs con pérdida
- Eliminan información irrelevante.
- No recuperable.
- Ejemplo: MP3, AAC, Vorbis.

### Códecs sin pérdida
- No eliminan información.
- Menor compresión.
- Ejemplo: FLAC, WAV.

## 9. Frecuencia de muestreo
Número de muestras por segundo. El audio es una onda analógica. Para digitalizarla hay que muestrearla, algo así como hacerle fotos cada X tiempo. 

Estándar: **44.1 kHZ** (calidad CD).

## 10. Profunidad de bits
La profundidad es la calidad de dicha foto. Se trata de la cantidad de bits que se transmiten por segundo
Bits por muestra.

Estándar: **16 bits**.
A mayor profundidad → más rango dinámico → más calidad.

## 11. Canales
Número de audios independientes que viajan en el mismo stream.
- Mono (1)
- Estéreo (2)
- 5.1
- 7.1
- ...

## 12. Cálculo de peso (audio)

``` Peso = Frecuencia x Bits x Canales x Segundos ```

Ejemplo WAV sin compresión:

```44.100 × 16 × 2 × 180 = 31.75 MB ```

## 13. Vídeo: conceptos clave
La lógica y conceptos para el vídeo son muy similares a los del audio. También contamos con protocolos y códecs pero se introduce el concepto de contenedor. 

### Contenedor
Formato de fichero que incluye:
-  Pistas de vídeo.
-  Pistas de audio.
-  Subtítulos.
-  Metadatos.
-  Ejemplos: ```MP4```, ```MKV```, ```MOV```

### Cálculo de peso sin comprimir

``` Peso = (Ancho x Alto) x Profundidad de Color x FPS x Tiempo ```

- Resolución:
  - 1080p → 1920 x 1080
  - 4K → 4096 × 2160
  - 8K → 7680 × 4320
- Profundidad de color: bits usados para definir el color de cada píxel (24 habitualmente bits: 8+8+8).
- FPS: FPS: frames, fotos, por segundo.

### Con códec (comprimido)
Al utilizar un códec, comprimo el vídeo y ya no se envía el vídeo píxel a píxel puesto que el códec ha decidido qué píxeles ha mantenido y cuáles ha eliminado. El bitrate es el dato que nos interesa en el caso de ficheros comprimidos.

``` Peso = Bitrate x Tiempo ```

El bitrate es la cantidad de información que puede enviarse por segundo.

## 14. Bitrates recomendados

| Resolución      | Calidad    | Bitrate Mínimo | Bitrate Recomendado |
|-----------------|------------|----------------|-----------------------|
| 4K (2160p)      | Ultra HD   | 15 Mbps        | 25 - 45 Mbps         |
| 1080p (Full HD) | Alta       | 4 Mbps         | 6 - 9 Mbps           |
| 720p (HD)       | Media      | 1.5 Mbps       | 3 - 4 Mbps           |
| 480p (SD)       | Estándar   | 500 kbps       | 1 Mbps               |
| 360p            | Baja       | 400 kbps       | 700 kbps             |

## 15. FFmpeg

### Remuxing
Cambiar contenedor sin recodificar:
```bash
ffmpeg -i original.mp4 -c:v copy -c:a copy salida.mkv
```

- No cambia tamaño significativamente.
- No usa CPU.

### Cambio de códec

**H.264:**
```bash
ffmpeg -i video.mp4 -c:v libx264 -b:v 2M -c:a copy h264.mp4
```

**H.265:**
```bash
ffmpeg -i video.mp4 -c:v libx265 -b:v 2M -c:a copy h265.mp4
```

H.265 = mejor compresión → menos artefactos a igual bitrate.

## 16. Conversión de unidades

<img width="1385" height="779" alt="Conversion Unidades" src="https://github.com/user-attachments/assets/d4f8f868-24a3-47a5-8ec7-a14dcdcf1484" />
