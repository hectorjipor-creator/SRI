# Fórmulas importantes para la resolución de ejercicios

## 1. Conversión entre unidades

### Bits ↔ Bytes

```bash
1 Byte = 8 bits
Bytes = bits / 8
```

## Unidades digitales

```bash
1 kB = 1000 B
1 MB = 1000 kB
1 GB = 1000 MB

1 kbps = 1000 bps
1 Mbps = 1000 kbps
1 Gbps = 1000 Mbps
```

## 2. Tamaño de archivo a partir del bitrate

### Fórmula general

```bash
Tamaño (bits) = bitrate × tiempo (s)
Tamaño (Bytes) = bits / 8
Tamaño (MB) = Bytes / 1.000.000
```

**Explicación:**
El bitrate indica cuántos bits se generan por segundo. Multiplicarlo por el tiempo da el tamaño total del archivo.

## 3. Bitrate sin compresión

### Audio PCM (sin compresión)

```bash
Bitrate = frecuencia_muestreo × profundidad_bits × canales
```

**Ejemplo:**
48.000 Hz × 24 bits × 2 canales = 2.304.000 bps

### Vídeo sin compresión

```bash
Bitrate = ancho × alto × bits_por_píxel × fps
```

**Ejemplo:**
7680 × 4320 × 30 bits × 60 fps

## 4. Horas almacenables en un disco

### Fórmula

```bash
Tiempo (s) = capacidad_total_bits / bitrate
Horas = Tiempo / 3600
```

**Explicación:**
Se divide el total de bits disponibles entre los bits consumidos por segundo.

## 5. Streaming Unicast

### Fórmula

```bash
Consumo_total = bitrate_usuario × número_usuarios
```

**Explicación:**
Cada usuario recibe su propia copia del flujo.

## 6. Streaming Multicast

### Fórmula

```bash
Consumo_total = bitrate_del_flujo
```

**Explicación:**
El servidor solo envía una copia, independientemente del número de oyentes.

## 7. Porcentaje de uso de red

### Fórmula

```bash
Porcentaje = (consumo / capacidad_total) × 100
```

## 8. Déficit de ancho de banda

### Fórmula

```bash
Déficit = consumo_total - capacidad_disponible
```

## 9. Conversión de tiempo

```bash
Segundos = minutos × 60
Segundos = horas × 3600
```

# Ejercicios Resueltos Streaming

## 1. Con un bitrate de 14.93 Gbps, ¿cuánto espacio de disco ocupará una toma de 10 segundos?

14,93 Gbps x 10 segundos = 149,3 Gb / 8 bits *(para pasar a Bytes)* = **18,66 GB**

## 2. Si emites un streaming de audio a un bitrate constante *(CBR)* de 128 kbps y tienes 25 oyentes simultáneos en una red Unicast *(uno para cada uno)*, ¿cuál es el ancho de banda total consumido?

128 kbps x 25 oyentes = 3200 kbps / 1000 *(de kb a Mb)* = **3,2 Mbps**

## 3. Tienes un disco de 500 GB. ¿Cuántas horas de vídeo HD a 2 Mbps podrías alojar aproximadamente?

2 Mbps / 8 *(de b a B)* = 0,25 MBps

500 GB / 0,25 MBps = 500000 MB / 0,25 MBps = 2000000 s / 3600 s = 555,5 horas = **555 horas**

## 4. Calcula el peso aproximado de un archivo de audio WAV de 5 minutos, con 44.1 kHz, 16 bits y estéreo.

5 minutos x 60 = 300 segundos

Peso = 300 s x 44100 Hz *(44,1 kHz en Hz)* x 16 bits x 2 canales = 423360000 bits

423360000 bits / 8 bits *(de bits a Bytes)* = 52920000 B

52920000 B / 10^6 *(de B a MB)* = **52,92 MB**

## 5. ¿Cuál es el bitrate de un flujo de audio que utiliza una frecuencia de muestreo de 48 kHz, una profundidad de 24 bits y un solo canal *(mono)*?

48 kHz x 24 b = 1152 kbps *(kbps = kb / s)* *(1 Hz = 1 / s)*

1152 kbps / 1000 *(pasar de kbps a Mbps)* = **1,152 Mbps**

## 6. Si tienes una conexión de 20 Mbps de subida y emites vídeo a 6 Mbps, ¿qué porcentaje de tu línea estás utilizando?

100 % es 20 Mbps

X es 6 Mbps

X = (6 x 100) / 20 = **30 %**

## 7. Si 4 alumnos emiten a 6 Mbps cada uno en una línea de 20 Mbps de subida, ¿qué ocurrirá?

4 alumnos x 6 Mbps = 24 Mbps

**La red se satura ya que se supera el tope de la línea (24 Mbps > 20 Mbps) y provoca buffering**

## 8. Un servidor tiene un límite de subida de 10 Mbps. ¿Cuántos oyentes simultáneos puede soportar si cada uno consume 192 kbps?

10 Mbps x 1000 (pasar de Mbps a kbps) = 10000 kbps

10000 kbps / 192 kbps = **52 oyentes**

## 9. Un estudio graba en RAW a 3840x2160, a 60 fps y 30 bits de color. ¿Cuál es el bitrate resultante en Gbps?

3840 x 2160 x 60 x 30 = 1,493 x 10^10 bps / 10 ^9 (bps a Gbps) = **14,93 Gbps**

*K = 10^3, M = 10^6, G = 10^9*

## 10. En una línea de 100 Mbps simétricos, ¿cuántos usuarios podrían ver un streaming de vídeo de 2 Mbps?

100 Mbps / 2 Mbps = **50 usuarios**

# Conversión de unidades

<img width="1385" height="779" alt="Conversion Unidades" src="https://github.com/user-attachments/assets/d44e2a8a-369b-4d3f-8e1f-af8935b90da8" />
