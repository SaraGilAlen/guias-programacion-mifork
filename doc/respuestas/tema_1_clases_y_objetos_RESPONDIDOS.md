<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta
Las cuatro características fundamentales de la programación orientada a objetos son **encapsulamiento**, **abstracción**, **herencia** y **polimorfismo**. Estas propiedades permiten organizar el código en torno a entidades llamadas *objetos*, facilitando su mantenimiento y reutilización. Aunque en C se trabaja con estructuras y funciones separadas, en orientación a objetos se combina en una misma unidad tanto los datos como las operaciones que los manipulan.

El **encapsulamiento** consiste en agrupar datos y métodos en una clase, controlando el acceso a ellos mediante niveles de visibilidad como `private`, `public` o `protected`. Esto evita que otras partes del programa modifiquen directamente información interna, reduciendo errores. En Java, por ejemplo, suele usarse métodos *getters* y *setters* para acceder a atributos privados, algo que contrasta con la exposición directa típica de las estructuras en C.

La **abstracción** implica ocultar los detalles internos de funcionamiento y mostrar solo lo esencial. Una clase proporciona una interfaz clara y deja en segundo plano la complejidad interna. De esta manera, se puede usar un objeto sin necesidad de conocer cómo están implementados sus métodos, de forma similar a cómo en C se emplea una función sin saber necesariamente su código interno, pero ahora aplicado a entidades más complejas.

La **herencia** permite crear nuevas clases a partir de otras existentes, reutilizando su comportamiento y extendiéndolo cuando sea necesario. Esto evita duplicación de código y facilita la creación de jerarquías lógicas. Por último, el **polimorfismo** posibilita que distintos objetos respondan de manera diferente a un mismo método, siempre que compartan un tipo base. Este concepto resulta especialmente útil cuando se quiere diseñar código flexible que funcione con múltiples tipos de objetos sin modificar la lógica principal.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta
Entre los lenguajes de programación más populares que permiten la programación orientada a objetos se encuentran **Java**, **C++**, **Python** y **C#**. Cada uno incorpora el paradigma orientado a objetos con distintos matices, pero todos permiten trabajar con clases, objetos, herencia y polimorfismo como pilares fundamentales del diseño del software.

**Java** destaca por estar diseñado desde cero alrededor de la orientación a objetos, siendo obligatorio trabajar con clases incluso para programas simples. **C++**, aunque permite programación estructurada como en C, añade extensiones orientadas a objetos que lo convierten en un lenguaje muy flexible, especialmente útil en sistemas de alto rendimiento. Por su parte, **Python** ofrece un enfoque más dinámico, donde la orientación a objetos convive de forma natural con otros paradigmas.

Finalmente, **C#** combina elementos de Java y C++, proporcionando un entorno moderno y muy utilizado en desarrollo de aplicaciones de escritorio, videojuegos y software empresarial. Estos cuatro lenguajes representan distintas filosofías, pero todos permiten aplicar de forma completa los principios de la programación orientada a objetos.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta
La **programación estructurada** es un paradigma previo a la orientación a objetos que organiza el código en secuencias, decisiones y bucles, evitando el uso indiscriminado de saltos como `goto`. Su objetivo es dividir un problema en bloques lógicos más pequeños y fáciles de entender. En lugar de pensar en objetos, este enfoque se centra en el flujo de ejecución del programa, siguiendo una estructura clara y lineal que facilita el razonamiento y la depuración.

La **programación modular** va un paso más allá al promover la descomposición del programa en partes independientes llamadas *módulos*. Cada módulo contiene funciones y datos relacionados entre sí, lo que permite desarrollar, probar y mantener distintas partes del programa sin afectar al resto. Esto resulta especialmente útil en proyectos grandes, donde diferentes componentes pueden evolucionar de forma relativamente aislada.

Mientras que la programación estructurada se centra en controlar el flujo del programa para hacerlo más legible, la programación modular se enfoca en organizar el código en unidades cohesionadas y reutilizables. Ambas sentaron las bases para la posterior aparición de la programación orientada a objetos, que lleva aún más lejos la organización del software al introducir la idea de clases y objetos para agrupar tanto datos como comportamientos.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta
Un objeto en programación orientada a objetos se define fundamentalmente por **estado**, **comportamiento** e **identidad**. Estos tres elementos permiten que un objeto represente una entidad coherente dentro del programa, combinando datos y operaciones de forma unificada. A diferencia de las estructuras en C, donde los datos se encuentran separados de las funciones, un objeto integra ambos conceptos dentro de una misma unidad lógica.

El **estado** corresponde a los datos que el objeto almacena, normalmente representados mediante atributos o variables de instancia. Este estado puede cambiar a lo largo del tiempo mediante las operaciones del propio objeto, lo que permite que dos objetos de la misma clase mantengan información distinta entre sí. Por ejemplo, dos objetos de una clase `Coche` pueden tener diferentes valores para atributos como velocidad o combustible.

El **comportamiento** describe las acciones que el objeto puede realizar, implementadas a través de métodos. Estas operaciones permiten modificar el estado interno o interactuar con otros objetos, definiendo la funcionalidad asociada a ese tipo de entidad. Así, el comportamiento establece cómo se manipula y se utiliza un objeto dentro del programa, añadiendo una capa de abstracción respecto a los datos internos.

Finalmente, la **identidad** distingue a un objeto concreto de cualquier otro, incluso si ambos tienen el mismo estado y comportamiento. En Java, esta identidad suele asociarse a la referencia del objeto en memoria. Esto implica que dos objetos pueden ser equivalentes en su contenido pero seguir siendo entidades distintas dentro de la ejecución del programa.


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
Una **clase** es una plantilla o modelo que define qué atributos y métodos tendrá un tipo de objeto. Se puede entender como una descripción general que especifica qué datos almacenarán sus objetos y qué operaciones podrán realizar. En esencia, la clase establece la estructura y el comportamiento común que compartirán todos los objetos creados a partir de ella, de forma similar a cómo en C se define una estructura, pero añadiendo funciones asociadas.

Un **objeto** no es lo mismo que una clase. Mientras que la clase es solo el diseño teórico, el objeto es la materialización concreta de ese diseño durante la ejecución del programa. Es decir, cuando se crea un objeto se está generando una entidad real en memoria basada en la definición de la clase. Por ello, varios objetos de la misma clase pueden existir simultáneamente y tener estados distintos, aunque compartan el mismo conjunto de comportamientos.

El término **instancia** se utiliza como sinónimo de objeto creado a partir de una clase. Instanciar una clase significa generar un objeto real que posea sus atributos y métodos. En Java, por ejemplo, esta acción se realiza mediante la palabra clave `new`, que reserva memoria y prepara el objeto para su uso. Cada instancia es independiente y mantiene su propio estado, aunque todas sigan el mismo modelo definido por la clase original.

No todos los lenguajes orientados a objetos manejan necesariamente el concepto de **clase**. Aunque lenguajes como Java, C++, C# o PHP sí se basan en clases, existen otros como JavaScript o Python que permiten un enfoque orientado a objetos sin depender estrictamente de ellas. En JavaScript, por ejemplo, la orientación a objetos se basa en *prototipos*, y las “clases” son una abstracción añadida posteriormente para facilitar el uso del lenguaje, mientras que Python permite crear objetos incluso sin definir una clase explícita mediante el uso de funciones o tipos dinámicos.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta
Los objetos en la mayoría de lenguajes orientados a objetos se almacenan en una zona de memoria conocida como **montículo** (*heap*). Esta área está destinada a la creación dinámica de entidades cuyo tamaño o duración no se conoce en tiempo de compilación. En lenguajes como Java, cada vez que se crea un objeto mediante `new`, se reserva espacio en el heap y se devuelve una referencia que permite acceder a él desde el programa. Este comportamiento contrasta con lenguajes no orientados a objetos donde gran parte de los datos se puede almacenar en la pila (*stack*), aunque en C++ los objetos también pueden colocarse en la pila si se definen como variables automáticas.

No todos los lenguajes orientados a objetos gestionan la memoria de la misma manera. Por ejemplo, en **C++** el programador puede decidir explícitamente si un objeto se guarda en la pila o en el heap, lo que implica la responsabilidad de liberarlo manualmente cuando ya no se necesita. En cambio, **Java**, **Python** o **C#** gestionan los objetos casi exclusivamente en el heap y evitan la destrucción manual, reduciendo ciertos tipos de errores comunes en lenguajes con gestión manual de memoria. Cada lenguaje adopta un modelo diferente según su filosofía de diseño y necesidades de rendimiento o seguridad.

La **recolección de basura** (*garbage collection*) es un mecanismo automático que libera memoria ocupada por objetos que ya no están siendo utilizados por el programa. En lenguajes como Java, este proceso se encarga de identificar objetos inaccesibles —aquellos a los que no se puede llegar desde ninguna referencia activa— y eliminarlos para recuperar memoria. Esto permite que el programador no tenga que preocuparse por liberar recursos manualmente, reduciendo problemas como fugas de memoria o accesos a zonas liberadas.

Es importante señalar que la recolección de basura no existe de forma nativa en lenguajes como C o C++, donde la liberación de memoria es responsabilidad directa del desarrollador. Esta diferencia marca una de las ventajas principales de lenguajes con gestión automática: menor riesgo de errores graves relacionados con memoria. No obstante, estos sistemas pueden introducir pausas o sobrecargas en la ejecución del programa, motivo por el cual algunos lenguajes orientados a objetos permiten ajustar o configurar el comportamiento del recolector.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta
Un **método** es una función asociada a una clase que define una acción o comportamiento que pueden realizar sus objetos. A diferencia de las funciones en lenguajes como C, un método actúa sobre el estado interno del objeto al que pertenece, accediendo a sus atributos y manipulándolos cuando es necesario. De esta forma, el método forma parte de la interfaz que la clase ofrece y establece cómo se debe interactuar con las instancias creadas a partir de ella. En Java, todos los comportamientos de un objeto se expresan mediante métodos definidos dentro de la clase.

La **sobrecarga de métodos** es una característica de la programación orientada a objetos que permite definir varios métodos con el mismo nombre dentro de una misma clase, siempre que sus parámetros sean distintos en número o tipo. El objetivo es ofrecer distintas formas de realizar una misma operación adaptándose a diferentes necesidades de entrada, sin tener que inventar nombres nuevos para cada variante. Esto facilita la legibilidad y coherencia del código, permitiendo que una función mantenga su identidad incluso cuando admite múltiples configuraciones.

Cuando se utiliza la sobrecarga, el compilador elige automáticamente qué versión del método se debe ejecutar en función de los argumentos proporcionados en la llamada. Esto permite construir clases más flexibles y expresivas, ya que se agrupan bajo un mismo nombre acciones conceptualmente iguales, pero implementadas con diferentes firmas. En Java, esta técnica se usa con frecuencia, incluso en clases estándar como `System.out`, donde el método `println` aparece sobrecargado para múltiples tipos de datos.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta
A modo de ejemplo mínimo, puede definirse una clase `Punto` con dos atributos `x` e `y` con **visibilidad por defecto** (sin modificador) y un método `calculaDistanciaAOrigen()` que devuelva la distancia euclídea al punto `(0,0)`. El cálculo se realiza aplicando la fórmula $$\sqrt{x^2 + y^2}$$, para lo cual en Java resulta apropiado usar `Math.sqrt` y `Math.pow` (o `x*x` y `y*y` para evitar la sobrecarga de `pow`). Al no usar modificadores de acceso en los atributos, su visibilidad queda limitada al *package*.

Se muestra también un ejemplo de uso creando una instancia, asignando coordenadas y llamando al método. Para mantener el ejemplo simple, se incluye una clase con `main` en el mismo archivo; alternativamente, podría crearse en un archivo distinto dentro del mismo *package*. El resultado se imprime por consola para verificar el funcionamiento.

```java
// Archivo: Punto.java
class Punto {
    double x;  // visibilidad por defecto (package-private)
    double y;  // visibilidad por defecto (package-private)

    double calculaDistanciaAOrigen() {
        // Puede usarse Math.sqrt(x * x + y * y) para evitar Math.pow
        return Math.sqrt(x * x + y * y);
    }
}

// Ejemplo de uso (puede estar en el mismo archivo o en otro del mismo package)
public class EjemploPunto {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3.0;
        p.y = 4.0;

        double d = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + d); // Debería imprimir 5.0
    }
}
```


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta
El **punto de entrada** de un programa en Java es siempre el método  
`public static void main(String[] args)`. Este método es el primero que la JVM ejecuta al iniciar la aplicación, por lo que actúa como la puerta de entrada del programa, similar a `int main()` en C. Aunque existan múltiples clases en un proyecto, solo aquellas que contengan este método pueden servir como punto de inicio. El formato exacto de la firma es obligatorio, ya que la JVM la busca explícitamente para comenzar la ejecución.

La palabra clave **`static`** indica que un método o atributo pertenece a la clase y **no** a una instancia concreta. Esto significa que puede usarse sin crear un objeto, algo imprescindible para `main`, dado que el programa todavía no ha instanciado nada cuando comienza. El modificador `static` se emplea en más contextos, por ejemplo, para métodos utilitarios (como en la clase `Math`) o para atributos compartidos por todas las instancias. De esta forma se permite definir comportamientos o datos a nivel de clase, lo que evita crear objetos cuando no son necesarios.

El uso de `static` no se limita al método `main`; también se emplea para declarar **atributos estáticos** y **métodos estáticos**. Estos miembros comparten un único valor o comportamiento para todas las instancias de la clase, lo que resulta útil para contadores globales, constantes o funciones auxiliares. Sin embargo, deben usarse con moderación, ya que un excesivo uso de elementos estáticos puede reducir la flexibilidad del diseño orientado a objetos.

Cuando `static` se combina con **`final`**, se obtiene un miembro que es estático (compartido por toda la clase) y que no puede modificarse una vez asignado. Esta combinación se utiliza para declarar **constantes**, como ocurre con `public static final double PI` en la clase `Math`. Gracias a esta combinación, el valor es único, inmutable y accesible sin necesidad de crear instancias, lo que facilita la definición de información constante asociada a la clase.


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta
Para compilar y ejecutar un programa Java desde la línea de comandos se utilizan los comandos **`javac`** y **`java`**. El proceso habitual comienza guardando el código en un archivo con extensión `.java`, por ejemplo `Ejemplo.java`. A continuación, se compila el archivo con `javac Ejemplo.java`, lo que genera uno o varios archivos `.class` con el *byte‑code*. Después, se ejecuta el programa con `java Ejemplo`, indicando el nombre de la clase que contiene el método `main` pero **sin** la extensión `.class`. Este flujo resulta similar al uso de un compilador en C, aunque Java separa claramente las fases de compilación y ejecución.

Java puede considerarse un lenguaje **compilado e interpretado a la vez**. Primero se compila el código fuente a *byte‑code* mediante `javac`, lo que lo convierte en un formato intermedio independiente del sistema operativo. Posteriormente, ese *byte‑code* es ejecutado por la **máquina virtual de Java (JVM)**, que actúa como un intérprete avanzado. Esta combinación permite que el mismo programa funcione en distintos sistemas sin necesidad de recompilarlo para cada plataforma, lo que se conoce como la filosofía *"write once, run anywhere"*.

La **máquina virtual** es un entorno de ejecución simulado que interpreta o compila dinámicamente el *byte‑code*. Proporciona una capa de abstracción entre el programa y el sistema operativo real, gestionando aspectos como la memoria, la ejecución de hilos o la recolección de basura. Gracias a esta arquitectura, el programa Java no depende de detalles específicos del hardware, lo que mejora la portabilidad y estabilidad del software. La JVM, por tanto, es el componente encargado de transformar el *byte‑code* en instrucciones nativas que la CPU puede ejecutar.

El **byte‑code** es un conjunto de instrucciones intermedias generadas por el compilador `javac`. No es directamente ejecutable por el sistema operativo, sino que está diseñado para ser interpretado por la JVM. Cada archivo `.class` contiene el byte‑code correspondiente a una clase concreta, por lo que un programa con varias clases generará varios archivos `.class`. Esta estructura modular facilita la carga dinámica de clases y hace posible que programas Java funcionen en cualquier sistema donde exista una implementación compatible de la máquina virtual.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta
La palabra clave **`new`** en Java crea un objeto en el *heap*, invoca su **constructor** y devuelve una **referencia** a esa nueva instancia. Es decir, `new` reserva memoria, inicializa el estado del objeto según el constructor elegido y permite trabajar con él mediante la referencia resultante. Este comportamiento contrasta con C, donde la reserva de memoria suele hacerse con `malloc` y no existe la llamada automática a una función de inicialización ligada a un tipo.

Un **constructor** es un método especial de la clase que se ejecuta justo en el momento de crear el objeto. No tiene tipo de retorno (ni siquiera `void`) y su nombre coincide exactamente con el de la clase. Su finalidad es **inicializar** los atributos para dejar el objeto en un estado válido desde el inicio. Puede haber **sobrecarga de constructores** (varias firmas con distintos parámetros) para ofrecer diferentes formas de creación, como inicializar con valores por defecto o con datos aportados por el usuario.

A continuación, se muestra un ejemplo de clase `Empleado` con tres atributos (`dni`, `nombre`, `apellidos`) y un **constructor con parámetros** que asegura que el objeto queda correctamente inicializado al crearlo:

```java
public class Empleado {
    private String dni;
    private String nombre;
    private String apellidos;

    // Constructor que inicializa todos los campos
    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }

    // (Opcional) Getters para acceder a los atributos
    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
    public String getApellidos() { return apellidos; }
}
```

> Ejemplo de uso (referencial): `Empleado e = new Empleado("12345678A", "Ana", "Pérez Gómez");` Aquí `new` crea la instancia, llama al constructor y devuelve la referencia que se asigna a `e`.


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta
La referencia **`this`** en Java es un puntero implícito al **objeto actual** sobre el que se está ejecutando el método o el constructor. Sirve para distinguir entre atributos de instancia y parámetros locales con el mismo nombre, para **encadenar constructores** (`this(...)`) y para pasar la propia instancia como argumento a otros métodos. En términos prácticos, `this` permite acceder de forma explícita al estado del objeto que recibe la llamada, evitando ambigüedades y mejorando la legibilidad cuando hay sombreado de nombres.

No se llama igual en todos los lenguajes. En **C++** también se usa `this` (como puntero al objeto), mientras que en **Python** se emplea el parámetro convencional **`self`** para representar la instancia actual. En **JavaScript**, `this` existe pero su valor depende del **contexto de invocación** (cómo se llama a la función), lo que difiere del comportamiento fijo de Java. Por tanto, aunque la idea de “la instancia actual” es común, la sintaxis y la semántica varían entre lenguajes.

A continuación, un ejemplo de uso de `this` en la clase `Punto`. Se incluye un **constructor** que desambigüa entre parámetros y atributos, y un **método** que utiliza `this` para acceder a los campos del objeto:

```java
class Punto {
    double x; // visibilidad por defecto (package-private)
    double y; // visibilidad por defecto (package-private)

    // Constructor: desambiguación de nombres mediante 'this'
    Punto(double x, double y) {
        this.x = x; // 'this.x' es el atributo; 'x' es el parámetro
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        // Uso explícito de 'this' para resaltar que se accede a los atributos de la instancia
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Método que devuelve la propia referencia de la instancia (patrones de encadenamiento)
    Punto traslada(double dx, double dy) {
        this.x += dx;
        this.y += dy;
        return this; // devuelve la instancia actual
    }
}
```

En este ejemplo, `this.x` y `this.y` aseguran que se modifican los **atributos** del objeto y no variables locales. Además, devolver `this` en `traslada` permite **encadenar** llamadas (por ejemplo, `p.traslada(1,0).traslada(0,2)`), una técnica habitual en APIs fluidas.


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta
Para calcular la distancia entre el objeto actual y otro punto, puede añadirse un método llamado `distanciaA` que reciba como parámetro un objeto `Punto`. Este método aplicará la fórmula de distancia entre dos coordenadas del plano:  
$$\sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$. En este contexto, `this` representa al punto actual, mientras que el parámetro representa al punto destino. El uso de la referencia `this` permite aclarar que los atributos que se emplean pertenecen a la instancia sobre la que se invoca el método.

Este método facilita operaciones geométricas entre objetos sin necesidad de funciones externas, manteniendo el comportamiento dentro de la propia clase. Además, permite reutilizar código y mantener la cohesión del diseño orientado a objetos, ya que la lógica relacionada con un punto se encuentra encapsulada en la clase `Punto`. Su incorporación demuestra también la interacción entre objetos de la misma clase y el paso de instancias como parámetros.

Aquí se presenta el método integrado dentro de la clase:

```java
class Punto {
    double x; // visibilidad por defecto
    double y; // visibilidad por defecto

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Nuevo método solicitado
    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Este método permite expresiones como `p1.distanciaA(p2)`, donde se obtiene la distancia entre dos objetos `Punto` sin necesidad de cálculos externos.


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta
En Java, el paso de parámetros se realiza siempre **por valor**, pero en el caso de los objetos ese “valor” es **la referencia** al objeto. Esto significa que, cuando se pasa un `Punto` como parámetro, el método recibe una copia de la *referencia*, no una copia del objeto. Ambas referencias apuntan al mismo objeto en memoria, por lo que **si se modifican sus atributos dentro del método, los cambios sí afectan al objeto original** fuera del método. Desde el punto de vista práctico, esto se comporta como “paso por referencia”, aunque técnicamente Java copia la referencia.

Este comportamiento no ocurre con los tipos primitivos como `int`. En ese caso, lo que se copia es directamente el **valor numérico**, no una referencia. Si dentro del método se cambia el valor del parámetro entero, ese cambio afecta únicamente a la copia local dentro de la función, sin modificar la variable original del exterior. Esto implica que operaciones internas sobre tipos primitivos no tienen efecto fuera del ámbito del método, reflejando un comportamiento clásico de paso por valor.

La diferencia principal, por tanto, radica en qué se está copiando: cuando se trata de objetos, se copia la referencia y se opera sobre el mismo objeto en memoria; cuando se trata de tipos primitivos, se copia el valor y se opera sobre una variable independiente. Este modelo permite trabajar con objetos de manera flexible, pero también exige cuidado para evitar modificaciones no deseadas en instancias compartidas entre varias partes del programa.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta
El método **`toString()`** en Java es una función definida en la clase base `Object` que devuelve una **representación textual** del objeto. Por defecto, produce una cadena con el nombre de la clase y un identificador basado en el *hash* del objeto, lo cual no suele ser informativo. Por ello, se acostumbra a **sobrescribirlo (`@Override`)** para retornar una descripción legible del estado interno, lo que facilita la depuración, el registro (*logging*) y la salida por consola (por ejemplo, al usar `System.out.println(obj)` o concatenaciones con strings).

Este concepto existe en otros lenguajes con distintas formas. En **C#** se utiliza `ToString()` y también se puede sobrescribir. En **Python**, el equivalente idiomático es `__str__` (y `__repr__` para una representación más técnica). En **C++**, no hay un método universal, pero se logra un efecto similar sobrecargando el **operador de inserción** `operator<<` para `std::ostream`, o con funciones como `std::to_string` para tipos básicos. La idea común es ofrecer una forma estándar de convertir un objeto a texto útil y legible.

Ejemplo de `toString()` en la clase `Punto`:

```java
class Punto {
    double x; // visibilidad por defecto (package-private)
    double y; // visibilidad por defecto (package-private)

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    @Override
    public String toString() {
        return "Punto(x=" + x + ", y=" + y + ")";
    }
}

// Ejemplo de uso:
// Punto p = new Punto(3, 4);
// System.out.println(p);            // Imprime: Punto(x=3.0, y=4.0)
// System.out.println(p.toString()); // Equivalente
```


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

### Respuesta
Una **clase** puede compararse parcialmente con un `struct` de C, ya que ambos permiten agrupar datos bajo un mismo tipo. En un `struct`, se definen campos que representan información, y cada variable declarada con dicho `struct` posee su propio conjunto de valores, igual que una instancia de una clase en Java. Desde esa perspectiva limitada, un `struct` puede verse como una forma rudimentaria de reunir datos asociados a una entidad concreta, lo que recuerda al estado de un objeto.

Sin embargo, a un `struct` de C le falta un elemento esencial para comportarse como una clase: **no puede contener métodos asociados**, solo datos. En una clase, además de atributos, existen funciones integradas llamadas métodos, que operan directamente sobre el estado del objeto y encapsulan el comportamiento. En C, las funciones deben definirse aparte y recibir explícitamente un puntero al `struct` si se quiere simular ese comportamiento, lo cual rompe la cohesión propia de la orientación a objetos.

Otra diferencia fundamental es la **visibilidad y el encapsulamiento**. En C, todos los campos del `struct` son públicos y accesibles sin restricciones. En una clase, en cambio, se pueden usar modificadores como `private`, `public` o `protected` para controlar el acceso a datos y evitar manipulaciones indebidas. La combinación de datos y métodos, junto con las reglas de visibilidad, permite que las clases ofrezcan una interfaz clara y un diseño más seguro y modular.

Por último, una clase puede incluir otros elementos que un `struct` en C no posee, como **constructores**, **sobrecarga de métodos**, **herencia**, **polimorfismo** y la posibilidad de definir comportamientos comunes entre instancias. Todo esto hace que las variables de un tipo definido por una clase sean auténticas **instancias de un objeto**, mientras que las de un `struct` son simplemente bloques de datos sin funcionalidad asociada.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
Para “emular” una clase `Punto` en C, puede definirse un `struct` que agrupe los datos y funciones **externas** que operen sobre él, pasando un puntero al `struct` como primer parámetro. Esa convención hace el papel de `this`: en lugar de ser implícito (como en Java), se pasa **explícitamente**. Suele emplearse un prefijo en los nombres de función (por ejemplo, `Punto_`) para simular el *namespacing* de los métodos de una clase y mantener la cohesión conceptual.

A continuación, un ejemplo mínimo con cálculo de la distancia al origen. Se incluye además una función de “inicialización” para simular el constructor (en C no hay constructores). Obsérvese que `calculaDistanciaAOrigen` recibe `const Punto* p`, reflejando que no debe modificar el estado.

```c
// punto.c (o .h si se declara la interfaz)
#include <math.h>   // sqrt
#include <stdio.h>  // printf

typedef struct {
    double x;
    double y;
} Punto;

// "Constructor" emulado (inicializador)
void Punto_init(Punto* p, double x, double y) {
    p->x = x;
    p->y = y;
}

// "Método" emulado: distancia al origen
double Punto_calculaDistanciaAOrigen(const Punto* p) {
    return sqrt(p->x * p->x + p->y * p->y);
}

// (Opcional) Otro "método" que modifica el estado
void Punto_traslada(Punto* p, double dx, double dy) {
    p->x += dx;
    p->y += dy;
}

int main(void) {
    Punto p;
    Punto_init(&p, 3.0, 4.0);            // emula constructor
    double d = Punto_calculaDistanciaAOrigen(&p); // emula p.calculaDistanciaAOrigen()
    printf("Distancia al origen: %.2f\n", d);     // Imprime 5.00

    Punto_traslada(&p, 1.0, -2.0);       // emula p.traslada(1, -2)
    printf("Nuevo punto: (%.2f, %.2f)\n", p.x, p.y);
    return 0;
}
```

En esta emulación, lo que en Java sería `this` se ha convertido en el primer argumento de las funciones: `Punto* p` (o `const Punto* p` si no se modifica). No existe **encapsulamiento** real ni **visibilidad** (`private/public`) a nivel de campos: todo es accesible salvo que se oculte deliberadamente la definición del `struct` en un `.c` y se exponga solo un *handle* opaco vía punteros en el `.h`. Tampoco hay **constructores**, **destructores**, ni **polimorfismo** nativo; cualquier patrón similar debe implementarse manualmente mediante funciones, convenciones de nombres y, si procede, gestión explícita de memoria.
