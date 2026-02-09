<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta
La **encapsulación** en POO busca agrupar datos y comportamientos relacionados dentro de una misma unidad —la clase— de modo que se controle cómo se accede y modifica ese estado interno. Este mecanismo permite que el objeto gestione su propia información mediante métodos definidos, evitando accesos directos que podrían provocar inconsistencias. Por su parte, la **ocultación de información** consiste en restringir el acceso a los detalles internos de la clase, exponiendo solo lo necesario a través de interfaces públicas. Así, se separa claramente lo que un objeto “muestra” de lo que realmente “hace” por dentro.

Con la ocultación, se evita que otras partes del programa dependan de la implementación interna de una clase, lo cual facilita modificar el código en el futuro sin romper el funcionamiento externo. Además, se mejora la seguridad del programa, ya que solo se permite modificar atributos mediante métodos controlados, reduciendo errores causados por cambios indebidos. También se promueve un diseño más modular, permitiendo trabajar con los objetos como “cajas negras”, donde únicamente importa su comportamiento público y no su estructura interna.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
La **interfaz pública** de un objeto o clase se entiende como el conjunto de métodos y atributos accesibles desde fuera de dicha clase. Esta interfaz define *cómo* puede interactuarse con el objeto, es decir, qué operaciones permite realizar sin revelar cómo están implementadas internamente. En Java, normalmente la interfaz pública está formada por los métodos declarados con el modificador `public`, que actúan como punto de entrada para manipular o consultar el estado del objeto.

Esta interfaz se relaciona directamente con la **ocultación de información**, ya que permite separar lo que la clase muestra al exterior de la lógica interna que mantiene oculta. Gracias a la interfaz pública, se proporciona una forma controlada de acceso a los datos internos, evitando que otras partes del programa dependan de detalles que podrían cambiar. De este modo, la implementación puede modificarse libremente sin afectar a quienes usen la clase, siempre que la interfaz pública se mantenga estable.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
La **interfaz pública** de una clase debe diseñarse con cuidado porque actúa como el contrato que otros módulos del programa utilizarán para interactuar con los objetos. Si esta interfaz es confusa, demasiado extensa o permite accesos innecesarios, el resto del software puede volverse dependiente de detalles que no deberían ser visibles. Un diseño poco pensado puede llevar a que el código sea difícil de mantener, ya que cualquier cambio interno podría afectar a múltiples partes del programa que utilizan esa interfaz.

Además, la interfaz pública determina el grado de control que se ejerce sobre el estado interno del objeto. Una interfaz mal diseñada puede permitir modificaciones indebidas, dificultar la validación de datos o incluso romper la coherencia interna del objeto. Por eso, se recomienda que la interfaz exponga únicamente las operaciones esenciales y que toda manipulación del estado pase por métodos cuidadosamente definidos.

En cuanto a la posibilidad de cambiarla, **no suele ser fácil**. Una vez que otras clases, módulos o incluso otros desarrolladores dependen de esa interfaz, modificarla implica adaptarlo todo, lo que puede ser costoso y propenso a errores. Aunque internamente la clase pueda modificarse con libertad gracias a la ocultación de información, la interfaz pública debe mantenerse estable para no comprometer la compatibilidad con el resto del sistema.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las **invariantes de clase** son condiciones lógicas que deben cumplirse siempre para que un objeto sea considerado válido durante toda su vida. Estas condiciones describen restricciones sobre sus atributos —por ejemplo, que un valor no sea negativo o que una relación entre campos se mantenga consistente— y deben cumplirse después de crear el objeto y tras la ejecución de cualquier método público. Aunque internamente pueda haber estados temporales incoherentes mientras se ejecuta un método, la invariante debe restablecerse antes de devolver el control al exterior.

La ocultación de información ayuda porque impide que código externo modifique directamente los atributos de la clase, lo que podría romper fácilmente la invariante. Al forzar que cualquier cambio pase por métodos controlados, la clase puede validar los datos y asegurar que su estado permanece coherente. Esto hace posible que la responsabilidad de mantener la invariante recaiga únicamente en la propia clase, permitiendo independencia respecto al resto del programa.

Además, al no exponer los detalles internos, la clase puede reorganizar su implementación o cambiar cómo gestiona sus datos sin que la invariante se vea comprometida desde fuera. La ocultación reduce así errores provocados por accesos indebidos y permite diseñar objetos más robustos, fiables y fáciles de mantener.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
A continuación se muestra un ejemplo sencillo de una clase `Punto` que encapsula sus datos internos mediante atributos privados y expone solo aquello que forma parte de su interfaz pública:

```java
public class Punto {

    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

En esta clase, las variables `x` e `y` son privadas, por lo que solo pueden ser accedidas desde dentro de la propia clase. Esto permite controlar cómo se crean y gestionan los valores internos, evitando que otras partes del programa los modifiquen sin pasar por los métodos definidos. La interfaz pública está formada por el constructor, los métodos `getX()`, `getY()` y el método `calcularDistanciaAOrigen()`, ya que todos ellos utilizan el modificador `public` y, por tanto, están disponibles desde fuera.

El modificador **`public`** indica que el elemento (método, constructor o atributo) es accesible desde cualquier otra clase del programa. Esto forma parte de la interfaz pública que se ofrece al usuario de la clase. Por el contrario, **`private`** oculta el elemento, haciendo que solo sea accesible desde dentro de la propia clase. Gracias a esto, se consigue aplicar la ocultación de información, ya que se protege el estado interno y se permite trabajar con el objeto sin necesidad de conocer su implementación interna.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta
En Java, los modificadores **`public`** y **`private`** se pueden aplicar principalmente a **clases**, **constructores**, **métodos** y **atributos**. Estos modificadores determinan el nivel de acceso permitido desde otras partes del programa y son fundamentales para implementar la encapsulación. En el caso de las clases, solo pueden declararse como `public` o con *acceso por defecto* (sin modificador), mientras que para métodos, atributos y constructores sí se permite usar tanto `public` como `private`. De este modo, se controla qué elementos forman parte de la interfaz pública y cuáles deben mantenerse ocultos como parte de la implementación interna.

La posibilidad de aplicar estos modificadores sobre los elementos internos de la clase permite proteger su estado y definir con claridad cómo deben usarse los objetos. Al declarar un atributo o un método como `private`, se evita que sea accesible desde fuera, lo que permite garantizar invariantes y mantener la coherencia interna. Por el contrario, declarar un elemento `public` indica que se ofrece como parte del contrato externo de la clase y que puede ser utilizado libremente por otros módulos o clases del programa. Esta separación resulta esencial para diseñar software modular, fácil de mantener y menos propenso a errores.

En resumen, `public` y `private` pueden aplicarse a **clases**, **constructores**, **métodos** y **atributos**, cumpliendo un papel clave en la construcción de la interfaz pública y en la implementación de la ocultación de información. El uso adecuado de estos modificadores permite establecer límites claros entre lo que debe exponerse y lo que debe mantenerse protegido dentro de la clase.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta
En POO existen más tipos de visibilidad además de la pública y la privada. En muchos lenguajes orientados a objetos se incluyen niveles intermedios para permitir diferentes grados de accesibilidad. Estos niveles adicionales permiten ajustar con más precisión qué partes del código pueden usar determinados elementos, especialmente cuando se trabaja con jerarquías de clases o con módulos. La existencia de varios niveles de visibilidad permite diseñar clases mejor estructuradas, donde cada elemento queda expuesto solo al ámbito que realmente lo necesita.

En Java, además de `public` y `private`, existen **dos niveles adicionales**: `protected` y el acceso **por defecto** (también llamado *package-private*). El modificador `protected` permite el acceso desde clases del mismo paquete y desde clases que hereden de la clase original, lo que resulta útil en herencia. El acceso por defecto se aplica cuando no se indica ningún modificador, permitiendo que los elementos sean visibles únicamente dentro del mismo paquete. De este modo, Java proporciona cuatro niveles de visibilidad que cubren distintos escenarios de encapsulación y modularidad.

En otros lenguajes orientados a objetos también existen categorías similares, aunque con variaciones. Por ejemplo, en C++ se dispone de `public`, `private` y `protected`, pero sin un equivalente directo al acceso por paquete, ya que su sistema de organización es distinto. Lenguajes como C# incluyen `internal` (similar al acceso por defecto de Java) y `protected internal`, que amplían las posibilidades de combinación entre herencia y módulos. Estas diferencias muestran cómo cada lenguaje adapta la visibilidad a su propio modelo de organización y modularidad.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
Los miembros de instancia declarados como **privados** están ocultos para **otras clases**, pero **no** están ocultos para **otras instancias de la *misma* clase**. Esto significa que, aunque desde fuera de la clase nadie pueda acceder directamente a esos atributos, una instancia sí puede acceder a los atributos privados de otra instancia siempre que ambas pertenezcan a la misma clase. Esta propiedad se debe a que el control de visibilidad se aplica por *clase*, no por *objeto*, permitiendo que los métodos internos trabajen libremente con los datos privados de cualquier objeto del mismo tipo.

Este comportamiento resulta útil cuando se implementan métodos que comparan o relacionan objetos entre sí, ya que permite acceder directamente a sus atributos privados sin necesidad de exponerlos mediante getters si no se considera oportuno. Así, se mantiene la ocultación de información respecto al exterior, pero se conserva la libertad de implementación dentro de la propia clase. Este diseño facilita operaciones complejas entre instancias sin debilitar la encapsulación.

Ejemplo ampliando la clase `Punto` con el método solicitado:

```java
public class Punto {

    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;   // Acceso permitido: mismo tipo de clase
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En este ejemplo, el método `calcularDistanciaAPunto` puede acceder a `otro.x` y `otro.y` a pesar de que dichos atributos son privados, porque ambos objetos son instancias de la clase `Punto`. Si se intentara acceder a esos atributos desde otra clase distinta, el compilador lo impediría. Así, se confirma que los miembros privados están ocultos frente a *otras clases*, pero no frente a *otras instancias de la misma clase*.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Los métodos **getter** y **setter** son funciones que permiten acceder y modificar, respectivamente, los atributos privados de una clase manteniendo la encapsulación. Un *getter* devuelve el valor de un atributo, mientras que un *setter* permite asignarle un nuevo valor. Ambos actúan como una capa de control entre el exterior y los datos internos del objeto, ya que no se accede directamente a los atributos, sino que se hace a través de estos métodos definidos por la propia clase.

El uso de getters y setters permite validar o restringir los valores antes de modificar un atributo, garantizando así que las invariantes de la clase se mantengan intactas. Además, facilitan cambiar la implementación interna en el futuro sin afectar al código que usa la clase, porque la interfaz pública permanece estable. De esta forma, se consigue un equilibrio entre ocultación de información y flexibilidad en el diseño.

En muchos lenguajes orientados a objetos, incluido Java, este patrón se considera una práctica habitual cuando se desea controlar el acceso a atributos que no deben ser públicos. Gracias a estos métodos, la clase conserva el control total sobre cómo se leen y escriben sus datos, reforzando la encapsulación y la coherencia interna del objeto.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta
No, cuando se afirma que la **ocultación de información mejora la “seguridad” del programa**, no se hace referencia a evitar que el software sea “hackeado” en el sentido de seguridad informática o ciberataques. El término se usa en un sentido más limitado y propio del diseño de software, relacionado con la integridad y coherencia interna de los objetos. La idea es impedir que otras partes del programa manipulen directamente los atributos internos, evitando errores lógicos, estados inválidos o comportamientos inesperados que puedan comprometer el funcionamiento correcto del sistema.

La “seguridad” a la que se alude en POO se refiere, por tanto, a que el objeto mantiene el control completo sobre su estado interno. Gracias a la ocultación, cualquier modificación debe hacerse a través de métodos bien definidos, donde es posible validar datos y preservar invariantes. Esto permite que la clase actúe como un mediador que garantiza que solo se produzcan cambios permitidos y que su estado nunca se corrompa. Aunque no protege contra ataques externos, sí protege contra usos incorrectos dentro del propio código.

Finalmente, este enfoque conduce a programas más robustos y fáciles de mantener, ya que evita dependencias innecesarias y reduce la posibilidad de introducir fallos por cambios no controlados. La “seguridad” mencionada debe entenderse, por tanto, como una protección interna del diseño orientado a objetos, y no como un mecanismo de defensa frente a amenazas externas o de red.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
Un **miembro de instancia** es un atributo o método cuyo valor pertenece a cada objeto individual creado a partir de una clase. Esto significa que cada instancia mantiene su propia copia de esos miembros, permitiendo que distintos objetos tengan estados diferentes entre sí. En Java, estos miembros se crean cuando se instancia el objeto con `new`, y solo existen mientras ese objeto esté vivo. Por ejemplo, las coordenadas `x` e `y` de un `Punto` son miembros de instancia porque cada punto tiene sus propios valores independientes.

Por el contrario, un **miembro de clase** (declarado con la palabra clave `static`) no pertenece a cada objeto, sino a la clase en sí. Esto implica que hay una única copia compartida por todas las instancias, y que incluso puede accederse a él sin necesidad de crear ningún objeto. Los miembros de clase se suelen utilizar para valores constantes, contadores globales o utilidades que no dependen del estado específico de un objeto. Este tipo de miembros existe desde que la clase se carga en memoria y permanece accesible mientras el programa se esté ejecutando.

Respecto a la visibilidad, los **miembros de clase también se pueden ocultar** mediante los mismos modificadores que los miembros de instancia, como `private`, `public` o `protected`. Esto permite controlar si ese miembro está accesible solo desde dentro de la clase, desde otras clases del mismo paquete o desde cualquier lugar del programa. Al igual que ocurre con los atributos de instancia, declarar un miembro de clase como `private` es una forma de proteger la implementación interna y evitar usos indebidos desde fuera.

En conjunto, la distinción entre miembros de instancia y miembros de clase permite modelar tanto el comportamiento individual de cada objeto como características compartidas entre todas las instancias. La posibilidad de ocultar ambos tipos contribuye a mantener la encapsulación y a estructurar mejor el diseño orientado a objetos.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta
Sí, tiene sentido que los constructores sean **privados** en ciertos patrones y diseños, aunque no sea lo más habitual en la mayoría de las clases. Un constructor privado impide crear objetos desde fuera de la propia clase, lo que significa que la clase controla completamente cómo y cuándo se crean sus instancias. Este enfoque resulta útil cuando se quiere restringir la creación de objetos para mantener un conjunto limitado o bien definido de instancias válidas.

Un caso típico donde los constructores privados tienen sentido es en el **patrón Singleton**, donde se asegura que solo exista una única instancia de la clase en todo el programa. Al declarar el constructor como privado, se obliga a que la clase proporcione un método público que gestione la creación y entrega de la única instancia. También se emplean constructores privados en clases que solo contienen métodos estáticos (por ejemplo, clases de utilidades), evitando así que se creen objetos sin necesidad.

Además, los constructores privados pueden utilizarse en conjunción con **métodos factoría** (`factory methods`), donde la clase ofrece métodos estáticos que devuelven instancias construidas de manera controlada. Esto permite abstraer la creación de objetos y ofrecer diferentes formas de inicialización sin exponer directamente el proceso de construcción. En estos casos, el constructor privado ayuda a reforzar la encapsulación y a asegurar que los objetos se creen siempre de forma coherente.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta
En Java, los **miembros de clase** se indican con la palabra clave `static`. Esto hace que el miembro pertenezca a la *clase* y no a cada instancia concreta: hay **una única copia compartida** por todos los objetos. Se accede a ellos preferentemente mediante el nombre de la clase (por ejemplo, `Punto.getMaxX()`), aunque también pueden usarse desde instancias. Son útiles para almacenar información global o agregada, así como utilidades que no dependen del estado de un objeto específico.

Ejemplo que amplía la clase `Punto` para mantener, a nivel de clase, los valores máximos de `x` e `y` observados entre **todas** las instancias creadas hasta el momento:

```java
public class Punto {

    // Miembros de instancia (estado propio de cada objeto)
    private double x;
    private double y;

    // Miembros de clase (compartidos por todas las instancias)
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximos(x, y);
    }

    public double getX() { return x; }
    public double getY() { return y; }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Getters estáticos (parte de la interfaz pública de clase)
    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    // Método de utilidad privado (de clase) para actualizar agregados
    private static void actualizarMaximos(double x, double y) {
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    // Si en el futuro se añaden setters, deberían actualizar los máximos:
    // public void setX(double x) { this.x = x; actualizarMaximos(this.x, this.y); }
    // public void setY(double y) { this.y = y; actualizarMaximos(this.x, this.y); }
}
```

En el ejemplo, `maxX` y `maxY` son **miembros de clase** porque están marcados como `static`; se inicializan a `Double.NEGATIVE_INFINITY` para que cualquier primer valor real los supere. El constructor actualiza estos agregados llamando a un método de clase privado `actualizarMaximos`. La **interfaz pública** relacionada con estos miembros de clase la componen los métodos `public static` `getMaxX()` y `getMaxY()`, que exponen la información de forma controlada sin permitir modificaciones externas, manteniendo así la ocultación de información. Si en el futuro se permitiese cambiar `x` o `y` tras la construcción, los *setters* deberían encargarse de mantener dichos máximos coherentes.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
```java
public static Punto crearRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}
```

Sí, en este tipo de métodos **se usa `static`**, porque un método factoría pertenece a la **clase**, no a una instancia concreta. Su función es producir objetos nuevos, y para ello no necesita que exista previamente un objeto `Punto`.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta
A continuación se muestra una posible **nueva implementación interna** de la clase `Punto` utilizando un **array privado de dos posiciones**, pero **sin modificar la interfaz pública** que ya existía (constructor, getters y método de distancia). La idea es mantener intacto todo lo que el exterior ve, cambiando únicamente la representación interna.

```java
public class Punto {

    // Nueva representación interna: un array con dos posiciones
    private double[] coords = new double[2];

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double getX() {
        return coords[0];
    }

    public double getY() {
        return coords[1];
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
    }
}
```

Esta modificación mantiene la **interfaz pública completamente intacta**, lo que significa que ningún código externo necesita cambiar para seguir usando la clase. Esto demuestra el poder de la **ocultación de información**, ya que permite cambiar la implementación interna sin afectar a los usuarios de la clase. A pesar de haber sustituido dos atributos individuales por un array, la lógica pública sigue comportándose exactamente igual y es accesible mediante los mismos métodos que antes.

Además, esta técnica permite reorganizar o extender la representación interna si fuera necesario en versiones futuras, sin comprometer la compatibilidad. Mientras la interfaz permanezca estable y las invariantes se preserven, la implementación puede evolucionar libremente dentro de la clase, reforzando así la robustez y modularidad del diseño orientado a objetos.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta
Aunque parezca que si un atributo tiene *getter* y *setter* públicos sería más sencillo declararlo directamente público, en realidad no es así. Declarar un atributo como público expone completamente su representación interna, lo que impide controlar cómo se accede o modifica. En cambio, incluso cuando existen getters y setters, la clase conserva la capacidad de validar datos, transformar valores o mantener invariantes. Con un atributo público, nada de esto sería posible y cualquier parte del programa podría modificar el estado sin restricciones, rompiendo fácilmente la coherencia interna del objeto.

La convención más habitual en Java —y en la mayoría de lenguajes orientados a objetos— es que **los atributos sean privados**. Esta práctica forma parte de la filosofía de la encapsulación: los datos deben estar protegidos y cualquier acceso debe pasar por los métodos que la clase pone a disposición. Los getters y setters permiten definir un punto de control, y aunque no siempre es necesario crear setters (muchas clases deberían ser inmutables), el hecho de tenerlos sigue siendo preferible a exponer directamente el atributo.

Esto se relaciona directamente con las **invariantes de clase**, que representan las condiciones que deben cumplirse para que un objeto sea válido. Si los atributos fueran públicos, cualquier código externo podría cambiarlos sin respetar esas invariantes, dejando al objeto en un estado inconsistente. Al obligar a que los cambios pasen por métodos bien definidos, es posible verificar que los valores cumplen las restricciones necesarias. De esta manera, la encapsulación y la ocultación de información son herramientas clave para proteger las invariantes y asegurar que los objetos mantienen siempre un estado coherente.

Por tanto, incluso si se proporciona acceso público mediante getters y setters, lo habitual y más seguro sigue siendo mantener los atributos privados. Esto garantiza que el diseño sea más robusto, flexible y menos propenso a errores, manteniendo la capacidad de adaptar la implementación interna sin afectar al código exterior.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta
Una clase **inmutable** es aquella cuyo estado no puede cambiar una vez que el objeto ha sido creado. Esto implica que todos los atributos deben establecerse en el constructor y que no existan métodos que permitan modificar ese estado posteriormente. En una clase inmutable, cada “modificación” se logra devolviendo un **nuevo objeto** con los valores actualizados, lo que garantiza que las instancias originales permanezcan siempre consistentes. Esta propiedad facilita el razonamiento sobre el comportamiento del programa, ya que un objeto nunca cambia una vez construido.

Un **método modificador** es un método que altera el estado interno del objeto, habitualmente cambiando el valor de uno o varios atributos. Aunque los setters son un tipo de método modificador, no todos los métodos modificadores son setters: cualquier método que cambie el estado, aunque no se llame `set...`, entra en esta categoría. Por ejemplo, un método como `mover(double dx, double dy)` que actualice coordenadas sería también un método modificador, aun sin ser un setter tradicional.

Las clases inmutables tienen varias ventajas. Por un lado, favorecen la seguridad y la simplicidad, ya que no pueden quedar en estados inconsistentes: una vez construido, el objeto cumple siempre su invariante. Por otro lado, se evitan muchos problemas de concurrencia, ya que los objetos inmutables pueden compartirse entre hilos sin necesidad de sincronización. Además, facilitan la reutilización y el razonamiento, al comportarse como valores y no como entidades mutables que cambian con el tiempo.

Por estos motivos, muchos lenguajes y bibliotecas recomiendan favorecer la inmutabilidad siempre que sea posible. Aunque no todas las clases pueden ser inmutables por naturaleza, el diseño inmutable reduce errores, simplifica el mantenimiento y encaja bien en estilos modernos de programación donde se prefieren estructuras que no cambian una vez creadas.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta
No, no es recomendable incluir métodos *setter* siempre ni como una convención automática. La decisión de ofrecer setters debería basarse en si realmente tiene sentido permitir cambios en el estado de la clase. Incluir setters sin necesidad puede debilitar la encapsulación, ya que cualquier parte del programa podría modificar atributos de forma indiscriminada, dificultando el mantenimiento de las invariantes de clase y aumentando la probabilidad de introducir estados inválidos. Por ello, los setters deben añadirse solo cuando exista una razón clara para permitir la mutación.

La convención más habitual en diseño orientado a objetos es que **los atributos sean privados** y que solo se añadan setters cuando la modificación del estado sea parte esencial del comportamiento de la clase. Muchas clases no deberían ser modificables después de su creación, especialmente aquellas que representan valores simples, como puntos, fechas o configuraciones. En estos casos, resulta preferible ofrecer únicamente getters o incluso diseñar la clase como inmutable, lo que elimina por completo la necesidad de setters.

Además, prescindir de setters favorece el mantenimiento de las **invariantes de clase**, ya que cualquier cambio en el estado debe pasar por métodos que pueden validar, comprobar restricciones, o actualizar información dependiente. En cambio, un setter público que asigna directamente un valor puede saltarse estas validaciones, exponiendo al objeto a inconsistencias. Controlar cuidadosamente qué métodos permiten modificar el estado ayuda a mantener la coherencia interna y reduce errores difíciles de rastrear.

En resumen, los setters no deben añadirse por defecto. Lo adecuado es analizarlos caso por caso y proporcionarlos solo cuando la mutación del estado sea necesaria y segura dentro del diseño de la clase. Esta práctica conduce a objetos más robustos, más fáciles de razonar y menos propensos a errores.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta
La clase **`String`** en Java es **inmutable**, lo que significa que una vez creada una cadena, su contenido no puede modificarse. Cada operación que parezca modificar una cadena—como concatenar, sustituir o convertir a mayúsculas—en realidad genera **un nuevo objeto `String`** con el resultado. Este diseño garantiza que las cadenas sean seguras, eficientes de compartir entre distintos lugares del programa y libres de efectos laterales, lo que simplifica mucho el razonamiento sobre su estado.

Cuando se **concatenan dos cadenas**, Java crea **un nuevo objeto `String`** que contiene el resultado de la concatenación. La cadena original permanece intacta, por lo que cada operación implica asignar memoria adicional para el nuevo objeto. Si se concatenan pocas veces, este coste es despreciable; sin embargo, cuando las concatenaciones se repiten muchas veces dentro de un bucle, el programa puede volverse ineficiente debido a la creación continua de nuevos objetos y al trabajo extra del recolector de basura.

Si se prevé realizar muchas concatenaciones para construir una cadena larga paso a paso, lo recomendable es usar **`StringBuilder`**, que es una clase **mutable** diseñada específicamente para estas situaciones. Con `StringBuilder`, las operaciones de añadir texto se realizan sobre el mismo objeto, sin necesidad de crear uno nuevo en cada paso, lo que mejora el rendimiento de manera significativa. Una vez finalizada la construcción, se puede convertir a `String` mediante el método `toString()`, obteniendo así un valor inmutable adecuado para el uso externo.

En resumen, `String` es inmutable, la concatenación crea nuevos objetos y, para operaciones repetidas de construcción de cadenas, la opción más eficiente y recomendada es utilizar `StringBuilder`.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta
En POO, la comparación de objetos puede hacerse por **identidad** o por **contenido**, pero no son lo mismo. La **identidad** se refiere a si dos referencias apuntan exactamente al mismo objeto en memoria; en Java, esta comparación se realiza con el operador `==`. En cambio, comparar por **contenido** implica comprobar si los valores internos de los objetos son equivalentes, lo cual depende de la lógica específica de la clase. Para ello, los lenguajes orientados a objetos suelen proporcionar mecanismos que permiten definir criterios propios de igualdad, adaptados a lo que cada clase representa.

En Java, este criterio de igualdad por contenido se implementa mediante el método **`equals`**, heredado de la clase base `Object`. El método `equals` sirve para indicar si dos objetos deben considerarse “equivalentes” según la semántica que la clase defina. **Por defecto**, la implementación de `equals` en `Object` se comporta igual que `==`, es decir, compara únicamente la identidad del objeto. Por ello, si una clase necesita comparar objetos por su estado interno, debe **sobrescribir** `equals` y proporcionar una definición adecuada de igualdad que tenga en cuenta sus atributos relevantes.

Con respecto a las cadenas, en Java **dos `String` deben compararse usando `equals`**, ya que se desea comprobar si contienen la misma secuencia de caracteres. Aunque `String` es una clase inmutable y optimizada, el operador `==` solo indicaría si ambas referencias apuntan al mismo objeto, lo cual no siempre coincide con tener el mismo contenido. Por esta razón, la forma correcta de comparar cadenas es mediante `cadena1.equals(cadena2)`, que evalúa carácter por carácter y determina si ambas representan el mismo texto.

En conjunto, Java distingue claramente entre comparar referencias (identidad) y comparar valores (contenido). Gracias a `equals`, las clases pueden definir qué significa realmente que dos objetos sean iguales, lo que permite construir programas más coherentes y acordes con la lógica que representan las abstracciones del sistema.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta
Las **clases *wrapper*** son clases que encapsulan un valor de un tipo primitivo dentro de un objeto, permitiendo tratar ese valor como una entidad de la programación orientada a objetos. En Java, existen wrappers para cada tipo primitivo (`Integer`, `Double`, `Boolean`, etc.), proporcionando métodos y comportamiento adicional que los tipos primitivos no tienen. Su propósito es integrar valores simples dentro del modelo orientado a objetos, permitiendo almacenarlos en colecciones genéricas, pasarlos como objetos o beneficiarse de métodos utilitarios específicos de cada clase.

En Java, este proceso puede hacerse de forma explícita creando una instancia del wrapper, pero también puede hacerse de forma automática gracias a **autoboxing** y **unboxing**. El autoboxing convierte automáticamente un valor primitivo en su wrapper correspondiente, mientras que el unboxing realiza la conversión inversa. Este mecanismo permite escribir código más natural, ya que se evita crear objetos manualmente cuando se trabaja con colecciones o APIs orientadas a objetos. Aunque es cómodo, sigue implicando una conversión real, por lo que puede impactar en el rendimiento si se abusa de él.

Las clases wrapper aportan varias ventajas. Permiten almacenar valores primitivos en estructuras genéricas como `ArrayList`, que solo aceptan objetos. También añaden métodos útiles, como conversiones, comparaciones o utilidades específicas del tipo. Además, facilitan el trabajo en entornos donde se requieren objetos, como en funciones que operan sobre tipos genéricos o frameworks que exigen tipos de referencia. Estas capacidades no están disponibles para los tipos primitivos, lo que convierte a los wrappers en herramientas esenciales de muchos lenguajes con distinción entre primitivos y objetos.

No todos los lenguajes orientados a objetos tienen tipos primitivos ni necesitan wrappers. Algunos lenguajes, como Python o Ruby, tratan todos los valores como objetos desde el principio, por lo que no existe esta separación. Otros lenguajes, como Java o C#, sí mantienen tipos primitivos por razones de eficiencia, y necesitan clases o estructuras equivalentes para trabajar en sistemas donde se requiere un tipo de referencia. Por tanto, el uso de wrappers depende del diseño y modelo de tipos de cada lenguaje, y no es una característica universal de la programación orientada a objetos.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta
En POO, un **tipo de dato enumerado** representa un conjunto finito y cerrado de valores posibles, cada uno de los cuales se considera una constante con un significado específico dentro del dominio del problema. Este tipo de dato permite modelar situaciones donde solo se admite un conjunto fijo de opciones —por ejemplo, los días de la semana, los colores básicos o los estados de un proceso— evitando valores inválidos. Los enumerados aportan claridad y seguridad al código, al impedir que se utilicen valores arbitrarios fuera del conjunto permitido.

En Java, un tipo enumerado (`enum`) es realmente una **clase especial**, aunque el lenguaje ofrece una sintaxis más simple para declararlo. Internamente, cada valor del enum es una **instancia única** (inmutable) de dicha clase. Esto significa que los enumerados pueden tener atributos, métodos, constructores privados e incluso comportamientos especializados, lo que los convierte en herramientas más potentes que los simples enumerados de lenguajes como C. A pesar de su apariencia sencilla, un enum en Java participa plenamente del modelo orientado a objetos.

En términos de encapsulación, los enumerados en Java ofrecen ventajas importantes. Al tratarse de una clase cerrada, solo pueden existir los valores declarados dentro del propio enum, lo que garantiza que nunca se generen valores inválidos. Además, el diseño interno del enum está completamente oculto al exterior: el programador utiliza únicamente las constantes definidas y los métodos públicos que la clase enum exponga. Esto permite añadir comportamiento o datos asociados a cada valor sin modificar el código que usa el enumerado. De esta manera, los enumerados aseguran un control total del dominio de valores, evitando errores y fortaleciendo la coherencia del diseño.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
Aquí tienes una implementación completa del tipo enumerado `Mes` en Java, siguiendo los requisitos: doce instancias, atributos privados, constructor privado, métodos para días, ordinal, y métodos estacionales considerando hemisferios.

He mantenido la implementación **clara, estructurada y orientada a objetos**, aprovechando que un `enum` en Java es realmente una **clase especial** con instancias predefinidas.

***

## **Código del tipo enumerado `Mes`**

```java
public enum Mes {

    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    private final int dias;
    private final int ordinalMes;

    private Mes(int dias, int ordinalMes) {
        this.dias = dias;
        this.ordinalMes = ordinalMes;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinalMes() {
        return ordinalMes;
    }

    public boolean esDePrimavera(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == MARZO || this == ABRIL || this == MAYO;
        } else {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        }
    }

    public boolean esDeVerano(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        } else {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        }
    }

    public boolean esDeOtono(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        } else {
            return this == MARZO || this == ABRIL || this == MAYO;
        }
    }

    public boolean esDeInvierno(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        } else {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        }
    }
}
```

***

### ✔ Características cumplidas

*   **12 instancias**, una por cada mes del año.
*   **Atributos privados**:
    *   `dias` → número de días del mes (supuesto sin años bisiestos).
    *   `ordinalMes` → posición del mes en el año (1–12).
*   **Constructor privado** (obligatorio en enums).
*   **Métodos públicos**:
    *   `getDias()`
    *   `getOrdinalMes()`
    *   Métodos estacionales según hemisferio:
        *   `esDePrimavera(boolean)`
        *   `esDeVerano(boolean)`
        *   `esDeOtono(boolean)`
        *   `esDeInvierno(boolean)`
