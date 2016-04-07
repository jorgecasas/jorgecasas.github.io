---
layout: post
title:  "Proyecto SONDA (V) - Sistemas de control"
date:   2015-02-15 17:35:00
categories: projects sonda
tags:
- projects
- sonda
---

El siguiente paso del [Proyecto SONDA]({{ site.url }}/2014/12/18/proyecto-sonda-i), es conseguir que la cápsula realice varias tareas en su vuelo: Que sea capaz de comunicarse con el centro de control móvil en tierra enviando su posición y ser capaces de obtener fotografías. En proyectos espaciales se utilizan equipos tecnológicamente avanzados, pero en nuestro proyecto casero utilizaremos elementos más cotidianos y asequibles.

![Raspberry Pi B+]({{ site.url }}/assets/images/2015-02-15-proyecto-sonda-v.jpg)


Sistemas de control de la sonda
===============================

Hay que tener en cuenta que **el principal objetivo de la misión** consiste en que la sonda llegue lo más alto posible, tome fotos de la estratosfera, y **pueda ser recuperada**. 

Como veremos en futuros posts, tendremos en cuenta el lugar, fecha y hora de lanzamiento y las condiciones meteorológicas, ya que haremos previamente varias _simulaciones de vuelo_ utilizando varios servicios disponibles en Internet y Google Maps / Google Earth. Con estos _análisis previos_ tendremos una idea del área en la que es probable que aterrice la sonda (ya que casi todo lo que sube vuelve a caer), pero no sabremos con 100% de precisión las coordenadas (latitud/longitud) en las que podremos recuperar la sonda (y con la sonda, la tarjeta / memoria con las fotografías obtenidas a 30 Km de altitud).

Es por ello que necesitaremos un dispositivo controlador que sea capaz de varias tareas:

* Obtener y almacenar en una tarjeta de memoria fotografías de forma periódica, e incluso vídeo.
* Obtener la posición de la sonda en cada momento (latitud / longitud... y altitud)
* Ser capaces de enviar a tierra (al centro de control móvil) los datos de geolocalización y vuelo.

Hay varias formas de obtener estos dispositivos de forma sencilla:

* **Microcontroladores electrónicos y placas electrónicas creados para tal efecto**: Se pueden encontrar en tiendas de electrónica, como la famosa [RadioShack](http://www.radioshack.com/) estadounidense. Problema: Hay que conocer la arquitectura de los elementos, lenguajes de programación que desconozco o que tengo oxidados (desde ensamblador a C) y bastante de electrónica. Por el tiempo que tengo yo disponible, no es opción.
* **Microcontroladores opensource**: Como puede ser [Arduino](http://www.arduino.cc/) y sus clones. Es más sencillo que lo expuesto en el paso anterior (no deja de ser unir componentes y programar en lenguajes como C), pero aún así me supondría más tiempo del que dispongo.
* **Microcomputadoras opensource**: Como es el caso de [Raspberry Pi](http://www.raspberrypi.org/) o sus clones. **Raspberry Pi** es una plataforma opensource que prácticamente con enchufarla a un cargador podemos disfrutar de un mini-ordenador con muy buenas características (1GB de RAM, procesador de doble núcleo ARM, ...). Es mi opción preferida.

Es decir, en el proyecto SONDA vamos a utilizar una Raspberry Pi (modelo B+), ya que:

* En pocos minutos puedes instalar en su tarjeta de memoria una variante del sistema operativo Debian llamada Raspbian. 
* En dicho sistema operativo se puede instalar con suma facilidad aquellos servicios que necesito para el proyecto SONDA: Apache, PHP, MySQL... es decir, todo el _stack_ básico de LAMP.
* A la Raspberry Pi se le puede poner muy fácilmente una cámara de 5 Mpx, y placas GPS y 3G / GPRS (o bien utilizar un _pincho_ USB 3G/GPRS). También se le pueden poner sensores de temperatura y humedad gracias a los pines de entrada / salida GPIO.
* En la tarjeta de memoria (de 8, 16, 32GB...) podré almacenar todas las fotografías del vuelo. Suponiendo que cada fotografía ocupe unos 5MB, que el vuelo dure unas 3.5 horas y que vaya a tomar 4 fotos por minuto, en total necesitaré para imágenes unos 4.25 GB de espacio... por tanto, más que de sobra...
* Existen complementos de la Raspberry Pi, como son las carcasas de 6 baterías AA, capaces de dar energía suficiente durante 16 horas con esas 6 pilas... y es que el dispositivo Raspberry Pi es un dispositivo de bajísimo consumo. Esto es bueno, porque así las baterías (que habrá que testearlas y acordarse de cargarlas para el día D) durarán todo el vuelo hasta ser recuperada la sonda.
* Y lo mejor de todo: Es muy fácilmente programable, con lenguajes que conozco y que prácticamente tenemos todo desarrollado.


Programación del dispositivo Raspberry Pi B+
============================================

Como ya hemos comentado, la Raspberry Pi B+ va a contar con el _stack_ LAMP básico: Sistema operativo Linux / Servicio web Apache / Base de datos MySQL / Motor de aplicaciones PHP.

Para obtener fotografías, la temperatura y humedad del sensor correspondiente, lectura de los pines de entrada y salida GPIO, etcétera, el firmware del dispositivo cuenta con varias aplicaciones de línea de comandos escritas en Python. Realizar llamadas a dichas aplicaciones desde código PHP es muy sencillo, por lo que ese es el plan: Utilizar los diferentes módulos de los que dispone el sistema [SmartRoads de Gestión de Infraestructuras](https://www.tecnocarreteras.es/smartroads) de [ITERNOVA](https://www.iternova.net) que desarrollamos en mi empresa a diario para obtener datos de los diferentes sensores, almacenarlos en la base de datos MySQL del dispositivo y utilizar clientes de API JSON Rest para enviar dichos datos a un sistema web. 

En dicho sistema, a través de un navegador web, podré consultar en tiempo real en dónde se encuentra el dispositivo (su geolocalización, tanto de latitud / longitud como de altitud), visualizando en mapas (Google Maps) el recorrido de la sonda. Al llegar a tierra, podremos encontrar fácilmente dónde ha aterrizado (en principio), y así recuperar las fotografías y el dispositivo (esperemos que en perfectas condiciones tras la caída). 

La idea es crear una tarea periódica que obtenga los datos y que los vaya enviando a tierra (a la vez que, como ya he dicho, los vaya almacenando en la base de datos MySQL, y las imágenes en la tarjeta de memoria). También podremos comunicarnos con el dispositivo de forma bidireccional gracias al servicio de API JSON Rest que irá instalado en el dispositivo (por ejemplo, para pedir que envíe datos cada 15 segundos en lugar de cada 30, o solicitarle una imagen en baja resolución de forma manual, o solicitar un informe de estado del dispositivo (como carga de batería, temperatura, carga de la CPU, espacio disponible en memoria, etcétera)).


A tener en cuenta: Posición con GPS y comunicaciones con GPRS
=============================================================

Los dispositivos GPS suelen estar _capados_. Es decir, que cuando sobrepasan una altitud (10.000 metros generalmente) y/o una velocidad máxima (más de 300 - 400 Km/hora), dejan de funcionar. Esto es así debido a que si no podrían ser utilizados para construir misiles intercontinentales. Hay ciertos modelos que sí que permiten sobrepasar la limitación de la altitud, pero suelen ser más caros o difíciles de conseguir.

Por tanto, si no se dispone de este tipo de GPS más avanzados, va a haber una ventana temporal en la que no tengamos información de latitud / longitud (y altitud, a no ser que introduzcamos un sensor de altitud en la Raspberry Pi). Es decir: En la subida iremos recibiendo datos de posición y haremos seguimiento de la sonda, luego llegará un momento de _oscuridad_, y tras unas horas, si los dispositivos / baterías no han fallado, volveremos a recibir señal de posición. Esperemos que no sea demasiado tarde...

Por otra parte, también hay que tener en cuenta que en las comunicaciones las sondas espaciales _profesionales_ cuentan con antenas parabólicas, sistemas radio avanzados, sistemas de seguimiento por láser... cosas de las que no disponemos en este proyecto casero. La idea es utilizar un dispositivo 3G / GPRS, pero aquí tenemos más limitaciones:

* Por supuesto que no vamos a tener cobertura 3G. No estaremos cerca de ciudades, con lo que nos quedará únicamente GPRS. El envío de alguna fotografía queda prácticamente descartado (si bien es posible que haga algún _script_ para reducir alguna imagen y tratar de enviar dicha imagen de baja resolución)
* Igualmente, GPRS está pensado para ser utilizado desde el suelo, que es donde la mayoría de los humanos nos encontramos a diario. Hacia donde nos encontramos la mayor parte de los humanos es hacia donde están las antenas de telefonía móvil apuntando, para aprovechar mejor la energía emitida... 
* Por eso, [conforme aumenta la altitud sobre el suelo, tendremos menos cobertura](http://11-s.eu.org/11-s/Llamadas%20desde%20m%F3viles): A unos 600 metros sobre el nivel del suelo se reduce entorno al 10%, a unos 2500 metros se ve reducida un 80%, y a unos 10000 metros se reduce a más de un 99%... Estos datos son para conseguir tener una conversación telefónica, para GPRS no es tan limitante, pero da una idea de cómo se van a ver afectadas las transmisiones de datos de posición de la sonda...
* Es decir, es probable que a partir de los 2000 - 3000 metros de altitud ya no tengamos comunicación en condiciones con la sonda. 

Por tanto, habrá que idear la forma para detectar cuándo tenemos cobertura y aprovechar para enviar la posición dentro de esas ventanas de tiempo (antes del impacto).

Dónde adquirir la Raspberry Pi y otros accesorios
=================================================

He adquirido la Raspberry Pi B+ y la carcasa a través del portal chupiganga.com (aunque ahora ya no está en servicio y puede encontrarse en otras webs), ya que dispone de un apartado de estos dispositivos con accesorios seleccionados.


Índice del Proyecto SONDA
=========================

* [Proyecto SONDA - Introducción]({{ site.url }}/2014/12/18/proyecto-sonda-i)
* [La cápsula (_payload_)]({{ site.url }}/2014/12/25/proyecto-sonda-ii)
* [Sistemas de recuperación (_parachute_)]({{ site.url }}/2014/12/28/proyecto-sonda-iii)
* [Sistemas de propulsión (globo de helio)]({{ site.url }}/2015/01/15/proyecto-sonda-iv)
* [Sistemas de control]({{ site.url }}/2015/02/15/proyecto-sonda-v)
