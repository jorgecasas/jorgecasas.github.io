---
layout: post
title:  "Andrés Calamardo"
date:   2015-10-22 18:35:00
categories: music linux
tags:
- music
- linux
---

A [Bruno](https://twitter.com/brunocasasabos) le gusta mucho la música. Su cantante favorito es Andrés Calamaro, o como él lo llama, _Andrés Calamardo_. 

<blockquote class="twitter-tweet" lang="en"><p lang="es" dir="ltr">- A mí me gusta mucho la música de Andrés Calamardo <a href="https://t.co/okZMHlfreo">https://t.co/okZMHlfreo</a> <a href="https://twitter.com/hashtag/bobesponja?src=hash">#bobesponja</a>!</p>&mdash; Bruno Casas Abós (@brunocasasabos) <a href="https://twitter.com/brunocasasabos/status/656448685660835841">October 20, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Por eso, y porque le gusta todo lo que tenga que ver con la música, especialmente con la música en directo, fuimos a un concierto. Se le puede ver en la siguiente foto sin parar de aplaudir y saltar.
![Bruno en el concierto de Deparamo]({{ site.url }}/assets/images/2015-10-22-andrescalamardo-01.jpg)

Nada más volver del concierto ya quería que subiéramos un viejo piano MIDI de la [época en la que me dio por aprender a usar sintetizadores y secuenciadores siendo una estrella de la música electrónica](https://play.spotify.com/track/2zEs4q3fAFTHDnxXe9R3ah), y me apunté a clases de _Audio digital_ en la Universidad.

Como desde hace una decena de años ya no uso Windows, no funcionaba ninguno de los secuenciadores de aquella época. Estuve buscando información para revivir el teclado **MK-249C MIDI Keyboard**. Encontré el siguiente enlace sobre [MIDI en Linux](http://tedfelix.com/linux/linux-midi.html) que es bastante completo. En resumen lo que hice es:

* Instalar servicio jackd y su gestor del servicio con interfaz gráfico qjackctl
* Instalar dos sintetizadores (para probar, con uno funcionando debería bastar): fluidsynth y qsynth
* Instalar patchage para realizar las conexiones entre la salida del teclado y la entrada del sintetizador, y la salida del sintetizador a la entrada de audio del sistema. Si tenemos qjackctl lo podemos realizar aquí también, pero patchage es muy visual.
* Instalar otras herramientas como aseqdump
* Conectar el cable USB entre el teclado y el ordenador.

Tras iniciar el servidor jackd (mediante qjackctl), el sintetizador (fluidsynth o qsynth, o ambos... todo sea por la música), se realizan las conexiones en patchage (o qjackctl) conectando entradas y salidas como corresponda:

![Conexiones en patchage]({{ site.url }}/assets/images/2015-10-22-andrescalamardo-03.png)

Y hecho esto, subimos el volumen, le ponemos a Bruno los auriculares y ya puede estar horas tocando sus canciones en el piano. Algún día sonarán bien (y es que la práctica es el camino a la perfección).

![Bruno en su estudio de grabación casero]({{ site.url }}/assets/images/2015-10-22-andrescalamardo-02.jpg)
