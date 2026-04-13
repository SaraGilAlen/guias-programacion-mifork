<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta
El **polimorfismo** en programación orientada a objetos es la capacidad de usar una misma referencia para acceder a comportamientos distintos, dependiendo del tipo real del objeto al que apunta. En Java, esto ocurre principalmente cuando una variable de una clase padre almacena un objeto de una clase hija, y la llamada a un método se resuelve en tiempo de ejecución. Su utilidad principal es permitir escribir código más flexible, reutilizable y fácil de mantener, ya que no es necesario conocer el tipo exacto del objeto para invocar su comportamiento.

Desde un punto de vista práctico, el polimorfismo permite tratar objetos diferentes de forma uniforme. Esto guarda cierta relación conceptual con C/C++, donde una misma función puede operar sobre distintos datos si se respeta un contrato común, aunque en Java se formaliza mediante herencia y métodos redefinidos. Gracias a ello, es posible ampliar un programa añadiendo nuevas clases derivadas sin modificar el código que ya existe.

La **sobreescritura de métodos** es el mecanismo que hace posible el polimorfismo en Java. Consiste en definir en una clase hija un método con el mismo nombre, parámetros y tipo de retorno que uno heredado de la clase padre, pero proporcionando una implementación diferente. Cuando se invoca ese método a través de una referencia al tipo padre, Java ejecuta la versión correspondiente al objeto real en tiempo de ejecución, no la del tipo de la referencia.

```java
class Animal {
    void hacerSonido() {
        System.out.println("El animal hace un sonido");
    }
}

class Perro extends Animal {
    @Override
    void hacerSonido() {
        System.out.println("El perro ladra");
    }
}

Animal a = new Perro();
a.hacerSonido(); // Se ejecuta el método de Perro
```

En este ejemplo, aunque la variable es de tipo `Animal`, se ejecuta el método sobrescrito de `Perro`, lo que demuestra el comportamiento polimórfico basado en la sobreescritura.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
La **ligadura dinámica**, también llamada **enlace tardío**, consiste en que la decisión de qué método concreto se ejecuta no se toma en tiempo de compilación, sino **en tiempo de ejecución**, según el tipo real del objeto y no el tipo de la referencia. Es decir, aunque una variable esté declarada como una clase padre, el método que se ejecuta es el de la clase hija si el objeto almacenado pertenece a ella. Este mecanismo es una pieza clave del funcionamiento interno de la programación orientada a objetos.

La relación entre ligadura dinámica y **polimorfismo** es directa: el polimorfismo se apoya en el enlace tardío para poder comportarse de forma distinta ante un mismo mensaje. Sin ligadura dinámica, una llamada a un método siempre estaría ligada al tipo de la variable, lo que impediría que distintas clases hijas redefinieran el comportamiento de ese método. Por tanto, puede decirse que el polimorfismo en tiempo de ejecución existe gracias a la ligadura dinámica.

En **C++**, la ligadura dinámica **no es automática**. Para que un método se enlace dinámicamente, debe declararse explícitamente como `virtual` en la clase base. Si no se hace, se utiliza ligadura estática (en tiempo de compilación), incluso aunque exista herencia. En **Java**, por el contrario, **la ligadura dinámica es el comportamiento por defecto** para los métodos de instancia: cualquier método puede ser sobrescrito y se resuelve dinámicamente sin necesidad de indicarlo explícitamente. Solo los métodos `static`, `final` o `private` quedan fuera de este mecanismo.

En **Python**, el enlace dinámico es aún más flexible, ya que el lenguaje es dinámicamente tipado. La resolución de métodos se realiza siempre en tiempo de ejecución, basándose en el objeto real y en el orden de búsqueda de métodos (MRO). No es necesario declarar nada de forma especial y no existe una distinción explícita entre enlace temprano y tardío como en C++ o Java. Esto hace que Python soporte polimorfismo de manera natural, incluso sin herencia, siempre que los objetos respondan a los mismos métodos.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
A continuación se muestra un ejemplo sencillo que ilustra el **polimorfismo** mediante herencia y sobreescritura de métodos en Java. Se parte de una clase base `Soldado` que define un método `saludar`, el cual representa un comportamiento común. Las clases `Zapador` y `Artillero` heredan de `Soldado`, y en el caso de `Zapador` se **sobreescribe completamente** el método `saludar`, cambiando su implementación respecto a la clase padre.

La sobreescritura permite que cada subclase tenga un comportamiento propio al responder al mismo mensaje (`saludar`). Aunque todas las clases comparten la misma interfaz pública heredada de `Soldado`, el código que usa referencias de tipo `Soldado` no necesita saber si está tratando con un `Zapador` o un `Artillero`. Esto es precisamente lo que demuestra el polimorfismo: un mismo método invocado de la misma forma produce resultados distintos según el objeto real.

El ejemplo se completa creando un array de `Soldado` que contiene objetos de distintas subclases. Al recorrer el array y llamar al método `saludar` usando referencias de tipo `Soldado`, Java decide en tiempo de ejecución qué versión del método debe ejecutarse. Este comportamiento se basa en la ligadura dinámica y permite escribir código genérico y extensible sin condicionales explícitos sobre el tipo del objeto.

```java
class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda de forma militar.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("El zapador saluda mientras prepara explosivos.");
    }
}

class Artillero extends Soldado {
    // No sobrescribe el método, usa el de Soldado
}

public class EjemploPolimorfismo {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[2];
        ejercito[0] = new Zapador();
        ejercito[1] = new Artillero();

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```

En este código, la llamada a `saludar` es la misma en todos los casos, pero el comportamiento varía según el tipo concreto del objeto, mostrando el uso práctico del polimorfismo en Java.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, al **sobreescribir un método** es posible invocar el método de la clase base para reutilizar su comportamiento y trabajar a partir de él. Esto resulta útil cuando se desea mantener la funcionalidad general definida en la superclase, pero ampliarla o modificarla ligeramente en una subclase, evitando duplicar código y respetando el diseño original.

En Java, esta posibilidad encaja bien con el polimorfismo, ya que la subclase no está obligada a sustituir completamente el comportamiento heredado. Al contrario, puede apoyarse en él y añadir nuevas acciones antes o después de la llamada al método base. De este modo, el método sobrescrito sigue cumpliendo el contrato definido por la superclase, pero proporciona un comportamiento más especializado.

Para invocar el método de la clase base desde un método sobrescrito se utiliza la palabra clave **`super`**. Esta palabra clave permite acceder explícitamente a los miembros (métodos o atributos) de la superclase, incluso cuando han sido redefinidos en la subclase. Su uso deja claro que se está llamando a la versión original del método.

```java
class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda de forma militar.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // Llamada al método de la clase base
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```

En este ejemplo, `Zapador` no sustituye completamente el saludo, sino que primero ejecuta el saludo estándar del `Soldado` y después añade un mensaje adicional. La palabra clave utilizada para invocar el método de la clase base es **`super`**.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
Al **sobreescribir un método en Java**, existen restricciones claras sobre su firma. Los **parámetros deben ser exactamente los mismos** en número, orden y tipo que en el método de la clase base; de lo contrario, no se considera una sobreescritura. En cuanto al **tipo de retorno**, debe ser el mismo o un subtipo del original (esto se conoce como *tipo de retorno covariante*). Además, la visibilidad del método sobrescrito no puede ser más restrictiva que la del método original (por ejemplo, no se puede pasar de `public` a `protected`).

La **sobreescritura (*overriding*)** y la **sobrecarga (*overloading*)** son conceptos distintos. La sobreescritura ocurre entre una clase base y una subclase, y permite cambiar el comportamiento de un método heredado, siendo clave para el polimorfismo en tiempo de ejecución. La sobrecarga, en cambio, ocurre dentro de la misma clase (o entre clase base y subclase) y consiste en definir varios métodos con el mismo nombre pero **distinta lista de parámetros**. La sobrecarga se resuelve en tiempo de compilación y no implica polimorfismo dinámico.

La anotación **`@Override`** se utiliza para indicar explícitamente que un método pretende sobrescribir otro heredado de una superclase. Su función principal no es cambiar el comportamiento del programa, sino ayudar al compilador a detectar errores, como escribir mal el nombre del método o usar una firma incorrecta. Si el método no sobrescribe realmente ninguno, el compilador generará un error.

Usar **`@Override` es altamente recomendable** porque mejora la legibilidad del código, facilita su mantenimiento y evita errores difíciles de detectar, especialmente en programas con jerarquías de herencia más grandes. Indica de forma clara la intención del programador y refuerza la correcta aplicación del polimorfismo en el diseño orientado a objetos.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Sí, al estudiar Java se empieza a **emplear polimorfismo desde etapas muy tempranas**, aunque no siempre se sea consciente de ello. Desde el momento en que se sobrescriben métodos heredados de una clase base, ya se está usando polimorfismo en tiempo de ejecución. Dado que en Java todas las clases heredan implícitamente de `Object`, cualquier redefinición de métodos como `toString`, `equals` o `hashCode` entra dentro de este mecanismo.

Cuando se sobrescribe `toString`, por ejemplo, se está permitiendo que una referencia de tipo `Object` invoque un comportamiento distinto según la clase concreta del objeto. Si un objeto se imprime por pantalla o se concatena con una cadena, Java invoca automáticamente su método `toString`, y gracias al polimorfismo se ejecuta la versión correspondiente a la clase real del objeto, no la de `Object`.

Lo mismo ocurre con `equals`. Aunque el método se invoque usando una referencia genérica, el comportamiento que se ejecuta es el definido en la clase que lo ha sobrescrito. Esto implica ligadura dinámica y resolución en tiempo de ejecución, que son características esenciales del polimorfismo, incluso aunque no se esté trabajando explícitamente con jerarquías complejas de clases propias.

Por tanto, puede afirmarse que **sí se utiliza polimorfismo desde el principio en Java**, muchas veces antes incluso de estudiarlo de forma teórica. La sobreescritura de métodos heredados de `Object` constituye uno de los primeros contactos prácticos con el polimorfismo y sienta las bases para comprender su uso más general en jerarquías de herencia definidas por el programador.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una **clase abstracta** es una clase que representa un concepto general e incompleto, pensado para ser heredado por otras clases más concretas. Sirve como base común para definir atributos y métodos compartidos, pero no describe por sí sola un objeto completamente utilizable. Por este motivo, **no se pueden crear instancias de una clase abstracta**, ya que su propósito no es representar objetos reales, sino establecer una estructura común y un contrato de comportamiento.

Un **método abstracto** es un método que se declara sin implementación, indicando únicamente su firma. Este tipo de método expresa que todas las clases hijas están obligadas a proporcionar su propia versión del comportamiento. La presencia de al menos un método abstracto obliga a que la clase que lo contiene sea declarada también como abstracta. Esto garantiza, en tiempo de compilación, que las subclases implementen esos comportamientos esenciales.

En el siguiente ejemplo, la clase `Soldado` se redefine como abstracta y se añade el método `atacar`, que también es abstracto. Cada tipo concreto de soldado deberá implementar su propia forma de atacar. La palabra clave **`abstract`** debe colocarse tanto en la declaración de la clase como en la declaración del método que no tiene cuerpo.

```java
abstract class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda de forma militar.");
    }

    public abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El zapador coloca y detona explosivos.");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El artillero dispara la artillería pesada.");
    }
}
```

En este diseño, `Soldado` define qué significa ser un soldado, pero deja sin concretar cómo se ataca. El uso de clases y métodos abstractos refuerza el polimorfismo, ya que permite tratar a todos los soldados de manera uniforme mientras se delega en cada subclase la responsabilidad de definir su comportamiento específico.


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
La palabra clave **`final`** en Java se utiliza para **restringir la herencia o la modificación del comportamiento**. Cuando se aplica a una **clase**, indica que dicha clase **no puede ser heredada**. Cuando se aplica a un **método**, impide que ese método sea **sobrescrito en las subclases**. Su objetivo principal es asegurar que una implementación concreta no pueda ser alterada, bien por razones de diseño, seguridad o eficiencia.

En relación con el **polimorfismo**, `final` actúa como una limitación directa. El polimorfismo en tiempo de ejecución se basa en la sobreescritura de métodos y en la ligadura dinámica; por tanto, un método declarado como `final` **no puede participar en el polimorfismo**, ya que su versión será siempre la de la clase donde se definió. Del mismo modo, una clase `final` no puede tener subclases, por lo que tampoco puede existir polimorfismo basado en herencia a partir de ella.

El uso de `final` no implica que desaparezca la orientación a objetos, sino que se controla dónde se permite la extensión del comportamiento. Es habitual encontrar clases base diseñadas para ser heredadas y métodos concretos declarados como `final` para evitar que ciertas partes críticas del código sean modificadas accidentalmente en las subclases, manteniendo la coherencia del sistema.

Un ejemplo clásico de **clase `final` en la API estándar de Java** es la clase `String`. Esta clase no puede ser heredada, lo que garantiza que su comportamiento (especialmente relacionado con la inmutabilidad y la seguridad) no pueda alterarse. Otros ejemplos incluyen clases como `Integer`, `Double` o `Math`. En estos casos, marcar las clases como `final` evita usos indebidos y refuerza un diseño seguro, aunque sacrifica la posibilidad de aplicar polimorfismo sobre ellas.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
En Java, una **interfaz** es un tipo que define un **conjunto de métodos que una clase se compromete a implementar**, pero sin proporcionar (en su forma clásica) la implementación de esos métodos. Su finalidad es expresar **qué puede hacer un objeto**, no cómo lo hace. De este modo, las interfaces permiten definir contratos comunes que pueden ser compartidos por clases muy distintas entre sí, incluso aunque no encajen en una misma jerarquía de herencia.

Las interfaces **se parecen a las clases abstractas**, pero no son lo mismo. Ambas pueden definirse sin poder instanciarse y ambas se usan para favorecer el polimorfismo. La diferencia principal es que una clase abstracta representa una relación de tipo *“es un”* y puede tener estado (atributos) y métodos con implementación, mientras que una interfaz se centra únicamente en el comportamiento que debe ofrecer una clase. En niveles iniciales de Java, puede entenderse una interfaz como una clase completamente abstracta y orientada a definir capacidades.

Una clase **puede implementar más de una interfaz**, lo cual es una característica clave del lenguaje. Java no permite herencia múltiple entre clases para evitar ambigüedades, pero sí permite que una clase implemente múltiples interfaces, combinando así distintos contratos de comportamiento. Gracias a esto, una misma clase puede ser tratada polimórficamente como varios tipos distintos, dependiendo de la interfaz que se utilice como referencia.

```java
interface Atacante {
    void atacar();
}

interface Saludador {
    void saludar();
}

class Soldado implements Atacante, Saludador {
    public void atacar() {
        System.out.println("El soldado ataca.");
    }

    public void saludar() {
        System.out.println("El soldado saluda.");
    }
}
```

En este ejemplo, `Soldado` implementa dos interfaces distintas, demostrando que Java permite combinar comportamientos mediante interfaces y fomentar un uso flexible y polimórfico del código.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta
En este diseño se utiliza **polimorfismo mediante clases abstractas** para representar el concepto general de un punto, sin fijar de antemano sus dimensiones. La clase `Punto` actúa como tipo base y define el método abstracto `calcularDistanciaA`, obligando a que cada subtipo concreto (2D o 3D) implemente su propia lógica de cálculo. De esta forma, se puede trabajar con referencias al tipo `Punto` sin conocer si se trata de un punto bidimensional o tridimensional.

Para garantizar que la distancia solo se calcule entre puntos compatibles, se emplea **`instanceof` junto con *downcasting*** dentro de cada implementación. Esto permite comprobar en tiempo de ejecución que el punto recibido pertenece al mismo subtipo y, una vez verificado, convertirlo al tipo correcto para acceder a sus coordenadas. Si se recibe un punto de otro subtipo, se puede lanzar una excepción, indicando un uso incorrecto del método.

A partir de este diseño, se puede crear una clase `Linea` que trabaje exclusivamente con referencias de tipo `Punto`. La clase `Linea` no necesita conocer ni las dimensiones ni el tipo concreto de los puntos que recibe; simplemente delega en el método polimórfico `calcularDistanciaA` el cálculo de la longitud. Esto demuestra cómo el polimorfismo permite escribir código general y reutilizable, desacoplado de las implementaciones concretas.

```java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    private double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p = (Punto2D) otro; // downcasting
            double dx = x - p.x;
            double dy = y - p.y;
            return Math.sqrt(dx * dx + dy * dy);
        }
        throw new IllegalArgumentException("Puntos incompatibles");
    }
}

class Punto3D extends Punto {
    private double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p = (Punto3D) otro; // downcasting
            double dx = x - p.x;
            double dy = y - p.y;
            double dz = z - p.z;
            return Math.sqrt(dx * dx + dy * dy + dz * dz);
        }
        throw new IllegalArgumentException("Puntos incompatibles");
    }
}

class Linea {
    private Punto a, b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.calcularDistanciaA(b);
    }
}
```

En este ejemplo, `Linea` puede calcular su longitud sin saber si trabaja con puntos 2D o 3D, apoyándose exclusivamente en el comportamiento polimórfico definido en `Punto` y concretado en sus subclases.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
La **herencia de interfaces** en Java consiste en que una interfaz puede **extender otra interfaz**, heredando todos sus métodos. Esto permite construir contratos de comportamiento de forma progresiva, partiendo de capacidades básicas e incorporando nuevas funcionalidades en interfaces más especializadas. Al igual que ocurre con las clases abstractas, las interfaces no pueden instanciarse, y su función principal es definir qué operaciones deben ofrecer las clases que las implementen.

En Java **sí existe herencia múltiple de interfaces**. Una interfaz puede extender **una o varias interfaces al mismo tiempo**, y una clase puede implementar **tantas interfaces como se desee**. Esto es posible porque las interfaces no definen estado ni implementaciones (en el modelo clásico), evitando así los problemas de ambigüedad que surgirían con la herencia múltiple de clases. Gracias a esto, una clase puede asumir distintos roles y ser tratada polimórficamente como varios tipos diferentes.

En el siguiente ejemplo, la interfaz `Fichero` define la capacidad básica de leer su contenido. La interfaz `FicheroEscribible` **hereda de `Fichero`** y añade nuevas operaciones relacionadas con la escritura y eliminación. Cualquier clase que implemente `FicheroEscribible` estará obligada a implementar todos los métodos definidos en ambas interfaces.

```java
interface Fichero {
    String leerContenido();
}

interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}
```

Este diseño permite tratar un objeto como un simple `Fichero` cuando solo interesa leerlo, o como un `FicheroEscribible` cuando se requieren más operaciones. La herencia de interfaces y su carácter múltiple refuerzan el polimorfismo y fomentan un diseño flexible, desacoplado y orientado a capacidades en lugar de jerarquías rígidas de clases.
