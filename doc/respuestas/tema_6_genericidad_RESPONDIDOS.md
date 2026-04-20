<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta
A continuación se muestran ejemplos de cómo puede construirse una **estructura de datos genérica** sin emplear mecanismos de genericidad propiamente dichos, utilizando en su lugar `void*` en C o `Object` en Java. En ambos casos se parte de un **array primitivo** que permite almacenar referencias a cualquier tipo de dato.

***

### Ejemplo en C usando `void*`

En C no existe soporte nativo para genericidad, pero puede simularse usando punteros genéricos (`void*`). Mediante un array de `void*`, es posible almacenar direcciones de memoria que apunten a datos de cualquier tipo. La estructura no conoce el tipo real del dato, por lo que esa responsabilidad recae completamente sobre el programador.

```c
#define MAX 10

typedef struct {
    void* elementos[MAX];
    int tam;
} ListaGenerica;
```

Esta estructura puede almacenar punteros a enteros, reales, estructuras u otros datos. Sin embargo, al recuperar un elemento, es necesario realizar una conversión explícita (cast) al tipo correcto. Esto implica que **no existe comprobación de tipos en tiempo de compilación**, pudiendo provocar errores graves si se realiza un cast incorrecto o se gestiona mal la memoria asociada.

***

### Ejemplo en Java usando `Object`

En Java, todas las clases heredan implícitamente de `Object`. Por este motivo, un array de `Object` puede contener instancias de cualquier clase. Esto permite implementar estructuras de datos “genéricas” sin usar genéricos propiamente dichos, como se hacía en Java antes de la versión 5.

```java
public class ListaGenerica {
    private Object[] elementos;
    private int tam;

    public ListaGenerica(int capacidad) {
        elementos = new Object[capacidad];
        tam = 0;
    }

    public void añadir(Object obj) {
        elementos[tam++] = obj;
    }

    public Object obtener(int i) {
        return elementos[i];
    }
}
```

Este enfoque ofrece más seguridad que en C, ya que Java gestiona la memoria automáticamente. No obstante, al recuperar un elemento es necesario hacer un *casting* explícito al tipo esperado. Si el tipo no coincide, se producirá una excepción en tiempo de ejecución (`ClassCastException`), ya que **el compilador no puede garantizar la corrección de tipos** utilizando únicamente `Object`.

***

Estos ejemplos ilustran las limitaciones clásicas de simular genericidad sin soporte específico del lenguaje. Precisamente para evitar estos problemas de seguridad y legibilidad, lenguajes como Java introdujeron más adelante los **tipos genéricos**, que permiten expresar este tipo de estructuras de forma segura y clara en tiempo de compilación.


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta
La **programación genérica** consiste en definir algoritmos y estructuras de datos de forma **independiente del tipo concreto de datos** con el que van a trabajar, permitiendo reutilizar el mismo código para múltiples tipos sin duplicación. La idea clave es expresar el comportamiento común sin fijar previamente si se operará con enteros, reales, objetos de una clase concreta, etc. En lenguajes con soporte explícito (como Java con genéricos), esta generalidad se expresa mediante **parámetros de tipo**, que el compilador puede comprobar en tiempo de compilación.

Desde un punto de vista conceptual, la programación genérica busca dos objetivos principales: **reutilización de código** y **seguridad de tipos**. Al no ligar el algoritmo a un único tipo, se evita escribir múltiples versiones casi idénticas del mismo código. Además, cuando el lenguaje lo permite, el uso de tipos genéricos hace posible detectar errores de tipo antes de ejecutar el programa, lo que mejora la robustez y la mantenibilidad del software.

El ejemplo anterior basado en `void*` en C o en `Object` en Java **no es programación genérica en sentido estricto**, aunque suele considerarse un antecedente o una aproximación muy básica. En esos casos, el código acepta “cualquier tipo”, pero pierde información sobre dicho tipo, obligando a realizar conversiones explícitas (*casting*) al recuperar los datos. Esto desplaza la responsabilidad de la corrección de tipos al programador y elimina la comprobación en tiempo de compilación.

Por tanto, puede afirmarse que el ejemplo anterior **simula genericidad**, pero no la implementa realmente. La auténtica programación genérica aparece cuando el lenguaje permite expresar los tipos como parámetros formales del código y el compilador puede verificar su uso correcto, como ocurre con los genéricos en Java (`<T>`) o con las plantillas en C++.


## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta
El principal problema respecto al **chequeo de tipos** al emplear `void*` en C o `Object` en Java es que **se pierde la información del tipo real de los datos almacenados**. La estructura de datos acepta cualquier elemento, pero no existe forma de garantizar, en tiempo de compilación, que el tipo del dato recuperado coincida con el tipo esperado. Como consecuencia, los errores de tipo no se detectan de forma temprana y pueden pasar desapercibidos hasta la ejecución del programa.

En el caso de **C con `void*`**, el compilador no realiza ningún tipo de comprobación al convertir un `void*` a otro tipo de puntero. Esto permite flexibilidad, pero también introduce un riesgo elevado de errores graves, como accesos indebidos a memoria, interpretaciones incorrectas del contenido o fallos de segmentación. La responsabilidad del uso correcto de los tipos recae completamente en el programador, sin ningún mecanismo automático que permita verificar la corrección de las conversiones realizadas.

En **Java utilizando `Object`**, aunque existe mayor seguridad gracias a la gestión automática de memoria y del sistema de tipos del lenguaje, el problema persiste. Al recuperar un objeto de una estructura basada en `Object`, es necesario realizar un *casting* explícito al tipo deseado. Si el objeto no es de ese tipo, el error solo se detecta en tiempo de ejecución mediante una `ClassCastException`, lo que puede provocar fallos inesperados durante la ejecución del programa.

En ambos casos, el uso de `void*` u `Object` al crear estructuras de datos genéricas **elimina el chequeo estático de tipos**, afectando negativamente a la seguridad y a la mantenibilidad del código. Estos problemas motivaron la introducción de mecanismos de genericidad reales, que permiten al compilador verificar la coherencia de los tipos desde el principio y reducir significativamente este tipo de errores.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta
Los **parámetros de tipo** son un mecanismo propio de la programación genérica que permite **definir clases, interfaces o métodos sin fijar de antemano el tipo de datos** con el que van a trabajar. En lugar de usar un tipo concreto, se emplea un identificador simbólico (como `T`, `E` o `K`) que representa un tipo que será especificado más adelante, cuando la estructura o el método se utilice. De este modo, el código se escribe una sola vez y puede adaptarse a distintos tipos de datos.

En lenguajes como Java, los parámetros de tipo se indican entre signos `< >` y pasan a formar parte de la definición de la clase o del método. Esto permite que el compilador conozca qué tipo concreto se está usando en cada instancia y pueda **comprobar la coherencia de los tipos en tiempo de compilación**. Como resultado, se eliminan la mayoría de los *casting* explícitos y se evitan muchos errores que, de otro modo, solo aparecerían en tiempo de ejecución.

Desde el punto de vista conceptual, los parámetros de tipo resuelven los problemas presentes en estructuras basadas en `Object` o `void*`. En lugar de perder información sobre el tipo real de los datos, dicha información se mantiene de forma explícita mediante el parámetro de tipo. Esto mejora notablemente la **seguridad**, la **legibilidad** y la **mantenibilidad** del código, ya que el uso incorrecto de los tipos se detecta de forma temprana.

En definitiva, los parámetros de tipo constituyen la base de la programación genérica moderna. Permiten combinar la reutilización de código con el chequeo estático de tipos, ofreciendo una solución más robusta y clara que las aproximaciones tradicionales basadas en punteros genéricos o en el uso de una superclase común como `Object`.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta
A continuación se muestran ejemplos de **programación genérica real** en Java y en C++, utilizando los mecanismos propios de cada lenguaje (**generics** y **templates**, respectivamente). En ambos casos se instancia una estructura dinámica que **solo admite elementos de tipo `String`**, garantizando el chequeo de tipos en tiempo de compilación y evitando conversiones explícitas.

***

### Ejemplo en Java usando *Generics*

En Java, la clase `ArrayList` está definida mediante un **parámetro de tipo**. Al indicar `ArrayList<String>`, se especifica que la lista únicamente puede contener objetos de tipo `String`. El compilador impide añadir elementos de otro tipo, y al recorrer la lista no es necesario realizar *casting*.

```java
import java.util.ArrayList;

public class EjemploGenerics {
    public static void main(String[] args) {
        ArrayList<String> lista = new ArrayList<>();

        lista.add("Hola");
        lista.add("Mundo");
        lista.add("Genéricos");

        for (String s : lista) {
            System.out.println(s.toUpperCase());
        }
    }
}
```

En este ejemplo, cada elemento obtenido del recorrido es directamente de tipo `String`, con **seguridad estática de tipos**. Si se intentase añadir un dato que no fuera `String`, el error se detectaría en tiempo de compilación, lo que evita errores en ejecución y mejora la claridad del código.

***

### Ejemplo en C++ usando *Templates*

En C++, la clase `vector` de la biblioteca estándar está implementada mediante **plantillas** (*templates*). Al escribir `vector<string>`, se genera una versión concreta del contenedor para el tipo `string`. El tipo queda fijado desde el momento de la instanciación.

```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> vec;

    vec.push_back("Hola");
    vec.push_back("Mundo");
    vec.push_back("Templates");

    for (const std::string& s : vec) {
        std::cout << s.size() << std::endl;
    }

    return 0;
}
```

Aquí, cada elemento del vector es un `std::string`, y el compilador garantiza que no se almacenarán datos de otro tipo. El acceso a los elementos se realiza de forma directa y segura, sin conversiones, manteniendo un alto rendimiento y un control estricto de tipos.

***

Estos dos ejemplos ilustran cómo **generics en Java** y **templates en C++** permiten implementar programación genérica auténtica: el código es reutilizable, los tipos están claramente definidos y los errores se detectan en fases tempranas del desarrollo. Esto supone una mejora sustancial frente a los enfoques basados en `Object` o `void*`.


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
Cuando se instancia una clase que tiene **parámetros de tipo**, el compilador utiliza esa información para **verificar la corrección del uso de los tipos** y adaptar el comportamiento del código genérico al tipo concreto indicado. Es decir, al escribir algo como `Lista<String>`, se está indicando que el parámetro de tipo toma el valor `String`, y el compilador comprueba que todas las operaciones realizadas sobre los elementos de la lista son coherentes con ese tipo. Este proceso permite detectar errores de tipo antes de la ejecución del programa.

Sin embargo, **Java y C++ no realizan este proceso del mismo modo**. Aunque ambos soportan programación genérica desde el punto de vista del programador, el tratamiento interno que hace el compilador de los parámetros de tipo es completamente distinto. Esta diferencia tiene importantes consecuencias en aspectos como el rendimiento, la información disponible en tiempo de ejecución y la compatibilidad hacia atrás.

En **Java**, el compilador aplica un mecanismo denominado **type erasure** (borrado de tipos). Esto significa que la información sobre los parámetros de tipo **se elimina tras el proceso de compilación**. En el bytecode resultante, los tipos genéricos se sustituyen por su límite superior (normalmente `Object`) y se insertan conversiones internas cuando es necesario. Como consecuencia, en tiempo de ejecución no es posible saber si una colección fue declarada como `List<String>` o como `List<Integer>`, aunque el compilador haya garantizado previamente el uso correcto de los tipos.

En **C++**, el enfoque es distinto y se basa en la **instanciación de plantillas** (*template instantiation*). El compilador genera una **versión concreta del código para cada tipo utilizado**. Así, `vector<string>` y `vector<int>` producen implementaciones distintas en código máquina. Esto conserva toda la información de tipo, permite optimizaciones más agresivas y mantiene el tipo concreto también en tiempo de ejecución. A cambio, puede aumentar el tamaño del binario y el tiempo de compilación, a diferencia del enfoque adoptado por Java.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta
A continuación se muestra un ejemplo de creación de una **clase genérica** en Java mediante **parámetros de tipo**. La clase `Par` permite alojar dos valores cuyos tipos pueden ser distintos y quedan fijados en el momento de la instanciación. De este modo, se consigue reutilización de código y chequeo de tipos en tiempo de compilación, evitando el uso de `Object` y conversiones explícitas.

La clase se define con dos parámetros de tipo, normalmente representados por letras mayúsculas (`T` y `U`). Estos parámetros actúan como marcadores de tipo dentro de la definición de la clase y pueden utilizarse para declarar atributos, constructores y métodos. Al instanciar la clase, se sustituyen por tipos concretos que el compilador comprueba de forma estática.

```java
public class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
}
```

Un uso habitual de este tipo de clases aparece cuando se desea **devolver más de un valor** desde un método sin definir una clase específica para cada caso. Por ejemplo, puede calcularse la media y la desviación típica de un array de `double` y devolver ambos resultados en un único objeto `Par<Double, Double>`, manteniendo claridad y seguridad de tipos.

```java
public static Par<Double, Double> mediaYDesviacion(double[] datos) {
    double suma = 0.0;
    for (double d : datos) {
        suma += d;
    }
    double media = suma / datos.length;

    double sumaCuadrados = 0.0;
    for (double d : datos) {
        sumaCuadrados += Math.pow(d - media, 2);
    }
    double desviacion = Math.sqrt(sumaCuadrados / datos.length);

    return new Par<>(media, desviacion);
}
```

En este ejemplo, el método devuelve explícitamente un `Par<Double, Double>`, por lo que queda garantizado que el primer valor es siempre un `Double` (la media) y el segundo otro `Double` (la desviación típica). El compilador puede comprobar esta coherencia sin necesidad de *casting*, lo que ilustra claramente las ventajas prácticas de la programación genérica en Java.


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta
En Java, además de declarar **parámetros de tipo a nivel de clase**, es posible definir **métodos genéricos** cuyos parámetros de tipo son locales al propio método. Esto resulta especialmente útil cuando la genericidad solo es necesaria para una operación concreta y no para toda una clase. Un ejemplo sencillo es un método que, dados dos objetos, devuelva aleatoriamente uno de ellos.

Si este método se define utilizando `Object`, se acepta cualquier tipo de objeto, pero se pierde información de tipo. El método no puede garantizar que ambos parámetros sean del mismo tipo ni que el valor de retorno coincida con el tipo esperado por quien lo invoque, obligando a realizar *downcasting* explícito al usar el resultado.

```java
import java.util.Random;

public static Object seleccionaUno(Object a, Object b) {
    Random r = new Random();
    return r.nextBoolean() ? a : b;
}
```

En este caso, si se espera recibir un `String`, será necesario hacer un *cast* manual al tipo adecuado, y además se podrían pasar dos objetos de tipos distintos sin que el compilador lo impida. Esto introduce riesgo de errores en tiempo de ejecución, como `ClassCastException`, y reduce la expresividad del código.

En cambio, al definir el método mediante un **parámetro de tipo**, se indica explícitamente que ambos argumentos deben ser del **mismo tipo**, y que el valor devuelto también lo será. El compilador puede comprobar esta coherencia en tiempo de compilación, eliminando la necesidad de *downcasting* y aumentando la seguridad del programa.

```java
import java.util.Random;

public static <T> T seleccionaUno(T a, T b) {
    Random r = new Random();
    return r.nextBoolean() ? a : b;
}
```

Con esta versión genérica, el método puede utilizarse con cualquier tipo (`String`, `Integer`, clases propias, etc.), pero siempre de forma coherente. El compilador garantiza que ambos parámetros son del mismo tipo y que el resultado se recibe ya como ese tipo concreto. Esto ilustra claramente cómo los **métodos genéricos** mejoran la seguridad y la claridad del código frente a soluciones basadas en `Object`.


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta
Sí, en Java es posible **establecer restricciones en los parámetros de tipo** mediante los llamados *bounded type parameters*. Esto permite indicar que un parámetro genérico debe ser, como mínimo, de un cierto tipo o una de sus subclases. De esta forma, el compilador puede autorizar el uso de determinados métodos y garantizar que el tipo genérico cumple unas condiciones mínimas, por ejemplo, que sea un número y pueda tratarse como tal.

Una primera solución sencilla consiste en **no usar genéricos** y declarar directamente las coordenadas como `Number`. Dado que `Number` es la superclase de tipos numéricos como `Integer`, `Double` o `Float`, permite trabajar con métodos como `doubleValue()`. Sin embargo, esta solución no impide que las coordenadas sean de tipos numéricos distintos, y el tipo concreto se pierde desde el punto de vista del chequeo estático.

```java
public class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(Punto otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Una segunda solución más robusta consiste en usar **genéricos acotados**, indicando que el parámetro de tipo debe extender `Number`. De este modo se refuerza el chequeo de tipos, garantizando que ambas coordenadas son del **mismo tipo numérico concreto** y permitiendo al compilador conocer exactamente con qué tipo se está trabajando. Esta versión es más expresiva y segura, especialmente en código genérico reutilizable.

```java
public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En cuanto al **type erasure**, en ambos casos el tipo genérico **no existe en tiempo de ejecución**. Tras la compilación, el parámetro `T` se borra y se sustituye por su límite superior, que en este caso es `Number`. Por tanto, el tipo final generado por el compilador es equivalente a una clase que trabaja internamente con `Number`, aunque el uso de genéricos haya permitido un chequeo de tipos mucho más preciso y seguro en tiempo de compilación.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta
Las dos soluciones planteadas permiten reutilizar la clase `Punto` para trabajar con distintos tipos numéricos, pero **no ofrecen el mismo nivel de refuerzo en el chequeo de tipos**. En la versión sin genéricos, donde las coordenadas son de tipo `Number`, no existe ninguna restricción que impida crear un punto con una coordenada entera y la otra real, por ejemplo `new Punto(3, 2.5)`. Esto es posible porque ambas coordenadas se almacenan como `Number`, y el compilador no distingue ni exige coherencia entre los tipos concretos utilizados.

En cambio, en la solución con **genéricos acotados** (`Punto<T extends Number>`), el parámetro de tipo `T` representa un **único tipo numérico concreto** para toda la instancia. Como consecuencia, no es posible crear un punto combinando tipos distintos, como un `Integer` y un `Double`, ya que el compilador exige que ambos argumentos correspondan al mismo `T`. Este refuerzo evita incoherencias semánticas y fuerza a que el diseño sea explícito respecto al tipo numérico utilizado.

Otra diferencia importante aparece en el **tipo de retorno de los métodos accesores**. En la solución sin genéricos, el método `getX` devuelve siempre un `Number`, independientemente de si internamente se almacenó un `Integer`, un `Double` u otro subtipo. Esto obliga, si se desea el tipo concreto, a realizar *casting* o a trabajar siempre con la abstracción `Number`, perdiendo precisión en el chequeo estático.

Por el contrario, en la solución con genéricos, el método `getX` devuelve el tipo concreto `T`. Si el punto se declara como `Punto<Integer>`, el compilador sabe que `getX` devuelve un `Integer`; si se declara como `Punto<Double>`, devolverá un `Double`. Esto supone una mejora clara en la expresividad del código y en la detección temprana de errores, ya que el tipo exacto se conoce y se verifica en tiempo de compilación, aunque posteriormente se borre mediante *type erasure*.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta
Para evitar el uso de `instanceof` y *downcasting*, y reforzar el **chequeo estático de tipos**, puede utilizarse un patrón basado en **interfaces genéricas auto‑referenciadas**. La idea es que la interfaz `Punto` tenga un **parámetro de tipo** que represente el tipo concreto de punto con el que puede calcular distancias. De este modo, el compilador garantiza que el método `distanciaA` solo se invoca entre puntos del mismo tipo geométrico.

La interfaz se redefine como genérica, indicando que el parámetro de tipo debe ser, a su vez, un tipo de `Punto`. Esta técnica obliga a que cualquier implementación especifique explícitamente su propio tipo al implementar la interfaz, fijando así el contrato del método sin posibilidad de confusión entre dimensiones distintas.

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
```

Con esta definición, `Punto2D` implementa la interfaz indicando que solo puede calcular distancia con otro `Punto2D`. El método sobrescrito recibe directamente el tipo correcto, sin comprobaciones dinámicas ni conversiones explícitas, y cualquier uso incorrecto será rechazado en tiempo de compilación.

```java
public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(
            Math.pow(x - p.x, 2) +
            Math.pow(y - p.y, 2)
        );
    }
}
```

De forma análoga, `Punto3D` fija su propio tipo y nunca podrá mezclarse con `Punto2D` en una llamada a `distanciaA`. Intentar calcular la distancia entre puntos de distintas dimensiones producirá un **error de compilación**, no un fallo en ejecución, lo que supone un refuerzo claro del sistema de tipos.

```java
public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(
            Math.pow(x - p.x, 2) +
            Math.pow(y - p.y, 2) +
            Math.pow(z - p.z, 2)
        );
    }
}
```

Esta solución muestra un uso avanzado de generics para **expresar restricciones semánticas en la jerarquía de tipos**, eliminando código defensivo innecesario y desplazando los errores al momento de compilación. Aunque en tiempo de ejecución el *type erasure* elimina la información genérica, el beneficio principal se obtiene en la fase de desarrollo, donde el compilador actúa como garante de la coherencia del diseño.


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta
Aunque `String` sea subtipo de `Object`, **no significa que `List<String>` sea subtipo de `List<Object>`** en Java. Sin embargo, **sí es cierto que `String[]` es subtipo de `Object[]`**. Esta diferencia se debe a decisiones de diseño distintas entre el sistema de tipos de los **arrays** y el de los **genéricos**, y tiene implicaciones importantes en la seguridad en tiempo de ejecución.

En el caso de los **genéricos**, Java define que las clases genéricas son **invariantes respecto a su parámetro de tipo**. Esto significa que, aunque `String` herede de `Object`, no existe relación de subtipado entre `List<String>` y `List<Object>`. Permitirlo provocaría errores de tipo: si `List<String>` pudiera tratarse como `List<Object>`, se podrían insertar objetos que no fueran `String`, rompiendo la coherencia interna de la lista. Al forzar la invariancia, el compilador impide este tipo de situaciones desde el principio.

Por el contrario, los **arrays en Java son covariantes**. Esto implica que `String[]` sí puede asignarse a una variable de tipo `Object[]`, ya que `String` es subtipo de `Object`. No obstante, esta flexibilidad introduce un problema potencial en tiempo de ejecución: a través de la referencia `Object[]` podría intentarse almacenar un objeto que no sea `String`. En ese caso, el programa compila correctamente, pero falla en ejecución con una `ArrayStoreException`, ya que el array real sigue siendo de `String`.

A partir de estos ejemplos, puede definirse que un tipo genérico es **covariante** si conserva la relación de subtipado de su parámetro de tipo, **contravariante** si la invierte, e **invariante** si no permite ninguna relación de subtipado basada en el parámetro. En Java, los arrays son covariantes (con riesgo en ejecución), mientras que los genéricos son invariantes por defecto, priorizando la **seguridad de tipos en tiempo de compilación** frente a la flexibilidad.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
Un **wildcard** (`?`) en Java representa un **tipo desconocido** dentro de una estructura genérica. Su objetivo es recuperar, de forma controlada, cierto grado de **covarianza o contravarianza** que los genéricos no permiten por defecto al ser invariantes. Mediante wildcards se puede relajar el sistema de tipos cuando solo se necesita **leer** valores de una colección o, por el contrario, **insertarlos**, manteniendo la seguridad en tiempo de compilación.

La expresión `List<? extends T>` indica que la lista contiene elementos de **algún subtipo de `T`**, aunque no se conoce exactamente cuál. Esta forma introduce **covarianza** y es adecuada cuando solo se necesita **consumir (leer)** elementos de la lista. Sin embargo, no se permite añadir nuevos elementos (salvo `null`), ya que el compilador no puede garantizar que el tipo añadido sea compatible con el subtipo concreto que se esté usando.

```java
public static double sumarNumeros(List<? extends Number> lista) {
    double suma = 0.0;
    for (Number n : lista) {
        suma += n.doubleValue();
    }
    return suma;
}
```

En este ejemplo, el método puede recibir una `List<Integer>`, `List<Double>` o cualquier otra lista de subtipos de `Number`. La seguridad está garantizada porque todos los elementos pueden tratarse al menos como `Number`, pero no es posible modificar la lista.

Por otro lado, `List<? super T>` indica que la lista contiene elementos de **algún supertipo de `T`**. Esta forma introduce **contravarianza** y es apropiada cuando se pretende **producir (insertar)** valores en la colección. En este caso sí se permite añadir objetos de tipo `T` o de sus subtipos, pero al recuperar elementos solo se garantiza que son de tipo `Object`.

```java
public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
    lista.add(3);
}
```

Este método puede recibir una `List<Integer>`, `List<Number>` o incluso una `List<Object>`. El uso de `? super` garantiza que añadir enteros es seguro, aunque al leer elementos no se conoce su tipo concreto. En resumen, `? extends` se emplea cuando se **leen** datos y `? super` cuando se **escriben**, lo que se resume habitualmente en la regla: *Producer Extends, Consumer Super (PECS)*.
