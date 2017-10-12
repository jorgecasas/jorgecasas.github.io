---
layout: post
title:  "Coche RC autónomo (VIII) - Usando threads para enviar y recibir datos del sensor ultrasónico e imágenes desde la Raspberry Pi"
date:   2017-10-12 21:45:00
categories: blog development projects
tags:
- blog
- development
- projects
thumbnail: /assets/images/assets/images/2017-10-12-autonomous-rc-car-threads.gif
---

Como ya expliqué en el post [Coche RC autónomo (VII) - Configurando el sensor ultrasónico HC-SR04 para detectar objetos]({{ site.url }}/2017/10/02/autonomous-rc-car-vii), el siguiente paso era enviar esa información desde nuestro cliente ([Raspberry Pi](http://amzn.to/2yjVkh6)) a nuestro servidor, para luego poder procesar esos datos de distancia a objetos junto con las imágenes de las videocámaras (que ya éramos capaces de enviar, como se explicó en el post [Coche RC autónomo (VI) - Probando la cámara usando Python, OpenCV y streaming]({{ site.url }}/2017/09/27/autonomous-rc-car-vi)).

![Imágenes de la videocámara y datos del sensor ultrasónico](/assets/images/2017-10-12-autonomous-rc-car-threads.gif)

La idea es, por tanto, actualizar nuestros scripts **Python 3** `server.py` (a ejecutar en nuestro ordenador durante el desarrollo, y luego exclusivamente desde la Raspberry Pi 3 cuando esté montada en el vehículo teledirigido) y `client.py` (a ejecutar en la Raspberry Pi 3 siempre). A tener en cuenta:

* Vamos a crear en el cliente un `thread` (hilo de ejecución) que lea datos del sensor ultrasónico con la distancia a objetos enfrente de dicho sensor y la envíe al servidor por un puerto definido (el `8001` en nuestro caso), y otro `thread` que obtenga imágenes de la videocámara y las envíe mediante _streaming_ a otro puerto definido (el `8000`)
* En el servidor crearemos igualmente dos `threads`, uno para gestionar las imágenes recibidas de la cámara y otro para obtener los datos de distancia obtenidos del sensor ultrasónico.

En el servidor, el _thread_ principal será el que procese imágenes de la videocámara, ya que desde él podremos leer los datos recibidos del sensor ultrasónico y podremos mostrar mensajes en la propia imagen que visualicemos. Desde este _thread_, como veremos en futuros posts, iremos llamando a diferentes funciones para controlar el vehículo (redes neuronales que procesen las imágenes, y lógica de si el coche debe girar, frenar, acelerar, etcétera)

En el ejemplo detallado en este post, a la vez que recibimos dichos datos (imágenes y distancia a obstáculos) vamos a ir preparando esta lógica: Vamos a mostrar en la imagen si tenemos obstáculo delante o no, escribiendo un texto.

El código del cliente (Raspberry Pi 3) es el siguiente (`client.py`). Como hemos comentado, la configuración del los circuitos y código del [sensor de ultrasonidos]({{ site.url }}/2017/10/02/autonomous-rc-car-vii) y [videocámara]({{ site.url }}/2017/09/27/autonomous-rc-car-vi) los puedes encontrar en los correspondientes posts, mientras que el código lo puedes encontrar actualizado en **[Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car)**:

```python
# Importamos librerias RPi.GPIO (entradas/salidas GPIO de Raspberry Pi) y time (para sleeps, etc...)
# Requiere previamente instalarla (pip install RPi.GPIO)
import RPi.GPIO as GPIO
import time
import io
import socket
import struct
import picamera
import threading

# Configure Raspberry Pi GPIO in BCM mode
GPIO.setmode(GPIO.BCM) 


# Config vars. These IP and ports must be available in server firewall
log_enabled = False
server_ip = '192.168.1.235'
server_port_ultrasonic = 8001
server_port_camera = 8000

# Camera configuration
image_width = 640
image_height = 480
image_fps = 10
recording_time = 600

# Definition of GPIO pins in Raspberry Pi 3 (GPIO pins schema needed!)
#   18 - Trigger (output)
#   24 - Echo (input)
GPIO_ultrasonic_trigger = 18
GPIO_ultrasonic_echo = 24

# Class to handle the ultrasonic sensor stream in client
class StreamClientUltrasonic():

    def measure(self):
        # Measure distance from ultrasonic sensor. Send a trigger pulse
        GPIO.output( GPIO_ultrasonic_trigger, True )
        time.sleep( 0.00001 )
        GPIO.output( GPIO_ultrasonic_trigger, False )
        
        # Get start time
        start = time.time()

        # Wait to receive any ultrasound in sensor (echo)
        while GPIO.input( GPIO_ultrasonic_echo ) == 0:
            start = time.time()

        # We have received the echo. Wait for its end, getting stop time
        while GPIO.input( GPIO_ultrasonic_echo ) == 1:
            stop = time.time()

        # Calculate time difference. Sound has gone from trigger to object and come back to sensor, so 
        # we have to divide between 2. Formula: Distance = ( Time elapsed * Sound Speed ) / 2 
        time_elapsed = stop-start
        distance = (time_elapsed * 34300) / 2

        return distance

  
    def __init__(self):

        # Connect a client socket to server_ip:server_port_ultrasonic
        print( '+ Trying to connect to ultrasonic streaming server in ' + str( server_ip ) + ':' + str( server_port_ultrasonic ) );

        # Configure GPIO pins (trigger as output, echo as input)
        GPIO.setup( GPIO_ultrasonic_trigger, GPIO.OUT )
        GPIO.setup( GPIO_ultrasonic_echo, GPIO.IN )

        # Set output GPIO pins to False
        GPIO.output( GPIO_ultrasonic_trigger, False ) 

        # Create socket and bind host
        client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        client_socket.connect( ( server_ip, server_port_ultrasonic ) )

        try:
            while True:
                # Measure and send data to the host every 0.5 sec, 
                # pausing for a while to no lock Raspberry Pi processors
                distance = self.measure()
                if log_enabled: print( "Ultrasonic sensor distance: %.1f cm" % distance )
                client_socket.send( str( distance ).encode('utf-8') )
                time.sleep( 0.5 )

        finally:
            # Ctrl + C to exit app (cleaning GPIO pins and closing socket connection)
            print( 'Ultrasonic sensor connection finished!' );
            client_socket.close()
            GPIO.cleanup()


# Class to handle the jpeg video stream in client
class StreamClientVideocamera():
  
    def __init__(self):

        # Connect a client socket to server_ip:server_port_camera
        print( '+ Trying to connect to videocamera streaming server in ' + str( server_ip ) + ':' + str( server_port_camera ) );

        # create socket and bind host
        client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        client_socket.connect((server_ip, server_port_camera))
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
            print( 'Videocamera stream connection finished!' );

 

# Class to handle the different threads in client 
class ThreadClient():

    # Client thread to handle the video
    def client_thread_camera(host, port):
        print( '+ Starting videocamera stream client connection to ' + str( host ) + ':' + str( port ) )
        StreamClientVideocamera()

    # Client thread to handle ultrasonic distances to objects
    def client_thread_ultrasonic(host, port):
        print( '+ Starting ultrasonic stream client connection to ' + str( host ) + ':' + str( port ) )
        StreamClientUltrasonic()

    print( '+ Starting client - Logs ' + ( log_enabled and 'enabled' or 'disabled'  ) )
    thread_ultrasonic = threading.Thread( name = 'thread_ultrasonic', target = client_thread_ultrasonic, args = ( server_ip, server_port_ultrasonic ) )
    thread_ultrasonic.start()
    
    thread_videocamera = threading.Thread( name = 'thread_videocamera', target = client_thread_camera, args = ( server_ip, server_port_camera ) )
    thread_videocamera.start()


# Starting thread client handler
if __name__ == '__main__':
    ThreadClient()
```

El código del servidor es el siguiente (`server.py`):

```python
# Import libraries
import threading
import socketserver
import socket
import cv2
import numpy as np
import math

# Config vars
log_enabled = False
server_ip = '192.168.1.235'
server_port_camera = 8000
server_port_ultrasonic = 8001

# Video configuration
image_gray_enabled = False
image_fps = 24 
image_width = 640
image_height = 480
image_height_half = int( image_height / 2 )

# Global var (ultrasonic_data) to measure object distances (distance in cm)
ultrasonic_sensor_distance = ' '
ultrasonic_stop_distance = 25
ultrasonic_text_position = ( 16, 16 )

# Other vars
color_blue = (211, 47, 47)
color_yellow = (255, 238, 88)
color_red = (48, 79, 254)
color_green = (0, 168, 0)

# Font used in opencv images
image_font = cv2.FONT_HERSHEY_PLAIN
image_font_size = 1.0
image_font_stroke = 2


# Datos para lineas de control visual. Array stroke_lines contiene 3 componentes:
#   0: Punto inicial (x,y)
#   1: Punto final (x,y)
#   2: Color de linea
#   3: Ancho de linea en px
# Para activarlo/desactivarlo: stroke_enable = True|False
stroke_enabled = True
stroke_width = 4
stroke_lines = [
   [ (0,image_height), ( int( image_width * 0.25 ), int( image_height/2 ) ), color_green, stroke_width ],
   [ (image_width,image_height), ( int( image_width * 0.75 ), int( image_height/2 ) ), color_green, stroke_width ]
];


# Class to handle data obtained from ultrasonic sensor
class StreamHandlerUltrasonic(socketserver.BaseRequestHandler):

    data = ' '

    def handle(self):
        global ultrasonic_sensor_distance
        distance_float = 0.0

        try:
            print( 'Ultrasonic sensor measure: Receiving data in server!' )
            while self.data:
                self.data = self.request.recv(1024)
                try:
                    distance_float = float( self.data )
                except ValueError: 
                    # No es float... porque hemos recibido algo del tipo b'123.123456.456' (es decir, por lag de la red
                    # o sobrecarga de nuestro sistema hemos recibido dos valores antes de ser capaces de procesarlo)
                    distance_float = 1000.0
                
                ultrasonic_sensor_distance = round( distance_float, 1)
                if log_enabled: print( 'Ultrasonic sensor measure received: ' + str( ultrasonic_sensor_distance ) + ' cm' )
 
        finally:
            print( 'Connection closed on ultrasonic thread' )


# Class to handle the jpeg video stream received from client
class StreamHandlerVideocamera(socketserver.StreamRequestHandler):
  
    def handle(self):
        stream_bytes = b' '
        global ultrasonic_sensor_distance

        # stream video frames one by one
        try:
            print( 'Videocamera: Receiving images in server!' )
            while True:
                stream_bytes += self.rfile.read(1024)

                first = stream_bytes.find(b'\xff\xd8')
                last = stream_bytes.find(b'\xff\xd9')
                if first != -1 and last != -1:
                    jpg = stream_bytes[first:last+2]
                    stream_bytes = stream_bytes[last+2:]
                    image = cv2.imdecode(np.fromstring(jpg, dtype=np.uint8), cv2.IMREAD_UNCHANGED)

                    # lower half of the image
                    if image_gray_enabled:
                        gray = cv2.imdecode(np.fromstring(jpg, dtype=np.uint8), cv2.IMREAD_GRAYSCALE)
                        half_gray = gray[image_height_half:image_height, :]

                    # Dibujamos lineas "control"
                    if stroke_enabled:
                        for stroke in stroke_lines:
                            cv2.line( image, stroke[0], stroke[1], stroke[2], stroke[3])


                    # Check ultrasonic sensor data (distance to objects in front of the car)
                    if ultrasonic_sensor_distance is not None and ultrasonic_sensor_distance < ultrasonic_stop_distance:
                        cv2.putText( image, 'OBSTACLE ' + str( ultrasonic_sensor_distance ) + 'cm', ultrasonic_text_position, image_font, image_font_size, color_red, image_font_stroke, cv2.LINE_AA)
                        if log_enabled: print( 'Stop, obstacle in front! >> Measure: ' + str( ultrasonic_sensor_distance ) + 'cm - Limit: '+ str(ultrasonic_stop_distance ) + 'cm' )
                    else:
                        cv2.putText( image, 'NO OBSTACLE ' + str( ultrasonic_sensor_distance ) + 'cm', ultrasonic_text_position, image_font, image_font_size, color_green, image_font_stroke, cv2.LINE_AA)

                    # Show images
                    cv2.imshow('image', image)
                    if image_gray_enabled:
                        cv2.imshow('mlp_image', half_gray)
    
                    if cv2.waitKey(1) & 0xFF == ord('q'):
                        break

        finally:
            cv2.destroyAllWindows()
            print( 'Connection closed on videostream thread' )


# Class to handle the different threads 
class ThreadServer( object ):

    # Server thread to handle the video
    def server_thread_camera(host, port):
        print( '+ Starting videocamera stream server in ' + str( host ) + ':' + str( port ) )
        server = socketserver.TCPServer((host, port), StreamHandlerVideocamera)
        server.serve_forever()

    # Server thread to handle ultrasonic distances to objects
    def server_thread_ultrasonic(host, port):
        print( '+ Starting ultrasonic stream server in ' + str( host ) + ':' + str( port ) )
        server = socketserver.TCPServer((host, port), StreamHandlerUltrasonic)
        server.serve_forever()

    print( '+ Starting server - Logs ' + ( log_enabled and 'enabled' or 'disabled'  ) )
    thread_ultrasonic = threading.Thread( name = 'thread_ultrasonic', target = server_thread_ultrasonic, args = ( server_ip, server_port_ultrasonic ) )
    thread_ultrasonic.start()
    
    thread_videocamera = threading.Thread( name = 'thread_videocamera', target = server_thread_camera, args = ( server_ip, server_port_camera ) )
    thread_videocamera.start()



# Starting thread server handler
if __name__ == '__main__':
    ThreadServer()

```

En ambos _scripts_ he ido incluendo variables para facilitar la configuración usada, como puede ser la activación / desactivación de logs, dimensiones de imágenes, etcétera, por lo que es recomendable revisar dicha configuración cuando estéis haciendo pruebas. Ahora sólo queda probarlo, arrancando primero el servidor y luego el cliente (accediendo previamente al entorno virtualizado python mediante `workon cv`):

* En el servidor: `python server.py`
* En el cliente: `python client.py`

El resultado será similar al de la imagen que acompaña este post. En próximos capítulos empezaremos a detectar señales de STOP mediante redes neuronales. ¡Espero que os guste!

## Más información sobre el proyecto

* [Código en Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car)
* [Coche RC autónomo - Índice del proyecto Construyendo un vehículo autónomo]({{ site.url }}/2017/08/22/autonomous-rc-car-construyendo-un-coche-autonomo)
