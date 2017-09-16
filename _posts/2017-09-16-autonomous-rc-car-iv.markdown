---
layout: post
title:  "Coche RC autónomo (IV)"
date:   2017-09-16 22:45:00
categories: blog development projects
tags:
- blog
- development
- projects
thumbnail: /assets/images/2017-09-16-autonomous-rc-car-iv-02.jpg
---

En esta cuarta entrega del proyecto **Construyendo un coche RC autónomo** voy a detallar cómo configurar la [cámara y sensores infrarrojos](http://amzn.to/2v5dt1P) en la [Raspberry Pi 3](http://amzn.to/2wmTn2S) para poder obtener vídeo que procesar. Trataré de poder enviar el vídeo en streaming con la menor latencia / _delay_ posible al ordenador principal utilizando el adaptador WiFi que ya viene incluido en la Raspberry Pi

![Raspberry Pi y la cámara](/assets/images/2017-09-16-autonomous-rc-car-iv-02.jpg)

Accederemos a la Raspberry Pi mediante SSH (ver post [Coche RC autónomo (III)]({{ site.url }}/2017/09/12/autonomous-rc-car-iii)). Tras [conectar la cámara a la Raspberry Pi](https://www.raspberrypi.org/documentation/usage/camera/), tendremos que activar el módulo de la cámara en el menú correspondiente de la aplicación de configuración:

```bash
sudo raspi-config

# Y activamos la opción Enable Camera
```

## Grabando vídeos

Para grabar vídeos de forma manual y ver que todo funciona, podemos ejecutar el siguiente comando:

```bash
raspivid -w 640 -h 480 -vf -b 10000000 -fps 30 -n -o testvideo.h264
```

* _-w_: Width (en pixels)
* _-h_: Height (en pixels)
* _-o_: Nombre del fichero de salida (con extensión .h264)
* _-vf_: Voltear verticalmente la imagen (si estamos tomando imágenes boca abajo)
* _-b_: Bitrate en bits/segundo (25Mbps = 25000000)
* _-fps_: Frames por segundo
* _-n_: Desactivar modo de previsualización

## Retransmitiendo en streaming

Al conectarnos por SSH, queremos visualizar en nuestro ordenador el vídeo en tiempo real. Hay varias formas para enviarlo.

Por ejemplo, podemos crear scripts (en Python, requiere cargar varias librerías como [python-picamera](https://www.raspberrypi.org/documentation/usage/camera/python/README.md)) tanto en el servidor (nuestro ordenador que recibirá las imágenes) como en la Raspberry Pi (cliente con la cámara que enviará las imágenes). De esta forma podremos configurar en el cliente la IP y puerto (que tendrá que estar abierto en el cortafuegos de nuestro ordenador) para visualizar en _streaming_ el vídeo obtenido por la cámara mediante una aplicación como VLC.

Pero como mi ordenador tiene GNU/Linux, bastará con ejecutar los siguientes comandos en nuestro ordenador (servidor) y en la Raspberry Pi (cliente). La idea es que el ordenador (que tiene la IP 192.168.1.100) escuche por el puerto 8000 lo que le vaya enviando la Raspberry Pi. Este método es muy sencillo pero puede producirse demasiado _lag_ en la visualización de las imágenes en el ordenador de control de incluso varios segundos si se utiliza la aplicación _vlc_, por lo que habrá utilizar en su lugar _mplayer_ con los siguientes parámetros de configuración:

```bash
# Comando para el servidor (nuestro 
# ordenador) a 6 frames por segundo para 
# que en cuanto lleguen imágenes de la 
# Raspberry Pi (que emite a 30fps) las muestre
nc -l 8000 | mplayer -fps 60 -cache 1024 -
```

```bash
# Comando para el cliente (Raspberry Pi 
# con la cámara, donde 192.168.1.100 es la 
# IP del ordenador, con -t 0 para duración ilimitada)
raspivid -w 640 -h 480 -t 0 -o - | nc 192.168.1.100 8000
```

El resultado será el siguiente: Un terminal donde hemos arrancado nuestro servidor de vídeo utilizando _netcat (nc)_ para recibir las imágenes, un terminal conectado mediante SSH a la Raspberry Pi donde habremos arrancado el envío de vídeo al servidor (nuestro ordenador) y una pantalla de _mplayer_ donde veremos en tiempo real lo que vaya viendo y enviando mediante la red WiFi el coche autónomo.

![Martina programando la cámara](/assets/images/2017-09-16-autonomous-rc-car-iv-01.jpg)

Si da error en el ordenador indicando _Failed to open VDPAU backend libvdpau_va_gl.so: no se puede abrir el archivo del objeto compartido_, se soluciona instalando los siguientes paquetes:
```bash
sudo apt-get install libvdpau-va-gl1
```

Asimismo, si no tenemos instalado _mplayer_, lo podemos instalar fácilmente:
```bash
sudo apt-get install mplayer
```

Podéis encontrar muchísima más información sobre el uso de la cámara y ejemplos prácticos en el ejemplar de la revista [MagPi - Camera Module Essentials](https://www.raspberrypi.org/magpi/camera-module-essentials/).

En los próximos posts del proyecto se detallará cómo acceder al streaming de la cámara usando Python para ser analizado con OpenCV.


## Más información sobre el proyecto

* [Coche RC autónomo (I)]({{ site.url }}/2017/08/23/autonomous-rc-car-i) - Introducción al proyecto
* [Coche RC autónomo (II)]({{ site.url }}/2017/09/09/autonomous-rc-car-ii) - Primer análisis de requisitos del proyecto
* [Coche RC autónomo (III)]({{ site.url }}/2017/09/12/autonomous-rc-car-iii) - Primeros pasos de configuración de la Raspberry Pi
* [Coche RC autónomo (IV)]({{ site.url }}/2017/09/16/autonomous-rc-car-iv) - Configurando la videocámara en la Raspberry Pi
