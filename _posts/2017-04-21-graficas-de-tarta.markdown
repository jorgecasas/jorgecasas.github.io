---
layout: post
title:  "Gráficas de tarta"
date:   2017-04-20 14:15:00
categories: blog development
tags:
- blog
- development
thumbnail: /assets/images/2017-04-21-charts-02.jpg
---


En los sistemas que desarrollamos en [ITERNOVA](https://www.iternova.net) tenemos varias cosas claras: **Nunca hay que poner música de fondo en un sistema web y nunca hay que usar una gráfica de tipo tarta**.

La primera de las cosas tiene una razón muy clara de ser: Ya no estamos en los años 90. La música suele ser molesta, generalmente no es la que el usuario quiere escuchar o le gusta, hace que las diferentes pantallas tarden más en cargar, se suele superponer a la que el usuario pueda estar escuchando, y generalmente no se sabe de dónde está sonando. Si la página está en varias pestañas del navegador, es muy difícil que no se superpongan entre sí.

Con respecto a las gráficas de tarta, hay que evitarlas. Alguna vez no queda más remedio porque el cliente así te lo exige (y no ha leído este post). Solo hay una razón para evitarlas, y es que no permiten comparar visualmente la información y datos contenidos en este tipo de gráficas, por lo que no son útiles. Con una gráfica de barras o de líneas se consigue poder comparar al instante si un valor es mayor o menor que otro... con las gráficas de tarta no lo puedes saber con exactitud.

Sin embargo, hay varias excepciones para las cuales es el mejor tipo de gráfica, y son las siguientes:

![Gráfico de tartas]({{site.url}}/assets/images/2017-04-21-charts-01.jpg)
![Gráfico de tartas]({{site.url}}/assets/images/2017-04-21-charts-02.jpg)
![Gráfico de tartas]({{site.url}}/assets/images/2017-04-21-charts-03.png)
![Gráfico de tartas]({{site.url}}/assets/images/2017-04-21-charts-04.jpg)
![Gráfico de tartas]({{site.url}}/assets/images/2017-04-21-charts-05.png)


Últimamente se está poniendo de moda hacer infografías y sacar datos en todo tipo de gráficas, por lo que hay que tener en cuenta otro parámetro a la hora de diseñar las gráficas: **Los colores de las leyendas**. Los colores utilizados en las series de datos deben cumplir algunos requisitos que vamos a enumerar a continuación.

**No utilizar colores similares para series de datos conceptualmente diferentes**. Por ejemplo, si estamos generando una gráfica con dos series (número de peras y número de manzanas), deberemos escoger dos colores opuestos visualmente. Es decir, no conviene usar un rojo anaranjado para las peras y un naranja rojizo para las manzanas, mejor utilizar un verde para las peras y un rojo para las manzanas. Por tanto, habrá que escoger una paleta lo más amplia posible de colores lo más diferentes posibles, utilizando colores parecidos solo cuando tengamos tantas series que no nos quede más remedio que repetir algún color, o bien cuando queramos crear un mapa de calor o gradientes.

**Mapas de calor, o gradientes de valor**: Estos tipos de gráficas o infografías suelen ser utilizadas para medir un porcentaje de uso, actividad o carga en diferentes elementos. En estos casos es importante elegir correctamente la escala de color, ya que estamos programados para asociar colores a situaciones en concreto. Por ejemplo:

* Verde: Todo marcha bien
* Naranja: Algo va regular
* Rojo: Algo falla
* Negro: No funciona nada

Si utilizamos una escala diferente, a primera vista el usuario puede llegar a pensar que todo va bien, cuando en realidad no es así... Aunque quizá sea lo que se está buscando (que el usuario piense que todo funciona correctamente aunque haya cosas que no sean tan bonitas como las pintan). Por ejemplo, una escala del siguiente tipo:

* Azul: Todo marcha bien
* Morado: Algo va regular
* Verde: Algo falla
* Naranja: No funciona nada
* Rojo: No funciona ni el panel de control...

Este caso sería el equivalente a mostrar gráficas en el que el eje Y no estuviera en el 0, sino en un valor arbitrario...

**Colores válidos para daltónicos**: En algunos sistemas, es probable que además de utilizar colores se deban utilizar símbolos, ya que algunos colores de la paleta pueden ser confundidos por usuarios con daltonismo. Si es posible, habría que escoger colores que no se confundieran entre sí. Existen [webs y aplicaciones para simular daltonismo en interfaces de usuario, imágenes](http://www.color-blindness.com/coblis-color-blindness-simulator), etcétera...

Y por último, lo más importante: **Que los datos sirvan para mostrar algo**.