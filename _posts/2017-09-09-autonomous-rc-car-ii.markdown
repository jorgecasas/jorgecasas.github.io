---
layout: post
title:  "Coche RC autónomo (II)"
date:   2017-09-09 14:15:00
categories: blog development projects
tags:
- blog
- development
- projects
thumbnail: /assets/images/2017-09-09-autonomous-rc-car-ii-01.jpg
---

Como ya comenté en el post **[Coche RC autónomo (I)]({{ site.url }}/2017/08/23/autonomous-rc-car-i)**, este año pretendo realizar un _side project_ tecnológico un tanto especial. 

![Autonomous RC Car](/assets/images/2017-09-09-autonomous-rc-car-ii-01.jpg)

Tras analizar los requisitos he ido pidiendo los materiales que vamos a necesitar y ya están comenzando a llegar:

* Coche teledirigido (escala 1:20)
* Raspberry Pi 3 (ya la teníamos en la oficina), que ya cuenta con adaptador WiFi
* Sensores requeridos para la Raspberry Pi 3: Cámara y sensores de infrarrojos
* Cinta para marcar el circuito por el que el coche deberá circular de forma autónoma, una vez tengamos el prototipo creado.

En principio, el proyecto que me iba a servir de referencia era del de **[Zheng Wang](https://zhengludwig.wordpress.com/projects/self-driving-rc-car/)**, el cual ofrecía su código fuente de forma abierta en [Github - hamuchiwa/AutoRCCar](https://github.com/hamuchiwa/AutoRCCar). Este proyecto consistía, brevemente explicado sin detalles, en lo siguiente:

* Colocar la Raspberry Pi en el vehículo teledirigido
* La Raspberry Pi tomaría las imágenes y las enviaría por WiFi al ordenador (conectado en la misma red local)
* El ordenador procesaría las imágenes mediante una red neuronal. 
* Las diferentes salidas obtenidas en la red neuronal (acelerar, frenar, girar a izquierda y girar a derecha) las transmitiríamos a una placa Arduino conectada mediante USB al ordenador.
* Las salidas del Arduino estarían conectadas a los cables del mando a distancia del coche teledirigido, por lo que la acción a realizar (acelerar, frenar, girar, etcétera) las enviaría el propio mando a distancia al vehículo. Es decir, el Arduino controlaría el mando a control como si se tratara de un humano.

De esta forma no tendríamos que tocar el vehículo, sólo el mando a distancia. El problema es que necesitamos una placa Arduino adicional.

Buscando en Internet (vuelvo a decir que Internet es una herramienta increíble si se utiliza bien) he encontrado otros proyectos parecidos interesantes, como el de **[Shaun Butler](https://github.com/shaunuk)**, que también tiene el código compartido en [Github - Pi Car](https://github.com/shaunuk/picar). Este proyecto no es en sí de conducción autónoma sino de control del vehículo teledirigido desde el móvil (y quien dice móvil, dice desde el ordenador), utilizando únicamente una Raspberry Pi. El proceso que sigue es el siguiente:

* Colocar la Raspberry Pi en el vehículo teledirigido. 
* Instalar en la Raspberry Pi varias librerías / aplicaciones para controlar las salida PWM (_Pulse Width Modulation_, o [modulación por ancho de pulsos](https://es.wikipedia.org/wiki/Modulaci%C3%B3n_por_ancho_de_pulsos)), de manera que conectaríamos dichas salidas de la Raspberry Pi directamente a los cables del propio coche teledirigido que se encargan de controlar los servos de acelerar / frenar, girar a izquierda o derecha.
* La Raspberry Pi estaría conectada por WiFi al teléfono móvil (en la misma red) o al ordenador. Utilizando aplicaciones ya existentes o programando la API correspondiente, desde el ordenador podríamos lanzar comandos directamente a la Raspberry Pi de manera que podamos indicarle que acelere o que gire desde el propio ordenador.

Así, sin necesidad de Arduino extra, tendríamos que modificar el coche teledirigido para conectarle las conexiones como corresponda (y no el mando a distancia). Y en este punto ya podríamos realizar los pasos del proyecto anterior relacionados con la conducción autónoma:

* La Raspberry Pi tomaría las imágenes y las enviaría por WiFi al ordenador (conectado en la misma red local)
* El ordenador procesaría las imágenes mediante una red neuronal. 
* Las diferentes salidas obtenidas en la red neuronal (acelerar, frenar, girar a izquierda y girar a derecha) las transmitiríamos directamente a la Raspberry Pi desde el ordenador utilizando la WiFi (en la misma red local ambos equipos)

Por tanto, esta va a ser (en principio, luego puede ser que cambie) la forma en la que pretendemos controlar el vehículo, ya que es muy interesante también poder controlar de forma manual desde el ordenador el coche teledirigido, como si de un _rover_ en suelo marciano se tratara, controlándolo desde lugares remotos.

En la búsqueda de información he encontrado además otros muchos proyectos interesantes para obtener el mismo resultado. Si os interesa saber más, podéis acceder a **[Donkey Car](http://www.donkeycar.com/)**, que tiene pasos muy detallados de cómo crear un vehículo autónomo muy autónomo (que incluso compita en carreras con otros vehículos).


## Más información sobre el proyecto

* [Coche RC autónomo (I)]({{ site.url }}/2017/08/23/autonomous-rc-car-i)
