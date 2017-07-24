---
layout: post
title:  "Los métodos de Runge Kutta"
date:   2016-08-25 16:25:00
categories: knowledge space math
tags:
- knowledge
- space
- math
thumbnail: /assets/images/2016-08-25-runge-kutta.jpg
---

El post de hoy va sobre métodos de aprendizaje, y **lo fácil que resulta aprender cosas cuando se cuentan bien, con pasión, con ejemplos prácticos, y con chistes tontos que sirven de reglas mnemotécnicas**. Luego no te acordarás de lo que aprendiste, pero sí del chiste tonto (el cerebro humano es así de maravilloso, olvida lo importante para recordar estupideces). El recordar esas anécdotas o chistes tontos permitirán, si así lo deseas, tirar del hilo para recordar el tema principal e importante.

Y es que para enseñar lo que se sabe, es mucho mejor hacerlo con ejemplos que sean de interés del receptor. Esto me lo ha recordado el siguiente cómic:

![Einstein - La bomba atómica]({{site.url}}/assets/images/2016-08-25-runge-kutta.jpg)

La gente que está atendiendo una explicación, como ya hemos comentado, se centra más en lo que le llama la atención. Por lo tanto, si queremos transmitir un conocimiento, hay que intentar que lo que contemos no despiste al receptor con información que a él le resulte más atractiva.

Por ejemplo, en mi caso me di cuenta de lo mal que se enseñan las _materias aburridas_ en la Universidad, cuando en la asignatura de cálculo (o alguna similar), nuestra profesora nos explicó los **[Métodos de Runge Kutta](https://en.wikipedia.org/wiki/Runge%E2%80%93Kutta_methods)**. Estos métodos / algoritmos iterativos se utilizan en análisis numérico que permiten obtener de manera aproximada soluciones de ecuaciones diferenciales ordinarias.

Solo con leer este párrafo anterior ya te habrá dado pereza, así que hay que imaginar el hastío que nos dio para la gran mayoría de humanos que estábamos presentes en dicha clase.

Pues bien, años después compré un libro de [Wernher von Braun](https://es.wikipedia.org/wiki/Wernher_von_Braun): [The Mars Project](http://amzn.to/2bDjKsz) (podéis comprarlo en [Amazon](http://amzn.to/2bDjKsz) si os interesa saber más). 

En este libro, el ingeniero de la NASA que consiguió con sus cohetes llevar el hombre a la Luna, explica **en el año 1953 (16 años antes de llegar a nuestro satélite)** todo lo necesario para realizar un viaje a Marte, y cómo éste era posible con la tecnología actual de su época (sólo necesitaban tiempo y dinero, porque el objetivo principal de Wernher von Braun no era la Luna, sino Marte...).

Es un libro bastante interesante, ya que habla del número de naves que se requeriría para llegar a Marte (no hay que enviar una única nave, sino varias, al igual que Cristobal Colón utilizó la Niña, la Pinta y la Santa María...), el coste económico y material que supondría, los cálculos del viaje de ida/vuelta y la estancia (trayectorias y duración), etcétera... 

Para demostrar que todos estos datos que se ofrecen en el libro son válidos, Wernher von Braun incluye un gran número de cálculos matemáticos, teniendo en cuenta que la mayoría de ellos se realizan de forma manual con lápiz y papel (en el año 1953 no tenían ordenadores en cada casa...).

Mientras leía los diferentes apartados, cuando llegaba a estos cálculos (algunos de ellos de varias páginas) los miraba un poco por encima y directamente saltaba a las conclusiones. Sin embargo, cuando llegué al apartado de cálculo de trayectorias para poner en órbita los diferentes módulos espaciales (que se deberían ensamblar en la órbita terrestre para reducir costes, al igual que se está haciendo con la estación espacial internacional ISS), un interruptor se encendió en mi cerebro. 

Hay que tener en cuenta que los cohetes que ponen cargas en órbita o enviaron al hombre a la Luna no dejan de ser grandes misiles cargados de combustible con una pequeña carga en su punta... y que **para calcular la trayectoria de estos misiles de largo alcance se utilizan los métodos de Runge Kutta**... Sí, los métodos de Runge Kutta... Los mismos métodos de Runge Kutta que una profesora nos comentó con desgana, desmotivando a todos los presentes, y que hicieran que mucha gente se planteara cambiarse de carrera...

**Si esa profesora nos hubiera contado con pasión que con los métodos de Runge Kutta podríamos calcular la trayectoria de misiles intercontinentales o llevar al hombre a Marte, otro gallo habría cantado...**

