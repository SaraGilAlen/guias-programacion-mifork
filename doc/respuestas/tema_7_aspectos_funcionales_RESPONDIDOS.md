<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta
Un **puntero a función** es una variable que almacena la dirección de memoria de una función, de forma análoga a como un puntero normal almacena la dirección de una variable. Esto permite pasar funciones como parámetros, almacenarlas en estructuras o invocarlas indirectamente. En C, el tipo del puntero a función debe coincidir exactamente con la firma de la función a la que apunta (tipo de retorno y tipos de parámetros).

El uso de punteros a funciones es habitual cuando se desea aplicar programación más flexible o genérica, por ejemplo, para implementar callbacks o seleccionar dinámicamente qué función ejecutar en tiempo de ejecución. En el contexto de “aspectos funcionales”, esto se acerca a la idea de tratar funciones como valores, aunque en C no se llega al nivel de lenguajes funcionales modernos.

A continuación se muestra un ejemplo donde se define una función que recibe una cadena (`char*`) y la transforma a mayúsculas. Luego se crea un puntero a esa función llamado `aMayusculas` y se utiliza para invocarla:

```c
#include <stdio.h>
#include <ctype.h>

// Función que convierte una cadena a mayúsculas
char* convertirAMayusculas(char* cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper(cadena[i]);
    }
    return cadena;
}

int main() {
    char texto[] = "Hola Mundo";

    // Definición del puntero a función
    char* (*aMayusculas)(char*) = convertirAMayusculas;

    // Invocación de la función a través del puntero
    char* resultado = aMayusculas(texto);

    printf("Resultado: %s\n", resultado);

    return 0;
}
```

En este ejemplo, `aMayusculas` es un puntero a una función que recibe un `char*` y devuelve un `char*`. La invocación se realiza como si fuese una función normal, lo que facilita su uso sin necesidad de sintaxis adicional complicada.


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta
Una **función lambda** es una función anónima (sin nombre) que se puede definir de forma inline y asignar a una variable o pasar como argumento. Se utiliza para expresar comportamiento de forma concisa, especialmente cuando la función es sencilla y se usa en un único lugar. En lenguajes modernos, las lambdas permiten tratar las funciones como valores, algo similar a lo que se conseguía en C con punteros a función, pero con una sintaxis más compacta y segura.

Desde el punto de vista conceptual, una lambda encapsula lógica sin necesidad de declarar una función completa. En programación funcional, esto es clave, ya que se facilita la composición de funciones, el uso de funciones de orden superior y un estilo más declarativo. En JavaScript y Java (a partir de Java 8), las lambdas son una forma habitual de trabajar con colecciones, callbacks y transformaciones de datos.

Ejemplo en **JavaScript** usando una variable local `aMayusculas`:

```javascript
// Función lambda que convierte una cadena a mayúsculas
let aMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

// Invocación
let resultado = aMayusculas("Hola Mundo");
console.log(resultado);
```

Ejemplo en **Java** usando `Function<String, String>`:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        // Función lambda asignada a la variable
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        // Invocación
        String resultado = aMayusculas.apply("Hola Mundo");
        System.out.println(resultado);
    }
}
```

En ambos casos, la variable `aMayusculas` actúa como referencia a una función. En JavaScript se invoca directamente como una función, mientras que en Java se utiliza el método `apply` definido en la interfaz funcional `Function`.


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta
El **paradigma funcional** es un estilo de programación en el que el objetivo principal es evaluar funciones y evitar el uso de estados mutables y efectos secundarios. En este enfoque, los programas se construyen mediante la composición de funciones, donde cada función recibe datos de entrada y produce una salida sin modificar variables externas. Esto contrasta con el paradigma imperativo (como en C), donde se describen paso a paso las instrucciones que cambian el estado del programa.

Entre las características más importantes del paradigma funcional destacan la **inmutabilidad de los datos**, el uso de **funciones puras** (que siempre producen el mismo resultado para los mismos argumentos) y la posibilidad de componer funciones para construir soluciones más complejas. Aunque lenguajes como Java no son puramente funcionales, a partir de Java 8 incorporan elementos funcionales como las expresiones lambda y las interfaces funcionales, lo que permite adoptar parcialmente este estilo.

Se dice que lenguajes como Java 8 son **multi-paradigma** porque permiten utilizar distintos estilos de programación dentro del mismo lenguaje. Es posible programar utilizando programación orientada a objetos (clases, herencia, polimorfismo), pero también incorporar técnicas funcionales (lambdas, streams, funciones de orden superior). Esto ofrece mayor flexibilidad al desarrollador, que puede elegir el enfoque más adecuado según el problema.

Finalmente, el concepto de que las funciones son **“ciudadanos de primera clase”** significa que las funciones se pueden tratar como cualquier otro valor del programa. Es decir, se pueden almacenar en variables, pasar como argumentos a otras funciones y devolver como resultado. En C esto se logra con punteros a función, mientras que en Java y JavaScript se hace de forma más natural mediante lambdas y referencias a funciones.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
En Java, una **función lambda** es una forma concisa de definir una implementación de una **interfaz funcional** (una interfaz con un solo método abstracto). Su sintaxis básica elimina la necesidad de escribir una clase anónima completa, permitiendo centrarse únicamente en los parámetros y en la expresión o bloque de código que constituye el comportamiento.

La forma general de una lambda es:

```java
(parámetros) -> expresión
```

o, si el cuerpo tiene más de una instrucción:

```java
(parámetros) -> {
    // varias instrucciones
    return valor;
}
```

Los **parámetros** se escriben de forma similar a los de un método, pero habitualmente se omiten los tipos porque el compilador los infiere. El operador `->` separa la lista de parámetros del cuerpo de la función. Si el cuerpo consiste en una sola expresión, no es necesario usar llaves `{}` ni la palabra clave `return`, ya que el valor se devuelve automáticamente.

Por ejemplo, utilizando la interfaz `Function<String, String>`, la sintaxis básica sería:

```java
Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();
```

En este caso, `cadena` es el parámetro de entrada y `cadena.toUpperCase()` es la expresión que se evalúa y se devuelve. Si se quisiera una versión más explícita o con varias instrucciones, se podría escribir:

```java
Function<String, String> aMayusculas = (String cadena) -> {
    String resultado = cadena.toUpperCase();
    return resultado;
};
```

Esta sintaxis permite escribir código más compacto y legible, especialmente cuando se trabaja con colecciones o se pasa comportamiento como argumento, lo cual es habitual en estilos de programación funcional dentro de Java.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
Para recibir una función como parámetro y utilizarla dentro de otro método, se recurre a la idea de **funciones de orden superior**, es decir, funciones que trabajan con otras funciones. Esto es característico del paradigma funcional y permite desacoplar el comportamiento de la lógica principal, haciendo el código más reutilizable y flexible. En este caso, el método `transformar` recibe una cadena y una función transformadora, y aplica dicha función internamente.

En **JavaScript**, esto resulta muy natural, ya que las funciones son ciudadanos de primera clase. Se puede pasar la función como argumento y ejecutarla directamente dentro del método `transformar`. La función transformadora (por ejemplo, `aMayusculas`) se comporta igual que cualquier otro valor:

```javascript
// Función lambda
let aMayusculas = (cadena) => cadena.toUpperCase();

// Función de orden superior
function transformar(texto, funcionTransformadora) {
    return funcionTransformadora(texto);
}

// Uso
let resultado = transformar("Hola Mundo", aMayusculas);
console.log(resultado);
```

En **Java**, el mismo concepto se implementa utilizando interfaces funcionales, como `Function<String, String>`. El método `transformar` recibe tanto el `String` como la función, y la ejecuta mediante el método `apply`:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        String resultado = transformar("Hola Mundo", aMayusculas);
        System.out.println(resultado);
    }

    public static String transformar(String texto, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(texto);
    }
}
```

En ambos casos, el método `transformar` queda completamente desacoplado del tipo específico de transformación, lo que permite reutilizarlo con cualquier función que cumpla la misma firma. Este patrón es fundamental en programación funcional, ya que facilita la composición de comportamiento y mejora la modularidad del código.


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta
En este caso se aplica el mismo concepto de función de orden superior, pero en lugar de reutilizar una función previamente definida (como `aMayusculas`), se define la función lambda **directamente en la llamada** a `transformar`. Esto es habitual cuando la lógica es simple y no se va a reutilizar en otros lugares, permitiendo escribir código más compacto.

En **JavaScript**, la lambda se puede escribir inline como segundo argumento de la función. Para invertir una cadena, se puede convertir en array, invertirlo y volver a unirlo en una cadena:

```javascript
function transformar(texto, funcionTransformadora) {
    return funcionTransformadora(texto);
}

// Invocación con lambda inline
let resultado = transformar("Hola Mundo", (cadena) => {
    return cadena.split("").reverse().join("");
});

console.log(resultado);
```

En **Java**, el mismo enfoque se logra utilizando una expresión lambda directamente en la llamada al método. Se sigue utilizando `Function<String, String>`, y dentro de la lambda se implementa la lógica de inversión de la cadena:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        String resultado = transformar("Hola Mundo", cadena -> {
            return new StringBuilder(cadena).reverse().toString();
        });

        System.out.println(resultado);
    }

    public static String transformar(String texto, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(texto);
    }
}
```

Este enfoque refuerza la idea de que el comportamiento se puede definir “en el momento de uso”, sin necesidad de declarar funciones auxiliares. Esto es especialmente útil en programación funcional, donde se busca expresar transformaciones de datos de forma directa y concisa.


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta
Un **cierre** o *closure* es una característica por la cual una función (incluida una lambda) puede **acceder a variables del contexto donde fue definida**, incluso aunque ese contexto ya no esté activo. Es decir, la función “captura” esas variables externas y las mantiene disponibles para su uso. Este comportamiento es muy común en programación funcional y permite crear funciones más flexibles y expresivas.

En Java, las lambdas también implementan cierres, pero con una restricción importante: solo pueden acceder a variables locales que sean **finales o efectivamente finales** (es decir, que no cambien después de su inicialización). Esto se debe a cómo Java gestiona internamente las lambdas, garantizando consistencia y evitando problemas con concurrencia o cambios de estado inesperados.

A continuación se muestra una modificación del ejemplo anterior, donde se define una variable local fuera de la lambda y esta la utiliza para concatenarla a la cadena de entrada:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        String sufijo = "!!!";  // Variable local (efectivamente final)

        // Lambda que usa la variable externa (closure)
        Function<String, String> concatenar = cadena -> cadena + sufijo;

        String resultado = transformar("Hola Mundo", concatenar);
        System.out.println(resultado);
    }

    public static String transformar(String texto, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(texto);
    }
}
```

En este ejemplo, la lambda asignada a `concatenar` **captura la variable `sufijo`** del contexto donde fue definida. Aunque `sufijo` no es un parámetro de la función, se puede utilizar dentro de la lambda gracias al mecanismo de cierre. Esto permite construir funciones que dependen parcialmente de su entorno, facilitando la reutilización de lógica y la composición de comportamiento.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta
Las **funciones lambda** y los **punteros a función en C** comparten la idea de permitir tratar funciones como valores, pero difieren significativamente en su nivel de abstracción y capacidades. Un puntero a función en C es simplemente una dirección de memoria que apunta a código ejecutable; no tiene contexto asociado ni capacidad de “recordar” el entorno en el que se definió la función. Por tanto, su uso es más limitado y cercano al nivel bajo del lenguaje.

En cambio, una función lambda en lenguajes como Java o JavaScript es una construcción de más alto nivel. No solo permite definir funciones de forma anónima y concisa, sino que además puede **capturar variables del entorno**, formando un closure. Esto implica que la función no es solo código, sino también una combinación de código + entorno, lo cual amplía enormemente su expresividad y utilidad.

Otra diferencia relevante es la **integración con el sistema de tipos**. En C, los punteros a función requieren una sintaxis compleja y poco intuitiva, y no existe un mecanismo estándar para definir interfaces de comportamiento más allá de la coincidencia exacta de firmas. En Java, en cambio, las lambdas se apoyan en **interfaces funcionales**, lo que permite tiparlas de forma clara y segura, facilitando su uso en colecciones, streams y APIs modernas.

Por último, las lambdas favorecen un estilo más declarativo y funcional, permitiendo escribir código más compacto y cercano al problema que se quiere resolver. Los punteros a función, aunque útiles, no proporcionan por sí mismos este cambio de paradigma, sino que se limitan a ser una herramienta de bajo nivel para invocar funciones indirectamente.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
En este caso se pretende construir una **función que devuelve otra función**, lo cual es una idea clave del paradigma funcional. La función `crearDescuento` recibe un porcentaje y devuelve una nueva función que, al aplicarse sobre una cantidad, calcula el precio con ese descuento. Esto es un ejemplo claro de función de orden superior que **produce comportamiento parametrizado**.

En Java, se puede implementar usando `Function<Double, Double>`. La función devuelta utilizará el porcentaje recibido como parámetro en `crearDescuento`. A continuación se muestra el código con dos descuentos distintos y su aplicación:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        // Crear dos funciones de descuento
        Function<Double, Double> descuento10 = crearDescuento(10.0);
        Function<Double, Double> descuento25 = crearDescuento(25.0);

        // Aplicar descuentos
        double precio = 100.0;

        System.out.println("Precio original: " + precio);
        System.out.println("Con 10% descuento: " + descuento10.apply(precio));
        System.out.println("Con 25% descuento: " + descuento25.apply(precio));
    }

    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return cantidad -> cantidad - (cantidad * porcentaje / 100);
    }
}
```

La **closure** aparece porque la lambda que se devuelve (`cantidad -> ...`) **captura la variable `porcentaje`** definida en el contexto de `crearDescuento`. Aunque el método termina su ejecución, la función devuelta sigue “recordando” el valor de `porcentaje` con el que fue creada. Por eso, `descuento10` y `descuento25` funcionan de forma distinta, aunque ambas proceden de la misma función generadora.

De esta forma, cada llamada a `crearDescuento` genera una nueva función con su propio entorno capturado. Esto permite construir funciones especializadas de manera muy flexible, algo que no es posible directamente con punteros a función en C, ya que estos no mantienen ningún estado asociado.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
Una **interfaz funcional** en Java es una interfaz que define un único método abstracto y que sirve como tipo objetivo para una expresión lambda. Es el mecanismo que permite integrar las funciones lambda dentro del sistema de tipos estático de Java. En otras palabras, cuando se escribe una lambda, en realidad se está proporcionando una implementación del único método abstracto de una interfaz funcional.

Para que una interfaz sea considerada funcional debe cumplir principalmente un requisito: **tener exactamente un método abstracto**. No obstante, puede contener otros elementos sin ningún problema, como métodos `default` (con implementación), métodos `static` o incluso métodos heredados de `Object` (como `toString`, `equals`, etc.). Para indicar explícitamente que una interfaz es funcional, se puede usar la anotación `@FunctionalInterface`, aunque no es obligatoria; el compilador puede inferirlo, pero dicha anotación ayuda a detectar errores.

Un ejemplo típico de interfaz funcional es `Function<T, R>`, que define un único método abstracto `R apply(T t)`. Cuando se escribe una lambda como `cadena -> cadena.toUpperCase()`, el compilador la interpreta como una implementación de ese método `apply`. Esto permite que las lambdas se usen de forma tipada y segura, integrándose con el resto del lenguaje.

En resumen, las interfaces funcionales son la base que permite a Java soportar programación funcional manteniendo su sistema de tipos estático. Gracias a ellas, las funciones lambda no son entidades independientes, sino implementaciones concretas de contratos bien definidos, lo que facilita su uso en APIs, colecciones y programación más declarativa.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
Una **interfaz funcional definida manualmente** permite representar un tipo de función específico dentro del sistema de tipos de Java. En este caso, se desea modelar una transformación de una cadena en otra cadena, por lo que la interfaz `Transformador` tendrá un único método abstracto que reciba un `String` y devuelva un `String`.

Como se explicó anteriormente, el requisito fundamental es que tenga **un solo método abstracto**, para que pueda utilizarse con expresiones lambda. Además, es recomendable utilizar la anotación `@FunctionalInterface`, ya que permite al compilador verificar que se cumple este requisito y evita errores si se añaden métodos abstractos adicionales accidentalmente.

La interfaz se puede definir de la siguiente forma:

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}
```

Una vez definida, esta interfaz puede utilizarse como tipo para variables que referencien funciones lambda. Por ejemplo:

```java
public class Main {
    public static void main(String[] args) {
        Transformador aMayusculas = cadena -> cadena.toUpperCase();

        String resultado = aMayusculas.transformar("Hola Mundo");
        System.out.println(resultado);
    }
}
```

En este ejemplo, la lambda proporciona la implementación del método `transformar`. De esta manera, se consigue un comportamiento equivalente al de `Function<String, String>`, pero utilizando una interfaz definida específicamente para este caso, lo que puede mejorar la legibilidad y expresar mejor la intención del código.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
Para hacer la interfaz funcional más genérica, se pueden utilizar **genéricos (generics)** en Java. Esto permite definir una interfaz que no esté limitada a `String`, sino que pueda transformar cualquier tipo `T` en otro tipo `R`. Esta generalización sigue el mismo patrón que la interfaz estándar `Function<T, R>`, pero definida de forma personalizada.

La interfaz `Transformador` genérica se puede definir así:

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T entrada);
}
```

En este caso, `T` representa el tipo de entrada y `R` el tipo de salida. Gracias a los genéricos, se puede reutilizar esta misma interfaz para múltiples transformaciones sin necesidad de crear una nueva para cada tipo de dato. Esto mejora la reutilización y hace el código más flexible y expresivo.

A continuación se muestra un ejemplo de uso creando un transformador que convierte un `Double` en un `Integer`, redondeando el valor:

```java
public class Main {
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear = valor -> (int) Math.round(valor);

        Double numero = 3.7;
        Integer resultado = redondear.transformar(numero);

        System.out.println("Resultado: " + resultado);
    }
}
```

En este ejemplo, la lambda implementa la lógica de redondeo y se asigna a una variable tipada como `Transformador<Double, Integer>`. De esta forma, se demuestra cómo una interfaz funcional genérica permite definir funciones reutilizables para distintos tipos, manteniendo la seguridad de tipos propia de Java.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
En Java ya existen varias **interfaces funcionales predefinidas** dentro del paquete `java.util.function`, pensadas precisamente para cubrir los casos más comunes de programación funcional. Estas interfaces evitan tener que definir manualmente otras como `Transformador<T, R>`, ya que proporcionan nombres estándar y bien conocidos para operaciones habituales como transformar, consumir o evaluar valores.

Algunas de las más importantes son:

*   **Function\<T, R>**: representa una función que recibe un valor de tipo `T` y devuelve otro de tipo `R`.

```java
Function<String, Integer> longitud = s -> s.length();
```

*   **Predicate<T>**: representa una función que recibe un valor y devuelve un booleano (condición).

```java
Predicate<Integer> esPar = n -> n % 2 == 0;
```

*   **Consumer<T>**: representa una operación que recibe un valor pero no devuelve nada.

```java
Consumer<String> imprimir = s -> System.out.println(s);
```

*   **Supplier<T>**: representa una función que no recibe parámetros y devuelve un valor.

```java
Supplier<Double> aleatorio = () -> Math.random();
```

Estas son las más básicas, pero también existen variantes especializadas para trabajar con varios parámetros o tipos primitivos, como:

*   **BiFunction\<T, U, R>**: recibe dos parámetros y devuelve un resultado.

```java
BiFunction<Integer, Integer, Integer> suma = (a, b) -> a + b;
```

*   **UnaryOperator<T>**: es un caso particular de `Function<T, T>` (mismo tipo de entrada y salida).

```java
UnaryOperator<Integer> cuadrado = x -> x * x;
```

*   **BinaryOperator<T>**: opera sobre dos valores del mismo tipo.

```java
BinaryOperator<Integer> max = (a, b) -> Math.max(a, b);
```

En conjunto, estas interfaces cubren la mayoría de necesidades habituales en programación funcional dentro de Java. Gracias a ellas, se consigue un código más estandarizado, legible y compatible con APIs modernas como Streams, evitando tener que crear interfaces propias salvo en casos muy específicos donde se quiera expresar mejor la intención del dominio.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta
El método `forEach` de la interfaz `List` permite recorrer una colección siguiendo un estilo más **declarativo y funcional** que el bucle `for` tradicional. En lugar de especificar explícitamente el índice o el control de la iteración, se indica simplemente qué acción se quiere aplicar a cada elemento. Internamente, `forEach` recibe una función (en este caso, un `Consumer<T>`) que se ejecuta para cada elemento de la lista.

Este enfoque se basa en el uso de funciones lambda, lo que permite expresar la lógica de forma más compacta y legible. En lugar de escribir varias líneas de control de flujo, se define directamente el comportamiento a aplicar. Esto es especialmente útil cuando se realizan operaciones simples sobre los elementos de una colección, como filtrados, transformaciones o mensajes.

A continuación se muestra un ejemplo donde se recorre una lista de enteros y se imprime un mensaje cuando el número es positivo:

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-3, 5, 0, 8, -1, 2);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println(n + " es positivo");
            }
        });
    }
}
```

En este código, la lambda `n -> { ... }` actúa como función que se aplica a cada elemento de la lista. Este estilo evita la necesidad de usar un bucle `for` explícito y acerca el código al paradigma funcional, donde se describe **qué hacer con los datos**, en lugar de **cómo recorrerlos paso a paso**.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
En la firma de `forEach`, se utiliza `Consumer<? super T>` en lugar de `Consumer<T>` para permitir una mayor **flexibilidad en el uso de tipos genéricos**. El uso de `? super T` indica que se acepta un `Consumer` de cualquier tipo que sea **superclase de `T`**, no necesariamente exactamente `T`. Esto permite, por ejemplo, pasar un `Consumer<Object>` a un `List<String>`, ya que un consumidor capaz de procesar objetos también puede procesar cadenas. Si se usara `Consumer<T>`, esta posibilidad no existiría y el sistema sería más restrictivo.

Esta idea se resume en el principio **PECS** (“**Producer Extends, Consumer Super**”), que es una regla práctica para trabajar con genéricos en Java. Según este principio:

*   **Producer Extends (`? extends T`)**: se usa cuando una estructura produce valores de tipo `T` (solo lectura).
*   **Consumer Super (`? super T`)**: se usa cuando una estructura consume valores de tipo `T` (escritura).

En el caso de `forEach`, se está pasando una función que **consume** elementos de la lista (los recibe como entrada), por eso se utiliza `? super T`. De esta forma, cualquier consumidor que sea capaz de manejar un tipo más general también será válido, aumentando la reutilización del código.

Este mismo principio se puede aplicar para mejorar el método `transformar`. En lugar de usar exactamente `Function<T, R>`, se puede hacer más flexible utilizando genéricos con bounds:

```java
public static <T, R> R transformar(T valor, Function<? super T, ? extends R> funcionTransformadora) {
    return funcionTransformadora.apply(valor);
}
```

Aquí se aplica PECS:

*   `? super T`: la función puede aceptar un tipo más general que `T` como entrada (consume `T`).
*   `? extends R`: la función puede devolver un subtipo de `R` (produce `R`).

De esta forma, el método `transformar` se vuelve más genérico y reutilizable, permitiendo trabajar con una mayor variedad de funciones sin perder seguridad de tipos.


## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Las **referencias a métodos** permiten tratar un método existente como si fuese una función, sin necesidad de envolverlo explícitamente en una lambda. Esto encaja con la idea de programación funcional, ya que se puede pasar comportamiento reutilizando métodos ya definidos. Tanto en JavaScript como en Java, es posible obtener una referencia a un método de un objeto y almacenarla en una variable para invocarla posteriormente.

En **JavaScript**, los métodos son simplemente funciones asociadas a objetos, por lo que se pueden asignar directamente a variables. Sin embargo, es importante tener en cuenta el contexto (`this`), ya que al separar el método del objeto puede perderse. Para evitar problemas, se puede usar `bind` para fijar el contexto:

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        return "Hola, soy " + this.nombre;
    }
}

// Código principal
let p = new Persona("Sara");

// Referencia al método (fijando el contexto)
let refSaludar = p.saludar.bind(p);

// Invocación
console.log(refSaludar());
```

En **Java**, las referencias a métodos se introdujeron en Java 8 y se expresan con el operador `::`. Estas referencias son compatibles con interfaces funcionales. En este caso, se puede usar `Supplier<String>` porque el método `saludar` no recibe parámetros y devuelve un `String`:

```java
import java.util.function.Supplier;

class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public String saludar() {
        return "Hola, soy " + nombre;
    }
}

public class Main {
    public static void main(String[] args) {
        Persona p = new Persona("Sara");

        // Referencia al método
        Supplier<String> refSaludar = p::saludar;

        // Invocación
        System.out.println(refSaludar.get());
    }
}
```

En ambos ejemplos, se observa que el método `saludar` se utiliza como un valor que se puede asignar a una variable y ejecutar posteriormente. Esto refuerza la idea de funciones como ciudadanos de primera clase, facilitando la reutilización y la composición del comportamiento.


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
En Java existen varios tipos de **referencias a método**, que permiten reutilizar métodos existentes como si fueran funciones, siempre que su firma sea compatible con una interfaz funcional. Estas referencias son una forma más compacta y legible que usar lambdas cuando simplemente se quiere delegar la llamada a un método ya definido. Se expresan con el operador `::` y se pueden clasificar en cuatro tipos principales.

El primer tipo es la **referencia a método estático**, donde se hace referencia a un método de clase. En este caso, no se necesita una instancia:

```java
import java.util.function.Function;

class Util {
    public static int doble(int x) {
        return x * 2;
    }
}

Function<Integer, Integer> f = Util::doble;
System.out.println(f.apply(5)); // 10
```

El segundo tipo es la **referencia a método de instancia de una instancia concreta**, donde se fija un objeto específico sobre el que se invocará el método:

```java
import java.util.function.Supplier;

class Persona {
    private String nombre;
    public Persona(String nombre) { this.nombre = nombre; }
    public String saludar() { return "Hola, soy " + nombre; }
}

Persona p = new Persona("Sara");
Supplier<String> s = p::saludar;
System.out.println(s.get());
```

El tercer tipo es la **referencia a método de instancia sobre cualquier instancia de un tipo**, donde la instancia se recibe como parámetro implícito:

```java
import java.util.function.Function;

Function<String, Integer> longitud = String::length;
System.out.println(longitud.apply("Hola")); // 4
```

Por último, la **referencia a constructor** permite crear objetos utilizando una interfaz funcional:

```java
import java.util.function.Function;

Function<String, Persona> creador = Persona::new;
Persona p2 = creador.apply("Luis");
System.out.println(p2.saludar());
```

En conjunto, estos cuatro tipos de referencias permiten trabajar de forma más expresiva y concisa, reutilizando métodos existentes sin necesidad de escribir lambdas equivalentes.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
Para ordenar una lista de objetos `Persona` utilizando un enfoque funcional, se puede emplear `Collections.sort` junto con un **comparador definido mediante una lambda**. En este caso, el criterio de ordenación es doble: primero por edad (ascendente) y, si dos personas tienen la misma edad, por nombre en orden alfabético. Esto se puede implementar manualmente o aprovechando utilidades de la clase `Comparator`.

Primero se define la clase `Persona`:

```java
class Persona {
    private String nombre;
    private int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }

    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}
```

### 1. Versión con comparación manual

En esta versión, se escribe explícitamente la lógica de comparación dentro de la lambda:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Ana", 30),
            new Persona("Luis", 25),
            new Persona("Carlos", 30),
            new Persona("Bea", 25)
        );

        Collections.sort(personas, (p1, p2) -> {
            if (p1.getEdad() != p2.getEdad()) {
                return Integer.compare(p1.getEdad(), p2.getEdad());
            } else {
                return p1.getNombre().compareTo(p2.getNombre());
            }
        });

        personas.forEach(System.out::println);
    }
}
```

En este caso, la lambda implementa toda la lógica: primero compara las edades y, si son iguales, compara los nombres. Aunque es clara, esta forma puede volverse más compleja si el criterio crece.

### 2. Versión usando `Comparator`

Java proporciona utilidades en `Comparator` que permiten escribir esta lógica de forma más declarativa y compacta:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Ana", 30),
            new Persona("Luis", 25),
            new Persona("Carlos", 30),
            new Persona("Bea", 25)
        );

        Collections.sort(personas,
            Comparator.comparing(Persona::getEdad)
                      .thenComparing(Persona::getNombre)
        );

        personas.forEach(System.out::println);
    }
}
```

En esta versión, se utiliza `Comparator.comparing` para definir la primera clave de orden (edad) y `thenComparing` para añadir el criterio secundario (nombre). Este estilo es más expresivo y fácil de mantener, ya que separa claramente los criterios de ordenación y aprovecha referencias a método.
