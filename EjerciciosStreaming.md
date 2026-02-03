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
