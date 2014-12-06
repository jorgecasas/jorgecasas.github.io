---
layout: post
title:  "Pajaritos"
date:   2014-12-06 00:35:00
categories: projects games retro
tags:
- projects
- games
- retro
---

| Pajaritos es [uno de mis proyectos inacabados]({{ site.url }}/2014/11/26/proyectos-inacabados) que podrían ser considerados _semiacabados_. Era el año 2001, en mitad de mi vida universitaria, y apenas habíamos programado nada en toda la carrera... Así que un verano decidí coger un libro de programación orientada a objetos (Java!) porque entonces apenas había recursos para programar en Internet y había que leer cosas en papel. Eran otros tiempos en los que en la Universidad se estudiaba Pascal como lenguaje de programación... | ![La vaca del juego]({{ site.url }}/assets/images/2014-12-06-pajaritos.gif) |

Y como **para aprender hay que hacer**, decidí realizar un proyecto con todas las fases... de manera que desde el primer día fui aplicando los conocimientos rudimentarios que iba teniendo en [OOP](http://es.wikipedia.org/wiki/Programaci%C3%B3n_orientada_a_objetos). El proyecto que decidí realizar era un juego que por aquellos entonces habíamos encontrado en [disquetes](http://es.wikipedia.org/wiki/Disquete) de los que regalaban con las revistas (o algún [CD](http://es.wikipedia.org/wiki/Disco_compacto), que ya existían...): **[Moorhuhn](http://www.moorhuhn.de)**.

_Moorhuhn_ es un juego alemán bastante conocido por el norte de Europa (hay hasta pósters con el pajarito protagonista del mismo, peluches y toda la mercadotecnia posible), y que puede considerarse el juego _[shoot'em up](http://es.wikipedia.org/wiki/Shoot_%27em_up)_ más famoso de Alemania de principios de milenio... El juego consiste en disparar a unos pobres pajarillos que salen volando por todas partes. 

Mi proyecto del _Juego de los Pajaritos_ me serviría como excusa para desarrollar un proyecto en Java tocando muchos temas: Desde la planificación y análisis del proyecto, pasando por el diseño gráfico y audiovisual, diseños de interfaz de usuario (rudimentarios), programación de una aplicación Modelo-Vista-Controlador (MVC) en un lenguaje que no conocía... y todo en el plazo de 2 meses en verano...

El resultado son unos GIFs animados de pájaros que vuelan a un lado y otro de la pantalla siguiendo líneas rectas y diferentes parábolas, o que se esconden y aparecen (como las vacas que les acompañan). Se puede disparar a todo lo que se mueve por la pantalla (gracias a la captura de eventos de _click_ del botón izquierdo y derecho el ratón) durante un minuto y medio, apareciendo en el _hall of fame_ aquellos jugadores que obtuvieran una mayor puntuación.

Los gráficos eran como eran en el año 2000:

* Fondos en 2D para los fondos (sin efectos de [paralaje](http://es.wikipedia.org/wiki/Paralaje), pero con unas montañas que parecía que estaban más cercanas que las del fondo), realizados con Paint Shop Pro con el ratón y poco más... 
* Personajes en 3D desarrollados con [POVRAY](http://www.povray.org/), entre los que se encuentran los GIF animados de los pájaros que vuelan sin control, los pollos asados en los que se convertirán al ser disparados, y la vaca.

El audio del videojuego era lo más llamativo:

* Sonido de fondo: Obtenido de un banco de sonidos que daban en una revista en aquellos entonces... Ni siquiera descargado con el Emule, Kazaa o similares... 
* Efectos de sonido: Obra maestra. Requería sonidos de una vaca mugiendo (yo mismo imitando) y los pájaros recibiendo un disparo (un muñeco de goma de los de los niños pequeños)

La base de datos para almacenar resultados, jugadores, configuraciones de los torneos... ¿Base de datos? Las bases de datos convencionales (MySQL, etcétera) son para los débiles, los hombres se construyen sus propias bases de datos _byte a byte_... Como en un principio no pensaba guardar nada (y tampoco tuve mucho tiempo), ni siquiera me planteé utilizar una base de datos (requeriría configuración, instalación, etcétera). Los datos se almacenaban en un fichero de texto, se parseaba y se volvía a escribir en disco...


Dejo aquí un vídeo demostrativo del juego (sin sonido!)... que aún funciona si lo instalas en la carpeta de usuario y tienes Java instalado... Es un juego multiplataforma...

<iframe width="560" height="315" src="//www.youtube.com/embed/PZDbcFIEFs8" frameborder="0" allowfullscreen></iframe>



Proyecto semiacabado
--------------------

Como ya he comentado antes, **Pajaritos es un proyecto semiacabado**. Además del juego, tenía hasta su propia web (que tenía alojada en **[Geocities de Yahoo!, en el barrio de Colosseum](http://es.wikipedia.org/wiki/GeoCities)**), y hasta [banda sonora original](http://grooveshark.com/s/Unidad+Sonora/3L6n3P?src=5)... pero le faltaban algunas ideas que no llegué a desarrollar por falta de tiempo... algunas de ellas dan risa cuando miras al pasado, pero entonces igual eran hasta innovadoras:

* Añadir la posibilidad de disputar ligas y torneos entre jugadores, y que se pueda grabar / salvar partidas... 
* Enviar emails y SMS con puntuaciones, y para decir que se unan a la partida
* Juego en red (en modo servidor - cliente)
* Ligas (por asaltos y de X rondas)
* Varios escenarios para 1 jugador (desafios)
* Vídeo presentación, con sonidos como espadas láser y audio...
* Comentaristas
* Que salgan números con el valor del pájaro que ha sido abatido
* Mejorar lo del pájaro asado, que dé vueltas
* Programa de instalación
* Manual (en PDF)
* Trucos y huevos de Pascua
* Chat durante el juego en red
* Mensajes aleatorios
* Poder imprimir los récords

**¡Qué tiempos!**

Si queréis ver / jugar / compartir... he subido las fuentes a [GitHub](https://github.com/jorgecasas/pajaritos)