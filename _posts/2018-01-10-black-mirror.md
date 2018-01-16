---
layout: post
title:  "Black Mirror"
date:   2018-01-10 18:20:00
categories: blog development
tags:
- blog
- development
- opensource
- apocalypse
thumbnail: /assets/images/2018-01-10-black-mirror.jpg
---

La serie [Black Mirror](https://www.netflix.com/title/70264888) trata sobre un futuro distópico no muy lejano (o lejano) donde todo está conectado gracias a la tecnología de una forma u otra, y que permitirá al ser humano disfrutar de nuevas capacidades, las cuales pueden tener sus riesgos y sobrepasar cualquier aspecto ético y moral que nos pueda quedar como especie. 

![Black Mirror]({{site.url}}/assets/images/2018-01-10-black-mirror.jpg)

Hay capítulos en los que gracias a la tecnología podemos [viajar sin tener que conducir]({{site.url}}/2017/08/22/autonomous-rc-car-construyendo-un-coche-autonomo), acceder a los recuerdos (para resolver un crimen), tener una videocámara permanente en nuestro cerebro (siendo capaces de ver todo lo que nos ha ido ocurriendo en el pasado), estar [monitorizados en todo momento](https://www.statnews.com/2017/06/07/wearable-devices-health-care-savings/) y [en toda acción](https://www.xataka.com/privacidad/20-millones-de-camaras-equipadas-con-inteligencia-artificial-hacen-que-china-sea-el-verdadero-gran-hermano) o tener una [vida virtual paralela](https://www.forbes.com/sites/quora/2017/05/25/how-close-are-we-to-creating-full-immersion-vr-worlds/). Hay otros capítulos en los que parece que alguien nos vigila y sabe todo de nosotros, sin tener privacidad alguna, o en la que nuestro círculo social nos [juzga](https://hipertextual.com/2017/04/puntuacion-usuarios-uber) según los [_me gusta_ del resto de personas que nos rodean](http://www.lavanguardia.com/tecnologia/20171107/432679958989/china-sistema-deteccion-personas.html). 

Cada capítulo te hace pensar y reflexionar sobre muchos de estos aspectos (sobre todo los éticos y morales) para darte cuenta de que en la época actual en la que nos está tocando vivir ya se dan muchos de conceptos de una forma u otra.

Lo que vengo a contar hoy tiene algo de similitud con **Black Mirror**. Como he comentado, el uso de la tecnología puede sobrepasar la delgada línea que separa el bien del mal. Un cuchillo (la tecnología o herramienta) no es ni bueno ni malo, pero en principio ha sido diseñado para el bien (cortar la comida que vas a comer), pero se puede usar también para hacer el mal (asestar veintisiete puñaladas). Así, la bondad o maldad de la tecnología dependerá más de la persona que la use, por lo que tenemos que tener siempre presente una cosa: **En este mundo hay gente buena, y gente mala**, y por desgracia en algunos aspectos el número de gente mala sobrepasa en número (y por mucho) a la gente buena.

El otro día (período comprendido entre una semana y la fecha de creación del Universo), leí sobre [cómo un personaje había sido capaz de robar miles de datos de personas](https://hackernoon.com/im-harvesting-credit-card-numbers-and-passwords-from-your-site-here-s-how-9a8cb347c5b5) (incluyendo números de tarjetas de crédito, etc...). Es decir, sin hacer nada, sin darnos cuenta, podemos haber sido monitorizados y desvalijados como si estuviésemos en ese futuro distópico no tan lejano.

Normalmente, si eres un ser maligno, puedes realizar diferentes pasos para tratar de acceder a bases de datos remotas, encontrar brechas de seguridad en sistemas, etcétera... o bien preguntarle al usuario directamente.

<blockquote class="twitter-video" data-lang="es" width="740"><p lang="en" dir="ltr">Passwords weakness: YOU <a href="https://t.co/3TzE3I7YHf">pic.twitter.com/3TzE3I7YHf</a></p>&mdash; Marcos Besteiro (@MarcosBL) <a href="https://twitter.com/MarcosBL/status/942944201876148224?ref_src=twsrc%5Etfw">19 de diciembre de 2017</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


El **usuario siempre es el eslabón más débil de la cadena de seguridad**, y en cuanto se rompe un eslabón, está todo vendido. **Pero ir usuario a usuario puede ser lento** (tan lento como los humanos), y puede hacer que alguien con dos dedos de frente se entere y dé la voz de alarma (nota mental: Nunca déis vuestras contraseñas, ni por teléfono, ni por email, ni por mensaje, ni escritas en un _post-it_...). 

Mejor hacemos lo siguiente: **Creamos una librería [open source](https://es.wikipedia.org/wiki/Software_de_c%C3%B3digo_abierto) y la compartimos con el mundo**. 

Al ser de código abierto, cualquier persona puede acceder al [repositorio donde se encuentre el código](https://github.com/jorgecasas/autonomous-rc-car), modificarlo, actualizarlo y mejorarlo. El disponer de **software de código abierto siempre es bueno**, porque cualquiera de los miles y miles de desarrolladores pueden echarle un ojo, encontrar _bugs_, mejorarlo, hacerlo más seguro... Además, hay gran cantidad de librerías que se ofrecen sin coste alguno para los desarrolladores (aunque hay que revisar sus licencias), por lo que pueden ser utilizadas para mejorar nuestros proyectos con nuevas funcionalidades de forma rápida, eficiente e indolora.

Ahora bien, **¿qué tipo de librería tenemos que desarrollar y compartir para tener éxito?** Una que resuelva una funcionalidad que mucha gente quiera resolver y que no existan otras librerías que ya lo solucionen. Esto es complicado, porque si alguien ha necesitado en el pasado una librería suele haber ya librerías muy utilizadas. 

Entonces, **¿qué es lo que más le gusta y llama la atención a los seres humanos?** Las luces de colores, cuanto más brillantes mejor. En serio, **las luces de colores es lo que más llama la atención a los humanos. Somos así de simples**. En algún proyecto en [ITERNOVA](https://www.iternova.net) nos han indicado nuestros clientes que no podían trabajar usando el sistema desarrollado hasta que no cambiáramos los colores de los botones a colores más chillones, porque si no se confundían de botón (a pesar de que la leyenda de uno era _Aceptar_ y la del otro _Cancelar_).

Entonces, ¿qué librería desarrollamos? Una que nos muestre **los _logs_ de errores con colores**. Color verde, azul, amarillo, naranja, rojo... y sus variantes de nombres rocambolescos: rojo tomate, rojo tierra, amarillo pollo, rosa chicle, azul aguamarina...

Una vez desarrollada la librería, se pone a disposición de los desarrolladores en diferentes repositorios. Se publican un par de _posts_ en Facebook, Reddit, Medium o en cualquier otra red social de moda donde puedan ser leídos por desarrolladores. La gente empieza a usarlo porque bueno... saca cosas de colores. No sirve para nada más, pero cada vez más desarrolladores la usan, más páginas y proyectos web la usan internamente... incluso webs de grandes corporaciones con miles y miles de usuarios diarios. 

En este momento, hay que tener en cuenta que las librerías Javascript, como la que hemos desarrollado, suelen tener un fichero `package.js` donde cualquier otro programador puede ver el código fuente y modificarlo si lo desea. Cuando se compila / reduce el fichero con dicho código (cosa que se suele hacer automáticamente sin que nadie tenga que hacer nada de forma manual), se genera un fichero `package-min.js`, de tamaño más reducido / minimizado / optimizado, que es el que se suele descargar y utilizar en las webs y proyectos. 

Aquí llega el punto interesante: Nadie se fija que realmente el fichero `package-min.js` tenga el código definido en `package.js`, por lo que el desarrollador maligno decide cambiar dicho fichero por uno creado por él de forma manual (sin tocar `package.js`, que es el que los desarrolladores que buscan _bugs_ y crean mejoras van a leer y modificar). Cuando miles de webs actualizan esta librería, el fichero `package-min.js` se actualiza en todas ellas. 

Si bien el comportamiento inicial de `package-min.js` era sacar mensajes de error de colores, ahora además de seguir sacando dichos mensajes lo que hará es buscar campos de formulario en las páginas en las que se ejecute el código: Campos de usuarios, contraseñas, cuentas bancarias, números de tarjeta... datos que serán enviados sin que nos demos cuenta al desarrollador maligno.

Y así, sin que nadie se lo espere, miles de usuarios, sin saberlo, están siendo robados. Y miles de desarrolladores, sin saberlo, han confiado en una librería de terceros que ha cruzado la línea del bien y del mal sin que se den cuenta. **Un caballo de Troya en toda regla**. 

Incluso hay desarrolladores malignos que usando esta misma técnica han conseguido que webs de ["grandes" empresas (como Movistar) minen criptomonedas para su beneficio personal](https://hipertextual.com/2017/12/movistar-usa-ordenadores-visitantes-su-web-minar-criptomonedas) gracias a los miles de usuarios que las visualizan a diario (algunos de ellos ni se dieron cuenta de que su ordenador iba un 100% más lento que de costumbre al visitar dichas webs).

Y tras leer este ladrillo, **si no tenéis un poco de miedo a este futuro distópico es que vivís en uno de los mundos de Black Mirror y ya lo tenéis asumido**. Bienvenidos a San Junípero.

