# Rust, a new hope

    Nos encontramos en un periodo de
    guerra civil. Las naves espaciales
    rebeldes atacando desde una base
    oculta, han logrado su primera
    victoria contra el malvado Imperio
    Galáctico.
    Durante la batalla, los espias
    rebeldes han conseguido apoderarse
    de los planos secretos del arma total
    y definitiva del Imperio: la web,
    una especificación enrevesada  
    con potencial suficiente
    para destruir todos los ordenadores de la galaxia.
    Perseguida por los siniestros agentes
    del Imperio, la princesa Leia vuela
    hacia su patria a bordo de su nave
    espacial llevando consigo los planos
    robados que podrán salvar los ordenadores
    y devolver la libertad a internet…


## Contexto

Los navegadores, renderizadores y máquinas virtuales, se han convertido en grandes monstruos difíciles de controlar.


Todas las tecnologías de navegadores web están en C++

El paso del tiempo y la complejidad de la especificación, han hecho que alcancen tamaños poco manejables.

A esto hay que añadirle que C++, tiene implícito "undefined behaviour". Esta característica, no exclusiva de C++, por supuesto, nos ha provocado no pocos problemas en la historia de la computación.

Programadores de gran nivel, han cometido errores tipo dangling pointer, out of bounds, race conditions...

Esto no sólo ha traído fama a la efectiva receta: "cierra y vuelve a abrir", o "apaga y enciende otra vez".

Los usuarios, lo ven como algo normal y creen ser felices con ello. Excel, word, mi teléfono... parece que no hay programas libres de fallos que los hacen explotar.

En este sentido, la antigua tecnología Erlang, no tiene rival. Y menos si le ponemos una cara renovada con Elixir.

Pero C/C++, los señores de los programas importantes del mundo, juegan en otra liga. La liga del rendimiento, consumir poco, ocupar poco...

Para ello, se sirven de la compilación nativa, tipado estático (sí, el tipado estático es una cuestión de rendimiento, no de corrección, veáse "homeostais del riesgo"), NO al recolector de basura, no a máquinas virtuales, runtimes ligeros...

Las piezas complejas e importantes del software están escritas en C y C++. O al menos sus corazones. Linux, MacOS, Windows kernel, Git, Postgresql, MongoDB, MySQL/MaríaDB, firebird, MicrosoftSQL, Sybase, Oracle, LibreOffice, Excel, Word, Firefox, Chrome, Webkit, V8, Spyder, Python, Ruby, la máquina virtual de Java, la máquina virtual de .NET...

Programas de backend, programas de front, programas de usuario, programas de sistema...

C/C++ is all arround...

Sí, C y C++, son peligrosos. Además C++ es muy complejo. Por eso, es fácil que aparezcan otras tecnologías con buena acogida. C y C++, no son para todo el mundo.

Si eres mediocre en C++, no intentes hacer algo no trivial en producción. El fracaso está garantizado. Por eso otras tecnologías como VisualBasic, Delphi, Java, .NET... se hicieron populares rápidamente.

Pero el tiempo nos ha demostrado, que incluso el software importante de grandes corporaciones, mucho dinero y buenos informáticos, falla.


## Seguridad

Ojalá los fallos se limitaran a que los programas exploten de vez en cuando. Sería triste, es triste, pero no peligroso.

La costumbre nos hace creer que una explosión expontánea sin razón, de un programa o sistema, porque en un determinado y oculto punto del mismo se hace una estadística interna sólo útil para el programador, si es que algún día lo consulta, ES UNA VERGÜENZA.

Sólo Erlang/Elixir parecen haberse preocupado en serio de este punto.

Pero aún hay más. Esos errores que forman parte de nuestro día a día, son peligrosos. Han sido peligrosos siempre.

¿Cómo es posible que al visualizar una imagen se ejecute un programa?

¿Cómo es posible que un programa escale en privilegios ignorando las restricciones del sistema operativo?

¿Cómo es posible modificar el comportamiento de un programa pudiendo espiar, símplemente dándole unos datos "especiales"?

...

Esto es lo que conseguimos con buffer overflow, statck overflow, out of bounds...

No diferenciar datos de código... tiene sus ventajas, y un alto precio.

Si la seguridad, la estabilidad, la disponibilidad... son tus prioridades, Erlang/Elixir son las herramientas.


## La batalla

C y C++ han dejado grandes huecos que han dado oportunidades a otros lenguajes. Algunos de ellos han tenido un éxito considerable.

Como comenté más adelante, VisualBasic, Java y .NET, son lenguajes empujados por grandes empresas que han aprovechado la complejidad de C++ para implantarse con gran éxito.

Pero al hacerlo, crearon otra liga de competición en la que C++ siguió siendo el rey.

C++ en los últimos años (C++11, C++14 y el próximo C++17) está sufriendo una revolución importante. Es un nuevo lenguaje de programación. Mucho más moderno.

Pero los compiladores de C++ siguen siendo extremadamente complejos. C++ sigue siendo enormemente complejo, ahora incluso más. Y todavía hay puntos importantes pendientes de renovar y modernizar...

* Módulos y velocidad de compilación
* Herramientas estándar sencillas
    * Compilación
    * Gestión de dependencias
    * Generación documentación
    * Test
* Mecanismos de concurrencia de alto nivel
* Modelo OOP limitado (Java y .NET no aportan en este punto)
* Complejidad lenguaje
* Seguridad
* Concepts y mejora mensajes error
* Retrospección de código en tiempo de compilación


## Sobrevivir con undefined behaviour

Escribí hace años una librería de utilidades en C++. El objetivo era tener el conjunto de herramientas imprescindibles y generales de C++ con mínimas dependencias o sin dependencias (sí, en C++ no tenemos un sistema de dependencias estándar).

En dicha librería, también me preocupé por hacer software seguro, que no explotara sin dar muchas pistas gracias, es decir, eliminar el "undefine behaviour".

Esto último tiene su coste, coste que pago muy gustosamente. Esta idea, no sólo me permite programar más eficazmente, también permite que otros programadores con menos experiencia aprendan y sobrevivan muy bien con C++.

El balance ha sido extraordinario. Desarrollo eficiente de código que no explota y da pistas para su corrección.

Pero en el mundo, hay otras alternativas...

* Analizadores estáticos de código
    * PVS
    * Coverite
    * cppcheck
    * ...
* Analizadores en tiempo de ejecución
    * Valgrind
    * Codeguard
    * ...


## Los nuevos rivales...

Estos puntos, han dejado huecos que trató de ocupar Ada y posteriormente D. Más recientemente Rust, Go y Swift.

GO propone mejoras en casi todos los puntos anteriores, pero al mismo tiempo supone un cambio significativo en:

* Recolector de basura
    * Monothread y stop the world
* Reducción mecanismos de abstracción

Y se meten en esta liga lenguajes funcionales como Haskell, LISP, OCalm... Con grandes ideas y propuestas.

Pero la programación funcional también pisa fuerte en otras competiciones como Java con Scala, .NET con F#... Pero esta es otra historia. Y nuevamente Erlang/Elixir salen muy reforzados en este punto.


## Rust

Para alguien con años de experiencia en C++, empeñado en eliminar el "undefined behaviour" a base de librerías y verificaciones en tiempo de ejecución... investigar Rust no es interesante, es **obligado**

Rust es un lenguaje moderno, y como tal, viene de serie con:

* Módulos
* Herramientas (test, compilación, gestión dependencias...)
* Concurrencia de alto nivel (está de moda ahora, pero lenguajes tan antiguos como Erlang y Ada ya lo hicieron).
* Mejor OOP
* Tipado algebraico (parcialmente)
* Seguridad


El último punto, es muy importante. Adiós dangling pointers, null pointers, race conditions, out of bounds...

Adios con todas las consecuencias...

* Adiós explosión de programas (sin lógica ni sentido ni traza útil en muchos casos).
* Adiós ataques de virus y troyanos que se aprovechan de estos puntos.

Y todo esto, con un coste en tiempo de ejecución bajo, no como las librerías que he escrito que requieren verificaciones en tiempo de ejecución en estos puntos.

No es que Rust no se pare por errores. De hecho algunos errores siguen siendo fáciles de cometer.

Por ejemplo el "out of bounds" en un vector.

La diferencia, es que este error y otros, no provoca un "undefined behaviour". Provocará una parada con información precisa de lo ocurrido.

    Rust no puede competir en disponibilidad y toleracia a fallos con Erlang/Elixir.

En el ecosistema C/C++ es frecuente utilizar analizadores estáticos de código y analizadores en tiempo de ejecución para detectar **muchos** errores.

Estas herramientas, suelen ser muy caras.

Rust, nos permite reducir sino eliminar casi todos esos errores.

La idea no es fabricar complejísimas herramientas para ello, la idea es empezar de cero y diseñar un lenguaje que nos permita detectar y eliminar estos problemas en tiempo de compilación.

No sabemos si Rust triunfará en este punto, la perspectiva es buena. Sí sabemos que C++ no tiene plan para ello.


## El campo de batalla

Rust es una apuesta muy ambiciosa.

Además, ha decido empezar por una pieza muy compleja, un navegador web.

Por el momento, *servo* (el motor web que se está escribiendo en Rust), está dando mejor rendimiento que Gecko y mejor paralelismo (free lunch is over).

Y no sólo eso, menos líneas de código.



Habrá que seguirlo de cerca, es un rival con mucho potencial.
