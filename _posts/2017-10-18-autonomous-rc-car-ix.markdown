---
layout: post
title:  "Coche RC autónomo (IX) - Detectando señales de STOP con clasificadores y OpenCV"
date:   2017-10-18 12:45:00
categories: blog development projects
tags:
- blog
- development
- projects
thumbnail: /assets/images/assets/images/2017-10-18-camera-stop-detection.gif
---

Al llegar este post llevamos ya varios puntos del [proyecto Coche RC autónomo - Construyendo un vehículo autónomo]({{ site.url }}/2017/08/22/autonomous-rc-car-construyendo-un-coche-autonomo) completados. Ya sabemos cómo configurar la [Raspberry Pi](http://amzn.to/2ghy6xY) con todas las librerías necesarias para la visión por computador usando OpenCV, sabemos utilizar un sensor (el de ultrasonidos) para detectar si hay un obstáculo delante nuestro y a qué distancia, y sabemos enviar las imágenes y estos datos desde la Raspberry Pi a nuestro ordenador.

Los siguientes pasos son:

* Aprender a procesar las imágenes para detectar señales de STOP y semáforos
* Aprender a calibrar las imágenes recibidas para saber a qué distancia aproximada se encuentran esas señales de STOP y los semáforos
* Aprender a crear una red neuronal que procese las imágenes para saber cuando tendremos que acelerar, frenar, girar a la izquierda o a la derecha
* Aprender a controlar servos con la Raspberry Pi para que el vehículo acelere, frene, gire a izquierda o derecha cuando corresponda.

En este post vamos a aprender cómo detectar señales de STOP. 


![Señales de STOP detectadas](/assets/images/2017-10-18-camera-stop-detection.gif)

Para ello, vamos a utilizar clasificadores en cascada que ofrece la propia librería OpenCV. Eso sí, necesitamos ficheros descriptores de los elementos que tenemos que detectar para que los clasificadores puedan detectar si en las imágenes aparecen o no esos objetos y la posición en la que aparecen. En nuestro caso, señales de STOP y semáforos, pero necesitaríamos descriptores para cada tipo de objeto adicional (cualquier otro tipo de señal, balizamiento, peatones, bicicletas, maceteros y bolardos...). 

Estos ficheros descriptores los podemos crear nosotros mismos, utilizando un montón de imágenes positivas (donde aparezca el objeto a detectar en multitud de posiciones, condiciones ambientales, etcétera) y un montón de imágenes negativas (donde no aparezca el objeto, pudiendo aparecer otros objetos similares diferentes). En muchos blogs indican que se necesitan como poco unas 60 imágenes positivas para la señal de STOP, y unas 600 para las imágenes negativas, pero cuantas más tengamos de cada una de ellas mejor.

## Obteniendo imágenes positivas y negativas

Las imágenes positivas (las de la señal de STOP que queremos detectar) tienen que ser recortadas para que aparezca únicamente el objeto. Por ejemplo:

![Señales de STOP para entrenar al clasificador en cascada](/assets/images/2017-10-18-cascade-classifier-positive-images.jpg)

Las imágenes negativas pueden ser de cualquier cosa, pero si vamos a crear un sistema de conducción lo suyo sería usar fotografías de situaciones comunes que nos vayan a surgir: Fotografías de carreteras, con otras señales diferentes, etcétera. En este punto, como es muy aburrido conseguir un banco de fotos de unas 1000 fotografías, podemos exportar de una película MP4 todos los fotogramas. Por ejemplo, si tenemos `ffmpeg` instalado podemos ejecutar el siguiente comando para obtener de un vídeo _INPUT.mp4_ grabado con el móvil 5 imágenes cada segundo, con nombre _image-000001.jpg_ (incrementándose el número de imagen de forma consecutiva). Así, si el vídeo dura 100 segundos, conseguiremos 500 imágenes en un momento:

```bash
ffmpeg -i INPUT.mp4 -f image2 -vf fps=5 image-%06d.jpg
```

Como veremos, hay más métodos para obtener imágenes, ya que OpenCV permite utilizar una imagen base para rotarla, distorsionarla, proyectarla... de forma automática sobre las imágenes negativas que tengamos. Ello permite crear en lote miles de imágenes en un momento (a más imágenes, el momento será más largo...).

Todas las imágenes tienen que ser `jpg`, por lo que si tenemos alguna en formato `png` tendremos que convertirlas. Para ello podemos usar en Linux la herramienta `mogrify`, que permite convertir todas en lote mediante el comando:

```bash
mogrify -format jpg *.png
```

Además, habrá que convertirlas todas al mismo tamaño, cosa que podemos hacer también en lote:

```bash
mogrify -path . -resize 256x256 *.jpg
```

Luego las podremos renombrar en lote con el programa `pyrenamer`, que podemos instalar usando `sudo apt-get install pyrenamer`. Así, podemos ponerlas todas con el mismo formato de nombre (por ejemplo, numérico incremental).

Una vez que tenemos las imágenes, vamos a crear los ficheros descriptores de los objetos (de nuestra señal de STOP). Para ello, necesitamos un listado (fichero `txt`) con los nombres de las imágenes. Suponiendo que las tenemos en los directorios `training_images/positive` y `training_images/negative`, ejecutaremos los siguientes comandos desde `training_images`:

```bash
find ./positive -iname "*.jpg" -exec identify -format '%i 1 0 0 %w %h\n' \{\} \; > positives.dat
find ./negative -iname "*.jpg" > negatives.dat
``` 

Con esos comandos creamos el fichero de positivos, que tendrá la siguiente forma:

```dat
[imagen] [Número de objetos en la imagen] [[x y width height] [Segundo objeto en la imagen ] ...]
[imagen] [Número de objetos en la imagen] [[x y width height] [Segundo objeto en la imagen ] ...]
[imagen] [Número de objetos en la imagen] [[x y width height] [Segundo objeto en la imagen ] ...]

```

Así, estamos indicando que en cada imagen positiva tenemos 1 objeto, que va desde pixel del vértice superior izquierdo (0,0) y que tienen una anchura / altura según el objeto dentro de la imagen. Si todas imágenes fueran 256x256 sería más rápido, porque no tendríamos que calcular el ancho y alto de cada imagen. Asimismo, si tuviéramos varias coincidencias en la misma imagen, habría que indicar que hay 2 objetos y las coordenadas y ancho / alto de ambos objetos. 
 
El fichero resultante será algo así:

```dat
./positive/0001.jpg 1 0 0 243 256
./positive/0002.jpg 1 0 0 219 256
./positive/0003.jpg 1 0 0 243 256
```


## Creando muestras (_samples_) positivas y negativas

OpenCV cuenta con dos aplicaciones que nos permiten crear _samples_ (`opencv_createsamples`) y entrenar al calsificador en cascada que vayamos a utilizar (`opencv_traincascade`). Se puede encontrar más información en los tutoriales de [OpenCV - _Cascade classifier_](https://docs.opencv.org/master/db/d28/tutorial_cascade_classifier.html)

Los _samples_ no dejan de ser las imágenes positivas (con el objeto a detectar) y las negativas (sin el objeto a detectar), solo que con `opencv_createsamples` vamos a crear muchas más imágenes _positivas_ mezclando las imágenes positivas con las negativas. Así, se modificará las imágenes positivas cambiándoles la orientación, color, luminosidad, deformándolas, etcétera, introduciéndolas en las imágenes negativas.

Ejecutaremos el siguiente comando (indicando que vamos a usar el fichero `positives.dat` creado en pasos anteriores), el cual nos devolverá un fichero `positive-samples.vec`. El parámetro `-num 248` es el número de imágenes positivas que tengamos, mientras que el `-w 50 -h 50` es la anchura / altura de los samples a generar:

```bash
opencv_createsamples -info positives.dat -num 248 -w 50 -h 50 -vec positive-samples.vec
```

Para crear, por ejemplo, 1000 _samples_ a partir de una imagen:

```bash
opencv_createsamples -img positives/0001.jpg -bg negatives.dat -num 1000 -w 50 -h 50 -maxxangle 1.1 -maxyangle 1.1 -maxzangle 0.5 -maxidev 40 -vec positive-samples.vec
```

Para visualizar los _samples_ generados, podemos ejecutar el siguiente comando:

```bash
opencv_createsamples -vec positive-samples.vec -w 50 -h 50
```

Una vez tengamos suficientes _samples_, tenemos que entrenar el clasificador. Necesitaremos miles de _samples_ (cuantos más, mejor... sobre todo cuanto más complejo sea el objeto), o nos dará error el siguiente paso indicando que tenemos insuficientes samples. Las opciones son conseguir más imágenes o distorsionarlas y crear varios ficheros `.vec` para luego unirlos. Para unirlos podemos usar [mergevec](https://github.com/wulfebw/mergevec). 

En nuestro caso, para detectar la señal de STOP utilizamos 248 _samples positivos_ (recortados para que únicamente se visualice el objeto, la señal de STOP) y 1000 negativos.

## Entrenando al clasificador con los _samples_ positivos y negativos

Utilizando todos los ficheros `.vec` anteriormente generados, pasamos a entrenar el clasificacdor. Necesitamos crear el directorio `obj-classifier-stop`, ya que es el directorio donde se almacenarán los parámetros generados. Es recomendable que este proceso se lleve a cabo en nuestro ordenador, ya que al ser más potente que la Raspberry Pi llevará bastante menos tiempo.

El entrenamiento se realiza mediante el siguiente comando (podemos ver más opciones en el [tutorial de OpenCV - Cascade Classifier Training](https://docs.opencv.org/trunk/dc/d88/tutorial_traincascade.html), a la vez que nos tomamos uno o varios cafés, porque puede llevar horas según los parámetros que utilicemos...). 

```bash
opencv_traincascade -data obj-classifier-stop -vec positive-samples.vec -bg negatives.dat -precalcValBufSize 2048 -precalcIdxBufSize 2048 -numPos 200 -numNeg 2000 -nstages 20 -minhitrate 0.999 -maxfalsealarm 0.5 -w 50 -h 50 -nonsym -baseFormatSave
```

Este comando iterará durante 20 rondas (_stages_), indicándonos para cada propiedad que está siendo entrenada (_N_) los ratios de tasa de acierto (_HR: Hit Rate_) y de falsas alarmas (_FA: False Alarm_). Nos devolverá para cada _stage_ información como la tabla que se muestra a continuación. Si sólo se visualizan unas pocas propiedades (por ejemplo, `N = 2`), es posible que haya problemas con las imágenes que estemos usando para el entrenamiento, por lo que deberemos corregirlas o conseguir nuevas (o mayor cantidad). 

```
===== TRAINING 12-stage =====
<BEGIN
POS count : consumed   200 : 200
NEG count : acceptanceRatio    2000 : 6.10376e-06
Precalculation time: 51
+----+---------+---------+
|  N |    HR   |    FA   |
+----+---------+---------+
|   1|        1|        1|
+----+---------+---------+
|   2|        1|        1|
+----+---------+---------+
|   3|        1|        1|
+----+---------+---------+
|   4|        1|   0.8165|
+----+---------+---------+
|   5|        1|   0.6775|
+----+---------+---------+
|   6|        1|    0.676|
+----+---------+---------+
|   7|        1|   0.6915|
+----+---------+---------+
|   8|        1|    0.506|
+----+---------+---------+
|   9|        1|   0.2185|
+----+---------+---------+
END>
Training until now has taken 0 days 12 hours 35 minutes 16 seconds.
```

Tras cada fase iterada (_stage_), se almacenará en el directorio indicado que creamos antes (en nuestro caso, `obj-classifier-stop`) ficheros XML. Podemos parar tras cada fase el procesado, modificar la configuración de nuestro ordenador o usar otro ordenador más potente, ya que el procesado seguirá desde la fase en la que lo dejamos. 

En nuestro caso, tras 15 horas 42 minutos y 27 segundos procesando el paso anterior (no tenemos _hardware_ específico para estos tipos de procesado, de ahí que cueste tanto tiempo...), habremos obtenido el fichero `obj-classifier-stop/cascade.xml`, que lo copiaremos a `cascade_xml/stop_sign.xml`. Este descriptor lo utilizaremos en nuestro script Python con OpenCV para detectar las señales de STOP.

Para detectar los semáforos el proceso sería similar, pero como ya hemos comentado, hay mucha gente que comparte su trabajo y ya lo han hecho por nosotros, como es el caso de [Hamuchiwa](https://github.com/hamuchiwa/AutoRCCar/tree/master/computer/cascade_xml). Suyo es el descriptor de detección de semáforos `cascade_xml/traffic_lights.xml` que vamos a usar.

Como se puede ver, **podemos crear un descriptor para poder clasificar cualquier tipo de señal u objeto** que encontremos en la carretera. **Los podemos compartir muy fácilmente, creando nuevas versiones y mejorando las existentes** poco a poco. **Al compartirlos, en todas las máquinas en las que se ejecuten se ejecutarán de forma similar**, por lo que si somos capaces de detectar un tipo de señal, lo podrán hacer absolutamente todos los vehículos que contengan nuestros descriptores actualizados. Es decir, **nuestros vehículos autónomos podrán ser actualizados de forma periódica como se actualiza nuestro teléfono móvil** con nuevas versiones mejoradas de las aplicaciones instaladas.

Estos clasificadores se pueden usar también para otras maravillas de la ciencia como detectar ciertos tipos de cáncer mediante imágenes con una eficiencia mayor a la que muchos especialistas puedan alcanzar en su vida. El código se puede traspasar y replicar en cualquier otra máquina para obtener los mismos resultados positivos con sólo copiar unos ficheros mientras que el conocimiento absoluto que tenga una persona sobre un tema es imposible compartirlo al instante al 100% con otra persona...

En este punto hay que tener en cuenta que cuando nuestro vehículo viaje a otros paises quizá tenga que revisar qué descriptores tiene instalados, porque lo habitual en nuestro país es no encontrarse con señales de canguros y otras similares. Incluso en otros países, la señal de STOP es algo diferente y pone PARE. En este punto tendremos que actualizar y añadir nuevos descriptores para clasificar de forma correcta.

En ambos _scripts_ he ido incluyendo variables para facilitar la configuración usada, como puede ser la activación / desactivación de logs, dimensiones de imágenes, etcétera, por lo que es recomendable revisar dicha configuración cuando estéis haciendo pruebas. Ahora sólo queda probarlo arrancando primero el servidor y luego el cliente (accediendo previamente al entorno virtualizado python mediante `workon cv`):

* En el servidor: `python server.py`
* En el cliente: `python client.py`

El resultado será similar al de la imagen que acompaña este post: Somos capaces de detectar semáforos y su estado, así como señales de STOP. En próximas iteraciones de nuestro script también trataremos de detectar a peatones (ya que existen modelos en OpenCV ya predefinidos para detectarlos).

En próximos capítulos continuaremos con la calibración de las imágenes obtenidas de la cámara para poder calcular a qué distancia está la señal de STOP. ¡Espero que os guste!

## Actualización 2017-10-19

En el directorio `script_tests` del repositorio de [Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car) he añadido el _script_ `cascade_classifier_test.py`, junto al directorio `cascade_classifier_test` en el que hay varias imágenes que contienen señales de tráfico en entornos reales. Ejecutando dicho _script_ podemos probar nuestro clasificador directamente en nuestro ordenador sin necesidad de conectar la Raspberry Pi, indicando qué imagen de prueba queremos usar y la ruta al descriptor XML de nuestro clasificador en cascada. Así lo podemos probar y podemos comprobar como nuestro clasificador está bien entrenado, comparándolo con otros ficheros descriptores.

```bash
python cascada_classifier_test.py -c ../cascade_xml/stop_sign.xml -i images/stop-0020.jpg 
```

Si creamos otros descriptores de otros objetos, este _script_ nos puede servir para probarlos. Asimismo, podremos ver que pueden darse algunos falsos positivos (encuentra señal de STOP donde no la hay), y algunos falsos negativos (hay señal de STOP pero no la reconoce). Tampoco nos tiene que preocupar en nuestro proyecto que no detecte alguna imagen, porque en modo vídeo lo detectará en los siguientes fotogramas. El resultado será algo como lo siguiente:

![Señal de STOP detectada](/assets/images/2017-10-18-cascade-classifier-stop-detected.jpg)


## Más información sobre el proyecto

* [Código en Github - jorgecasas/autonomous-rc-car](https://github.com/jorgecasas/autonomous-rc-car)
* [Coche RC autónomo - Índice del proyecto Construyendo un vehículo autónomo]({{ site.url }}/2017/08/22/autonomous-rc-car-construyendo-un-coche-autonomo)
