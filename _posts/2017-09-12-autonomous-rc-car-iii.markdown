---
layout: post
title:  "Coche RC autónomo (III)"
date:   2017-09-12 14:45:00
categories: blog development projects
tags:
- blog
- development
- projects
thumbnail: /assets/images/2017-09-12-autonomous-rc-car-iii-01.jpg
---

En la tercera entrega del proyecto **Construyendo un coche RC autónomo** voy a detallar los primeros pasos con la [Raspberry Pi](http://amzn.to/2wmTn2S). La idea es que este mini ordenador, que cuenta con WiFi en su versión 3, se ancle al vehículo radiocontrol para obtener imágenes (mediante su videocámara) para su posterior procesado mediante una red neuronal.

![Raspberry Pi](/assets/images/2017-09-12-autonomous-rc-car-iii-01.jpg)

Como la [Raspberry Pi](http://amzn.to/2wmTn2S) no deja de ser un ordenador, voy a instalar en ella la distribución GNU/Linux [Raspbian](https://raspbian.org) (que no es más que una variante de [Debian](https://www.debian.org) pero adaptado a las capacidades y recursos de la [Raspberry Pi](http://amzn.to/2wmTn2S)). Tras descargarla, seguiremos los pasos indicados en la [documentación](https://www.raspberrypi.org/documentation/installation/) para grabar la distribución en una tarjeta [micro SD](http://amzn.to/2gZp503) de 16 GB. 

Para grabarla, existen varias aplicaciones. La recomendada en la propia página de [Raspbian](https://raspbian.org) es [Etcher](https://etcher.io), una mini aplicación que permite seleccionar una tarjeta SD destino y una imagen ISO, y con sólo pulsar un botón tendremos la tarjeta micro SD con la distribución lista para ser utilizada.

![Etcher - Grabando la imagen de Raspbian en una tarjeta micro SD](/assets/images/2017-09-12-autonomous-rc-car-iii-02.jpg)

Una vez grabada la imagen en la tarjeta micro SD la introducimos en la [Raspberry Pi](http://amzn.to/2wmTn2S), y usando un cargador de móvil con cable USB - micro USB la enchufamos a la corriente, y con un cable DVI a la televisión o a un monitor para poder configurarla (incluido el adaptador WiFi para así más tarde poder conectarnos de forma remota mediante SSH utilizando un gestor de conexiones como [Gnome Connection Manager](http://kuthulu.com/gcm/)). Necesitaremos también un teclado de ordenador USB para poder realizar la configuración.

El usuario por defecto de la distribución [Raspbian](https://raspbian.org) es _pi_, con la contraseña _raspberry_. Como queremos que nuestro vehículo autónomo no sea hackeado (o al menos, tan fácilmente), lo mejor será cambiar esta contraseña por defecto por una privada, secreta e intransferible.

Para realizar la configuración inicial y cambiar el password del usuario _pi_, activar el servidor SSH (para poder acceder remotamente), expandir todo el sistema de archivos usando todo el espacio de la tarjeta micro SD, etcétera, vamos a ejecutar el siguiente comando:

```bash
sudo raspi-config
```

Una vez configurado los diferentes apartados, vamos a configurar la WiFi para poder conectarnos de forma remota mediante SSH. Para ello, configuramos mediante línea de comandos la nueva [Raspberry Pi 3](http://amzn.to/2wmTn2S) según la [documentación oficial](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md), que ha cambiado y ya no utiliza los ficheros de configuración de red _/etc/network/interfaces_. 

En primer lugar queremos conocer el ESSID de la red WiFi a la que nos vamos a conectar, por lo que necesitamos un listado de las disponibles si no lo conocemos:

```bash
sudo iwlist wlan0 scan
```

Ahora editamos el fichero _/etc/wpa_supplicant/wpa_supplicant.conf_:

```bash
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

Añadimos al final las siguientes líneas para identificar las redes WiFi a las que nos vamos a conectar. En mi caso, la de casa y la de la oficina de [ITERNOVA](https://www.iternova.net), poniendo los identificadores de cada red SSID y su contraseña secreta correspondiente:

```
network={
        ssid="WIFI-DE-CASA"
        psk="clave-secreta-de-wifi-de-casa"
        priority=1
        id_str="HOME"
}

network={
        ssid="WIFI-DE-ITERNOVA"
        psk="clave-secreta-de-wifi-de-iternova"
        priority=2
        id_str="ITERNOVA"
}
```

Ahora necesitamos editar el fichero _/etc/dhcpcd.conf_ ya que queremos asignar IP estáticas (tanto de Ethernet como de WiFi). En nuestro caso, queremos asignar las IP 192.168.1.198 para el interfaz _eth0_ y 192.168.1.199 para el interfaz WiFi _wlan0_.

```bash
sudo nano /etc/dhcpcd.conf
```

Al final del fichero incluiremos las siguientes líneas:

```
# eth0 static configuration
interface eth0
static ip_address=192.168.1.198/24
static routers=192.168.1.1
static domain_name_servers=8.8.8.8 8.8.4.4

# wlan0 static configuration
interface wlan0
static ip_address=192.168.1.199/24
static routers=192.168.1.1
static domain_name_servers=8.8.8.8 8.8.4.4

```

Con esto, reiniciamos la Raspberry Pi y ya debería estar conectada a la red. 

```
# Reiniciamos 
sudo reboot

# Tras reboot, comprobamos los interfaces de red activos
sudo ifconfig
```

Como además hemos activado el servicio SSH, podremos conectarnos de forma remota desde nuestro ordenador para actualizar los diferentes paquetes desde la propia aplicación _raspi-config_. Como veremos en próximos posts, desde aquí también podremos activar la cámara que vayamos a usar e instalar los paquetes y librerías que vamos a necesitar.

![Conectados a la Raspberry Pi mediante SSH usando la WiFi](/assets/images/2017-09-12-autonomous-rc-car-iii-03.jpg)

Si os está gustando y empezáis a ver mil millones de posibilidades utilizando la Raspberry Pi para experimentos, no dejéis de visitar los artículos de la revista [MagPi](https://www.raspberrypi.org/magpi) (que se puede descargar de forma gratuita en PDF).


## Más información sobre el proyecto

* [Código en Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car)
* [Coche RC autónomo (I)]({{ site.url }}/2017/08/23/autonomous-rc-car-i) - Introducción al proyecto
* [Coche RC autónomo (II)]({{ site.url }}/2017/09/09/autonomous-rc-car-ii) - Primer análisis de requisitos del proyecto
* [Coche RC autónomo (III)]({{ site.url }}/2017/09/12/autonomous-rc-car-iii) - Primeros pasos de configuración de la Raspberry Pi
* [Coche RC autónomo (IV)]({{ site.url }}/2017/09/16/autonomous-rc-car-iv) - Configurando la videocámara en la Raspberry Pi
* [Coche RC autónomo (V)]({{ site.url }}/2017/09/24/autonomous-rc-car-v) - Instalando OpenCV (visión por computador)
* [Coche RC autónomo (VI)]({{ site.url }}/2017/09/27/autonomous-rc-car-vi) - Probando la cámara usando Python, OpenCV y streaming
* [Coche RC autónomo (VII)]({{ site.url }}/2017/10/02/autonomous-rc-car-vii) - Configurando el sensor ultrasónico HC-SR04 para detectar objetos


