---
layout: post
title:  "Construyendo con Arduino"
date:   2017-09-23 23:45:00
categories: blog development projects
tags:
- blog
- development
- projects
thumbnail: /assets/images/2017-09-23-arduino.gif
---

Además de estar realizando poco a poco el proyecto [Coche RC autónomo]({{ site.url }}/2017/08/23/autonomous-rc-car-i), junto con [Bruno](https://twitter.com/brunocasasabos) estamos aprendiendo a realizar pequeños proyectos con Arduino: Aprendemos a programar y un poco de electrónica, además de aprender cómo funcionan las cosas. Por ejemplo, hoy hemos hecho un [semáforo](https://github.com/jorgecasas/arduino-projects/tree/master/street_light)

![Arduino](/assets/images/2017-09-23-arduino.gif)

En Internet hay miles de recursos y tutoriales (además de los mil ejemplos que vienen incluidos en el IDE de programación de Arduino), como pueden ser los blogs y recursos de [Programar Fácil](https://programarfacil.com/), [Prometec](https://www.prometec.net/indice-tutoriales) o [Fábrica Digital](https://fabricadigital.org/courses)

Los proyectos que vayamos realizando los iremos compartiendo en **[Github - jorgecasas/arduino-projects](https://github.com/jorgecasas/arduino-projects)**. En cada carpeta se indica en el fichero `README.md` los pasos, materiales necesarios... junto con el código para ser cargado y modificado.

## Primeros pasos con Arduino

Si queremos aprender, además de un Arduino habrá que tener cables, resistencias, algún sensor, diodos led... Hay [kits de iniciación Arduino](http://amzn.to/2ykLOr8) bastante completos (aun siendo clones del Arduino original, de menor calidad... pero para empezar sobra)

Podemos descargar el IDE de Arduino (o bien directamente desde el [IDE web](https://www.arduino.cc/en/main/software)) e instalarlo, o bien desde repositorio. En Ubutnu / Debian:

```bash
sudo apt-get install arduino
```

Una vez instalado, podemos seleccionar de los menús la placa Arduino que vayamos a utilizar (por ejemplo, Arduino UNO) y cargar el programa de ejemplo _Blink_ (que es tan simple que sólo enciende y apaga el led integrado de la placa, pero que permite comprobar que la comunicación entre el ordenador y la placa es correcta mediante el cable USB). Una vez cargado el programa, pulsando el icono de _Cargar / Load_ del IDE podremos transferir el programa al Arduino, y el led comenzará a parpadear.
 
Si tenemos niños pequeños o estamos aprendiendo a programar, podemos realizar la programación mediante bloques (similar a Scratch). Descargamos [S4A (Scratch For Arduino)](http://s4a.cat/) del apartado de descargas. En Ubuntu / Debian es un paquete `.deb`, por lo que tras su descarga habrá que instalarlo mediante el comando:

```bash
sudo dpkg -i S4A16.deb
```

En el caso de que dé errores (falta alguna librería), podremos ejecutar el siguiente comando para que se instalen de forma automatizada:

```bash
sudo apt-get install -f
```

También hay que instalar un **firmware** adicional para comunicar el Arduino con el IDE S4A. En el apartado de [descargas](http://s4a.cat/) nos descargamos dicho _firmware_, que no deja de ser un fichero `.ino` (similar a un proyecto). Lo abriremos con el IDE de Arduino y lo cargaremos en la placa, y una vez cargado podremos abrir ya el IDE S4A para programar mediante bloques.

Y ahora sólo queda la imaginación (y un poco de tiempo)...
