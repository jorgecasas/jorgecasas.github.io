---
layout: post
title:  "Optimizando la optimización"
date:   2015-06-14 19:35:00
categories: development
tags:
- development
- optimization
- code
- process
---

En los últimos días hemos estado hablando en la oficina sobre optimizaciones. En el día a día de un desarrollador siempre tienes en cuenta las optimizaciones, pero muchas de ellas las introduces sin ni siquiera pensar: Es como conducir un coche y cambiar las marchas. Cuando eres un conductor experimentado entran solas, pero los primeros días que conducías por ti mismo tenías que mirar a ver dónde estaba el cambio de marchas para no meter la marcha de _modo Rally_ cuando ibas por al autopista.

![Optimizando]({{ site.url }}/assets/images/2015-06-14-optimizando-la-optimizacion.png)

En muchos blogs sobre desarrollo puedes leer los típicos posts del tipo _"10 cosas que debes hacer para que tu web sea más rápida"_, o _"7 cosas que todo desarrollador tiene que saber para que su web sea más rápida"_, o _"10 consejos que he copiado directamente de otro blog y que te recomiendo que sigas aunque no los haya yo probado en mis propias carnes"_. Estos 10 consejos están bien, pero son los mismos en todas las páginas (y como ya se habrá podido apreciar, en muchos sitios no saben ni de qué están hablando).

Algunos de estos consejos tienen un mayor impacto que otros. Suelen ser del tipo:

* Usa API de terceros cuyas funcionalidades sean más rápidas y eficientes que si las tuvieras que ejecutar en tu servidor.
* A ser posible, no envíes HTML. Envía solo los datos en formatos como JSON, y que sea el navegador del cliente el que procese los datos.
* Sube los ficheros CSS y Javascript comprimidos. Y en un único fichero a ser posible. Y que carguen al final de la página.
* Usa CSS Sprites para sustituir imágenes de botones / iconos que sean muy utilizadas, para así realizar muchas menos peticiones. Y si puedes utilizar _font-icons_ (fuentes de iconos) mejor, porque enviarás menos datos todavía y serás más _cool_.
* Sube imágenes comprimidas, y cuantas menos mejor.
* Comprime los datos que envíe Apache / Nginx / lighttpd con los módulos correspondientes para enviar cuantos menos datos posibles
* Cachea, cachea, cachea. Usa CDN (_Content Delivery Network_) donde sea posible.
* Y alguna cosa más así...

Con estas (y otras técnicas) se consigue que el sistema sea más fluido y rápido en la carga de datos y en el uso del sistema.

Hay que tener en cuenta que una de las optimizaciones que más se notan en cuanto a rendimiento es cambiar el sistema a una máquina / nube de máquinas más potentes, y esto hay que analizarlo en muchas ocasiones (sobre todo cuando toda la tecnología _hardware_ se ha convertido en una _commodity_, y que el coste de mano de obra humana versus coste de mano de obra _hardware_ es mucho mayor). Es decir, suele ser mucho más costoso optimizar un trozo de código que hacerlo correr en una máquina más potente. 

Suena un poco triste, pero en el proceso de aprendizaje de desarrollo de código hay momentos en los que esto se convierte en una realidad: O haces un módulo nuevo desde cero, o pones una máquina más potente (porque meterle mano a un código que tiene ya varios años puede ser dañino incluso para la salud de los desarrolladores). Y cuando lo que se requiere es inmediatez, puedes levantar una máquina adicional con un click. Revisar código para micro-optimizarlo puede llevar días, semanas, meses... y no conseguir los resultados que se pretenden.

Por eso, a los nuevos desarrolladores siempre les decimos nuestro mantra _KISS_: _Keep It Simple, Stupid_... Cuanto más sencillo sea el código que vayas a desarrollar, más fácil será que sea óptimo. Y aun cuando creas que es el código óptimo, seguro que lo puedes refactorizar y optimizar. Y aquí volvemos a lo mismo: Habrá que analizar si es más eficiente optimizarlo, y qué coste tendrá dicha optimización.

Porque al final, **todo depende del coste y de la rentabilidad que se pueda obtener con el código**. En otros muchos blogs de desarrollo se habla (y se hacen estudios de eficiencia y rendimiento) sobre microoptimizaciones. Las microoptimizaciones son hacer pequeños cambios en el código (por ejemplo, usar un método u otro para obtener un mismo resultado o funcionalidad), que si se ejecuta una única vez apenas habrá una optimización de pocos micro o nanosegundos, pero que si se realiza un millón de veces se puede obtener una mejora de incluso segundos. 

Eso sí, revisar miles de línea de código para introducir estas microoptimizaciones tiene un coste humano muy grande, por lo que estas microoptimizaciones son estúpidas en el 99,9999% de los casos. Conocerlas hace que al escribir código puedas utilizarlas de primeras, y entonces podrían llegar a tener sentido. Y puede ser que en algún caso muy muy crítico sean necesarias (en el 0,0001% de los casos restantes).

Pero hay otras optimizaciones que no se ven a primera vista. Ya no se tratan de estilos de desarrollo, ni de conocer mejor o peor el lenguaje utilizado, ni de saber cómo funciona a la perfección la arquitectura tecnológica que estás utilizando. Son pequeñas optimizaciones de usabilidad. Estas pequeñas mejoras las aprendes, en muchos casos, con el tiempo (porque sabe más el Diablo por viejo que por diablo...). Por ejemplo, haciendo que tras pulsar un botón de envío de formulario se genere un desplazamiento del contenido hasta el siguiente punto en el que el usuario tiene que actuar, evitándole que tenga que realizarlo él manualmente. Así puedes hacerle ganar al usuario varios segundos por click, y todo el tiempo que le haces ganar al usuario (que como ya hemos dicho, es mucho más costoso que el tiempo máquina) puede llegar a ser brutal, y más cuando la tarea la tiene que realizar miles de veces a lo largo del año.

Mediante el análisis de los procesos que siguen los usuarios se puede mejorar el interfaz de usuario (gracias a UX principalmente) para que sean estos los que menos tiempo pierdan. Es decir, la optimización muchas veces no depende de mejorar la calidad del código, ni de nuevas técnicas de procesado, sino que cambiando un botón de sitio puedes ser capaz de de hacer perder menos tiempo al usuario y gestor de la información. Y el tiempo es oro (al menos para algunos).
