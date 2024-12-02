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



## Documentación
[The Rust Book](https://doc.rust-lang.org/book/title-page.html)