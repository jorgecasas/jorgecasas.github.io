---
layout: post
title:  "Coche RC autónomo (V)"
date:   2017-09-24 14:45:00
categories: blog development projects
tags:
- blog
- development
- projects
thumbnail: /assets/images/2017-09-24-autonomous-rc-car-v-01.jpg
---

El siguiente paso en el proyecto de construir un coche RC autónomo es el de instalar las librerías necesarias para poder procesar las imágenes que recibamos de la cámara de la Raspberry Pi. La librería de visión por computador en cuestión es [OpenCV](https://www.opencv.org), que puede ser usada muy fácilmente mediante programación en Python.

![OpenCV - Raspberry Pi](/assets/images/2017-09-24-autonomous-rc-car-v-01.jpg)

La idea principal es instalar la versión más reciente de Python (versión **python3.5**), así como la versión más reciente de OpenCV (**opencv-3.3.0**). Aunque existen paquetes en Debian / Ubuntu que se pueden instalar directamente con `apt-get`, voy a realizar la instalación de forma manual para poder integrar ciertas funcionalidades que no vienen en los paquetes por defecto (además de que no suelen estar actualizados a las últimas versiones).

Instalaré estas librerías tanto en la **Raspberry Pi** (ya que lo necesitará el coche para ser completamente autónomo) como en el ordenador (ya que así es más fácil realizar las pruebas). Para ello, seguiré los tutoriales de [PyImageSearch - Raspberry Pi](http://www.pyimagesearch.com/2016/04/18/install-guide-raspberry-pi-3-raspbian-jessie-opencv-3/) y [PyImageSearch - Ubuntu 16.04](http://www.pyimagesearch.com/2016/10/24/ubuntu-16-04-how-to-install-opencv/). En el fondo son muy parecidos los dos, por lo que pondré los pasos principales.

Se va a instalar un entorno virtualizado para Python, lo que nos permitirá acceder a un entorno u otro por si queremos instalar otras versiones de Python (como **python2.7**). Hay que tener en cuenta que de la versión 2 a la 3 de Python se han realizado muchas actualizaciones no compatibles, por lo que si estás siguiendo este tutorial y usando el código que voy subiendo a [Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car) has de tener en cuenta que yo estoy usando la versión **python3.5**.

Antes de nada, si no lo hicimos al configurar la Raspberry Pi, tendremos que expandir el sistema de ficheros para ocupar todo el espacio de la tarjeta SD (porque OpenCV requiere más espacio del original). Para ello accedemos a `raspi-config` y seleccionando la opción de _Expand filesystem_. Tras guardar los cambios tendremos que reiniciar la Raspberry Pi.

El primer paso en la instalación es actualizar el sistema y eliminar algún paquete no necesario que ocupa mucho espacio (si es que está instalado). En la Raspberry Pi actualizaremos hasta el firmware, e instalaremos paquetes requeridos de desarrollo:

```bash
sudo apt-get purge wolfram-engine
sudo apt-get update
sudo apt-get upgrade -y 

sudo apt-get install build-essential cmake pkg-config
sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
sudo apt-get install libxvidcore-dev libx264-dev
sudo apt-get install libgtk2.0-dev
sudo apt-get install libatlas-base-dev gfortran
sudo apt-get install python2.7-dev python3-dev

```

Instalamos la herramienta `pip`, que nos permitirá instalar fácilmente paquetes y librerías Python mediante comandos como `pip install paquete`. Acto seguido instalaremos los paquetes para crear los entornos virtuales de Python que hemos comentado:

```bash
wget https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py

sudo pip install virtualenv virtualenvwrapper
sudo rm -rf ~/.cache/pip get-pip.py
```

Actualizaremos el fichero `~/.profile` (en Ubuntu / Debian el equivalente es `~/.bashrc`) con las siguientes líneas al final del todo:

```
# virtualenv and virtualenvwrapper
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```

Recargamos el fichero `~/.profile` (o `~/.bashrc`) con el comando `source`, o bien cerrando y abriendo un nuevo terminal:

```bash
source ~/.profile
```

Y creamos un entorno virtual para nuestros procesados. Lo vamos a denominar `cv` (por _Computer Vision_), podríamos crear otros entornos con otro nombre y versión de Python (nosotros vamos a usar la 3, como ya hemos comentado):

```bash
mkvirtualenv cv -p python3
```

Al crear este entorno virtual, nos aparecerá en la línea de comandos `(cv) $` para indicar que estamos dentro del entorno virtualizado. Ahora ya lo tenemos creado, por lo que en las próximas ocasiones ya no tendremos que volver a crearlo, simplemente lo activaremos mediante el comando:

```bash
workon cv
```

Una vez dentro del entorno virtualizado, instalamos los paquetes de desarrollo de Python en la versión 3.5 y la librería `numpy`.

```bash
sudo apt-get install python3.5-dev
sudo pip install numpy
```

Ahora vamos a descargar de Github las fuentes de [OpenCV](https://github.com/opencv/opencv) (versión 3.3.0, la más reciente) y [OpenCV_contrib](https://github.com/opencv/opencv_contrib) (también la versión 3.3.0), para luego compilarla:

```bash
wget -O opencv.zip https://github.com/opencv/opencv/archive/3.3.0.zip
wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/3.3.0.zip
unzip opencv.zip
unzip opencv_contrib.zip
```

Nos habrá creado dos directorios **opencv-3.3.0** y **opencv_contrib-3.3.0**. Accedemos al directorio _opencv-3.3.0_ y creamos lo necesario para la compilación. Hay que tener en cuenta tanto las rutas como la versión correspondiente en el parámetro `OPENCV_EXTRA_MODULES_PATH` del comando `cmake`:

```bash
cd opencv-3.3.0
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D INSTALL_PYTHON_EXAMPLES=ON -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-3.3.0/modules -D BUILD_EXAMPLES=ON ..
```

Si todo se ejecuta correctamente, aparecerán unas líneas al final indicando que está instalado Python 3 y que tiene _Interpreter_, _Libraries_, _numpy_ y _packages path_ correctos. Si no aparece alguno de ellas (por ejemplo, _numpy_), es que no está instalada la librería (_numpy_) o el paquete de desarrollo de _python3-dev_ correspondiente. En estos casos, instalamos el paquete faltante y eliminamos el directorio _build_ y lo volvemos a crear, volviendo a ejecutar `cmake`.

Si todo ha ido bien, ejecutamos el siguiente comando mientras nos vamos a tomar un café, porque le costará una hora como mínimo:

```bash
make -j4
```

Si falla la compilación será debido al parámetro `-j4` que hemos usado para indicar que utilice todos núcleos de la Raspberry Pi y se han dado condiciones de carrera. En este caso, tendremos que ejecutar los siguientes comandos y nos vamos a por otro café:

```bash
make clean
make
```

Tras compilar, lo instalamos:

```bash
sudo make install
sudo ldconfig
```

Accederemos ahora al directorio `/usr/local/lib/python3.5/site-packages/` (o al correspondiente a la versión de Python que estemos usando), y veremos que existe un fichero con un nombre similar a _cv2.XXXX.so_ (en mi caso, _cv2.cpython-35m-arm-linux-gnueabihf.so_. Crearemos un enlace simbólico que apunte a dicha librería y que se llame `cv2.so`, y lo enlazaremos también desde el directorio de nuestro entorno virtualizado (o no lo encontraremos al importar la librería en dicho entorno):

```bash
cd /usr/local/lib/python3.5/site-packages/
ln -s cv2.cpython-35m-arm-linux-gnueabihf.so cv2.so

cd ~/.virtualenvs/cv/lib/python3.5/site-packages/
ln -s /usr/local/lib/python3.5/site-packages/cv2.so cv2.so
```

Por último probamos que podemos utilizar OpenCV desde Python. Abrimos el intérprete `python` y ejecutamos en el shell:

```bash
$ python
Python 3.5.3 (default, Jan 19 2017, 14:11:04)

>>> import cv2
>>> cv2.__version__
'3.3.0'
```

Nos indica así que podemos importar la librería. Si nos diera error, probablemente es que no hemos creado correctamente los enlaces simbólicos de los pasos anteriores.

En el próximo post veremos cómo utilizar OpenCV desde script Python. ¡Os espero!


## Más información sobre el proyecto

* [Código en Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car)
* [Coche RC autónomo (I)]({{ site.url }}/2017/08/23/autonomous-rc-car-i) - Introducción al proyecto
* [Coche RC autónomo (II)]({{ site.url }}/2017/09/09/autonomous-rc-car-ii) - Primer análisis de requisitos del proyecto
* [Coche RC autónomo (III)]({{ site.url }}/2017/09/12/autonomous-rc-car-iii) - Primeros pasos de configuración de la Raspberry Pi
* [Coche RC autónomo (IV)]({{ site.url }}/2017/09/16/autonomous-rc-car-iv) - Configurando la videocámara en la Raspberry Pi
* [Coche RC autónomo (V)]({{ site.url }}/2017/09/24/autonomous-rc-car-v) - Instalando OpenCV (visión por computador)
* [Coche RC autónomo (VI)]({{ site.url }}/2017/09/27/autonomous-rc-car-vi) - Probando la cámara usando Python, OpenCV y streaming
* [Coche RC autónomo (VII)]({{ site.url }}/2017/10/02/autonomous-rc-car-vii) - Configurando el sensor ultrasónico HC-SR04 para detectar objetos


