<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta
En orientación a objetos, la **herencia** es un mecanismo mediante el cual una clase nueva se define a partir de otra ya existente, reutilizando sus características. Conceptualmente, la herencia expresa una relación de tipo **“A es-un B”** (*is‑a*), lo que significa que una instancia de la subclase puede considerarse también una instancia de la superclase. Por ejemplo, si `Artillero` hereda de `Soldado`, se afirma que *un artillero es un soldado*. Esta relación no es meramente descriptiva, sino que tiene consecuencias técnicas concretas en el lenguaje.

La primera implicación importante es la **compatibilidad de tipos**. Cuando una clase hereda de otra, el subtipo es compatible con el tipo de la superclase, lo que permite tratar objetos más específicos como si fueran más generales. En Java, esto significa que una variable de tipo `Soldado` puede referenciar un objeto `Artillero` o `Zapador` sin necesidad de conversiones explícitas. Esta compatibilidad es la base del polimorfismo y permite escribir código más genérico, por ejemplo al almacenar distintos tipos de soldados en una misma estructura de datos basada en la superclase.

La segunda implicación es la **herencia de estado y comportamiento**. Las subclases heredan los atributos y métodos accesibles de la superclase, evitando duplicación de código. En este caso, tanto `Artillero` como `Zapador` heredan el atributo privado `nombre` y el método `saludar()`, que define un comportamiento común a todos los soldados. Cada subtipo añade su propio estado específico (cohetes o minas) y sus métodos de acceso, manteniendo el comportamiento común en la clase base.

```java
public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}
```

```java
Soldado[] ejercito = new Soldado[3];
ejercito[0] = new Artillero("Luis", 5);
ejercito[1] = new Zapador("Ana", 3);
ejercito[2] = new Artillero("Carlos", 8);

for (Soldado s : ejercito) {
    s.saludar();
}
```


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta
Al crear un objeto de una clase que **hereda** de otra, **no se ejecuta un único constructor**, sino **varios**, uno por cada clase de la jerarquía. En concreto, al instanciar un `Artillero` o un `Zapador`, se ejecutan **dos constructores**: primero el constructor de la **clase base (`Soldado`)** y después el constructor de la **subclase concreta**. Este orden es obligatorio y siempre va **de arriba hacia abajo en la jerarquía**, garantizando que el estado común definido en la superclase quede correctamente inicializado antes de inicializar la parte específica del subtipo.

La palabra clave `super` dentro de un constructor se utiliza precisamente para **invocar explícitamente un constructor de la clase base**. Mediante `super(...)` se indican los parámetros que necesita el constructor de la superclase y se fuerza su ejecución antes de que continúe el constructor de la subclase. Conceptualmente, `super` representa *la parte del objeto que pertenece a la clase padre*, y su llamada asegura que dicha parte se construye correctamente. En Java, la llamada a `super(...)` debe aparecer siempre como **primera instrucción del constructor**.

Si no se escribe ninguna llamada a `super(...)`, el compilador intentará insertar automáticamente una llamada a `super()` **sin parámetros**. Esto solo es válido si la clase base tiene un constructor sin argumentos que sea accesible. Si la clase base **no dispone de un constructor sin parámetros visible** (por ejemplo, solo tiene constructores con argumentos), entonces **es obligatorio llamar explícitamente a `super`** desde todos los constructores de la subclase, pasando los parámetros adecuados.

Por tanto, cuando la clase base define constructores parametrizados para inicializar su estado (como ocurre con `Soldado` y el atributo `nombre`), la subclase no puede ignorarlos. La subclase es responsable de proporcionar esos valores y llamar a `super`, ya que sin esa llamada explícita el código no compila. Esta exigencia refuerza la idea de que la subclase no sustituye a la superclase, sino que **la amplía y depende de una correcta inicialización previa**.


## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta
Sí, los **atributos privados de la superclase forman parte físicamente de la instancia de la subclase en memoria**. Cuando se crea un objeto de una subclase, este objeto contiene **toda la estructura definida por la superclase**, además de la estructura adicional definida por la subclase. En otras palabras, un objeto `Artillero` incluye internamente el atributo `nombre` declarado en `Soldado`, aunque dicho atributo sea `private`. No existen “dos objetos separados”, sino **un único objeto cuya construcción se reparte entre varias clases de la jerarquía**.

Ahora bien, que un atributo forme parte de la instancia **no implica que sea accesible desde cualquier punto del código**. El modificador `private` limita el acceso al **código de la propia clase donde se declara**, no a la existencia del dato en memoria. Por ello, aunque `Artillero` herede de `Soldado`, el código de `Artillero` **no puede acceder directamente** al atributo `nombre`, ya que este fue declarado como privado en la superclase. Esta restricción es puramente de visibilidad y se comprueba en tiempo de compilación.

En el ejemplo de `Soldado`, el atributo `nombre` puede usarse sin problemas dentro de métodos definidos en `Soldado`, como `saludar()`, y esos métodos funcionan correctamente también cuando se invocan sobre objetos `Artillero` o `Zapador`. Esto ocurre porque el método `saludar()` pertenece a la superclase y tiene derecho a acceder a sus propios atributos privados, independientemente del tipo concreto del objeto que lo ejecuta.

```java
public class Artillero extends Soldado {
    public Artillero(String nombre, int cohetes) {
        super(nombre);
        // nombre = "Pedro"; // ERROR: nombre es private en Soldado
    }
}
```

Por tanto, la herencia garantiza que el **estado privado de la superclase existe y se mantiene** en cada instancia de las subclases, pero al mismo tiempo la encapsulación asegura que ese estado **solo pueda manipularse a través de los métodos definidos o expuestos por la superclase**, preservando así la seguridad y coherencia del diseño orientado a objetos.


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta
La **compatibilidad a nivel de tipos** que proporciona la herencia tiene una consecuencia directa sobre la **extensibilidad del código**. Permite que el código cliente trabaje con referencias de la superclase sin necesidad de conocer los tipos concretos de los objetos que maneja. De este modo, el sistema queda **abierto a la incorporación de nuevos subtipos** sin que sea necesario modificar el código ya escrito que opera sobre la superclase. Este principio reduce el acoplamiento y favorece diseños más mantenibles.

Gracias a esta compatibilidad, cualquier clase nueva que herede de `Soldado` podrá ser tratada como un `Soldado` más. El código que itera sobre un conjunto de soldados y solicita que saluden no depende de si el objeto es un `Artillero`, un `Zapador` u otro subtipo futuro. Basta con que el nuevo tipo respete la relación *“es‑un Soldado”* y herede su comportamiento común. En consecuencia, **no es necesario añadir condicionales ni modificar bucles existentes**, lo que evita errores y duplicaciones.

Por ejemplo, puede añadirse un nuevo tipo `Medico` que también sea un `Soldado`, con su propio estado específico. El código encargado de recorrer el ejército y pedir el saludo permanece exactamente igual, demostrando que el sistema es extensible por adición y no por modificación.

```java
public class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
}
```

```java
Soldado[] ejercito = new Soldado[4];
ejercito[0] = new Artillero("Luis", 5);
ejercito[1] = new Zapador("Ana", 3);
ejercito[2] = new Medico("Eva", 2);
ejercito[3] = new Artillero("Carlos", 8);

for (Soldado s : ejercito) {
    s.saludar(); // no se modifica aunque aparezcan nuevos tipos
}
```

En términos de diseño orientado a objetos, este comportamiento ilustra cómo la herencia y la compatibilidad de tipos permiten **evolucionar el sistema de forma controlada**, añadiendo nuevas funcionalidades mediante nuevas clases, sin necesidad de reescribir el código que ya funciona correctamente.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta
En Java es perfectamente posible que una **referencia del supertipo apunte a un objeto real de un subtipo**. Esto es consecuencia directa de la compatibilidad de tipos introducida por la herencia. Por ejemplo, una variable de tipo `Soldado` puede referenciar un objeto `Artillero` o `Zapador` sin problema alguno. Este uso es habitual y recomendable, ya que permite escribir código más genérico y desacoplado del tipo concreto del objeto que se está manipulando.

Sin embargo, al utilizar una referencia del supertipo, **solo pueden invocarse los métodos definidos en el supertipo**, aunque el objeto real sea de una subclase. Esto se debe a que el compilador verifica las llamadas a métodos en función del **tipo de la referencia**, no del tipo real del objeto. Por tanto, no es posible llamar directamente a métodos públicos que existen únicamente en el subtipo (como `getCohetes()`), a menos que se realice una conversión explícita al tipo adecuado.

El término **upcasting** se refiere a la conversión de una referencia de un subtipo a una referencia de su supertipo. Este proceso es **implícito y seguro**, ya que siempre es válido tratar un objeto concreto como uno más general. El **downcasting**, en cambio, consiste en convertir una referencia del supertipo a una referencia de un subtipo concreto. Este proceso es **explícito y potencialmente peligroso**, ya que solo es válido si el objeto real pertenece efectivamente a ese subtipo; de lo contrario, se produce una excepción en tiempo de ejecución.

Para evitar errores en el downcasting, Java proporciona el operador `instanceof`, que permite comprobar el tipo real del objeto antes de convertirlo. En el recorrido de un array de `Soldado`, puede comprobarse si el objeto es un `Artillero` y, solo en ese caso, acceder a su funcionalidad específica de forma segura.

```java
Soldado[] ejercito = new Soldado[3];
ejercito[0] = new Artillero("Luis", 5);
ejercito[1] = new Zapador("Ana", 3);
ejercito[2] = new Artillero("Carlos", 8);

for (Soldado s : ejercito) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // downcasting seguro
        System.out.println("Cohetes: " + a.getCohetes());
    }
}
```

Este mecanismo permite combinar **generalidad y comportamiento específico** sin renunciar a la seguridad, siempre que el uso de `instanceof` y del downcasting se haga de forma controlada y justificada.


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta
El acceso **protegido** es un mecanismo intermedio de ocultación de información que se sitúa entre `private` y `public`. Un atributo o método protegido **no es accesible desde cualquier clase**, pero **sí puede ser usado directamente por las subclases**, incluso aunque estas estén en otro paquete. De este modo, se permite que las clases hijas reutilicen o amplíen parte del estado o comportamiento interno de la superclase sin exponerlo completamente al exterior.

En Java, el acceso protegido se implementa mediante la palabra clave `protected`. Un miembro declarado como protegido puede ser utilizado por: (1) cualquier clase del **mismo paquete**, y (2) cualquier **subclase**, esté donde esté. Esto resulta especialmente útil cuando se desea que ciertas partes del estado interno formen parte del “contrato” entre la superclase y sus subclases, pero no se quiere que dicho estado sea accesible desde código ajeno a la jerarquía.

Aplicado al ejemplo, si el atributo `nombre` de `Soldado` se declara como `protected`, entonces una subclase como `Zapador` puede acceder directamente a él y usarlo en su propio comportamiento específico. Esto no significa romper la encapsulación, sino **relajarla de forma controlada** dentro de la jerarquía de herencia, permitiendo mayor flexibilidad en las clases derivadas sin hacer el atributo público.

```java
public class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}
```

```java
public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerMina() {
        System.out.println(nombre + " ha puesto una mina");
        minas--;
    }

    public int getMinas() {
        return minas;
    }
}
```

En este caso, `Zapador` puede usar directamente `nombre` gracias a su visibilidad protegida, mientras que dicho atributo sigue sin ser accesible desde código externo que no pertenezca a la jerarquía de `Soldado`. Esto muestra cómo el acceso `protected` facilita la cooperación entre superclases y subclases manteniendo un buen nivel de encapsulación.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
En los lenguajes orientados a objetos suele plantearse la idea de una **clase base común** de la que, directa o indirectamente, derivan todos los objetos del sistema. Esta clase raíz proporciona un conjunto mínimo de comportamientos y propiedades compartidas, como la identidad del objeto o la posibilidad de compararlo con otros. No obstante, **no todos los lenguajes orientados a objetos imponen obligatoriamente la existencia de una clase base universal**, y su presencia depende del diseño y las decisiones del propio lenguaje.

En algunos lenguajes orientados a objetos, especialmente aquellos que nacieron como extensiones de lenguajes procedimentales, puede no existir una clase raíz explícita o esta puede ser opcional. En otros casos, aunque conceptualmente todo sea un objeto, el programador no interactúa directamente con una superclase común. Por tanto, la existencia de una clase base para todos los objetos **no es una característica universal de la orientación a objetos**, sino una elección concreta de cada lenguaje.

En **Java**, sí existe una clase base común: `java.lang.Object`. Todas las clases definidas en Java **heredan directa o indirectamente de `Object`**, incluso aquellas que no especifican `extends` de forma explícita. Esto significa que cualquier objeto en Java es, como mínimo, un `Object`, y puede utilizar los métodos que esta clase define, como `toString()`, `equals()` o `hashCode()`.

Esta decisión de diseño tiene implicaciones prácticas importantes. Gracias a `Object`, Java garantiza un **comportamiento mínimo común** para todos los objetos y permite escribir código muy genérico, por ejemplo, trabajar con colecciones de `Object` o definir métodos que acepten cualquier tipo de objeto. Al mismo tiempo, esta herencia es transparente para el programador principiante, ya que no es necesario conocer `Object` en detalle para utilizar clases y herencia correctamente en los primeros niveles del aprendizaje.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
La **herencia múltiple** es una característica de algunos lenguajes orientados a objetos que permite que una clase **herede directamente de más de una clase base**. Es decir, una clase puede ser subtipo de varias superclases al mismo tiempo, adquiriendo estado y comportamiento de todas ellas. Conceptualmente, esto permitiría modelar situaciones donde *un objeto es‑un A y es‑un B* simultáneamente, heredando atributos y métodos de ambas jerarquías.

Aunque la herencia múltiple puede resultar expresiva, también introduce **problemas importantes de complejidad**, como conflictos de nombres de métodos o atributos y ambigüedades en la jerarquía (por ejemplo, el conocido “problema del diamante”, donde una clase hereda dos veces el mismo método a través de caminos distintos). Estos problemas dificultan tanto la implementación del lenguaje como el razonamiento sobre el código, especialmente en sistemas grandes.

En **Java no existe herencia múltiple de clases**. Una clase solo puede extender **una única clase base** mediante `extends`. Esta es una decisión deliberada del diseño del lenguaje para mantener la simplicidad del modelo de herencia y evitar las ambigüedades mencionadas. Por tanto, en Java no es posible que una clase herede directamente el estado (atributos) de dos clases distintas.

No obstante, Java ofrece una alternativa controlada mediante las **interfaces**. Una clase puede implementar múltiples interfaces, heredando de ellas únicamente **contratos de comportamiento** (métodos) pero no estado. De este modo, Java permite expresar relaciones múltiples de tipo *“es‑un”* a nivel de comportamiento, conservando la claridad del modelo y evitando los problemas clásicos de la herencia múltiple de clases.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
En los lenguajes orientados a objetos, las **excepciones son objetos**, lo que implica que forman parte de una jerarquía de clases y pueden extenderse para representar errores específicos del dominio del problema. Crear **excepciones personalizadas** permite expresar fallos de forma más precisa y facilita su tratamiento en capas superiores del programa. En Java, todas las excepciones heredan directa o indirectamente de `Throwable`, y las excepciones *no controladas* heredan de `RuntimeException`.

Una excepción **no controlada** es aquella que el compilador no obliga a capturar ni a declarar con `throws`. Este tipo de excepciones se utiliza normalmente para errores de programación o situaciones excepcionales que no se espera manejar localmente. Para lograrlo, basta con heredar de `RuntimeException`. Además, al ser objetos, las excepciones pueden estar **compuestas con otros objetos**, permitiendo adjuntar información relevante sobre el contexto del error.

En este caso, la excepción `UsuarioNoEncontradoException` se compone con un objeto `Usuario`, de modo que sea posible identificar cuál fue el usuario que causó el problema. Asimismo, se sobrecargan los constructores para permitir tanto la creación de la excepción con un mensaje simple como la inclusión de una **causa subyacente**, encadenando excepciones, una práctica habitual en Java para no perder información del error original.

```java
public class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java
public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

Este diseño muestra cómo las excepciones personalizadas permiten **modelar errores de alto nivel**, transportar información contextual mediante composición y mantener la trazabilidad del error gracias al encadenamiento de causas, todo ello respetando los principios de la orientación a objetos en Java.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
La herencia **no debe emplearse únicamente como un mecanismo de reutilización de código** porque establece una relación semántica muy fuerte entre las clases: la relación *“es‑un”*. Cuando una clase hereda de otra, se afirma que el subtipo puede sustituir al supertipo en cualquier contexto. Si esta relación no es conceptualmente cierta y se usa solo para “aprovechar código ya escrito”, el diseño se vuelve incorrecto y difícil de mantener. En otras palabras, no todo lo que reutiliza código *es* realmente una especialización lógica de la clase base.

Otro problema importante es que la herencia genera un **acoplamiento fuerte** entre la superclase y las subclases. La subclase depende de los detalles internos de la clase base, incluso de decisiones que no fueron pensadas para su reutilización. Si la clase base cambia (por ejemplo, se modifica un método o se añade estado), las subclases pueden verse afectadas de forma indirecta. Esto produce jerarquías frágiles, donde pequeñas modificaciones tienen efectos inesperados en muchas clases derivadas.

La **composición**, en cambio, permite reutilizar código sin imponer una relación “es‑un”, sino una relación **“tiene‑un”**. En lugar de heredar comportamiento, una clase contiene objetos de otras clases y delega en ellos parte de su funcionalidad. Este enfoque reduce el acoplamiento, mejora la flexibilidad del diseño y facilita el cambio de comportamientos sin alterar jerarquías de herencia. Además, es más cercana a la programación estructurada conocida en C/C++, donde se combinan módulos sin depender de jerarquías rígidas.

Por estas razones, en el diseño orientado a objetos se recomienda **preferir composición frente a herencia** cuando el objetivo principal es reutilizar código. La herencia debe reservarse para casos en los que exista una verdadera relación de especialización y sustitución lógica, mientras que la composición resulta más adecuada cuando solo se desea compartir funcionalidad de forma segura y mantenible.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
La recomendación de **favorecer la composición frente a la herencia** surge porque la composición produce diseños **más flexibles, menos acoplados y más fáciles de mantener**. Mientras que la herencia crea una relación rígida de tipo *“es‑un”* que queda fijada en tiempo de compilación, la composición permite construir clases combinando objetos y delegando responsabilidades, sin imponer una jerarquía estricta. Esto resulta especialmente valioso en sistemas que evolucionan con el tiempo.

La herencia introduce un **acoplamiento fuerte** entre la clase base y las subclases, ya que estas dependen directamente de la implementación interna de la superclase. Cambios en la clase base pueden propagarse de forma inesperada a todas las clases derivadas, incluso aunque dichas clases no usen directamente la funcionalidad modificada. En cambio, con composición, los cambios suelen quedar encapsulados dentro de los componentes, reduciendo el impacto sobre el resto del sistema.

Otro aspecto clave es que la composición **permite variar el comportamiento en tiempo de ejecución**. Al trabajar con objetos compuestos, es posible sustituir una implementación por otra (por ejemplo, mediante interfaces) sin modificar la clase que las utiliza. La herencia, por el contrario, fija el comportamiento en la propia definición de la clase, lo que obliga a crear nuevas subclases para cada variación, aumentando artificialmente el número de clases y la complejidad del diseño.

Por estas razones, favorecer la composición frente a la herencia conduce a diseños más **modulares, reutilizables y adaptables**, donde la herencia se reserva para los casos en los que exista una verdadera relación de especialización lógica. Esta recomendación no elimina la herencia, sino que la sitúa como una herramienta potente pero que debe usarse con criterio y no como solución por defecto.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
Cuando se afirma que **la herencia rompe la encapsulación**, se hace referencia a que las subclases quedan **expuestas a los detalles internos de la superclase**, debilitando una de las ideas centrales de la orientación a objetos. La encapsulación busca que una clase oculte su estado interno y exponga solo una interfaz clara y estable. Sin embargo, al heredar, la subclase suele depender no solo de la interfaz pública, sino también de decisiones internas de diseño de la clase base.

Esto ocurre porque la herencia permite (y a veces obliga) a las subclases a interactuar con miembros `protected` o a depender del comportamiento concreto de métodos heredados. Aunque dichos miembros no sean públicos, pasan a formar parte del “contrato implícito” entre superclase y subclase. Como resultado, si la implementación interna de la clase base cambia —por ejemplo, el orden en que se llaman métodos, el significado de ciertos atributos o las invariantes internas—, las subclases pueden dejar de funcionar correctamente, aun cuando el código externo siga usando la misma interfaz pública.

Desde el punto de vista del diseño, esto significa que la superclase **ya no está completamente encapsulada**, ya que sus decisiones internas afectan directamente a las subclases. La clase base no puede modificarse libremente sin conocer en detalle cómo la utilizan sus hijas, lo que genera lo que se conoce como *clase base frágil*. Este efecto es especialmente problemático en jerarquías grandes o cuando las clases se reutilizan fuera del contexto para el que fueron diseñadas originalmente.

Por contraste, la composición preserva mejor la encapsulación porque la clase que reutiliza comportamiento **solo depende de la interfaz pública del objeto compuesto**, no de su implementación interna. De este modo, cuando se dice que la herencia rompe la encapsulación, no se afirma que sea incorrecta, sino que **introduce una dependencia estructural fuerte** que debe asumirse conscientemente y usarse solo cuando la relación de especialización esté plenamente justificada.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
A continuación se muestran **dos formas distintas de modelar el mismo problema**, ilustrando claramente la diferencia entre **herencia** y **composición**. En ambos casos se parte de la misma realidad: `Estudiante` y `Trabajador` comparten datos comunes (DNI y nombre), pero se modela esa coincidencia de forma diferente según el enfoque elegido.

***

## Modelo 1: Herencia (superclase `Persona`)

En el modelo basado en herencia, se identifica una abstracción común (`Persona`) que representa aquello que **ambos son**. Tanto `Estudiante` como `Trabajador` se consideran especializaciones de `Persona`, estableciendo una relación clara de tipo *“es‑una Persona”*. Esta opción es conceptualmente adecuada cuando existe una relación natural de generalización y se desea que los subtipos sean compatibles con el tipo base.

La ventaja principal es la simplicidad: los atributos comunes se declaran una sola vez en la superclase y se reutilizan automáticamente. Sin embargo, este diseño introduce un **acoplamiento fuerte** entre `Persona` y sus subclases, que quedan ligadas a su estructura y evolución futura.

```java
public class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

public class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

public class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}
```

***

## Modelo 2: Composición (clase `DatosPersonales`)

En el modelo basado en composición, se separan los **datos compartidos** en una clase independiente (`DatosPersonales`) y se establece una relación de tipo *“tiene‑unos datos personales”*. `Estudiante` y `Trabajador` **no son** los datos, sino que **los contienen**, lo cual evita crear una jerarquía forzada cuando solo se comparte estado.

Este enfoque ofrece mayor flexibilidad: la clase `DatosPersonales` puede reutilizarse en otros contextos sin imponer relaciones de herencia, y las clases que la usan solo dependen de su interfaz pública. Además, se preserva mejor la encapsulación y se reduce el acoplamiento estructural.

```java
public class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}

public class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}
```

***

Ambos modelos son válidos desde el punto de vista técnico, pero expresan **decisiones de diseño distintas**. La herencia es adecuada cuando existe una relación clara de especialización (*es‑un*), mientras que la composición resulta preferible cuando solo se desea reutilizar datos o comportamiento sin imponer una jerarquía rígida.
