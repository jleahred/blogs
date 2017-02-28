# No dejes que la inmutabilidad te amargue un buen rendimiento

La inmutabilidad está de moda.
Esto da un nuevo vigor a los lenguajes funcionales.
Y en muchos lenguajes no funcionales, cada vez se valora más la inmutabilidad.

El código es más legible. Esto es importante porque el código se empieza una vez, pero se mantiene durante años.

Los problemas son más fáciles de razonar. Es más difícil tener inconsistencias, evitas race-conditions en programación concurrente y esto supone la puerta al paralelismo.

Además, es más fácil diseñar correctamente con esta restricción.

La inmutabilidad conlleva copias, en ocasiones muchas copias.

Pero los compiladores son listos, muy listos y son capaces de remplazar una copia por cambio insitu (mutación) sin que el diseño y tu código expresen ni padezcan de mutaciones.

Esto es fantástico, ¡¡¡Qué más se puede pedir!!!

Pues quizá sí, si el rendimiento te preocupa mucho, quizá te gustaría representar en el diseño que no quieres que se copie.

Está bien que el compilador en muchos casos evite la copia. Está fenomenal que el programador tenga cuidado para evitar la copia, pero... ¿No sería chulo poder representar en el diseño esto?

De esta forma, evitaríamos que, sea por olvido, descuido o desconocimiento, hagamos una operación que no le permita al compilador modificar in-situ y zás!!!  haga una copia cara.

La mayoría de lenguajes imperativos tiene poco o nulo soporte de inmutabilidad.

Llegamos al extremo terroríficos en los que al pasar valores a una "pseudo-función", esta nos los puede cambiar.

Esto convierte a toda pseudo-función invocada en sospechosa durante la investigación de un fallo.

E incluso más, algunos tipos sí los puede modificar y otros no. Y muchos desarrolladores no son conscientes de ello, ni de cuándo.

La pregunta es fácil. ¿Qué sucede en tu lenguaje imperativo cuando escribes **a = b**?

Podría ser una copia del valor, un puntero dirigido a *b* o incluso un *movimiento*.

Generalmente la respuesta es "depende". Y depende de lo que contenga *b*.

Algunos lenguajes lo explican de forma que parece una justificación de lo injustificable (no, no me has convencido con las "labels" Python, fíjate en sus horribles y sorprendentes consecuencias).

Que los desarrolladores no sean conscientes tiene su lado positivo. Ese susto y pánico que se ahorran.

A costa claro, de los que sí lo saben, que tienen dos razones para morirse de miedo. Una el hecho de que el llamado te pueda modificar lo que le das.  (booooooo!!!!!). La otra razón para no dormir, está en que ven a compañeros que por pura inconsciencia y desconocimiento, te lo pueden modificar.

En C++ todo funciona por copia (bueno, muy recientemente se ha añadido la semántica de movimiento, eso para otro día). Pero ojo, hay valores, referencias y punteros. La copia de un puntero... no protege mucho contra la mutabilidad.

Además la copia de un valor puede contener punteeeeerooooossss...  ya estáaaaan aquíiiiiii!!!!

Pero C++ tiene una característica muy interesante casi exclusiva entre los lenguajes imperativos (carecen de ella, python, ruby, java...). Puedes definir que un valor tiene que ser inmutable. Yuuuuuhuuuuu!!!

En C++ puedes definir parámetros que son referencias constantes a valores. Es decir que no tienen el coste de la copia, pero no los pueden cambiar. Chúpate esa Fredy, hoy duermo tranquilo!!!

El *const* además permite aplicar optimizaciones agresivas al compilador. Esto pinta muy bien.

Pero si algo es const, toda modificación implica una copia y modificación sobre esta. Esta copia además es explícita y difícilmente puede ser eliminada por el compilador.

C++ busca el rendimiento, el uso de *const* permite aplicar optimizaciones interesantes, pero también, obliga a copias con todo su coste.

Esto limita mucho la utilización de *const* en C++

Pero hay otro caso a considerar. Las referencias trajeron soluciones y nuevos problemas.

Por un lado, son punteros, "copia" barata. Pero no tienen aritmética de punteros ni pueden estar sin inicializar (no es fácil y no se hará por error).

Podemos recibir parámetros por referencia, y gracias al **const** garantizamos que no se cambiarán.

Esto es un patrón muy utilizado, pero...

¿Qué sucede si el creador de la librería le quita el **const**?
Peligro!!!! Ahora nos puede cambiar el valor. ¿Y el compilador, que dice de todo esto? Nada!!!

La característica del parámetro pasado la ha decidido el receptor.
Esto no es muy bueno. Pero tampoco terrible, no es un error fácil de cometer, pero es un error al fin y al cabo.

C++ permite expresar la prohibición de copia en tipos que defines. Entonces tendrás que utilizar punteros, listos o tontos y la mutabilidad compartida (de lo más peligroso), será la forma de trabajo.

Y en la programación funcional dicen:

    A mi me da igual.
    No tengo que pensar, no tengo que razonar, no me puedo equivocar en todo ese tipo de cosas.

Y hay gente dice que la programación funcional es más difícil. "¿A dónde?"

Bueno, podríamos aceptar que la programación imperativa es más fácil si el objetivo es hacer algo rápido y aprender en poco tiempo. Lo aceptamos si dejamos de lado que no sabes muy bien lo que haces y probablemente no funcionará correctamente. Lo aceptamos si hablamos código de sólo una escritura y no de lectura. Lo aceptamos si hablamos de código de usar y tirar. Detalles "pequeños".

La mayoría de los imperativos dan más miedo que Fredy. C++ ayuda pero no soluciona. La programación funcional está genial, pero podemos caer sin darnos cuenta en costosas copias.

## ¿Y Rust, que opina de esto?

Rust quiere competir en rendimiento con el más rápido.

Al mismo tiempo quiere ser seguro y evitar toda una familia de errores muy comunes.

Y además quiere ser seguro.

Para ello, pretende que le digas qué quieres y el verificará que lo que estás haciendo, es consistente con dichas definiciones.

Esto no es nuevo. Es una de las razones por las que tenemos sistemas de tipos, especialmente los estáticos.

Entonces yo quiero decirle a Rust, esto no quiero que se copie porque es caro. Además quiero que sea inmutable porque es más fácil de razonar mantener y más difícil cometer errores.

Ja!!!  ¿No estamos pidiendo cosas contradictorias?
Quizá esta sea la razón por la que no se puede hacer en otros lenguajes a pesar de su gran experiencia.

Pero le daremos una oportunidad. Para eso estamos aquí.

En Rust, si no dices lo contrario, todo es inmutable (como Dios manda).

Además, si creas un tipo, por defecto no se puede copiar (como debe ser).

Y como no podría ser de otra forma, aunque permitas que se pueda "clonar", el operador de asignación siempre moverá (a no ser que le indiques tú lo contrario).

Pero claro, como dependiendo del tipo el operador de asignación puede significar copia o movimiento... me podría equivocar. Pero lenguaje permite instruir al compilador para que lo detecte y te avise con un cariñoso "no compilo" cuando te equivoques.

Otro detalle interesante, es que el propietario decide cómo es el tipo, no hay coherciones en este sentido que pueda aplicar el receptor y una opinión diferente de este le lleve a cambiarlo y esto  pase inadvertido para el llamante.

Supongamos que hemos definido un tipo llamado "Expensive". Es caro copiarlo. Así que no le diremos que se puede clonar ni copiar (tacaños nos hemos puesto).

Ahora declaramos una variable de tipo Expensive, que como no decimos nada, es... **INMUTABLE**

```Rust
let prev_exp: Expensive;
```

**Inmutabilidad** y **no copia** de *Expensive* ¿Qué más se puede pedir?.

Pues... ¿que sirva para algo?

Hasta ahora tenemos un te miro pero no te toco. No era esto de lo que hablábamos inicialmente.

Supogamos que *Expensive* es una especie de lista, que puede ser gigante (y por eso no queremos que se pueda copiar ni por accidente ni intencionadamente). Tenemos una función *push* para añadir.

¿Añadir? Si es inmutable. Cierto, cierto, un poco de paciencia.

¿Cómo sería en programación funcional?
Sería algo así:

```Haskell
nw_exp = push pev_exp val
```

Tenemos nuestro *Expensive* inmutable llamado *old_exp*, llamamos a una función para añadir y esta nos devuelve otro *Expensive* que es igual pero con un elemento más. Es otro conceptualmente, aunque el compilador **si puede** evitará la copia y te venderá el mismo diciéndote, aquí tiene su nuevo *Expensive*

Y confiamos en nuestro saber, atención y buen hacer del compilador, para que lo optimice haciendo internamente mutabilidad y no copiando.

Aunque se puede hacer con una notación estilo *OOP*, propongo la notación funcional (es más claro y es un detalle poco relevante).

```Rust
let prev_exp: Expensive;
...
let nw_exp = push(prev_exp, val);
```

No podemos hacer una copia, y es **inmutable**, ¿Estamos en un callejón sin salida?

Nop. Porque podemos hacer esto en la función:

```Rust
fn push (mut exp: Expensive, v: Value) -> Expensive
```

Oigo desde aquí los gritos de ¡¡¡trampa, trampa!!!

Era inmutable, no puedes recibirlo como mutable!!!

Si eso fuera posible, violaría el principio de quien lo define decide. Eso es muy peligroso.

Y efectivamente es así. Estamos violando ese principio. Pero el que yo dije previamente era: "el **propietario** decide". Y eso no ha cambiado.

Está claro que hemos dicho que sea mutable, pero...

    ¿Es una copia?
    ¿Es un paso de puntero?
    O... ¿es un movimiento?

Si fuera una copia, no se podría, lo hemos prohibido.

Y no es un paso de algún tipo de puntero. Para ello el invocante debería informarlo.

Así que es... un **movimiento**

¿Y qué es un **move**?

Es un regalo. Ahora es tuyo, "pa ti pa siempre".

Y el compilador vigila para que no haya trampas. Ni llaves ocultas, ni compartir... si ha cambiado el propietario a cambiado.

¿Y no es razonable que pierdas el control sobre algo que has dado (regalado o vendido)?

¿No es razonable que el nuevo dueño haga con eso lo que quieras?

```Rust
fn push (mut exp: Expensive, v: Value) -> Expensive {
    exp.mutate_with(value);
    exp.last_value = v;
    exp     // this is the return 
}
```
Pues en este caso, el receptor ha decido que sea mutable. Lo muta, y **lo devuelve**, lo vuelve a dar, se deshace de ello.

Con esto se consigue lo mismo que haría automáticamente un compilador de un programa funcional al optimizar, pero hay algo más.

No es una opción, no es una mejora que se puede aplicar a veces. Siempre se aplicará, porque lo hemos definido **explícitamente**

Es más complicado que si programas en funcional sin preocupación por el rendimiento. Si el compilador puede, que lo haga. No lo piensas, no lo razonas.

Pero es más fácil, que si programas en funcional preocupado por el rendimiento. Tienes que pensar igual, tienes que razonar igual, pero en funcional, el compilador no te ayudará a asegurarte de que no te equivocas y generas copias caras.

No le decimos al compilador, apáñatelas **si puedes**.

Le decimos, esto es lo que quiero. Sintáctica y semánticamente es igual, pero en un caso sabemos lo que ocurrirá, y no nos podremos equivocar, el compilador nos vigila.

Esta no es la base de Rust. El "borrowing" es probablemente más rompedor, pero eso para otro día.

Y hay más opciones sobre este esquema. Devolver un tipo algebraico en el que una de las opciones puede ser el *mutado* y definir tipos *mutables opacos* y trabajar estilo *SSA* son dos ejemplos