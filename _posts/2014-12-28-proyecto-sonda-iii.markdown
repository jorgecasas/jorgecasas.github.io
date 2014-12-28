---
layout: post
title:  "Proyecto SONDA (III)"
date:   2014-12-28 10:35:00
categories: projects sonda
tags:
- projects
- sonda
---

Hoy comentaremos los elementos de recuperación de la cápsula en el [Proyecto SONDA]({{ site.url }}/2014/12/18/proyecto-sonda-i). Los elementos de recuperación son aquellos mecanismos o artilugios que permitirán recuperar la cápsula (ya que dentro irán las cámaras de fotografía / vídeo, cuyos datos tendrán que ser recuperados en su mayor parte de forma manual). Por tanto, los elementos de recuperación deberán permitir recuperar la cápsula lo más entera posible... es decir, necesitamos... **un paracaídas**.


![Un paracaídas]({{ site.url }}/assets/images/2014-12-28-proyecto-sonda-iii-01.jpg)

Sistemas de recuperación: Paracaídas
====================================

La idea es obtener un paracaídas lo más barato posible, y que sea de suficiente calidad como para que no acabe destrozado debido a las condiciones atmosféricas. Hay empresas que se dedican a confeccionar estos tipos de paracaídas (para hobbies directos, ya que hay gente que se dedica a crear cohetes y otros artilugios que los requieren). 

Algunos comentarios sobre paracaídas:

* Hay que utilizar materiales como **nylon (que soporta rayos UV)** y a ser posibles de colores vistosos (por ejemplo, naranja fosforito), para poder ser visto sin ser confundido con los árboles o la tierra... y si es fosforito probablemente pueda ser visto incluso en la fase de descenso / caída...
* No debe estar construido con materiales porosos o que no soporten bajas temperaturas (ya comentamos que llegará a zonas de -60ºC).
* Debe atarse a unos 10 metros del globo de helio, y a otros 10 metros de la cápsula, de manera que nos aseguremos que no vaya a enredarse ni con el cable que unirá el globo con la cápsula ni con ella misma. 
* Nada más abrirse el paracaídas, al no existir prácticamente atmósfera, no habrá fricción, y la cápsula podrá descender a 150Km/h. Al llegar a la atmósfera, la velocidad se irá reduciendo hasta llegar a la velocidad adecuada.

Cálculo de tamaño de paracaídas
-------------------------------

Ahora bien... ¿Qué tamaño tiene que tener el paracaídas?. El cálculo del diámetro del paracaídas puede realizarse de forma completa o abreviada. Hay que tener en cuenta que **la velocidad de descenso de la cápsula deberá estar entre 3.35 y 4.25 m/s para ser considerado descenso seguro**

**Cálculo completo**

<pre>
Ap (área) = 2*g*m / ( gamma* Cd * V2 )
g = Aceleración debido a la gravedad (9.81 m/s2 a nivel del mar) 
m = masa de la cápsula
gamma = Densidad del aire a nivel del mar (1225 g/m3)
Cd = Coeficiente de frenado del paracaídas (aproximadamente 0.75 )
V = Velocidad de descenso de la cápsula, normalmente entre 3.35 y 4.25 m/s para ser considerado descenso seguro
Diámetro = 2 * sqr ( (2*Ap) / n * sin(360/n) )
n = Número de lados del paracaídas (p.e. n=6 para paracaídas hexagonal, n=8 para octogonal)
</pre>

**Cálculo abreviado**
<pre>
Diámetro (inches) = sqr (peso (pounds) * 0.454 ) * 39.6
1 inch = 2.54 cm
1 pound = 0.453 Kg
</pre>

Es decir, tras crear [la cápsula]({{ site.url }}/2014/12/25/proyecto-sonda-ii) e introducirle todos los dispositivos (lo veremos en otros posts más adelante), la pesaremos y calcularemos el diámetro requerido del paracaídas para un descenso seguro.

Hay tiendas en las que venden paracaídas ya preparados (pongo enlaces más adelante)

Tests de paracaídas
-------------------

Antes de enviar a la estratosfera nuestra sonda, habrá que testear todos los componentes que la forman. Para realizar los tests del paracaídas, aquí dejo algunos consejos:

* Utilizar un puente, lanzando la cápsula atada al paracaídas con el peso similar al que va a ser lanzado. Un edificio no funciona.
* Correr con el paracaídas atado, para ver que está equilibrado y no va de un lado a otro. Consejo: No hacerlo delante de los vecinos.
* Otra opción (más costosa al requerir helio): Usar un globo de helio, y atar a él una cuerda con un nudo al final (además de otras cuerdas para tenerlo controlado y que no se vaya). Usando otra cuerda, atar al final de ella una tope cilíndrico (como los utilizadas para unir varios tablones en los muebles del IKEA). Pondremos el cilindro de madera en el nudo de la cuerda que irá atada al globo, de manera que al estirar de la cuerda con el cilindro de madera se libere y caiga la cápsula...


Anillo de sujección
-------------------

El anillo de sujección es un anillo circular (de diámetro similar al ancho de la cápsula), para atar a él de forma centrada el paracaídas (y el globo), y la cápsula. De esta manera, todos los tirones se los lleva el anillo circular y no la cápsula: Tirones al abrirse el paracaídas en el descenso, y al ir tirando el globo en el ascenso.

* La anilla, que tendrá forma circular, puede ser un aro de los que se utilizan para coser y hacer bordados con dibujos (es decir, se vende en cualquier mercería o tienda de manualidades).
* La anilla deberá tener agujeros de forma simétrica y equidistante, que serán los agujeros en los que se atarán por un lado el paracaídas y el globo, y por otro la cápsula. Así servirá, a su vez, de sistema de estabilización.

Otras ideas para este anclaje de elementos a la cápsula y al anillo de sujección:

* Para atar el globo y el paracaídas a la cápsula (y si se quiere una segunda cápsula, una debajo de otra), se pueden coser / anclar arandelas (8 arandelas) en las 8 esquinas de la cápsula (que como comentamos, será una bolsa / nevera portátil), y atar allí todo lo demás. 
* Además se pueden hacer "latiguillos" con mosquetones de tamaño pequeño (cierres de "escalada"), que permiten anclar / soltar rápidamente los diferentes elementos, a la vez que quedan mejor sujetos que con un nudo simple realizado rápidamente. Estos mosquetones pueden comprarse en tiendas de deportes / escalada, o bien en **tiendas de cometas** (en donde también suelen tener paracaídas). Por ejemplo, [IntoTheWind - Tienda de cometas](http://www.intothewind.com/shop/Line_and_Accessories/Kite_Swivels) tienen una gran variedad.
* También necesitaremos **cuerdas para paracaídas** (_parachute cords_), que resistan los rayos UV. Se pueden buscar en ''Ebay'' o ''Amazon'', también hay en tiendas especializadas (los pongo en el siguiente apartado)


Enlaces de interés
------------------

Se pueden encontrar paracaídas y todo el material necesario en muchas tiendas de _hobbies_ (en las que buscando _rockets, parachutes, kites_... encontraremos todo lo necesario, desde paracaídas hasta el cordaje necesario). En algunos casos, nos puede servir incluso paracaídas de los que venden en tiendas de deportes o atletismo para realizar entrenamientos con lastre (pero vale más la pena buscar uno en tiendas de _hobbies_, que serán de más calidad o adaptados).

Recordad que tienen que ser de **nylon** para resistir los rayos UV...

Por ejemplo:

* [Apogee Rockets](http://www.apogeerockets.com/) - Tienda muy especializada en hobbies de lanzamiento y creación de cohetes (_rockets_). Tienen una gran variedad de paracaídas de nylon, así como otros complementos que pueden ser de utilidad.
* [Model Rockets Parachutes](http://www.modelrocketparachutes.com/) - Tienda especializada en hobbies de lanzamiento y creación de cohetes (_rockets_). Tienen una gran variedad de paracaídas de nylon, así como otros complementos que pueden ser de utilidad.
* [GreatHobbies.com](http://www.greathobbies.com/) - Tienda de hobbies (_rockets, radio control, planes_). Varios paracaídas de todo tipo (de pĺástico y nylon) desde unos pocos dólares.
* [HobbyLinc.com](http://www.hobbylinc.com/model-rocket-recovery) - Tienda de hobbies (_rockets_). Tienen una sección muy extensa de paracaídas para cohetes, que nos servirían (buscar _parachutes_, hay que asegurarse que sean de nylon)
* [Amain.com](http://www.amain.com/search?s=parachute) - Tienda de hobbies, con varios paracaídas de todo tipo (de pĺástico y nylon)
* [IntoTheWind.com ](http://www.intothewind.com/shop/Line_and_Accessories/Kite_Swivels) - Tienda de cometas, tienen anclajes, cuerdas...
* [Parachute-Cord.com]](http://www.parachute-cord.com) - Cuerdas de paracaídas profesionales


Índice del Proyecto SONDA
=========================

* [Proyecto SONDA - Introducción]({{ site.url }}/2014/12/18/proyecto-sonda-i)
* [La cápsula]({{ site.url }}/2014/12/25/proyecto-sonda-ii)
* [Sistemas de recuperación: Paracaídas]({{ site.url }}/2014/12/28/proyecto-sonda-iii)
