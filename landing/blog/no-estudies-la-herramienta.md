Sobre el final de la aceleración, terminando una clase, un alumno me hizo la pregunta más común del mundo: si le convenía aprender alguna base tipo Redis.

Le dije que dependía. Y que ese "depende" era, en serio, la respuesta que más iba a escuchar en toda su carrera. No como excusa para no comprometerse: al revés. "Depende" es lo más honesto que podés contestar una vez que entendés de qué se trata esto de verdad.

Lo que sigue es lo que traté de que se llevaran esos dos meses y medio. Y, de paso, cómo hago para que las buenas prácticas no aplasten a alguien que recién arranca.

## "Depende"

Redis, o el caché en general, es una tecnología que apareció para resolver un problema puntual: la velocidad de respuesta —y acá estoy reduciendo muchísimo la definición, el caché da para mucho más—. Y como toda tecnología, mientras resuelve una cosa, te trae otras: un recurso más que desplegar, más que mantener, más que puede fallar. Cuando lo dije, saltó lo esperable: "pero mejora la performance", "sirve para escalar". Y sí. El tema es *cuándo eso importa*.

Ahí es donde entran los atributos de calidad de un sistema: performance, escalabilidad, seguridad, mantenibilidad, y una lista larga. El error es creer que hay que cumplirlos todos. No se puede, y no hace falta. Son trade-offs, y cuáles priorizás depende del negocio para el que estás construyendo.

Se los puse simple: si estoy haciendo el software de un almacén de barrio, la escalabilidad no me quita el sueño. No va a atender un millón de pedidos por segundo. Meterle Redis para escalar sería traer un problema que todavía no tengo. Con indexar bien una tabla en la base relacional probablemente sume toda la performance que ese negocio necesita, sin desplegar ni mantener nada nuevo.

Y acá está el punto que quería dejarles: **si solo sabés Redis, vas a resolver todo con Redis.** Si entendés qué es un caché —el concepto, no el producto—, ves todo el abanico. El mercado tiene mil soluciones para lo mismo. Para ese almacén, capaz una plataforma como Supabase te resuelve de una: trae autenticación, autorización, storage, colas, caché. Armar todo un backend a mano sería lindísimo, pero quizás no es lo que el cliente necesita.

Por eso no les enseño tecnologías. Les enseño los conceptos que hay debajo —el caché, las colas, la búsqueda por indexación, los tipos de memoria, qué ofrece cada servicio en la nube— y qué lugar ocupa cada solución. Las herramientas cambian y pasan de moda. El concepto queda.

## No somos programadores

Hay una frase que les repetí hasta cansarlos: no somos simples programadores. Somos creadores de sistemas que optimizan procesos complejos de una empresa. Nuestro trabajo es que esa empresa produzca más, a menor costo. El foco va por ahí.

Cuando lo mirás así, cambia todo. Muchas veces la mejor solución para el cliente no es escribir más código: es saber de sistemas. Es elegir la herramienta que el cliente necesita y no la que está de moda —sea no-code, low-code o una implementación bien técnica—. El cliente no entiende de tecnicismos, ni tiene por qué. Le importa que su problema real, puntual, quede resuelto. No lo último, no lo más moderno, no lo más brillante. Lo que funciona para él.

## Primero el problema, después la herramienta

Esto no es solo un discurso; es también cómo doy la clase. Enseño con conceptos core y ejemplos de la vida real, y casi siempre en el mismo orden: primero te hago sentir el problema, después te muestro la herramienta que lo resuelve.

Un ejemplo concreto. Antes de mostrarles Spring, agarré una app Java común y la partí en capas a mano. El controlador eran unos `print` con toda la lógica del menú; el servicio hacía las validaciones; el repositorio leía y escribía en memoria. Sin framework, tenían que instanciar todo ellos, en el `main`. Ahí les mostré inyección de dependencias e inversión de dependencias —justo entre el servicio y el repositorio—, para que vieran lo bueno: como el servicio no dependía de la clase concreta, cambiar una implementación por otra era trivial.

Fui obsesivo separando las capas, para que quedara clarísimo qué va en cada lado. Y cuando vieron que instanciar todo eso se comía la memoria, aplicamos Singleton y reutilizamos instancias —lo que me sirvió, de paso, para hablarles de heap y stack—.

Recién ahí llegó Spring. Y cuando llegó, no fue magia: fue alivio. Ya entendían qué problema venía a resolver, porque lo habían sufrido a mano. Spring instancia por ellos, hace singletons por defecto, y hasta pueden cambiar ese comportamiento con beans. **La herramienta tuvo sentido porque primero existió el problema.** Al revés —el framework antes que el problema— se aprende de memoria y se olvida igual de rápido.

## Lo que decidí no enseñar

Enseñar bien también es saber qué dejar afuera.

Tenía ganas de mostrarles arquitectura de puertos y adaptadores (lo que muchos buscan como "hexagonal") junto con DDD. No lo hice. El modelo en capas ya les estaba costando lo suyo, y apilar esa abstracción encima habría sido sumar complejidad sobre algo que todavía se estaba asentando. Una buena práctica metida antes de tiempo no ayuda: es peso muerto que el otro memoriza sin entender. Hay tiempo. Ya van a llegar.

Lo que sí bajé fue lo que iban a usar sí o sí en el proyecto: capas, SOLID, GitFlow, y mucho espacio para que se organizaran entre ellos.

## La clase se pone buena cuando preguntan

Si hay algo en lo que insistí desde el primer día, fue que preguntaran. Los molesté bastante con eso. Al principio costaba —nadie quiere quedar como el que no sabe—, pero de a poco se fueron soltando, y empezaron a preguntar cada vez más.

Ahí la clase cambió de temperatura. Dejó de ser yo hablando y pasó a ser un ida y vuelta. Y eso aceleró el aprendizaje de todos, el mío incluido, porque cada pregunta te obliga a explicar de otra manera algo que creías tener claro.

Me causó gracia darme cuenta de dónde venía yo. Cuando aprendí a programar por mi cuenta, el insoportable que preguntaba todo era yo: le escribía a desconocidos, armaba grupos de estudio, molestaba a cualquiera que supiera más. Ahora me tocó el otro lado del mostrador: tirar del hilo para que ellos se animaran. Aprender por tu cuenta nunca fue aprender en soledad. Enseñar, descubrí, tampoco.

## Por qué lo hago

Disfruto de enseñar. Pero hay algo más. Me viene incomodando ver puestos senior ocupados por gente sin una base real, y en vez de quejarme, esto es lo que puedo aportar: capacitar gente de verdad. Es mi grano de arena.

Y hay una parte más interesada, si querés, pero igual de honesta: estas personas son las que quizás me cruce en un trabajo. Hoy teníamos una relación de profe y alumno; mañana pueden ser colegas con los que me toque construir algo codo a codo. Formar bien a la gente con la que después voy a trabajar es de las mejores inversiones que puedo hacer.

Porque las personas que se forman hoy son las que mañana van a construir sistemas que le cambian la vida a un montón de gente. Instruir bien a quien recién arranca no es un gesto simpático: es una responsabilidad con la industria.

Y algo que no esperaba: en dos meses y medio no armamos un curso, armamos un grupo. Se apoyaban, se preguntaban entre ellos, se ayudaban a deshora con un conflicto de Git. En la retrospectiva —que hice con tema pirata, buscando "oro oculto" y esquivando "piratas en la costa"— lo que más valoraron fue la sinceridad para plantear dudas. Y ese, para mí, es el mejor síntoma de que algo se aprendió: no que sepan Redis, sino que pierdan el miedo a preguntar y entiendan por qué eligen lo que eligen.

Estoy orgulloso de todos ellos. De verdad. De dónde arrancaron y de dónde llegaron en dos meses y medio, del laburo que le pusieron y de las ganas que no aflojaron ni un día. Ver cuánto aprendieron —lo lejos que se fueron de donde empezaron— es la mejor devolución que me podía llevar de todo esto.

Yo aprendí a programar sin un guía que me marcara el camino. Estas 120 horas —que fueron muchas más, nos juntábamos incluso fuera de hora— me tocó ser, para otros, esa mano que a mí me faltó. Así que si estás dudando en dar esa clase, ese taller, esa mano a alguien que sabe menos que vos: dala. Vas a entender tu oficio más hondo que nunca. Y capaz, sin darte cuenta, termines siendo para alguien el guía que vos no tuviste.
