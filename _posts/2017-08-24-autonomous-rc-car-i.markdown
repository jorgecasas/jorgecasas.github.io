---
layout: post
title:  "Coche RC autónomo (I)"
date:   2017-08-23 14:15:00
categories: blog development projects
tags:
- blog
- development
- projects
thumbnail: /assets/images/2017-08-23-autonomous-rc-car-i-01.jpg
---

A pesar de que aún tengo varios [proyectos inacabados]({{ site.url }}/2014/11/26/proyectos-inacabados) en marcha, este año tengo decidido hacer un _side project_ (proyecto a ir haciendo poco a poco en ratos libres, si es que los tengo), para construir un [coche teledirigido autónomo](http://amzn.to/2veFVdV). Es decir, aprender sobre el coche autónomo y su funcionamiento (que es la base de los coches _inteligentes_ y con conducción autónoma que ya estamos empezando a ver en las carreteras).

![Autonomous RC Car](/assets/images/2017-08-23-autonomous-rc-car-i-01.jpg)

Para ello, y gracias a que Internet permite a la gente compartir información y conocimiento, me basaré en el proyecto de [Zheng Wang](https://github.com/hamuchiwa/AutoRCCar), que es de los más completos, detallados y fáciles de entender que podréis encontrar sobre la materia (además de compartir íntegramente el código utilizado en [Github](https://github.com/hamuchiwa/AutoRCCar)). La fotografía que abre este _post_ es de su proyecto. Así, algunos de los componentes necesarios son:

## Coche teledirigido

Se podría hacer el chasis donde ir colocando los componentes con una impresora 3D, o reciclando materiales de andar por casa (cartones, botellas de plástico, etcétera), pero para simplificar el desarrollo usaremos un [coche RC de los baratos](http://amzn.to/2veFVdV), ya que así nos soluciona parte de otros problemas (dirección, montaje de servos, montaje de sistemas de alimentación, acoplamiento del motor a las ruedas motrices, etcétera). También hay chasis ya específicos para este tipo de proyectos (para ir montando todas piezas del vehículo), e incluso como [paquete completo](http://amzn.to/2vnIJEs).

[![Coche teledirigido](/assets/images/2017-08-23-autonomous-rc-car-i-02.jpg)](http://amzn.to/2veFVdV)

## Controlador (Raspberry Pi)

Usaré una [Raspberry Pi](http://amzn.to/2wmTn2S) que tenemos para hacer pruebas e inventos en la oficina, junto con varios componentes adicionales (cámara y sensores infrarrojos). Al tener instalado GNU/Linux (Raspbian) podré instalar librerías programables y el código que desarrolle / adapte para utilizar toda la información obtenida (imágenes, datos de sensores, etcétera), de forma muy sencilla, a la vez que podré utilizar las salidas que dispone (junto con algo más de electrónica adicional) para activar y controlar los servos de dirección y de aceleración del vehículo.

[![Raspberry Pi](/assets/images/2015-02-15-proyecto-sonda-v.jpg)](http://amzn.to/2wmTn2S)

## Sensores (cámara y sensor de infrarrojos)

Para obtener datos que pueda procesar, utilizaré una [cámara](http://amzn.to/2v5dt1P) (para procesar imágenes y saber por dónde va la carretera) que ya tiene integrados [sensores infrarrojos](http://amzn.to/2v5dt1P) (para detectar obstáculos frontalmente y evitar colisión), que es compatible con la [Raspberry Pi](http://amzn.to/2wmTn2S).

[![Sensores: Cámara y sensor infrarrojos](/assets/images/2017-08-23-autonomous-rc-car-i-04.jpg)](http://amzn.to/2v5dt1P)

## Redes neuronales y procesado

El cerebro del coche autónomo (la inteligencia artificial _AI_) se basa en parte en redes neuronales que procesan las imágenes y datos de sensores disponibles (bien sean datos de [radar](https://blog.nxp.com/automotive/radar-camera-and-lidar-for-autonomous-cars), datos obtenidos mediante láser ([LIDAR](https://www.nytimes.com/2017/05/25/automobiles/wheels/lidar-self-driving-cars.html)), imágenes, otros sensores instalados en el vehículo...). 

Con esos datos de entrada y el entrenamiento correspondiente la red neuronal puede obtener una salida en cada momento y situación (por ejemplo, acelerar, frenar, girar... según su posición y el entorno en el que se encuentre el vehículo)

![Red neuronal](/assets/images/2017-08-23-autonomous-rc-car-i-03.jpg)

Usaré una librería para realizar el procesado de imágenes de la cámara como [OpenCV](http://opencv.org) (que es abierta y cualquiera la puede utilizar), o librerías para generar redes neuronales como [Tensorflow](https://www.tensorflow.org/). 

Asimismo, como en el mundo hay mucha gente y alguna es brillante en lo suyo, en [Github](https://github.com/search?q=rc+car+raspberry) hay compartidos cientos de proyectos con código de ejemplo para [crear sus propios coches autónomos](https://github.com/hamuchiwa/AutoRCCar). Incluso hay gente que ha creado sistemas para [controlar el vehículo de forma remota y autónoma utilizando un teléfono móvil Android](http://blog.davidsingleton.org/nnrccar/)

## Resultado esperado

Tras la configuración correspondiente, el resultado esperado es un coche que sea capaz de avanzar por una _carretera virtual_ marcada en el suelo sin ayuda humana, e incluso respetar señales y semáforos. Un ejemplo sería el que se puede visualizar en el siguiente vídeo:

<iframe width="740" height="416" src="https://www.youtube.com/embed/BBwEF6WBUQs" frameborder="0" allowfullscreen></iframe>

## Más información sobre el proyecto

* [Código en Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car)
* [Coche RC autónomo (I)]({{ site.url }}/2017/08/23/autonomous-rc-car-i) - Introducción al proyecto
* [Coche RC autónomo (II)]({{ site.url }}/2017/09/09/autonomous-rc-car-ii) - Primer análisis de requisitos del proyecto
* [Coche RC autónomo (III)]({{ site.url }}/2017/09/12/autonomous-rc-car-iii) - Primeros pasos de configuración de la Raspberry Pi
* [Coche RC autónomo (IV)]({{ site.url }}/2017/09/16/autonomous-rc-car-iv) - Configurando la videocámara en la Raspberry Pi
* [Coche RC autónomo (V)]({{ site.url }}/2017/09/24/autonomous-rc-car-v) - Instalando OpenCV (visión por computador)
* [Coche RC autónomo (VI)]({{ site.url }}/2017/09/27/autonomous-rc-car-vi) - Probando la cámara usando Python, OpenCV y streaming

