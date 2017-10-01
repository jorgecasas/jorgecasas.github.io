---
layout: post
title:  "Coche RC autónomo (VI)"
date:   2017-09-27 14:45:00
categories: blog development projects
tags:
- blog
- development
- projects
thumbnail: /assets/images/2017-09-27-autonomous-rc-car-vi-01.png
---

Llegados a este punto ya tenemos [OpenCV](https://www.opencv.org) instalado (tanto en la Raspberry Pi como en el ordenador para hacer pruebas de forma más sencilla). Con esta librería podremos usar la cámara y Python para la visión por computador. Así, en este post vamos a comentar nuestros primeros scripts en Python para conseguir capturar imágenes de la cámara de la Raspberry Pi usando la librería `picamera` y OpenCV para procesarlos. En el post [Coche RC autónomo (IV) - Configurando la videocámara en la Raspberry Pi]({{ site.url }}/2017/09/16/autonomous-rc-car-iv) ya habíamos conseguido enviar imágenes desde el cliente (Raspberry Pi) al servidor (nuestro ordenador), pero usábamos `netcat` para ello, y nosotros necesitaremos obtener _objetos de tipo imagen_ para poder ser procesadas mediante OpenCV.

![Python en la Raspberry Pi](/assets/images/2017-09-27-autonomous-rc-car-vi-01.png)

Para ello, igual que hicimos en el post anterior, vamos a enviar imágenes entre la Raspberry Pi (IP en el interfaz WiFi: `192.168.1.199`) al servidor (nuestro ordenador, con IP `192.168.1.235`, con el puerto `8000` abierto en el cortafuegos). En este post vamos a crear los dos primeros _scripts python_, los cuales nos servirán de base para ir programando más adelante todo lo necesario para que el vehículo autónomo pueda _ver y procesar lo que ve_. Los scripts y todo el código que vaya a utilizar los podés descargar de **[Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car)**.

## Ejecutando scripts Python

Para ejecutar los scripts (tanto en la Raspberry Pi como en el ordenador, donde tenemos instalado las librerías [OpenCV]({{ site.url }}/2017/09/24/autonomous-rc-car-v) y los entornos virtualizados de Python), basta con ejecutar los comandos:

* Acceder al entorno virtualizado:

```bash
workon cv
```

* Ejecutar el script Python:

```bash
python script_python.py
```

## Utilizando `git` para hacer el despliegue de código

Para copiar los ficheros tanto a la Raspberry Pi como a nuestro ordenador, puede ser interesante crear un repositorio (o un branch de mi repositorio de [Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car)) tanto en la Raspberry Pi como en nuestro ordenador, de manera que podamos compartir nuestro código y actualizarlo fácilmente en los diferentes dispositivos. Tendremos que instalar `git`:

```bash
sudo apt-get install git
```

Ahora clonamos el repositorio (tanto en la Raspberry Pi como en nuestro ordenador), lo que creará una carpeta `autonomous-rc-car`. En este caso necesitaremos tener antes una cuenta de Github, que es gratuita:


```bash
git clone git@github.com:jorgecasas/autonomous-rc-car.git
cd autonomous-rc-car
```

Podemos crear ahora una nueva rama (`branch`) para nuestros desarrollos. En el ejemplo esta rama de desarrollo se llamará _branch-de-desarrollo_ (le puedes llamar como quieras):

```bash
git checkout -b branch-de-desarrollo
```

Podemos actualizar el código y crear nuevos ficheros. Los podremos añadir y hacer `commit`:

```bash
git add nuevo_fichero.py
git commit -m "Nuevo fichero creado"
```

Por último, este nuevo fichero lo podemos subir a Github para tener una copia de seguridad y poder compartir nuestro código:

```bash
git push origin branch-de-desarrollo
```

Y ahora que está el código subido a Github, lo podemos descargar en la Raspberry Pi o en otro ordenador. Para ello, una vez conectados a la Raspberry Pi por SSH, necesitamos clonar el proyecto (igual que hemos hecho arriba), y acceder a nuestro branch de desarrollo:

```bash
git clone git@github.com:jorgecasas/autonomous-rc-car.git
cd autonomous-rc-car
git checkout branch-de-desarrollo
```

Y una vez en nuestra rama, podemos actualizar los ficheros a la última versión que hayamos subido a Github (a `origin`):


```bash
git pull
```

Podemos aprender más sobre `git` con libros como [Aprende Git y de paso Github](http://amzn.to/2ynTngx), o con tutoriales como los de [Atlassian - Git](https://www.atlassian.com/git). Ahora que ya sabemos cómo utilizar un poco `git` para tener el código actualizado en el ordenador y en la Raspberry Pi, vamos a ver los _scripts_, teniendo en cuenta que la **versión de Python que yo estoy utilizando es la python3.5** (ya que en versiones anteriores, como la _python2.7_ algunas funciones y librerías se llaman diferente y nos darán errores de ejecución de los scripts).


## Servidor - Ordenador con Ubuntu

En el servidor incluiremos el siguiente _script_ **stream_server_computer.py**. Su función es la siguiente:

* Importamos las librerías (como OpenCV _cv2_ para procesar las imágenes recibidas, _socketserver_ para poder crear un servidor de sockets...)
* Indicamos variables de configuración (como el puerto que va a utilizar el servidor, en nuestro caso el 8000)
* Creamos un hilo de ejecución (_thread_), que creará un servidor de _socket_, esperando hasta recibir la primera petición de un cliente (de la Raspberry Pi). Creamos un hilo porque en el futuro necesitaremos crear varios sockets en paralelo (uno para recibir imágenes, otro para recibir datos de distancia del sensor ultrasónico, etcétera).
* En cuanto recibamos la primera conexión, iremos leyendo el buffer de bits que nos vaya enviando el cliente (la Raspberry Pi). Este cliente irá enviando imagen a imagen (_frames_), que serán obtenidos como objetos de imagen de OpenCV.
* Al obtener un objeto de imagen OpenCV, lo procesaremos decodificando los datos mediante `cv2.imdecode()` (por ejemplo, lo podemos convertir a escala de grises), y lo mostraremos en una ventana mediante `cv2.imshow()`
* Y así hasta que pulsemos sobre la ventana la tecla `q`, que finalizará la ejecución.

Es importante tener en cuenta que tendremos que **ejecutar este script servidor antes que el script cliente**. Para ello, como hemos comentado, basta con ejecutar desde el entorno virtualizado `python stream_server_computer.py`.

El código del servidor es el siguiente (descargable desde [Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car)):

```python
# Import libraries
import threading
import socketserver
import socket
import cv2
import numpy as np
import math

# Config vars
server_ip = '192.168.1.235'
server_port = 8000
image_fps = 24 

# Class to handle the jpeg video stream received from client
class VideoStreamHandler(socketserver.StreamRequestHandler):
 
    def handle(self):
 
        stream_bytes = b' '

        # stream video frames one by one
        try:
            while True:
                stream_bytes += self.rfile.read(1024)

                first = stream_bytes.find(b'\xff\xd8')
                last = stream_bytes.find(b'\xff\xd9')
                if first != -1 and last != -1:
                    jpg = stream_bytes[first:last+2]
                    stream_bytes = stream_bytes[last+2:]
                    gray = cv2.imdecode(np.fromstring(jpg, dtype=np.uint8), cv2.IMREAD_GRAYSCALE)
                    image = cv2.imdecode(np.fromstring(jpg, dtype=np.uint8), cv2.IMREAD_UNCHANGED)

                    # lower half of the image
                    half_gray = gray[120:240, :]
 
                    # Mostramos imagenes
                    cv2.imshow('image', image)
                    #cv2.imshow('mlp_image', half_gray)

                    # reshape image
                    image_array = half_gray.reshape(1, 38400).astype(np.float32)
                    
                    if cv2.waitKey(1) & 0xFF == ord('q'):
                        break

            cv2.destroyAllWindows()

        finally:
            print( 'Connection closed on videostream thread' )

# Class to handle the different threads 
class ThreadServer():

    # Server thread to handle the video
    def server_thread(host, port):
        server = socketserver.TCPServer((host, port), VideoStreamHandler)
        server.serve_forever()

    print( '+ Starting videostream server in ' + str( server_ip ) + ':' + str( server_port ) )
    video_thread = threading.Thread(target=server_thread( server_ip, server_port))
    video_thread.start()

# Starting thread server handler
if __name__ == '__main__':
    ThreadServer( server_ip, server_port )

```


## Cliente - Raspberry Pi

En el cliente incluiremos el siguiente _script_ **stream_client_raspberry.py**. Su cometido es el siguiente:

* Importar librerías (como _socket_ para crear un cliente de sockets, _picamera_ para poder obtener imágenes de la cámara de la Raspberry Pi, etc...)
* Indicar parámetros de configuración (IP del servidor, puerto del servidor, dimensiones de la imagen a capturar, frames por segundo, duración máxima de envío...)
* Abrimos un _socket_ a la dirección IP y puerto del servidor.
* Inicializamos la cámara de la Raspberry Pi, dándole 2 segundos para inicializarse
* Enviamos imágenes en forma de _stream JPEG_ al servidor hasta que pase el tiempo máximo _recording_time_ o se cierre la comunicación (pulsando `Ctrl + C`, etcétera)

El código del cliente es el siguiente (descargable desde [Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car)). Lo ejecutaremos mediante `python stream_client_raspberry.py`.

```python
# Import libraries
import io
import socket
import struct
import time
import picamera


# Config vars
server_ip = '192.168.1.235'
server_port = 8000
image_width = 320
image_height = 240
image_fps = 10
recording_time = 600


# Connect a client socket to server_ip:server_port
print( 'Trying to connect to streaming server in ' + str( server_ip ) + ':' + str( server_port ) );

# create socket and bind host
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect((server_ip, server_port))
connection = client_socket.makefile('wb')

try:
    with picamera.PiCamera() as camera:
        camera.resolution = (image_width, image_height)
        camera.framerate = image_fps

        # Give 2 secs for camera to initilize
        time.sleep(2)                       
        start = time.time()
        stream = io.BytesIO()
        
        # send jpeg format video stream
        for foo in camera.capture_continuous(stream, 'jpeg', use_video_port = True):
            connection.write(struct.pack('<L', stream.tell()))
            connection.flush()
            stream.seek(0)
            connection.write(stream.read())
            if time.time() - start > recording_time:
                break
            stream.seek(0)
            stream.truncate()
    connection.write(struct.pack('<L', 0))

finally:
    connection.close()
    client_socket.close()
    print( 'JPEG streaming finished!' );


```

Y con esto, podremos ver en nuestro ordenador las imágenes que vayamos obteniendo de la cámara. En principio no deberían tener mucho _lag_, por lo que para las pruebas nos va a servir. 

En los siguientes posts iremos actualizando estos scripts para ir introduciendo algo de inteligencia a nuestro vehículo (por ejemplo, crear una red neuronal para detectar una señal de STOP). ¡Nos vemos!


## Más información sobre el proyecto

* [Código en Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car)
* [Coche RC autónomo (I)]({{ site.url }}/2017/08/23/autonomous-rc-car-i) - Introducción al proyecto
* [Coche RC autónomo (II)]({{ site.url }}/2017/09/09/autonomous-rc-car-ii) - Primer análisis de requisitos del proyecto
* [Coche RC autónomo (III)]({{ site.url }}/2017/09/12/autonomous-rc-car-iii) - Primeros pasos de configuración de la Raspberry Pi
* [Coche RC autónomo (IV)]({{ site.url }}/2017/09/16/autonomous-rc-car-iv) - Configurando la videocámara en la Raspberry Pi
* [Coche RC autónomo (V)]({{ site.url }}/2017/09/24/autonomous-rc-car-v) - Instalando OpenCV (visión por computador)
* [Coche RC autónomo (VI)]({{ site.url }}/2017/09/27/autonomous-rc-car-vi) - Probando la cámara usando Python, OpenCV y streaming
* [Coche RC autónomo (VII)]({{ site.url }}/2017/10/02/autonomous-rc-car-vii) - Configurando el sensor ultrasónico HC-SR04 para detectar objetos

