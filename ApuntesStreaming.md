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
- Fórmula de ancho de banda: ```$$ BW_{total} = BW_{stream} \times N_{usuarios} $$ ```
