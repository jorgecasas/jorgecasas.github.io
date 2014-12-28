---
layout: post
title:  "Proyecto SONDA (I) - Introducción"
date:   2014-12-18 19:25:00
categories: projects sonda
tags:
- projects
- sonda
---

Hoy voy a dar las primeras pinceladas a [uno de mis proyectos inacabados favoritos]({{ site.url }}/2014/11/26/proyectos-inacabados), que a modo de ''serial de posts'' voy a ir detallando. Se trata del **Proyecto SONDA**, o lo que es lo mismo, conseguir mandar un dispositivo al espacio cercano (unos 30 Km de altitud) mediante una sonda (globo de helio), que tome un par de fotos de la estratosfera y que vaya enviando su posición para poder recuperarlo.

A continuación voy a ir detallando en qué consistirá el proyecto, así como un buen número de enlaces a recursos de interés que he ido recopilando a lo largo de los años. Hay que decir que uno de los requisitos que me he autoimpuesto en mis proyectos es que **deben ser lo menos costoso posible** (en término económicos). Esto requerirá utilizar materiales caseros / reciclados (desde una nevera vieja hasta corchopán)...


Idea del proyecto
=================

La idea es:

* Crear una cápsula casera en la que irá una cámara de fotos con un disparador automático (con temporizador) para sacar fotos cada X segundos, un GPS y un módulo de radio para que vaya enviando datos a tierra (también hay que crear la antena de tierra, que es direccional). Además, hay que poner aislante, protecciones y mil historias más a la cápsula, ya que en el espacio hace frío, entre otras cosas.
* Usar un globo de helio (como los usados en las sondas meteorológicas), para lanzar la cápsula al espacio cercano. La idea es que puede llegar a superar los 30 Km de altitud.
* Una vez explote el globo (que explota entorno a esa altitud, en función del helio y calidad del globo utilizado), se abre un paracaídas.
* Desde tierra hay que ir siguiendo la cápsula (mediante radio o bien 3G/GPRS y los datos que la cápsula vaya transmitiendo), y luego con GPS y demás encontrar dónde ha aterrizado y recuperarla. Si no encontramos la cápsula, no vemos qué fotos han salido. El resultado será algo como la siguiente foto... fijarse que el cielo ya es negro porque está ya en el espacio y no hay atmósfera:

![La estratosfera]({{ site.url }}/assets/images/2014-12-18-proyecto-sonda-i.jpg)

Otros pioneros
==============

Esto lo han hecho en un montón de sitios, pongo unos cuantos enlaces interesantes:

* **[High Altitude Science](http://highaltitudescience.com)** - Web didáctica y simple sobre lanzamientos de este tipo, tienen incluso tienda (cara) pero que puede servir para saber qué hay que comprar o algunas cosa más detallada (sobre todo, los protocolos a la hora de utilizar el helio para el inflado / atado del globo)

* **[Meteotek08](http://teslabs.com/meteotek08)**: Estos son españoles, de un instituto de Barcelona (varios estudiantes y su profesor de tecnología), con un proyecto realizado en el 2008: 
  * [Blog Meteotek08](http://teslabs.com/meteotek08)
  * [Memoria (obtenida del blog) en PDF](http://www.teslabs.com/meteotek08/fitxers/docs/meteotek08_castella.pdf)
  * [Galería de fotos del proyecto](http://www.flickr.com/photos/meteotek08)


* **[EOSS.org](http://www.eoss.org)**: Estos son unos americanos que son como la NASA en pequeño. Llevan desde 1990 enviando sondas, y ya han superado más de 150 misiones. Tienen bastante infraestructura, incluso hacen mini-experimentos en el espacio (como usar un medidor geiser (o algo así) para medir el número de rayos cósmicos captados en función de la altitud, etc). En el enlace ''Flight Recaps'' tenéis un listado con todas las misiones, fotos y vídeos, la telemetría, las simulaciones previas que hicieron... 

* **[Near Space](http://www.nearsys.com/pubs/book/)**: Libro sobre la exploración del espacio cercano. Pueden descargarse los diferentes capítulos en PDF

En los siguientes capítulos de esta serie de **Proyecto SONDA** iré recopilando más información sobre cómo pretendemos crear la cápsula paso a paso, así como diferentes decisiones tomadas, etcétera... **Se admiten sugerencias!**