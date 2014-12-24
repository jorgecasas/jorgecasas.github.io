---
layout: post
title:  "Proyecto SONDA (II)"
date:   2014-12-24 19:35:00
categories: projects sonda
tags:
- projects
- sonda
---

Hoy voy a seguir dando detalles del [Proyecto SONDA]({{ site.url }}/2014/12/18/proyecto-sonda-i) dentro de la serie de posts de [mis proyectos inacabados]({{ site.url }}/2014/11/26/proyectos-inacabados). En este segundo post voy a comentar algunos detalles teóricos con ideas para ir creando la sonda (desde su estructura hasta los componentes que llevará dentro). Hoy toca **la cápsula**...

Construcción de la sonda: La cápsula
====================================

La cápsula es la estructura en la que introduciremos todos los componentes electrónicos necesarios para poder realizar el seguimiento de la sonda (GPS y radio de comunicaciones), así como las baterías que dichos dispositivos necesiten para el vuelo (que será de más de 2 horas). También servirá para anclar en ella la cámara (para tomar fotografías) así como los anclajes necesarios para el paracaídas (requerido en la vuelta a la Tierra) y el globo de helio.

Como la sonda va a subir bastante alto (unos 30 Km), la cápsula debe ser el aislante necesario para proteger los dispositivos electrónicos. Estos dispositivos tienen un rango de temperaturas de operación (es decir, por encima o debajo de dicho rango de temperaturas o no funcionarán o es fácil que deje de hacerlo de forma correcta). Por ello, el aislante de la cápsula debe soportar temperaturas de hasta -60 ºC. El siguiente gráfico muestra la temperatura aproximada según la altitud

![Temperatura según altitud]({{ site.url }}/assets/images/2014-12-25-proyecto-sonda-ii-02.jpg)

Por ello, la cápsula debe tener ciertas características:

* Lo suficientemente ligera como para que no se necesite un gran globo de helio. En la subida, cuanto menos peso, más eficiencia y más rápido será capaz de subir con menor coste de helio ([el cual es finito en la Tierra y no es barato](http://www.bbc.co.uk/mundo/noticias/2013/11/131120_helio_escaso_finde))
* Debe tener un espacio interior suficientemente grande como para que quepan los dispositivos electrónicos y baterías, así como la cámara de fotos...
* Debe ser lo suficientemente aislante como para que los dispositivos electrónicos estén en el rango de temperatura adecuado.

La **solución más barata**, que como ya comenté era uno de los objetivos del proyecto, es... **una nevera portatil**:

![La cápsula espacial... ideal para refrescar las latas de bebida]({{ site.url }}/assets/images/2014-12-25-proyecto-sonda-ii.jpg)

Cumple con todos los requisitos:

* No pesa mucho
* Hay de diferentes tamaños. El de la foto que tiene capacidad de 6 latas es un tamaño más que adecuado.
* Es aislante ya por sí misma. 
* Es barata (comparada con otras opciones)...

Además, a la hora de realizar la cápsula se puede tener en cuenta otros detalles:

* Se puede completar o distribuir su interior utilizando tabletas de corchopán o incluso las bolas de corchopán que se utilizan para el envío por correo de objetos frágiles... que además de separar, es aislante (barato)
* Habrá que hacer los agujeros mínimos que necesiten (por ejemplo, en un lado para la cámara, en otro para sacar la antena GPS / GPRS / radio...), y sellarlas lo más posible (con aislante, cinta americana, etcétera)
* Se pueden añadir otros materiales de los que se utilizan para el envío de componente electrónicos / ordenadores... que es blando, de poco peso, y permitirá añadir un extra de protección ante impactos (en el aterrizaje)
* En la parte inferior de la cápsula habrá que poner protección adicional (de un par de dedos de material blando), para absorber el impacto.
* Se pueden utilizar anclajes (en inglés, _booms_) a los que anclar algunos experimentos en el exterior (lateral) de la cápsula (por ejemplo, un _trípode_ o _palo de selfies_ para la cámara de fotos, o sensores de temperatura, etcétera...). Igualmente, en estos anclajes / barras se pueden añadir sistemas de servos (controlados por el dispositivo procesador / de control del interior de la cápsula), por ejemplo para rotar la cámara y sacar fotos a la propia cápsula. En los documentos comentados en [el primer post del Proyecto SONDA]({{ site.url }}/2014/12/18/proyecto-sonda-i) hay fotografías de este tipo de anclajes, pero es necesario que sean lo menos pesados posible.
* Igualmente, habrá que crear anclajes para atar el globo (para la ascensión) y el sistema de paracaídas (para el descenso), así como la posibilidad de anclar una segunda sonda en la parte inferior de la primera (para transportar más experimentos).
* Se puede usar velcro para crear diferentes aperturas en la cápsula (pensando en su reutilización para futuros experimentos)

En el interior de la cápsula también se pueden tener en cuenta varios detalles, como por ejemplo los anclajes e inmobilizaciones de los componentes de la cápsula. Además de usar cajas aislantes individuales para cada componente (especialmente baterías), se deben anclar a la cápsula (usando velcro, gomas, anillas plásticas, etcétera). Hay que anclar bien los dispositivos, ya que van a llevar bastante meneo, sobre todo en el descenso. Con corchopán o el material de protección de componentes electrónicos que hemos comentado se pueden hacer recubrimientos en condiciones. Asimismo:

* Se puede usar velcro para anclar la estructura interna a la "bolsa" aislante, e incluso para "cerrar la caja" con una tapa aislante...
* También se puede introducir bolsitas de absorción de humedad para proteger los componentes electrónicos
* Si sobra espacio, el resto de espacio de la cápsula puede ser llenado con bolas de _styrofoam_ (corchopán), una vez esté todo anclado.
* Conviene etiquetar todo correctamente para comprobar rápidamente que está todo puesto en su sitio, y que estamos usando los módulos requeridos (por ejemplo, las baterías recien compradas / cargadas...).
* Los cables entre módulos deben ir bien sujetos y protegidos, usando bridas para tenerlos ordenados y que no molesten en la manipulación de los dispositivos electrónicos.
* En el exterior de la cápsula conviene poner bolsillos transparentes (como los de meter tu tarjeta en la maleta por si se pierde en el aeropuerto) cosidos a la bolsa exterior, y en ella meter (para que sea visible) una tarjeta con el nombre de la sonda, e instrucciones por si la encuentra alguien a la caída.

En los siguientes capítulos de esta serie de **Proyecto SONDA** iré recopilando más información sobre cómo pretendemos crear la cápsula paso a paso, así como diferentes decisiones tomadas, etcétera... **Se admiten sugerencias!**