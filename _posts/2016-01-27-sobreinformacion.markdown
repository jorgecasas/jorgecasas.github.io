---
layout: post
title:  "Sobreinformación"
date:   2016-01-27 15:15:00
categories: development
tags:
- development
---

Hoy en día se utilizan en exceso las palabras información, datos y _Big-Data_: Es el nuevo oro que todo el mundo quiere. Cuantos más datos y más información se tengan en bases de datos gigantescas más valor tendremos. Aunque como suelo comentar a menudo, por mucho que se te llene la boca de palabras rimbombantes (por ejemplo, _Big-Data_), lo que importa es qué haces con esos datos y si lo haces bien.

<blockquote class="twitter-tweet" lang="es"><p lang="es" dir="ltr">Big Data es como el sexo adolescente: todos hablan de él, nadie sabe realmente cómo hacerlo y todos piensan que los demás lo hacen...</p>&mdash; Javier Padilla (@elpady) <a href="https://twitter.com/elpady/status/393695767657201664">octubre 25, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

En ese sentido, si eres capaz de ofrecer un valor añadido a esa información (y alguien está dispuesto a pagar por obtener dicho valor), el producto será un éxito. Ese valor añadido que le va a dar el procesar toda la información de una forma u otra (mezclando, correlando e interconectando datos, obteniendo tendencias, etcétera) puede no verse reflejado si no somos capaces de mostrarlo al usuario de forma simple, correcta, intuitiva y significativa. En definitiva, que si tenemos datos y obtenemos información procesada valiosa para el usuario pero no somos capaces de mostrársela de forma correcta, al usuario no le va a servir de nada.

La experiencia de usuario al consumir esta información puede verse deteriorada por varios aspectos. Por ejemplo: 

* Utilizar diferentes técnicas de visualización de datos (texto básico, datos tabulados, gráficas...) que no sean adecuadas para la naturaleza de los datos a visualizar. Por ejemplo, para el usuario suele ser mucho más útil si mostramos geolocalizaciones utilizando mapas interactivos en vez de mostrar una tabla con los campos numéricos de latitud y longitud.
* Utilizar interfaces de usuario que dificulten la legibilidad y entendimiento de los datos. Por ejemplo, no separar correctamente los datos a visualizar, o mostrarlos en un órden que puedan confundir al usuario. La posición de los datos visualizados con respecto al resto de datos debe seguir una ordenación lógica para el usuario, ya que mentalmente va a interrelacionar el significado de los mismos (es, por decirlo así, el contexto de un dato con respecto a la información global que se está mostrando).
* Mostrar menos información de la necesaria. Por ejemplo, si tenemos una aplicación de control de tareas a realizar que tengan una fecha límite tendremos que mostrar siempre esa fecha límite. Sería estúpido no mostrarla.
* Mostrar más información de la necesaria. 

Este último punto es sobre el que trata este post: La **sobreinformación**. En algunos casos, mostrar más información de la necesaria puede ser perjudicial para el usuario e incluso para el desarrollo y mantenimiento de la aplicación.

En algunas fases del flujo de acciones que se realizan en las aplicaciones que estemos utilizando no va a ser indispensable mostrar ciertos datos, e incluso puede ser muy beneficioso no mostrarlos. Vamos a poner como ejemplo una aplicación en la que el usuario pueda registrar una tarea. Según los datos que introduzca en el formulario de registro (por ejemplo, tipo de tarea, lugar donde debe realizarse, etcétera), se calculará automáticamente una fecha y hora límite para finalizar dicha tarea. En este ejemplo:

* En la página de registro de la tarea no conviene mostrar el campo de "Fecha límite para finalizar la tarea", a pesar de que la podamos calcular automáticamente al vuelo a partir de los datos introducidos en el resto de campos de formulario. 
* Una vez registrada la tarea en el sistema, podremos acceder a la página de visualización de datos de dicha tarea. En este apartado sí que incluiremos la fecha límite, ya que el usuario debe conocerla.

¿Por qué no hemos calculado y mostrado al vuelo la fecha límite de finalización en el formulario de registro? Por una sencilla razón: **El usuario no necesita conocer dicho dato hasta que no haya registrado la tarea**. Si conoce dicha fecha de finalización mientras registra la tarea, podría ir modificando el resto de campos de formulario para que le ofrezca, por ejemplo, una fecha de finalización menos estricta.

En este caso, el usuario estaría falseando la información a su favor, haciendo que la aplicación de control no sirva realmente para controlar (ya que el usuario podría adaptar los plazos a su conveniencia, en lugar de realizar su trabajo como corresponde). Y en ciertos entornos corporativos esto es un problema.

Por tanto: Hay que pensar siempre qué información se debe mostrar en cada apartado de la aplicación para dar al usuario todos los datos que necesite y le sean de utilidad en cada momento. **Ni más, ni menos**

