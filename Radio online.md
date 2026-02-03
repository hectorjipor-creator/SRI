# Práctica 1 – Streaming con Icecast2 y Mixxx

## 1. Servidor de Streaming (Ubuntu 24 + Icecast2) 

### 1.1 Configuración de red (Adaptador puente) 
Se configura la máquina virtual en modo *Adaptador puente* y se obtiene la IP: 
```bash
ip a
```
Editamos nuestra ip como estática para poder realizar de manera correcta la práctica ya que luego vamos a tener que conectar con la máquina DJ

### 1.2 Comprobación de sonido

Lo primero es instalar las utilidades necesarias mediante el comando:
```bash
sudo apt install alsa-utils -y
```

Una vez instalado alsa-utils, hacemos una verificación de dispositivos mediante el comando:
```bash
aplay -l
```

Y una reproducción de prueba mediante el comando:
```bash
speaker-test -c 2
```

### 1.3 Instalación de Icecast2

Instalamos Icecast2 de la siguiente manera:
```bash
sudo apt update
sudo apt install icecast2 -y
```

Durante la instalación se nos pedirá introducir los siguientes datos:
- Hostname -> localhost
- Cotraseña admin -> La que consideremos
- Contraseña de Streaming (source-password) -> Esta contraseña es importante para usar luego Mixxx

Si quisieramos recofingurar alguno de estos datos se puede realizar mediante el comando:
```bash
sudo dpkg-reconfigure icecast2
```

Y el archivo de configuración está en la siguiente ruta:
```/etc/icecast2/icecast.xml```

Tras haber introducido todos estos datos, reiniciamos Icecast mediante el comando:
```bash
sudo systemctl restart icecast2
```

Antes de realziar el acceso a la inerfaz web, comprobamos el puerto 8000 mediante el comando:
```bash
sudo ss -tulnp | grep 8000
```

Debería aparecer icecast2 escuchando en 0.0.0.0:8000

Tras haber reiniciado el servicio ycomprobado el puerto 8000, si todo ha funcionado correctamente, deberíamos poder acceder a la interfaz web a través de la siguiente URL:
```http://172.30.16.101:8000```

En este caso mi IP es la siguiente: 172.30.16.101

## 2. Máquina DJ (Ubuntu 24 + Mixxx)

### 2.1 COnfiguración de red
AL igual que con la máquina de servidor de streaming, ponemos el adaptador de red en modo adaptador puente y configuramos la IP que nos de el adapatador como IP estática.

### 2.2 Comprobación de sonido

Al igual que en el servidor, hacemos una comprobación de sonido mediante el comando:
```bash
aplay -l
```

### 2.3 Instalación de Mixxx
Una vez realizada la prueba de sonido, instalamos Mixxx con los siguientes comandos:
```bash
sudo add-apt-repository ppa:mixxx/mixxx
sudo apt update
sudo apt install mixxx
```

### 2.4 Configuración de emisión en Mixxx
Una vez instalado Mixxx, accedemos a este servicio y en Mixxx -> Preferencias -> Live Broadcasting modificamos lo siguiente:
- Tipo: Icecast 2
- Servidor: 172.30.16.101 (la IP de nuestro servidor Icecast)
- Puerto: 8000
- Mountpoint: /hectorsri
- Usuario: source
- Contraseña: la que habíamos configurado en Icecast
- Formato: MP3 u OGG

Una vez configurado, al pulsar ```Conectar```, en el panel de Icecast aparecerá el mountpoint ```/hectorsri``` con los oyentes conectados.

### 2.5 Pruebas desde la máquina anfitrión
1. Escuchar desde el navegador
   - URL: ```http://172.30.16.101:8000/hectorsri```
   - Si todo está bien configurado, el navegador empezará a reproducir el audio de Mixxx.
2. Escuchar desde VLC
   - En VLC -> "Medio" -> "Abrir ubicación de red":
     - Introducimos:  ```http://172.30.16.101:8000/hectorsri```
     - Se reproducirá el mismo stream

### 2.6 Escuchar la radio de un compañero
Para poder escuchar la radio de un compañero, esté tiene que tener su propio servidor o mountpoint configurado para poder acceder a su radio.

1. En nuestra máquina introducimos en el navegador por URL lo siguiente:
   ```http://172.30.16.102:8000/radio-asir```
   
En este caso, la ip del compañero es 172.30.16.101 y su mountpoint es radio-asir. Si todo está configurado correctamente tanto en nuestra máquina como en la suya, una vez introduzcamos la URL deberíamos empezar a escuchar la emisión de nuestro compañero en nuestra máquina.
