Optimizaciones Rust.

Rust utiliza LLVM y toda su infraestructura de opctimizaciones.

A persar de lo dicho durante años con los compiladores ```jit``` la realidad ha
demostrado que las estrategias más efectivas para conseguir el mejor rendimiento,
son las optimizaciones en tiempo de compilación apoyadas en tipado estático.

En este sentido, Rust es fuerte.

Pero tanto en Rust como en C++, hay cuestiones que se dejan al compilador.

Por ejemplo el ```RVO```, hasta hace poco, era una opción del compilador.

En otros veteranos lenguajes, como Pascal, por ejemplo, es algo explícito que
viene de serie. Y por ser explícito es sencillo y fácil de entender.

En C++, saber cuándo y cómo el compilador hace su magia con el ```RVO``` u otras
optimizaciones complejas, dificulta el poder afinar al máximo en rendimiento.

Otro ejemplo

```
let mut a : [i32; 4];
```

Podemos crear una variable sin darle un valor. Podemos dárselo más tarde, pero
en el caso del array, Rust nos pide amablemente que inicialicemos los valores,
por si acaso.

Este código vale...
```
fn main() {
    let mut a = [0i32; 4];
    a[0] = 99;
    println!("{:?}", a[0]);
}
```

Este también...
```
fn main() {
    let mut a : [i32; 4];
    a = [0i32; 4];
    a[0] = 99;
    println!("{:?}", a[0]);
}
```


Este NO vale...
```
fn main() {
    let mut a : [i32; 4];
    //a = [0i32; 4];
    a[0] = 99;
    println!("{:?}", a[0]);
}
```


Este TAMPOCO vale (aunque sea correcto)...
```
fn main() {
    let mut a : [i32; 4];
    for i in 0..3 {
        a[i] = 0;
    }
    a[0] = 9;
    println!("{:?}", a[0]);
}
```


Entonces podemos escribir...
```
fn main() {
    let mut a =  [0i32; 4];
    for i in 0..3 {
        a[i] = i;
    }
    a[0] = 9;
    println!("{:?}", a[0]);
}
```

Pero, aquí nos encontramos con un código desagradable.
Le digo a Rust, que rellene con ceros, y luego yo relleno con un índice.

Perdemos tiempo, gastamos ciclos de CPU, hacemos una cosa que ignoramos.

Podemos evitarlo, podemos hacerlo mejor (pero sigue siendo un poco feo)...

```
use std::mem;

fn main() {
    //let mut a = [0i32; 4];
    let mut a : [i32; 4] = unsafe { mem::uninitialized() };
    for i in 0..3 {
        a[i] = 0;
    }
    println!("{:?}", a[0]);
}
```

Esto tiene mucha mejor pinta, pero tampoco es buena idea.
uninitialized es unsafe, peligroso, delicado. En este caso, es correcto y explícito,
pero no es estilo Rust.


Para ver el rendimiento, no hay como mirar el código generado. En este caso, gracias
a LLVM y su sencillo lenguaje, no requiere mucho esfuerzo.

Compilamos con...

```
rustc -O  --emit=llvm-ir main.rs
```

Si a las optimizaciones de Rust le añadimos las del propio LLVM, veremos como
nos quitan la inicialización que nos sobra.

En otras ocasiones, quitarán la verificación de límites (bounds check).

He incluso pueden quitarnos Rc cuando vean claro que no es necesario (igual que
hace el compañero Swift)

En ocasiones, será Rust quien optimice, y en otras, será LLVM quien haga ese trabajo.

Pero las optimizaciones implícitas, mágicas, nos alejan del control y entender lo que
ocurre.

Volviendo al ejemplo original en C++ con RVO, es fácil hacer un pequeño cambio
en el programa que sin querer y sin darte cuenta, evite que el compilador realice
dicha optimización.

Las optimizaciones con estrategias implícitas, casi mágicas, hacen el lenguaje mucho
más complejo, porque lo que no es una opción, es escribir cualquier cosa y esperar
que el compilador lo arregle todo.

Puesto que nuestra obligación es escribir buen código, al final, esa supuesta ventaja
de "no te preocupes, yo me encargo", es más un inconveniente porque son nuevas reglas
a aprender (y en ocasiones, muy difíciles y cambiantes)
