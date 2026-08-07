Encontré el peor bug del año usando mi propia app.

El feed se terminaba. Bajabas dos pantallas y se acababa el contenido, como si no hubiera nada más para ver. Y había miles de publicaciones vivas ahí adentro.

Nadie me lo reportó. Lo vi yo, porque uso la app todo el tiempo: es lo primero que hago a la mañana, antes de mirar cualquier métrica. Esa costumbre —usar lo que construís como si fueras un usuario más— me ahorró más problemas que cualquier suite de tests.

Voy a contar qué encontré y cómo lo resolví, con el detalle técnico que a mí me hubiera gustado leer. Pero antes necesito contar de dónde venimos, porque explica todo lo demás.

## El contexto

Branded es una startup en etapa temprana que co-fundé. Somos cinco: el CEO, la CMO, el que se ocupa de finanzas y legales, y del lado técnico estamos Mati y yo.

Dos personas. Y del otro lado: alrededor de catorce microservicios en Kotlin y Spring Boot, una app móvil en Expo, un backoffice interno, la landing, integraciones con plataformas de e-commerce, pipelines de CI/CD e infraestructura en Google Cloud.

Ah, y ninguno de los dos trabaja acá full-time. Yo tengo un empleo, doy clases todos los días y tengo dos hijos.

Cuando cuento esto, la reacción es casi siempre la misma: *"¿microservicios siendo dos? Eso es sobre-ingeniería"*.

Es una crítica razonable, y merece una respuesta larga y honesta.

## No empezamos así (ni cerca)

La primera versión de Branded no tenía backend propio. Era una app construida en **FlutterFlow**, una plataforma lowcode, conectada directamente contra Supabase. La app hablaba con la base de datos y listo.

Y para lo que necesitábamos en ese momento, estaba perfecto. Nos permitió validar la idea sin escribir un backend.

El problema del lowcode conectado directo a la base es previsible: **la lógica de negocio no vive en ningún lado**. Está desparramada entre pantallas de la app y consultas sueltas. Cualquier regla que necesite orquestar dos cosas, o correr sin que haya un usuario mirando la pantalla, no tiene dónde ir.

Así que apareció el primer backend: un **monolito**, bien separado por responsabilidades, que empezó resolviendo unas pocas cosas puntuales. Y fue creciendo. Y creciendo.

Ahora sí, la parte interesante: cada pieza de infraestructura que agregamos después entró por un dolor concreto y medible. Ninguna por moda.

### Kafka y el primer microservicio: los webhooks que nos tiraban el sistema

Cuando nos integramos con una plataforma de e-commerce para que las marcas pudieran conectar su tienda, aparecieron los webhooks. Cada vez que una marca modificaba cualquier detalle de cualquier producto, nos llegaba una notificación.

Cada cambio mínimo, un webhook. Por cada marca instalada.

Y para mantener nuestra base sincronizada, muchas veces teníamos que **cargar todos los productos de esa tienda en memoria** y reconciliar. Con pocas marcas era tolerable. Con cada marca nueva que se sumaba, el costo crecía de forma insostenible: más webhooks, más reconciliaciones, más memoria.

El golpe final fue de arquitectura, no de performance: como todo vivía en el mismo proceso, **el procesamiento de productos me bloqueaba hilos**. Y cuando el servidor se caía por un problema de productos —algo que no tenía nada que ver con el resto del producto—, **se caía todo**. El feed, el login, la app entera.

Ese es exactamente el momento en que un microservicio deja de ser una moda y pasa a ser la respuesta correcta. Sacamos ese dominio afuera: un servicio de productos y un servicio dedicado al protocolo de esa plataforma, comunicándose por **Kafka**, procesando actualizaciones de forma asíncrona.

El resultado: si la sincronización de productos explota, explota sola. El resto del sistema ni se entera.

Costó al principio. Kafka tiene una curva, y operarlo en producción siendo dos también. Hoy es nuestro bus de eventos principal y lo usamos para todo — pasó de ser lo más difícil que habíamos hecho a ser infraestructura invisible, rutina.

### Redis: cuando la query del feed se volvió el cuello de botella

El siguiente dolor fue el feed. Antes del algoritmo, armar el feed de un usuario era **una sola query enorme y compleja**: joins por todos lados, filtros, ordenamientos, todo calculado en el momento de la petición.

Funcionaba, hasta que dejó de funcionar. Ese tipo de consulta escala mal por naturaleza: cuanto más contenido y más relaciones, más caro cada scroll de cada usuario.

La solución no fue optimizar la query. Fue **cambiar el momento en que se hace el trabajo**.

En lugar de calcular el feed cuando el usuario lo pide, lo calculamos **cuando pasan las cosas**: alguien publica, alguien visita, alguien guarda, alguien sigue. Cada uno de esos eventos viaja por Kafka y va actualizando un ranking por usuario en Redis.

Servir una página pasó de "resolver una consulta compleja" a "leer un ranking ya ordenado". El costo se movió de la lectura (que ocurre todo el tiempo) a la escritura (que ocurre mucho menos).

Y una vez que tuvimos ese ranking, apareció el lugar natural para el algoritmo: un sistema de puntajes donde cada interacción suma según cuánto interés demuestra, con multiplicadores por afinidad, decaimiento temporal para que el feed se sienta fresco, y una pizca de aleatoriedad para el descubrimiento.

### La IA: la ventana que aprovechamos a tiempo

Con la carga de contenido ya asíncrona y el feed desacoplado, se abrió la puerta para la IA. Y acá tuvimos suerte con el timing: en ese momento teníamos **pocas publicaciones en la base**, así que pasarlas todas por los modelos fue barato y rápido.

De cada publicación generamos:

- **Vectorización** (embeddings multimodales de imagen y texto), que vive en Postgres con búsqueda vectorial.
- **Palabras clave** alineadas al vocabulario que se usa en el mercado.
- **Colores** dominantes de la prenda o el look.

Lo de palabras clave y colores es lo más reciente que implementamos, y cambió el producto: habilitó **filtros por color y por términos reales de búsqueda**, algo que antes era imposible porque esa información simplemente no existía en ninguna parte.

Y los embeddings habilitaron el sistema de **publicaciones similares**, que —lo digo sin exagerar— tiene una precisión que a veces da miedo. Le mostrás una prenda y te trae otras genuinamente parecidas, sin que nadie las haya etiquetado a mano.

Si hubiéramos esperado a tener cien mil publicaciones, procesarlas todas habría sido caro y lento. Hacerlo temprano, cuando el catálogo era chico, fue una de las mejores decisiones de timing que tomamos.

### La regla

Mirando esa evolución hacia atrás, todo se ordena bajo una sola idea:

> **La complejidad se paga sola cuando resuelve un problema que ya te duele. Si la agregás antes, es deuda.**

Lowcode mientras alcanzó. Monolito cuando hizo falta lógica. Microservicios cuando un dominio empezó a tirar abajo a los demás. Kafka cuando lo sincrónico se volvió insostenible. Redis cuando la lectura no daba más. IA cuando había un problema que ninguna heurística resolvía.

Ninguna de esas decisiones fue tomada mirando qué estaba de moda. Todas fueron tomadas mirando qué nos dolía.

## La arquitectura orientada al dominio no es purismo: es velocidad

Acá está lo que más quiero transmitir, porque es lo que la mayoría subestima.

Cuando saqué el dominio de productos del monolito, no hice simplemente "otro servicio". Lo modelé siguiendo **Domain-Driven Design** y **arquitectura hexagonal**, igual que todos los que vinieron después. Y soy muy estricto con eso: la lógica de negocio en el centro, las integraciones —base de datos, Kafka, APIs externas, HTTP— en los bordes, comunicándose por puertos y adaptadores. La regla de dependencia no se negocia: el dominio no sabe que existe Postgres, ni Kafka, ni Spring.

Suena a manual. Pero el motivo por el que lo sostengo no es estético, es puramente práctico. Cuando sos dos personas y catorce servicios, esto es lo que compra:

**1. Los nombres dicen dónde está el problema.** Si algo anda mal con la sincronización de una tienda, sé exactamente en qué servicio mirar antes de abrir el editor. El mapa mental del sistema coincide con el mapa del negocio. Cuando el CEO me describe un problema en su idioma, yo puedo traducirlo a un lugar del código casi de forma directa — porque el código está organizado con los mismos conceptos que él usa para hablar.

**2. Se lee más rápido de lo que se recuerda.** Vuelvo a un servicio que no toco hace tres meses y no necesito reconstruir nada: entro por el caso de uso, leo el flujo del dominio en lenguaje de negocio, y recién bajo al adaptador si necesito el detalle técnico. Con dos personas, tu memoria es el recurso más escaso que tenés; la arquitectura funciona como memoria externa.

**3. Cambiar la tecnología deja de ser un evento traumático.** Migrar el buscador, cambiar cómo se guarda algo o reemplazar un proveedor toca un adaptador y nada más. La lógica de negocio ni se entera. Esto lo aprendí en sistemas de pago de misión crítica, y es lo que más se paga cuando la escala aprieta.

**4. Onboarding sin acompañamiento.** Un desarrollador nuevo puede abrir un servicio y entender el negocio leyendo el código, sin que nadie tenga que explicarle nada. Cuando el equipo es de dos y ninguno está full-time, no hay tiempo para tutorías largas.

La conclusión, dicha sin vueltas: **la gente cree que las buenas prácticas te frenan. En equipos chicos son literalmente lo contrario: son lo único que te permite ir rápido durante años en lugar de durante meses.**

La deuda técnica se paga con lo único que a un equipo chico no le sobra: atención.

## El otro desarrollador

Cuando arrancó todo esto, llamé a Mati. Había sido alumno mío, acababa de conseguir su primer trabajo formal y quería hacer algo propio.

Tenía poca experiencia y una capacidad de aprendizaje que me sorprendió muchas veces. Se metió con colas de mensajes, con búsqueda vectorial, con la app, con carga adaptativa de video para que los clips arranquen rápido y se adapten a la conexión de cada usuario. Cosas que no había tocado nunca y que muchos desarrolladores con años de oficio no tocan jamás.

Ahí hay algo que quiero decir con todas las letras, porque va en contra de cómo se contrata en la industria: **con una arquitectura clara y alguien que acompañe, la falta de experiencia se compensa mucho más rápido de lo que la mayoría cree.** La estructura hace la mitad del trabajo de enseñar: cuando el código está ordenado por dominio, el sistema te explica a sí mismo.

Los dos crecimos una barbaridad en estos tres años. Y si tenés equipo chico y no te alcanza el presupuesto para contratar seniors: buscá gente con ganas, dales estructura y acompañá la curva. Funciona mejor de lo que parece.

## El incendio

Volvamos al feed roto.

El diagnóstico me llevó bastante, y resultó que no era una causa: eran tres, componiéndose entre sí. Vale la pena verlo con números, porque el efecto combinado no es intuitivo.

### F1 — Tumbas con puntaje alto

El feed de cada usuario es, en esencia, una lista ordenada por relevancia: para cada publicación candidata guardamos su puntaje, y servir una página es leer los primeros de esa lista.

El problema: cuando una publicación se borraba, **nadie la sacaba de esas listas**. Quedaba ahí, como una tumba: una referencia a contenido que ya no existe.

Y no quedaban en cualquier lado: quedaban **arriba**. Porque el puntaje sube con la interacción, y las publicaciones con las que la gente más interactuó eran justamente las de mayor puntaje. Al medirlo, alrededor de la mitad de las primeras posiciones apuntaban a contenido inexistente.

Peor: la lista tenía un tope de entradas mayor que el catálogo vivo. Con el tiempo, eso garantiza que el espacio se llene de basura. La contaminación no era la mala suerte de un usuario: era **estructural**.

Es un caso de libro de **integridad referencial diferida**: cuando duplicás información en una capa de lectura rápida, cada borrado en el origen necesita su contrapartida. Si no la escribís, la divergencia no aparece de golpe — se acumula en silencio hasta que rompe algo.

### F2 — El fetch no compensaba los descartes

El sistema traía una tanda fija de candidatos (llamémosla *N*) y recién **después** filtraba: descartaba los que el usuario ya había visto e intentaba recuperar los datos completos de cada publicación contra la base, que es donde se caían las tumbas.

Si llamamos *m* a la proporción de ids muertos y *v* a la de ya vistos, la página efectiva era:

```
items_servidos = N × (1 − m) × (1 − v)
```

Con `N = 10`, `m ≈ 0.5` y `v` alto en la zona contaminada, eso daba entre **0 y 2 items**. Una página de feed prácticamente vacía.

Y acá está la parte perversa: el cursor avanzaba **N posiciones por request**, no N items servidos. Para cruzar un "desierto" de *D* ids muertos hacían falta:

```
requests_necesarios = D / N
```

Con cientos de tumbas por delante, eso son decenas de peticiones que ningún cliente va a hacer jamás.

### F3 — El relleno era siempre el mismo

Cuando la página no se llenaba, el sistema completaba con un pool de descubrimiento. Pero ese pool solo se había alimentado con publicaciones nuevas desde el día del deploy: **nunca se cargó el catálogo histórico**. Eran unas pocas decenas de publicaciones, siempre las mismas.

### El efecto combinado

La app deduplica por sesión y decide si hay más contenido con una regla simple: `hasMore = (items_nuevos > 0)`.

Con las tres causas juntas:

```
Página 1 → ~0 vivos del ranking + pool completo   → parece normal
Página 2 → ~0-2 vivos + pool ya servido (repetido) → casi nada nuevo
Página 3 → 0 nuevos                               → hasMore = false → "se acabó el feed"
```

Cada causa por separado era tolerable. Las tres juntas cortaban el producto en seco.

Cuando fui a medir cuántas tumbas había en total, la cifra me dejó helado: **cientos de miles de referencias muertas**, repartidas de forma pareja entre los rankings de toda la base de usuarios. Promedio de más de cien por usuario. No era mala suerte: venía acumulándose en silencio hacía meses.

## Primero destrabar, después arreglar bien

Lo resolví en dos tiempos, y creo que esa separación es lo más útil que puedo compartir de todo esto.

**Primero, la solución operativa: sin deploy, el mismo día.** Un script recorrió todos los rankings y eliminó las referencias cuyo contenido ya no existía, y precargamos el pool de descubrimiento con el catálogo real. El síntoma desapareció esa misma tarde y el uso de memoria bajó de paso.

**Después, la solución estructural: en código.**

- **Limpieza perezosa**: cuando una referencia no resuelve al armar la página, se la elimina de la lista en ese mismo momento. El sistema se auto-cura con el uso, gratis y para siempre. Es el patrón inverso al de un proceso de limpieza programado: en vez de barrer todo cada tanto, se arregla exactamente lo que se está mirando.
- **Sobre-fetch compensado**: en lugar de traer *N* fijo, traer más y seguir bajando el cursor hasta juntar *N* items vivos o agotar un máximo de ventanas. Despejando la fórmula anterior, el factor de sobre-pedido necesario es:

```
N_fetch = N_objetivo / ((1 − m) × (1 − v))
```

- **Relleno con variedad real** y un tope de página estricto, para no quemar el pool entero en una sola respuesta.
- **Al borrar contenido**, limpiar también los pools globales.

La tentación, cuando encontrás algo así, es ir directo al fix elegante. Pero el usuario tiene el feed roto **hoy**, no cuando termines tu refactor. Primero destrabás, después arreglás bien. Y nunca solo lo primero: la solución operativa sin la estructural es una bomba de tiempo, porque el problema se vuelve a acumular solo.

Y algo que hago siempre: escribí todo. El síntoma, la evidencia medida, las tres causas con sus números, las opciones con su riesgo y su costo, qué se ejecutó, con qué resultado y qué quedó pendiente. Ese documento existe y cualquiera del equipo puede leerlo.

## Cómo llegamos siendo dos

Cuatro cosas, en orden de importancia.

**1. La arquitectura es apalancamiento.** Ya lo desarrollé arriba, pero lo repito porque es la única respuesta real a la pregunta del título: modelar por dominio y sostener las capas es lo que hace que dos personas puedan navegar catorce servicios sin perderse. Es lo único en lo que no cedí nunca.

**2. Documentar el porqué, no el qué.** En mi código, los comentarios no explican qué hace la línea — eso ya se lee. Explican **por qué está así**: qué se probó antes, qué se rompía, qué alternativa se descartó y con qué criterio. Ese contexto es el primero que se pierde y el más caro de recuperar. Yo mismo, seis meses después, soy otra persona.

**3. La IA como copiloto, pero con contexto de verdad.** Trabajo mucho con IA, y no pidiéndole que escriba código suelto. Mantengo documentos vivos de arquitectura, de flujos y de modelos de dominio; y antes de construir algo grande, escribo un análisis: cuál es el problema, qué opciones hay, qué riesgo tiene cada una, qué decido y por qué. Con ese contexto cargado, la IA deja de ser un autocompletado caro y pasa a ser alguien con quien discutir un diseño a las once de la noche. Y el efecto secundario resultó ser el más valioso: el equipo hereda documentación que de otra forma no habría existido nunca.

**4. Comprar lo que no es tu negocio.** Acá es donde más cedí, y me costó. Al principio quise construir nuestra propia infraestructura de métricas, recolectando todo con nuestras propias herramientas. Le agregó muchísima complejidad al proyecto y terminó dándonos datos peores que los de una herramienta ya hecha. Migramos, y ganamos tiempo y precisión. Es mi error favorito, porque me enseñó a detectar temprano cuándo estoy complicando algo que no es mi negocio.

## Lo que le diría a un founder

**Un equipo chico no significa construir peor.** Significa elegir mejor qué construís. Nosotros cedimos en un montón de cosas —herramientas, features, funcionalidades enteras que quedaron para después—; en la arquitectura no cedimos, y es lo único que nos permite seguir avanzando tres años después.

**Empezá simple, pero dejá lugar para crecer.** Arrancar con lowcode fue correcto. El error habría sido quedarnos ahí cuando el producto ya pedía otra cosa — o haber arrancado con catorce microservicios el primer día.

**Las métricas van desde el principio.** Es de lo poco de lo que me arrepiento de verdad. Sin medir, las decisiones de producto son opiniones, y con equipo chico no te sobra tiempo para equivocarte por opinión.

**Usá tu propio producto, todos los días.** El bug más grave que tuvimos no lo encontró un test, ni un monitor, ni un usuario que se quejó. Lo encontré yo, scrolleando un domingo. Ninguna herramienta reemplaza eso.
