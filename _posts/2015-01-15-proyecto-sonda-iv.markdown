---
layout: post
title:  "Proyecto SONDA (IV) - Sistemas de propulsión (globo)"
date:   2015-01-15 22:35:00
categories: projects sonda
tags:
- projects
- sonda
---

¡Feliz año! Continuando con el [Proyecto SONDA]({{ site.url }}/2014/12/18/proyecto-sonda-i), hoy voy a comentar los **mecanismos de propulsión de la cápsula**. Es decir, el globo que llenaremos con helio y que arrastrará todo el peso del lastre (cápsula y paracaídas) hacia el espacio cercano.

![Un globo científico]({{ site.url }}/assets/images/2015-01-15-proyecto-sonda-iv.jpg)


Globos científicos y para experimentos meteorológicos
=====================================================

En primer lugar, tendremos que conseguir un **globo meteorológico (globos científicos)**, ya que no nos van a servir los globos de latex convencionales. Requerimos un globo lo suficientemente grande como para que sea capaz de arrastrar la cápsula y todo el peso del conjunto.

Para calcular volumen cúbico de helio requerido (y por tanto, adquirir un globo que lo soporte), tendremos que hacer los siguientes cálculos:

<pre>
Peso del conjunto (cápsula + paracaídas + cables...) [Kg] + Peso del globo [Kg] = Volumen cúbico de helio requerido [m<sup>3</sup>] + Volumen de Helio adicional [m<sup>3</sup>]
</pre>

Es decir, si ponemos el siguiente ejemplo:

* Peso del conjunto (cápsula, paracaídas, cables...) = 1.5 Kg
* Peso del globo = 0.5 Kg

Requeriremos, por tanto, 2 m<sup>3</sup> de Helio, más una cantidad adicional (para que el volumen total de Helio sea mayor y el globo tienda a subir, en lugar de quedarse flotando a la misma altura). En este ejemplo, **añadiremos unos 0.25 m<sup>3</sup> de Helio adicional**, de manera que el globo pueda alcanzar unos 35 Km de altitud en unas dos horas, teniendo así un total de 2.25 m<sup>3</sup> de Helio.

Nuestro globo, por tanto, tendrá que ser un globo de 500 gramos con una capacidad de al menos 2.25 m<sup>3</sup> de volumen.

Eso sí... **¿Cuál es la cantidad óptima de Helio adicional?**. Cuanto más se infle el globo más rápido subirá, pero también explotará antes, por lo que alcanzará una altitud menor... si bien al subir más rápido el vuelo durará menos tiempo y la sonda aterrizará más cerca de nuestra posición de origen (ya que será arrastrada menos tiempo por el viento). Si la cantidad de Helio adicional es muy pequeña, el globo ascenderá mucho más lentamente pero tardará mucho más en explotar, por lo que podrá alcanzar casi los 40 Km de altitud, pero caerá muy lejos del punto de partida (probablemente a más de 100 Km de distancia). Por ello será importante, como veremos en futuros posts, elegir correctamente el área de lanzamiento.

También hay que tener en cuenta **el peso del globo**. A mayor peso, será más resistente, por lo que estallará más tarde (podrá expandir su volumen en mayor grado). En [www.weatherballoons.asia](http://www.weatherballoons.asia/weather-balloons-data-sheets/sounding-balloons-data) se pueden encontrar tablas de diámetros (nada más inflado y en el momento de la explosión o _burst_) y pesos de globos, así como a qué altura se estima que puedan llegar. A modo de resumen:

  * **Globo de 500g** - Altitud aproximada: 25000m - Diámetro inicial máximo aproximado: 380 cm - Diámetro de _burst_: Más de 500cm - Payload máximo: 900g
  * **Globo de 750g** - Altitud aproximada: 28000m - Diámetro inicial máximo aproximado: 450 cm - Diámetro de _burst_: Más de 650cm - Payload máximo: 1200g
  * **Globo de 1000g** - Altitud aproximada: 31000m - Diámetro inicial máximo aproximado: 500 cm - Diámetro de _burst_: Más de 750cm - Payload máximo: 1300g
  * **Globo de 1500g** - Altitud aproximada: 33000m - Diámetro inicial máximo aproximado: 600 cm - Diámetro de _burst_: Más de 950cm - Payload máximo: 2250g
  * **Globo de 2000g** - Altitud aproximada: 38000m - Diámetro inicial máximo aproximado: 600 cm - Diámetro de _burst_: Más de 1100cm - Payload máximo: 2500g

Estos valores son aproximados (habría que calcularlos según peso de la cápsula, paracaídas, etcétera... el _payload_), pero sirven de ejemplo para ver que si queremos que llegue más o menos alto requeriremos unos tamaños y calidades del globo específicos.


Hay más información sobre **como anudar el globo** en [High altitude science](http://www.highaltitudescience.com/pages/tying-off-a-weather-balloon)

Helio
=====

El Helio se vende en bombonas:

* Desechables, suelen utilizarse en fiestas de cumpleaños. Una vez agotada, la bombona va al contenedor amarillo para su reciclaje.
* Retornables, se suelen alquilar (además de tener que dar una fianza que podría llegar a 200 Euros) y devolver tras un período de tiempo estipulado. Son de un tamaño mucho mayor (de hasta varios m<sup>3</sup> de Helio).

En nuestro ejemplo, requerimos un globo de 2.25 m<sup>3</sup>. Teniendo en cuenta que los globos van a tener una forma casi esférica (en principio), la fórmula del volumen será:

<pre>
Volumen [m<sup>3</sup>] = 4/3 pi * r<sup>3</sup> [m<sup>3</sup>]
</pre>

Luego el radio del globo de 500 gramos (condición del punto anterior para nuestro ejemplo) requerido tiene que ser de al menos unos 0.82 metros de radio (1.65 metros de diámetro). Habrá que buscar un globo que para un peso reducido tenga las dimensiones válidas (conviene hacer una hoja de cálculo para ir probando con varios modelos de forma más eficiente). 

Hay que tener en cuenta que el diámetro inicial del globo (a nivel de tierra) será mucho menor que el diámetro que vaya adquiriendo a gran altitud (ya que hay menos presión atmosférica, y el gas helio tenderá a expanderse aún más). De ahí que los diámetros de _burst_ (de explosión) sean mucho mayores.

Si nos fijamos, hay botellas desechables para inflado de globos de fiesta que indican que da para llenar 50 globos de 20 cm de diámetro. Cada uno de estos globos representa aproximadamente 0.004 m<sup>3</sup> de volumen, por lo que una de estas botellas desechables son aproximadamente 0.2 m<sup>3</sup> de volumen... Por tanto, **requeriremos alquilar una botella retornable de mayor capacidad para minimizar los costes**.

Como curiosidad adicional. El globo podría ser llenado con **hidrógeno** en lugar de con helio, ya que es más ligero... pero al ser combustible (el helio es un gas inerte) es mucho más peligroso, y por tanto no es nada recomendable su uso para esta actividad lúdica. Eso sí, aquí dejo un vídeo de cómo fabricar vuestro propio hidrógeno...

<iframe width="420" height="315" src="//www.youtube.com/embed/O6Mr4-uMkak" frameborder="0" allowfullscreen></iframe>


Enlaces de interés
------------------

Se pueden encontrar globos en diferentes tiendas, así como helio (en botellas desechables o reciclables). Por ejemplo:

* [Scientific Sales](http://www.scientificsales.com/Meteorological-Weather-Sounding-Balloon-s/25.htm) - Página con globos meteorológicos y científicos de diferentes tamaños, y otros accesorios.
* [High altitude science](http://www.highaltitudescience.com) - Web informativa en el que podemos encontrar información sobre globos / helio, así como realizar pedidos en su tienda. También explican [como anudar el globo](http://www.highaltitudescience.com/pages/tying-off-a-weather-balloon) o [cómo calcular el helio necesario](http://www.highaltitudescience.com/pages/helium)
* [Don Globo](https://www.donglobo.com) - Alquiler de bombonas y venta de helio


Índice del Proyecto SONDA
=========================

* [Proyecto SONDA - Introducción]({{ site.url }}/2014/12/18/proyecto-sonda-i)
* [La cápsula (_payload_)]({{ site.url }}/2014/12/24/proyecto-sonda-ii)
* [Sistemas de recuperación (_parachute_)]({{ site.url }}/2014/12/28/proyecto-sonda-iii)
* [Sistemas de propulsión (globo de helio)]({{ site.url }}/2015/01/15/proyecto-sonda-iv)
