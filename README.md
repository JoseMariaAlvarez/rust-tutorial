# Rust-tutorial

## Variables and mutabilidad

Declaración de una variable cuyo contenido no se puede cambiar y de otra variable cuyo contenido sí se puede cambiar. El tipado de las variables es estático, pero en este caso son declaraciones con tipado implícito. En ambos casos el tipo es `i32`.

~~~{.rust}
    fn main() {
        let x = 5;
        println!("The value of x is: {x}");
        let mut y = 5;
        y = 6;
    println!("The value of y is: {y}");
    }
~~~

Se puede incluir el tipo explícitamente en la declaración. De hecho, a veces es necesario porque el compilador no tiene suficiente información.

~~~{.rust}
    fn main() {
        let x : i32 = 5;
        println!("The value of x is: {x}");
        let mut y: i32 = 5;
        y = 6;
        println!("The value of y is: {y}");
    }
~~~

También se pueden definir constantes:

~~~{.rust}
    const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
~~~

## Ensombrecimiento

La declaración de una variable puede *ensombrecer* u *ocultar* la declaración previa de una variable con el mismo nombre en el mismo ámbito o en un ámbito más externo.

~~~{.rust}
    fn main() {
        let x: i32 = 5;

        let x = String::from("hello");

        {
            let x = 2;
            println!("The value of x in the inner scope is: {x}");
        }

        println!("The value of x is: {x}");
    }
~~~

## Tipo de datos

### Tipos escalares

Tipos enteros: 

|Length	| Signed  | Unsigned|
|-------|---------|---------|
|8-bit	| i8      | u8      |
|16-bit	| i16     | u16     |
|32-bit | i32     | u32     |
|64-bit	| i64     | u64     |
|128-bit| i128    | u128    |
|arch   | isize   |usize    |

Tipos reales: `f32`y `f64`.

Tipo `bool`y `char`.

### Tipos compuestos

Las *tuplas* son colecciones de varios valores de tipos diferentes y de tamaño fijo tras su definición.

~~~{.rust}
    fn main() {
        let tup = (500, 6.4, 1);

        let (x, y, z) = tup;

        println!("The value of y is: {y}");
        println!("The value of first component is: {}", tup.0);
    }
~~~

Los *arrays son colecciones de varios valores del mismo tipo accesibles por índice. Son de tamaño fijo tras su definición.

~~~{.rust}
    fn main() {
        let a = [1, 2, 3, 4, 5];

        let first = a[0];
        let second = a[1];
    }
~~~

Si se accede a una posición del array fuera de sus límites, se produce un error en tiempo de ejecución y se aborta el programa.

## Funciones

Similares a las de Java o C++. Por defecto, se devuelve como resultado de la función el valor de la evaluación de la última expresión de la función, aunque también se puede usar la instrucción `return`.

~~~{.rust}
    fn main() {
        let x = plus_one(5);
        let y = plus_two(5);

        println!("The value of x is: {x}");
        println!("The value of y is: {y}");
    }

    fn plus_one(x: i32) -> i32 {
        x + 1
    }

    fn plus_two(x: i32) -> i32 {
        return x + 2;
    }
~~~

## Estructuras de control

### Sentencia condicional

La sintaxis de la sentencia condicional es muy similar a la de Java/C++:

~~~{.rust}
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
~~~

### Sentencias bucle

El bucle *mientras* también es muy similar al de Java/C++:

~~~{.rust}
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{number}!");

        number -= 1;
    }

    println!("LIFTOFF!!!");
}~~~

El bucle *for* es distinto al tradicional de Java/C++, se parece más al *foreach* de Java:

~~~{.rust}
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }

    // (1..4) genera un intervalo de 1 a 3.
    for number in (1..4).rev() {
        println!("{number}!");
    }
    println!("LIFTOFF!!!");
}
~~~

## Propiedad

La *propiedad* de un valor es la característica más definitoria del lenguaje Rust, en particular en la gestión de la memoria y el acceso seguro a los datos.

### Qué es la propiedad

Las reglas que controlan la propiedad de un valor son:
- Cada valor en Rus tiene un propietario.
- El propietario de un valor es único en cada momento, aunque puede variar a lo largo de la vida del valor.
- Cuando se termina el ámbito del propietario, se libera el valor.

### Asignación de variables con copia

Para entender la propiedad, primero es necesario conocer cómo se copian variables en Rust. Hay que distinguir en primer lugar la copia de los tipos simples (enteros, reales, `char` y `bool`). Para estos tipos, cuando se asignan variables, se crea una nueva copia del valor del tipo correspondiente, por lo que no hay conflictos de propiedad:

~~~{.rust}
    fn main() {
        let x = 5;
        let y = x;

        println!("x = {x}, y = {y}");
    }
~~~

En el ejemplo anterior, hay dos variables de tipo `i32`y ambas estás asociadas a dos valores distintos, es decir, dos copias del valor `5i32`.

Esa regla también se aplica a las tuplas y arrays si, recursivamente, la regla anterior de puede aplicar a los tipos de sus componentes.

### Asignación de variables con movimiento

Hay tipos que no tienen implementada la operación de copia, como el tipo predefinido `String`. Para esos tipos, una asignación de variables significa que se *mueve* la propiedad del valor de la variable que está en la derecha de la asignación a la variable que está a la izquierda de la asignación. A partir de ese momento, la variable a la derecha ya no se puede usar.

~~~{.rust}
    fn main() {
    let s1 = String::from("hello");
    let mut s2 = s1;

    s2.push_str(" world!");

    println!("{s2}");

    }
~~~

Desde la asignación `let mut s2 = s1;`, la variable `s1` ya no es utilizable, porque el valor que tenía en propiedad se ha movido a otra variable.

### Asignación de variables a parámetros de funciones

La regla a la hora de aplicar parámetros reales a parámetros formales de funciones es la misma que en la asigación de variables. Para los tipos con copia (los escalares, tuplas y arrays de elementos copiables), se crea una copia del parámetro real y se le asigna al parámetro formal. Para los tipos sin copia, como `String`, se hace un movimiento del valor y el parámetro formal deja de ser utilizable.

~~~{.rust}
    fn main() {

        let s1 = String::from("hello");
        let x = 5;

        my_function(x, s1);
        println!("{x}");

    }

    fn my_function(i:i32, ss: String){
        println!("{i} {ss}");
    }
~~~

### Referencias. Préstamo de propiedad

La regla de mover un valor e inutilizar una variable cuando se asigna con movimiento es muy restrictiva. Las referencias proporcionan la posibilidad de pedir prestada la propiedad de un valor sin inutilizar una variable.

Se pueden usar referencias en la asignación entre variables o definir un parámetro formal de una función como una referencia. También se puede usar como valor devuelto por una función.

~~~{.rust}
    fn main() {

        let s1 = String::from("hello");

        let s2 = &s1;

        my_function(s2);
        my_function(&s1);

        println!("s1: {s1}");
        println!("s2: {s2}");

    }

    fn my_function(s: & String) {
        println!("my_function: {s}");
    }
~~~

Para pedir prestada la propiedad de un valor con la intención de modificarlo, hay que usar una referencia mutable.

~~~{.rust}
    fn main() {
        let mut s = String::from("hello");

        change(&mut s);
    }

    fn change(some_string: &mut String) {
        some_string.push_str(", world");
    }
~~~

Para evitar condiciones de carrera en el acceso por referencia a un valor, el uso de referencias está restringido según las siguientes reglas:

- Pueden haber múltiples referencias no mutables activas a la vez.
- Una referencia mutable no puede estar activa a la vez que otra referencia, ya sea mutable o no.

### Referencias colgantes

Tener una referencia que apunte a un valor cuyo ámbito se ha borrado y que el compilador ha retirado de memoria es una situación errónea que debemos evitar. Esas referencias son conocidas como *colgantes* o *huérfanas* (traducción del término *dangling*). Una referencia, por tanto, no puede tener una vida más larga que el valor al que apunta. Una posible situación de este tipo, prohibida por el compilador, es que una función devuelva una referencia a una variable creada localmente.

~~~{.rust}
    fn main() {
        let reference_to_nothing = dangle();
    }

    fn dangle() -> &String {
        let s = String::from("hello");

        &s
    }
~~~

### El tipo `slice`

Una `slice` en Rust es una referencia a una secuencia continua de elementos en una colección. Un *slice* se define indicando cuál es el rango al que va a hacer referencia. Para ello, se indica la posición de inicio y la de fin separadas por dos caracteres punto. Si la posición de inicio es la primera (la 0), se puede omitir. Si la posición de final es la última de la variable, también se puede omitir.

## Estructuras

Las estructuras de Rust permiten crear datos con componentes de tipos distintos. La visibilidad de los componentes se puede limitar (que sean públicos o privados). También se pueden definir operaciones para efectuar sobre las variables de un tipo estructura.

~~~{.rust}
    struct User {
        active: bool,
        username: String,
        email: String,
        sign_in_count: u64,
    }

    fn main() {
        let user1 = User {
            active: true,
            username: String::from("someusername123"),
            email: String::from("someone@example.com"),
            sign_in_count: 1,
        };
    }
~~~

## Documentación
[The Rust Book](https://doc.rust-lang.org/book/title-page.html)