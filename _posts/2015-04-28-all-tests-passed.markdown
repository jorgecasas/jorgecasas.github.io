---
layout: post
title:  "All tests passed"
date:   2015-04-28 17:35:00
categories: development test
tags:
- development
- test
---

Una de las claves en cualquier desarrollo (o cualquier cosa que realicemos de forma manual / programada), sea del tipo que sea, es comprobar que funciona. Si funciona es que es_probablemente_ la hayamos desarrollado de forma correcta. Y digo _probablemente_ porque no siempre es así. A veces algo que funciona mal funciona bien (o eso creemos). O el mal funcionamiento es una nueva funcionalidad.

![100% tests passed succesfully!]({{ site.url }}/assets/images/2015-04-28-all-tests-passed.jpg)

En el caso del desarrollo de aplicaciones y herramientas, los tests que realicemos pueden ser manuales y sistemáticos (no recomendable, pero a veces no queda más remedio que un humano revise el buen funcionamiento) y automatizados (es lo recomendable, siempre que los propios tests sean adecuados y correctos).

Varios aspectos a tener en cuenta:

* El desarrollo del los tests también requiere programación. Hay que programarlos bien, sin errores, o puede que den un resultado erróneo.
* Hay que conocer qué _inputs y outputs_ (entradas y salidas de datos) deben recibir y deben utilizar los diferentes casos de test que desarrollemos. Sin conocer internamente qué debería devolver una funcionalidad (método, función, API...) para unos datos de entrada o datos disponibles dados, no podremos comprobar que esté dando la salida esperada. 
* Hay que comprobar todos los casos (o la mayor parte de ellos) de _inputs_, con sus correspondientes _outputs_. Si un caso de test únicamente comprueba un _output_ para un _input_ dado, puede que para otros _inputs_ falle (y no nos demos cuenta hasta que sea demasiado tarde).
* Hay funcionalidades muy difíciles de testear si se hace de forma automatizada. Por ejemplo, el caso en el que se deban calcular los días festivos entre 2 fechas, suponiendo que el usuario pueda almacenar diferentes fechas festivas en la base de datos y que estos varíen según el usuario. En este caso, calcular los días festivos supone la necesidad de obtener fechas de la base de datos y recalcular los días festivos... pero eso mismo es lo que se está haciendo en el método a ser testeado... Es decir, hay cosas que dependen de la configuración del sistema (y pueden ser muy variables).
* Es necesario testear todo lo posible para encontrar posibles bugs, no solo en nuevos desarrollos, sino en actualizaciones de funcionalidades ya existentes. De esta forma, nos aseguramos también de compatibilidades hacia atrás, o que en cambios de versiones _principales_ siga funcionando todo como era de esperar.
* Se deben automatizar los tests que sean posibles. Y se debe testear todo aquello que sea posible (tests unitarios, de integración, de UX, de funcionamiento de UI, de eficiencia... incluso de la calidad de documentación...). Para ello hay innumerables herramientas disponibles.
* Realizar el desarrollo de los tests lleva tiempo. Es un tiempo que no se ve, pero que se debe tener en cuenta (tanto en planificación temporal como en presupuestos). Y también se debe incluir la documentación y comentarios que sean necesarios en los casos de test desarrollados (ya que sigue siendo código).
* No se debe testear en ambientes de producción. Por el bien de todos.

En este punto se puede abrir otra vía de debate: **¿Es bueno que la misma persona que realiza el desarrollo de una funcionalidad realice los casos de test para comprobar su correcto funcionamiento?**. En principio es lo más rápido, pero no es recomendable (ya que si el desarrollador comete un error en la asimilación de lo que debería realizar la funcionalidad desarrollada la propagará también a los casos de test). En caso de que sean dos personas diferentes, llevará un coste y esfuerzo adicional (ya que otra persona, además del desarrollador principal, deberá estudiar y asimilar los requisitos de las funcionalidades desarrolladas)

También conviene realizar una monitorización en tiempo real del código / sistema desarrollado. Existen herramientas que, tras instalar un agente, son capaces de obtener cuellos de botella, analizar los métodos más utilizados y que más recursos consumen, etcétera... por lo que ayudan a mejorar la calidad del código, y por tanto, a cometer menos errores en el desarrollo y programación.

Y sobre todo, hay que recordar que cualquier sistema es mejorable y rompible. Y si está roto hay que arreglarlo. Y aunque tú no te percates de algún error, **siempre habrá alguien que haga algo extremadamente idiota o inteligente capaz de romper una funcionalidad de tu sistema (o el sistema completo)**.
