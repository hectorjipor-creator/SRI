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
- Flujo continuo por TCP.
- Puertos: 80, 443, 8000.
- Formatos: MP3, OGG, AAC.

**HTTP Adaptativo (HLS/DASH)**
- No es flujo continuo.
- El vídeo se divide en chunks de 2–10 segundos.
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
