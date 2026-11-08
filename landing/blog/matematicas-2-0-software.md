Un día, a principios de mes, me quedé sin tokens.

No en sentido figurado. Al equipo nos habían asignado una cuota generosa de IA a cada integrante para sacar adelante una migración enorme, y yo me la había gastado entera en un solo día. Me quedaban varias semanas por delante, el mismo plazo apretado de siempre y ninguna intención de bajar el ritmo.

Así que hice lo que hacíamos antes: abrí el código y me puse a leerlo.

Lo que encontré no fue un bug. Fue algo más incómodo: la medida exacta de lo poco que entendía de lo que venía construyendo. Llevaba semanas migrando ese sistema y no sabía, en serio, qué hacía.

Tiempo después leí una charla de Terence Tao —para muchos, el mejor matemático vivo— llamada *Math 2.0*. Habla de cómo la inteligencia artificial está cambiando las matemáticas. Y mientras la leía tuve una sensación rara: **estaba describiendo lo que me había pasado a mí ese día, en otra disciplina.**

## Lo que pasó en las matemáticas

OpenAI publicó de golpe 722 manuscritos matemáticos generados con sus modelos. La empresa habló de progreso en más de 300 problemas abiertos, algunos con décadas encima, varios de los que generaciones de matemáticos no habían podido resolver.

Uno esperaría festejos. No los hubo.

Ningún resultado estaba verificado del todo, y revisar semejante volumen puede llevar meses. Tres manuscritos se retiraron en un solo día y otros catorce se corrigieron. Varios investigadores dijeron que algunos textos eran casi imposibles de leer sin ayuda de una IA. Uno de ellos sintió que los resultados le habían "aniquilado" su campo de investigación. Tao, antes de la publicación, los había llamado *toxic waste* (residuo tóxico); después de leerlos, los rebajó a *maybe treated waste* (quizá residuo tratado).

Antes de todo eso, más de veinte medallistas Fields —el premio más alto de la disciplina, Tao entre ellos— ya habían firmado una declaración con un título que no deja mucho margen: *A Severe Misalignment of AI in Mathematics*. Advertían que tratar los problemas abiertos como benchmarks de las empresas de IA, sin sumar comprensión humana, le hace daño al campo.

Hasta acá, podría parecer una pelea de gremio: gente que ve amenazado su trabajo y se defiende. Lo interesante es que el diagnóstico de Tao no va por ahí. No dice que la IA sea mala para las matemáticas. Dice que cambió qué es lo escaso, y que nadie ajustó el sistema a eso.

## Math 1.0: cuando demostrar era escaso

Tao llama *Math 1.0* a la forma en que funcionaron las matemáticas durante siglos.

Resolver un problema difícil podía llevar años. A veces una vida entera, o la vida de varios matemáticos encadenados, cada uno avanzando un poco sobre lo que dejó el anterior. Las demostraciones eran escasas, caras, y dependían de una experiencia humana muy profunda.

Y como eran escasas, todo el sistema se construyó alrededor de ellas. Las publicaciones, los premios, las carreras académicas: **se premiaba a quien encontraba la solución.**

Tenía sentido. Si alguien resolvía un problema que había resistido décadas, era una señal bastante confiable de que entendía ese terreno mejor que nadie. Resolver era una forma indirecta de medir comprensión, y durante siglos esa medida funcionó.

La IA rompió esa escasez. Tao lo llama el paso a una época de *proof abundance* (abundancia de demostraciones). Y ahí aparece un problema viejo con nombre propio: la ley de Goodhart. Cuando una medida se vuelve el objetivo, deja de medir bien.

Resolver problemas era un indicador de comprensión. Si ahora lo optimizás directamente —más problemas resueltos, más rápido—, podés tener resultados sin la comprensión que antes venía pegada. Según Tao, optimizar solo la resolución de problemas pasó a ser *actively harmful* (activamente dañino) para la salud de las matemáticas a largo plazo.

## El faro y el helicóptero

La imagen que más me quedó de la charla es la del faro.

Para Tao, en la matemática pura un problema abierto no es tanto un destino como una guía: *less destinations than guides for exploring the surrounding landscape* (menos destinos que guías para explorar el terreno de alrededor). Intentar llegar a él te obliga a inventar técnicas, a conectar ideas que parecían lejanas, a entender el terreno que lo rodea. Lo que aprendés en el intento —incluso cuando fracasás— muchas veces vale más que la respuesta.

Ahora imaginate que alguien te deja arriba del faro en helicóptero. Tenés la respuesta. Pero te perdiste el viaje, y con él, todo lo que ese viaje le iba a enseñar a tu campo. Tao lo dice con más crudeza: llegar antes de tiempo con herramientas automatizadas corta la exploración de los caminos que nadie tomó, y empobrece el terreno de alrededor.

## El embudo

Hay otra idea de Tao que me parece todavía más útil, porque se puede dibujar.

Él describe una cadena de cinco etapas por las que pasa una demostración:

1. Generarla.
2. Verificarla: comprobar que es correcta.
3. Explicarla: que alguien pueda contar por qué funciona.
4. Publicarla.
5. Canonizarla: que la comunidad la absorba como conocimiento propio, sobre el que otros construyen.

La IA aceleró muchísimo las dos primeras. Las otras tres siguen yendo a ritmo humano. El resultado es un embudo: las demostraciones se generan y se verifican a velocidad de máquina, y se amontonan en el medio esperando a que alguien las entienda. Tao lo llama, con bastante humor, *proof indigestion* (indigestión de demostraciones).

En un ensayo previo a la charla dejó una frase que me parece de las más precisas que leí sobre IA en mucho tiempo:

> *"A proof that no human can properly explain should be viewed as incomplete, even if it has been formally verified."*
>
> (Una demostración que ningún humano puede explicar adecuadamente debería considerarse incompleta, aunque haya sido verificada formalmente.)

Fijate lo fuerte que es eso en una disciplina donde la verificación es todo. Está diciendo que el sello de "correcto" no alcanza. Que si nadie lo entiende, falta algo.

## Software 1.0: cuando escribir código era caro

Ahora cambiá "demostración" por "código" y volvé a leer todo lo anterior.

Durante décadas, escribir código fue lo escaso. Era lento, caro y dependía de gente que sabía hacerlo. Y, igual que en las matemáticas, organizamos toda la industria alrededor de esa escasez: estimamos en horas de desarrollo, medimos cuánto se entrega por sprint, y buena parte de las entrevistas técnicas siguen midiendo si podés resolver un problema en una pizarra.

Producir código que funciona era una buena señal de que alguien entendía el sistema. No perfecta, pero buena.

Incluso cuando alguien copiaba, se notaba. Antes de la IA, el que no entendía lo que estaba haciendo quedaba bastante en evidencia: el fragmento sacado de Stack Overflow no encajaba del todo, había que adaptarlo, y en esa adaptación se veía quién sabía y quién no. Copiar te obligaba, aunque sea un poco, a entender. **Entender el código que uno mismo escribía era moneda corriente.** Casi no hacía falta decirlo.

Esa señal se está rompiendo igual que en las matemáticas. Hoy un modelo te escribe en minutos lo que antes llevaba días, y lo escribe encajado, prolijo, sin costuras que delaten que vos no lo pensaste.

Y la cadena del software tiene la misma forma que la de Tao: escribir, verificar que funcione, entender, integrar al sistema y sostenerlo en el tiempo. La IA aceleró las dos primeras. Las otras tres siguen a la velocidad de una persona.

## Lo que me pasó a mí

Vuelvo a ese día sin tokens, porque ahí está la historia completa.

El trabajo era migrar un sistema legacy enorme, de uso interno, de Java 8 a Java 21. Un sistema muy viejo, por el que pasaron muchas manos a lo largo de los años, cada una dejando su marca. Mal diseñado en varios lugares, y con una característica que cualquiera que trabajó con legacy reconoce: nadie entendía realmente cómo funcionaba. No había documentación confiable del negocio. **La única explicación de las reglas era el propio código**, y el código no era fácil de leer.

En algún momento el equipo empezó a meter IA en cada parte del proceso de desarrollo. A cada uno le dieron una cuota grande de tokens. El plazo era corto para el tamaño del trabajo, y la consigna era avanzar: no había tiempo para sentarse a entender el negocio.

Mi parte fue migrar la capa de JSP a una capa REST. Para quien no trabajó con eso: las JSP eran las pantallas del sistema viejo, y en muchos casos tenían lógica de negocio metida adentro, mezclada con la presentación. Mi trabajo era sacar esa lógica y armar contratos REST limpios para cada operación.

Y avancé bastante. Iba rápido. Me apoyaba en documentación que generaba la IA a partir del código, y en base a eso definía cada contrato. Leía poco código. Los contratos salían, los endpoints respondían, el avance se veía.

Lo que no se veía es que yo no sabía qué hacían. Armaba el contrato de una operación sin entender del todo qué validaciones atravesaba, por qué existían, qué caso del negocio cubrían.

Hay algo incómodo que tengo que admitir acá. En un artículo anterior escribí que un documento generado y no verificado es peor que no tener nada, porque después le vas a creer. Lo sigo pensando. Y aun así, con el apuro, me apoyé en documentación generada sin contrastarla contra el código. No porque no supiera que estaba mal, sino porque el ritmo empuja para ese lado y es muy fácil dejarse llevar.

Entonces se terminaron los tokens.

Con el mismo plazo y sin querer bajar el rendimiento, tuve que trabajar a la antigua. Abrir los archivos. Seguir una operación de punta a punta. Leer cada validación y preguntarme qué protegía. Y en ese proceso pasó algo que no esperaba: empecé a ver con claridad todo lo que no sabía. Mi desconocimiento dejó de ser una niebla y pasó a tener forma. Esta validación no la entiendo. Este caso no sé de dónde sale. Esta regla parece contradecir a esta otra.

No fue rápido. Pero de golpe podía explicar lo que hacía el sistema. Podía anticipar qué se iba a romper. Podía responder preguntas que antes tenía que derivar a la IA.

Y eso me hizo destacar un poco. No por algo extraordinario: solo porque entendía el código.

Hoy, cuando alguien necesita saber algo de ese módulo, me pregunta a mí. Y tengo respuestas rápidas. No solo porque generé mucha documentación con IA, sino porque lo tengo en la cabeza. La documentación está ahí para cualquiera que la quiera leer; la comprensión, no. Esa la tuve que construir leyendo, despacio, una validación por vez.

Eso es lo que más me quedó dando vueltas. Lo que me diferenció fue algo que, hasta hace poco, era lo mínimo esperable de cualquier desarrollador.

## El que aprueba permisos

Desde entonces empecé a prestar atención a cómo trabajamos alrededor mío, y a cómo trabajo yo.

Hay un modo de usar la IA que se volvió muy común: le pedís algo, el asistente propone un cambio, te pide permiso para seguir, aprobás, propone otro, aprobás. El desarrollador queda en el rol de vigilante: mira que no se rompa nada evidente y hace clic para avanzar. No se detiene en los detalles que la IA no contempla. No nota las decisiones que la IA toma por él, porque vienen bien argumentadas y funcionan.

Es el helicóptero de Tao, versión software. Llegás arriba del faro con cada tarea resuelta. Lo que no tenés es el terreno de alrededor.

Y el problema no aparece en el momento. Aparece cuando algo falla en producción y nadie del equipo sabe explicar por qué el sistema hace lo que hace. Cuando el negocio cambia una regla y hay que encontrar dónde vive. Cuando un incidente pide una decisión rápida y la única fuente de verdad es un código que todos ayudaron a generar y nadie leyó.

Es la indigestión de Tao, pero con pull requests: código que se genera y se verifica a velocidad de máquina, y se amontona esperando una comprensión que no llega.

## Que pase los tests no alcanza

Acá también tengo que revisar algo que escribí.

En *No dejo que la IA piense* defendí que los tests son el contrato: la forma automática de saber si lo que se construyó hace lo que especifiqué. Lo sigo pensando. Pero Tao agrega un matiz que me ordenó la cabeza: **la verificación es necesaria, no suficiente.**

Un test te dice que el código hace lo que pediste. No te dice si alguien entiende por qué lo hace así, qué se descartó en el camino, ni qué se rompe si cambia el negocio. Y en un sistema legacy como el que migraba, el problema es todavía más profundo: si no entendés el negocio, tampoco sabés qué tests escribir. Podés verificar con mucha prolijidad algo que está mal.

En ese mismo artículo también dije que el código es ganado, no mascota: que los bordes de un sistema se pueden regenerar y tirar sin drama. Puede parecer que esto lo contradice, pero creo que lo completa. Un adaptador que habla con un proveedor externo lo podés regenerar mil veces. El dominio —las reglas del negocio convertidas en código— no. Y fijate que lo que yo estaba migrando eran justamente reglas de negocio escondidas en pantallas viejas. Ahí, un código que nadie entiende es exactamente la demostración incompleta de Tao: puede estar bien, pero nadie puede asegurarlo, ni cambiarlo sin miedo.

## El viaje también existe en el software

La metáfora del faro me pegó por otro lado también.

Cuando acompañé a un grupo en una aceleración de Alkemy, antes de mostrarles Spring los hice armar una app Java en capas a mano, instanciando todo ellos mismos. Cuando llegó el framework, no fue magia: fue alivio, porque ya habían sufrido el problema que venía a resolver. Ese recorrido era el faro. Si les hubiera dado el proyecto de Spring armado desde el primer día, habrían tenido la respuesta sin el viaje.

Con la IA pasa lo mismo, a otra escala. Un desarrollador que nunca tuvo que pelearse una tarde entera con un bug recibe la solución en segundos. Tiene la respuesta. Lo que no tiene es todo lo que ese bug lo iba a obligar a entender del sistema que lo rodea. Y es justamente eso lo que después le permite darse cuenta de que una solución que "anda" está mal.

Lo viví en carne propia sin buscarlo. Quedarme sin tokens me obligó a caminar hasta el faro, y en el camino aprendí más del sistema que en todas las semanas anteriores.

No digo que haya que trabajar sin IA. La uso todos los días, y en esa misma migración me ahorró muchísimo trabajo mecánico. Digo que conviene elegir a conciencia cuándo querés el helicóptero y cuándo necesitás caminar. Y que si nunca caminás, en algún momento vas a estar parado arriba de un faro sin saber cómo llegaste ni cómo bajar.

## Lo que propone Tao, traducido

Tao no se queda en la queja. Propone cosas concretas para la *Math 2.0*, y varias tienen un equivalente directo en lo que hacemos.

*Open exposition problems* (problemas de exposición abiertos). Tao propone tratar como problema valioso no solo demostrar algo, sino explicar bien una demostración opaca que ya existe. En software, eso es entender y documentar un sistema legacy que funciona y nadie entiende. Durante años fue el trabajo menos glamoroso del oficio. Creo que se va a volver de los más valiosos.

*Bibliotecas formales.* Propone construir grandes bibliotecas de matemática verificada por máquina, como Mathlib, donde cada resultado queda formalizado y conectado con los demás. El equivalente más cercano que tenemos es un dominio bien modelado, con sus reglas escritas en tests que explican el negocio. No una documentación que se pudre, sino conocimiento que se ejecuta.

*Evaluaciones transparentes.* Pide que se informe cuántos recursos se usaron y cuáles fueron los resultados negativos, no solo los éxitos. En equipos de desarrollo, sería medir algo más que velocidad: cuánto del código que sale alguien puede explicar, cuántos incidentes vienen de partes que nadie entiende.

## Software 2.0: el recurso escaso cambió

Si producir demostraciones deja de ser escaso, dice Tao, lo valioso pasa a ser entenderlas, conectarlas y decidir qué preguntas vale la pena hacerse.

En el software, el corrimiento es el mismo. Si escribir código deja de ser escaso, lo valioso pasa a ser entender el sistema, explicar por qué está hecho así y decidir qué vale la pena construir.

Eso cambia qué significa ser bueno en esto. El que más líneas produce deja de destacarse, porque eso ya lo hace una máquina. Se destaca el que entiende el negocio, el que puede explicar un diseño en dos minutos, el que sabe qué no construir, el que cuando algo se rompe sabe dónde mirar.

Tomarse el tiempo de entender algo despacio era lo normal. Hoy se volvió raro, y por eso mismo vale más. No creo que sea una habilidad nueva. Es una habilidad vieja que se está volviendo escasa, y lo escaso es lo que te diferencia.

La última diapositiva de la charla de Tao lo resume así:

> *"The primary bottleneck to achieving 'Math 2.0' is not technology, or even institutional culture. It is imagination."*
>
> (El principal cuello de botella para llegar a *Math 2.0* no es la tecnología, ni siquiera la cultura de las instituciones: es la imaginación.)

En el software creo que pasa algo parecido. Las herramientas ya están, y son increíbles. Lo que nos falta es cambiar qué premiamos: dejar de medir solo cuánto código sale y empezar a valorar cuánto de ese código entendemos.

A mí me lo enseñó un mes en el que me quedé sin tokens el primer día. Ojalá a vos no te haga falta.
