---
layout: post
title:  "Coche RC autónomo (VII) - Configurando el sensor ultrasónico HC-SR04 para detectar objetos"
date:   2017-10-02 14:45:00
categories: blog development projects
tags:
- blog
- development
- projects
thumbnail: /assets/images/assets/images/2017-10-02-sensor-ultrasonico-hc-sr04.jpg
---

Además de utilizar la cámara, los coches autónomos tendrán muchos otros sensores que obtendrán datos que podrán ser utilizados para garantizar la seguridad e integridad de los vehículos y todo lo que les rodea. Así, en este post voy a detallar cómo utilizar el **[sensor de ultrasonidos HC-SR04](http://amzn.to/2yC24UO)** para detectar obstáculos delante del vehículo.

El mismo script y circuito que vamos a utilizar puede servir como sensor de aparcamiento, ya que nos permitirá detectar a qué distancia nos encontramos de un obstáculo o pared. También puede ser utilizado para medir cuán lleno está un pozo o un bidón de almacenamiento de líquidos.

![Sensor HC-SR04](/assets/images/2017-10-02-sensor-ultrasonico-hc-sr04.jpg)

El mecanismo de este sensor es el siguiente:

* Emite un pulso de sonido durante un pequeño período de tiempo. El pulso lo emitimos de forma periódica, y es como un disparo (_trigger_).
* El sonido avanza hasta chocar con un objeto y rebota. 
* El sonido rebotado es el eco (_echo_), que se recibirá y detectará en el sensor receptor.
* Conociendo la velocidad del sonido en el aire y calculando el tiempo transcurrido desde que se lanzó el pulso sónico y se recibe su eco, podemos conocer la distancia (teniendo en cuenta que el sonido habrá ido y habrá vuelto, por lo que tendremos que dividir la distancia entre 2).

Así, en nuestro diseño definiremos una distancia límite. Si el vehículo se encuentra más cerca de un obstáculo que dicha distancia, se detendrá para no chocar.

En las pruebas que hemos realizado (ver el _script_ a continuación, que puede ser descargado de [Código en Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car)), vamos a encender un led cuando estemos a menos distancia y mostraremos un mensaje de alerta. De esta forma, y con un pequeño circuito, aprenderemos también a utilizar las entradas y salidas **GPIO** de la Raspberry Pi.

En primer lugar, accedemos a la Raspberry Pi y accedemos al entorno virtual de Python, instalando con `pip` la librería requerida para poder gestionar las entradas y salidas _GPIO_:

```bash
workon cv
pip install RPi.GPIO
```

El script `sensor-ultrasonidos-HC-SR04.py` es el siguiente:

```python
# Importamos librerias RPi.GPIO (entradas/salidas GPIO de Raspberry Pi) y time (para sleeps, etc...)
# Reqiuere previamente instalarla (pip install RPi.GPIO)
import RPi.GPIO as GPIO
import time

# Ponemos la placa en modo BCM
GPIO.setmode(GPIO.BCM) 

# Definimos los pines GPIO de la Raspberry Pi 3 (segun esquema)
#   12 - Led (output)
#   18 - Trigger (output)
#   24 - Echo (input)
GPIO_LED = 12
GPIO_TRIGGER = 18
GPIO_ECHO = 24

# Configuramos los pines como salidas (trigger y led) o entradas (detector de eco)
GPIO.setup(GPIO_TRIGGER,GPIO.OUT) #Configuramos Trigger como salida
GPIO.setup(GPIO_ECHO,GPIO.IN) #Configuramos Echo como entrada
GPIO.setup( GPIO_LED, GPIO.OUT ) # Pin de led

# Inicializamos los pines de salida (apagados)
GPIO.output( GPIO_TRIGGER, False ) 
GPIO.output( GPIO_LED, False )

# Distancia en cm
distance_limit = 15

try:
    print( 'Sensor ultrasonico HC-SR04' )

    # Iniciamos un loop infinito
    while True: 

        # Enviamos un pulso de ultrasonidos durante un poco de tiempo
        GPIO.output(GPIO_TRIGGER,True)
        time.sleep(0.00001)
        GPIO.output(GPIO_TRIGGER,False)
        
        # Desde que dejamos de enviar el pulso, obtenemos tiempo actual
        start = time.time()

        # Si el sensor no recibe sonido, mantenemos el tipo actual actualizado
        while GPIO.input(GPIO_ECHO)==0:
            start = time.time()

        # Si el sensor recibe sonido, obtenemos el tiempo fin
        while GPIO.input(GPIO_ECHO)==1:
            stop = time.time()

        # Obtenemos el tiempo transcurrido. 
        # La distancia sera igual al tiempo transcurrido por la velocidad (partido por 2, porque 
        # el sonido va y vuelve desde el sensor al objeto y del objeto al sensor): 2 D = (T x V)/2
        elapsed = stop-start
        distance = (elapsed * 34300) / 2
        
        # Si la distancia es menor que la distancia limite fijada... Paramos y encendemos led de alerta!
        if distance < distance_limit:
            print( 'Stop! Objeto a ' + str( distance ) +'cm' )
            GPIO.output( GPIO_LED, True )
        else:
            # En este caso, podemos continuar avanzando sin obstaculos!
            print( 'Go! No hay objetos delante' )
            GPIO.output( GPIO_LED, False )

        # Pausamos un poco para no saturar el procesador de la Raspberry Pi
        time.sleep( 0.25 )

except KeyboardInterrupt: 
    # Si el usuario pulsa CONTROL+C... fin de aplicacion (limpieando los pines GPIO)
    print( 'Sensor Stopped' )
    GPIO.cleanup()

```

Siguiendo el _script_, tenemos que crear un circuito teniendo en cuenta la distribución de los pines _GPIO_ de la Raspberry 3:

![Raspberry Pi 3 - GPIO Schema](/assets/images/2017-10-02-raspberrypi-gpio-schema.jpg)

* Conectaremos pin VCC+ al pin VCC y el pin GROUND al pin GROUND del sensor HC-SR04, para alimentarlo.
* Conectaremos el pin de salida GPIO 18 de la Raspberry Pi directamente al pin TRIGGER del sensor HC-SR04
* Conectaremos el pin de entrada GPIO 24 de la Raspberry Pi al pin ECHO del sensor HC-SR04, pero no lo haremos directamente sino mediante una resistencia de 1K.

Adicionalmente, conectaremos un led que encenderemos cuando el obstáculo se encuentre más cerca de la distancia límite:

* Conectaremos el pin de salida GPIO 12 de la Raspberry Pi directamente al pin positivo del led
* Conectaremos una resistencia de 1K del otro pin del led al pin GROUND de la Raspberry Pi

Y sólo queda probarlo ejecutando el siguiente comando. El led deberá encenderse y apagarse según acerquemos objetos al sensor.

```bash
python sensor-ultrasonidos-HC-SR04.py
```

![Sensor HC-SR04](/assets/images/2017-10-02-sensor-ultrasonico-hc-sr04.gif)

Este código lo adaptaremos en los próximos pasos dentro de nuestro _script_ global, encapsulándolo en un hilo independiente, de manera que podamos utilizar la información obtenida a la hora de controlar el vehículo.

## Más información sobre el proyecto

* [Código en Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car)
* [Coche RC autónomo - Índice del proyecto Construyendo un vehículo autónomo]({{ site.url }}/2017/08/22/autonomous-rc-car-construyendo-un-coche-autonomo)
