Branded es una red social de social shopping: conecta a creadores de contenido, marcas y compradores en un mismo lugar. La co-fundé en 2023 y desde el día uno me tocó lo mismo que sigo haciendo hoy: la visión tecnológica, la arquitectura y la infraestructura.

Éramos cinco. Ninguno de esos cinco era un ejército de plataforma. Y aun así había que construir algo que no se rompiera cuando creciera.

Ese es el problema real de una startup temprana, y casi nadie lo cuenta con honestidad: **tenés que elegir bien sin tiempo, sin gente y sin margen para rehacer.**

## El problema

Una startup de cinco personas tiene dos formas clásicas de morir en la parte técnica.

La primera es ir demasiado rápido: una app lowcode, todo pegado con alambre, y a los seis meses cada cambio rompe otra cosa. La segunda es ir demasiado prolijo: montás la arquitectura de una empresa de mil personas para un producto que todavía no sabe qué va a ser, y te fundís en complejidad antes de tener usuarios.

Las dos se pagan caro. Y la trampa es que las dos se sienten productivas mientras las hacés.

Lo que yo necesitaba era una tercera cosa: **una base que aguante crecer, pero que no me cueste más de lo que me devuelve hoy.**

## La decisión

Elegí arquitectura seria desde el principio —microservicios con puertos y adaptadores (hexagonal) y Domain-Driven Design— pero la fui metiendo por pieza, cuando el dolor lo justificaba. No catorce microservicios de entrada. Cada uno entró cuando ya había una razón concreta para que existiera.

El trade-off lo asumo de frente: DDD y hexagonal tienen un costo de arranque. Escribís más al principio. Pero compran algo que en un equipo chico vale oro: **poder tocar una parte sin entender el todo, y sin miedo.** Cuando sos dos personas sosteniendo la plataforma, esa propiedad no es un lujo de purista. Es lo que te deja dormir.

> La regla que seguí: alta cohesión, bajo acoplamiento, y cada pieza entra cuando duele — no antes.

Esa historia la conté en detalle acá: [Dos desarrolladores, catorce microservicios](/blog/dos-devs-catorce-microservicios).

## Cómo lo construí

El corazón es event-driven. La mensajería asíncrona va por **Kafka**, el caché de alto rendimiento por **Redis**, y todo corre sobre **Google Cloud Platform**. Los pipelines de CI/CD, con Jenkins en un entorno distribuido.

La parte que más me preguntan es el feed. Branded tiene un algoritmo de contenido que personaliza lo que ve cada usuario, y sí, tiene IA — pero la decisión más importante que tomé ahí fue **dónde no ponerla**.

Podía construir "el algoritmo de YouTube", una red neuronal recomendando el feed. Me senté a medir antes de escribir el modelo. Y decidí no hacerlo: para el tamaño del producto, la latencia y el costo de mantener eso no se justificaban frente a lo que ganaba. La IA se ganó su lugar en otras partes, donde sí rendía. El feed prioriza responder rápido.

Esa decisión —medir antes de enamorarme de la solución más vistosa— está contada acá: [Tengo IA en producción. Por eso no le puse una red neuronal a mi feed](/blog/ia-algoritmo-feed).

## El resultado

Branded es un producto vivo, no un demo. Tiene una comunidad de **~38.000 seguidores** y corrió activaciones reales en eventos como **BAFWEEK** y **BRANDHAUS**.

Pero el resultado del que estoy más seguro no es una métrica de vanidad. Es que **seguimos sosteniendo la plataforma siendo un equipo mínimo**, y que agregar algo nuevo no implica rezar. La arquitectura que elegí al principio es la razón de que eso siga siendo verdad hoy.

## Stack

Kotlin · Spring Boot · Kafka · Redis · Google Cloud Platform · Jenkins · Arquitectura hexagonal (puertos y adaptadores) · Domain-Driven Design · Event-driven

## Qué me llevo

Que la arquitectura buena, en un equipo chico, no es la más impresionante. Es la que te deja moverte rápido durante años sin que la deuda te alcance. Elegir bien los trade-offs —y saber cuándo no construir algo— fue más valioso que cualquier tecnología que haya usado.
