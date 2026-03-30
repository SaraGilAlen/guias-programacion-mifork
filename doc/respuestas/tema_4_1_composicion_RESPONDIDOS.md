<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta
A continuación se presenta un ejemplo en C que muestra claramente la composición entre estructuras: un `Punto` formado por dos coordenadas y una `Linea` formada por dos puntos. El diseño sigue el patrón “A tiene-un B”, donde la línea contiene dos puntos que determinan su geometría. Este enfoque resulta natural en C y permite modelar entidades más complejas a partir de estructuras sencillas, manteniendo el código modular y fácil de entender. También se añaden funciones auxiliares para trabajar con estas estructuras, manteniendo la lógica separada de los datos.

La función de distancia entre puntos emplea la fórmula clásica basada en raíces cuadradas y diferencias entre coordenadas. Por su parte, la función que calcula la longitud de una línea delega el cálculo de la distancia utilizando directamente los puntos que la componen. Este tipo de organización resulta una buena práctica porque evita duplicación de código y facilita el mantenimiento.

Este ejemplo ilustra bien cómo la composición en C se asemeja conceptualmente a la composición en lenguajes orientados a objetos, aunque sin métodos dentro de las estructuras. Se muestra así cómo construir modelos complejos que dependen de varias estructuras relacionadas entre sí, pero con funciones que operan externamente sobre esos datos.

Aquí tienes el código solicitado:

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto p1;
    Punto p2;
} Linea;

double distancia(Punto a, Punto b) {
    double dx = b.x - a.x;
    double dy = b.y - a.y;
    return sqrt(dx*dx + dy*dy);
}

double longitudLinea(Linea l) {
    return distancia(l.p1, l.p2);
}

int main() {
    Punto a = {0.0, 0.0};
    Punto b = {3.0, 4.0};
    Linea l = {a, b};

    printf("Distancia entre puntos: %.2f\n", distancia(a, b));
    printf("Longitud de la línea: %.2f\n", longitudLinea(l));

    return 0;
}
```


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta
A continuación se muestra la versión orientada a objetos en Java del ejemplo anterior, siguiendo el principio de **composición** y aplicando **encapsulación** para garantizar inmutabilidad. En este diseño, cada `Punto` se crea con coordenadas que no pueden modificarse posteriormente, y cada `Linea` fija sus dos puntos en el momento de la creación, evitando que la línea pueda alterarse después. Esta idea es una mejora respecto a C, ya que la orientación a objetos permite controlar el acceso a los atributos mediante modificadores de visibilidad adecuados.

Además, las operaciones se encapsulan dentro de las propias clases. El cálculo de la distancia se delega al método de la clase `Punto`, lo que permite que `Linea` simplemente combine dichos puntos sin necesidad de conocer detalles internos sobre cómo se calcula la distancia. De esta forma, cada clase mantiene la responsabilidad sobre su propio comportamiento, lo que conduce a un diseño modular, claro y fácil de mantener.

Este enfoque refuerza la idea de que la composición permite construir clases complejas a partir de otras más simples. El uso de objetos inmutables también proporciona seguridad adicional al evitar estados inconsistentes y facilita razonar sobre el comportamiento del programa. El resultado es una estructura más robusta y expresiva que la versión en C, aprovechando plenamente la orientación a objetos.

Aquí tienes el código correspondiente:

```java
final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}

final class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }

    public Punto getP1() { return p1; }
    public Punto getP2() { return p2; }
}

public class Main {
    public static void main(String[] args) {
        Punto a = new Punto(0.0, 0.0);
        Punto b = new Punto(3.0, 4.0);
        Linea l = new Linea(a, b);

        System.out.println("Distancia entre puntos: " + a.distanciaA(b));
        System.out.println("Longitud de la línea: " + l.longitud());
    }
}
```


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta
La **multiplicidad** en composición describe cuántas instancias de una clase pueden o deben estar asociadas a instancias de otra dentro de una relación “tiene‑un”. Se utiliza para indicar si un objeto contiene uno, varios o ningún objeto del otro tipo, y también si esa relación es obligatoria o simplemente opcional. En composición, estas multiplicidades suelen ser estrictas, ya que el objeto compuesto no puede existir de manera coherente sin sus componentes internos, reforzando así la idea de dependencia fuerte entre las clases implicadas.

En el ejemplo anterior, una `Linea` está formada por **exactamente dos** puntos. La multiplicidad desde `Linea` hacia `Punto` es por tanto **2**, lo que suele expresarse como `2..2` para remarcar que no puede haber ni más ni menos puntos. Esto refleja la naturaleza geométrica de una línea segmentaria, que queda completamente definida por sus dos extremos.

En la dirección inversa, desde `Punto` hacia `Linea`, la multiplicidad es **0..**\* (cero o muchas). Esto significa que un mismo `Punto` puede ser extremo de ninguna línea, de una sola, o de múltiples líneas, sin que ello afecte a su existencia. El punto es independiente y puede existir por sí solo, pero una línea no puede existir sin sus dos puntos.

Así, resumiendo:

*   **De `Linea` → `Punto`: multiplicidad 2..2**
*   **De `Punto` → `Linea`: multiplicidad 0..**\*


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta
La distinción entre **composición fuerte** y **composición débil** se refiere al grado de dependencia entre los objetos que forman parte de una relación “todo–parte”. En la **composición fuerte**, el objeto “todo” controla completamente la existencia de sus partes: si el objeto compuesto deja de existir, sus componentes también deben desaparecer. Esta relación implica dependencia total y un vínculo muy estrecho, donde las partes no tienen sentido fuera del todo y no pueden compartirse con otros objetos.

En cambio, la **composición débil** describe una situación en la que el “todo” contiene o utiliza partes, pero estas pueden existir de manera independiente. Aquí, si el objeto compuesto se destruye, las partes pueden continuar existiendo y ser compartidas entre otros objetos. Esta forma de relación refleja un acoplamiento más flexible, donde los componentes no dependen estrictamente del ciclo de vida del objeto que los utiliza, lo que permite reusarlos en otros contextos sin restricciones fuertes.

La consecuencia principal sobre el **ciclo de vida** es que, en la composición fuerte, la destrucción del objeto compuesto implica necesariamente la destrucción de sus partes. Por el contrario, en la composición débil, los componentes sobreviven más allá del objeto que los contenía. Este aspecto es relevante tanto en lenguajes con gestión de memoria automática como en aquellos donde se debe gestionar manualmente, ya que determina la responsabilidad sobre la creación y eliminación de los objetos.

En la terminología habitual, a la **composición débil** se la denomina **asociación o agregación**, mientras que la **composición fuerte** es lo que normalmente se llama **composición** propiamente dicha. Si quieres, puedo ayudarte a ver ejemplos concretos de cada tipo aplicados a Java o UML.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta
En este caso no se habla de **composición**, sino de **dependencia**. La dependencia describe una relación puntual y débil entre dos clases, en la que una necesita usar a la otra únicamente para llevar a cabo alguna operación concreta. Esta relación no implica que la clase dependiente "tenga" permanentemente objetos de la otra, sino solo que los utiliza de manera temporal para cumplir una tarea.

Cuando una clase recibe un objeto como parámetro, lo devuelve como resultado, lo declara como variable local o crea instancias dentro de un método mediante `new`, se está mostrando precisamente esta relación de dependencia. El objeto utilizado no forma parte de la estructura interna de la clase, ni se almacena en sus atributos, por lo que no existe un vínculo permanente ni un ciclo de vida asociado.

En composición, en cambio, los objetos componentes se guardan como atributos internos, pertenecen al objeto "todo" y forman parte de su identidad. La dependencia es mucho más ligera: la colaboración ocurre solo durante la ejecución de un método y después se pierde la relación. Esto permite distinguir claramente cuándo una clase solo necesita apoyo puntual de otra y cuándo forma parte esencial de su estructura.

En resumen, las situaciones descritas —parámetros, valores de retorno, variables locales o instanciaciones temporales— representan relaciones de **dependencia**, no de composición. Si quieres, puedo ayudarte a diferenciar dependencia, agregación y composición en un mismo diagrama UML para verlo más visualmente.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta
A continuación se muestran **dos implementaciones en Java** de la relación entre `Linea` y `Punto`: una representando **composición fuerte** y otra representando **composición débil**. Cada explicación mantiene el estilo impersonal y la longitud solicitada.

***

## **Composición fuerte (los puntos “pertenecen” a la línea)**

En composición fuerte, los objetos que forman parte del todo son creados y gestionados internamente por dicho todo. Esto implica que los puntos no existen fuera de la línea, y por tanto la propia clase `Linea` debe crearlos en su constructor. Como consecuencia, cuando deje de existir la línea, también dejarán de existir sus puntos. Esta relación refleja dependencia total y es la forma más estricta de composición.

Este enfoque resulta apropiado cuando las partes no tienen sentido fuera del objeto que las contiene o cuando no se desea permitir su reutilización. Además, refuerza el encapsulamiento al impedir que el exterior pueda modificar o acceder directamente a los componentes internos salvo a través de getters que pueden devolver copias o valores derivados.

### **Código (composición fuerte)**

```java
final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

final class Linea {
    private final Punto p1;
    private final Punto p2;

    // La línea crea sus propios puntos → composición fuerte
    public Linea(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }
}
```

***

## **Composición débil (la línea usa puntos ya creados)**

En composición débil, la clase `Linea` recibe desde fuera los puntos que va a utilizar, en lugar de crearlos ella misma. Esto permite que los mismos puntos puedan ser compartidos por varias líneas, que puedan existir por separado y que su ciclo de vida no dependa del de la línea. Aunque se mantiene una relación “todo–parte”, el vínculo es mucho más flexible y no implica propiedad total.

Este tipo de relación se acerca a lo que normalmente se denomina **agregación**, donde las partes pueden existir independientemente y ser reutilizadas. Se mantiene igualmente la inmutabilidad para evitar inconsistencias, pero se ofrece más libertad respecto a cómo se crean y gestionan los objetos que la línea usa como extremos.

### **Código (composición débil / agregación)**

```java
final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

final class Linea {
    private final Punto p1;
    private final Punto p2;

    // La línea recibe puntos externos → composición débil / agregación
    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }
}
```


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta
En Java, incluso en situaciones de **composición fuerte**, el contenedor *no destruye explícitamente* los objetos que contiene. Esto ocurre porque Java incorpora **gestión automática de memoria** mediante el *Garbage Collector* (GC). En este modelo, la destrucción de objetos no es responsabilidad del programador ni de las clases que los usan, sino del propio sistema de ejecución. Por tanto, aunque `Linea` sea la “dueña” conceptual de sus objetos `Punto`, no tiene mecanismos para liberarlos manualmente. Lo único que debe hacer es dejar de mantener referencias hacia ellos.

En el momento en que ya no exista ninguna referencia activa a una instancia de `Linea`, esta se vuelve candidata a ser eliminada por el GC. Y, dado que los `Punto` contenidos en composición fuerte sólo están referenciados desde la propia `Linea`, también dejarán de ser accesibles al desaparecer ella. Este proceso hace que tanto la línea como sus puntos se consideren “alcanzables únicamente por el recolector”, y por tanto sean liberados juntos automáticamente.

Este comportamiento explica por qué no se observa un “delete”, como en C o C++. En Java no existe destrucción explícita porque el lenguaje prioriza la seguridad y automatización en la gestión de memoria. El ciclo de vida de los objetos depende exclusivamente de su accesibilidad, no de acciones manuales del programador. Como consecuencia, la composición fuerte se implementa conceptualmente, pero el sistema se encarga del aspecto de destrucción sin intervención de la clase `Linea`.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta
A continuación se muestra un ejemplo completo de **composición débil**, siguiendo todas las restricciones solicitadas: un `Departamento` que tiene varios `Profesor`, con un máximo de 50, sin revelar el uso de arrays, manteniendo encapsulación, y con un **director** que siempre debe ser un profesor del departamento. Además, se lanzan excepciones cuando se viola la invariante de clase.

En composición débil, el departamento **no crea** a los profesores; simplemente mantiene referencias a ellos. Los profesores pueden existir fuera del departamento y no dependen de él para su ciclo de vida. Sin embargo, el departamento controla su propia estructura interna y mantiene la consistencia garantizando que el director siempre se encuentre incluido en la lista de profesores. Este tipo de relación permite flexibilidad, sin imponer dependencia fuerte del contenedor hacia sus partes.

La implementación incluye métodos seguros para:

*   Añadir un profesor.
*   Eliminar un profesor por posición.
*   Obtener un profesor por posición.
*   Saber cuántos profesores hay.
*   Cambiar el director, siempre validando la invariante.

Todo ello preserva la ocultación de información, impidiendo exponer detalles internos como el uso de arrays o índices concretos.

***

# **Código solicitado**

```java
final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank())
            throw new IllegalArgumentException("El nombre no puede ser vacío.");
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

final class Departamento {
    private final Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;

    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null)
            throw new IllegalArgumentException("El director no puede ser null.");

        // Añadir automáticamente al director como primer profesor
        profesores[0] = directorInicial;
        numProfesores = 1;
        this.director = directorInicial;
    }

    public int getNumeroProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores)
            throw new IndexOutOfBoundsException("Posición fuera de rango.");

        return profesores[posicion];
    }

    public void añadirProfesor(Profesor p) {
        if (p == null)
            throw new IllegalArgumentException("No se puede añadir un profesor null.");

        if (numProfesores >= profesores.length)
            throw new IllegalStateException("No caben más profesores en el departamento.");

        profesores[numProfesores] = p;
        numProfesores++;
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores)
            throw new IndexOutOfBoundsException("Posición fuera de rango.");

        Profesor aEliminar = profesores[posicion];

        // Verificar invariante: no se puede eliminar al director
        if (aEliminar == director)
            throw new IllegalStateException("No se puede eliminar al director del departamento.");

        // Compactar array
        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }

        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null)
            throw new IllegalArgumentException("El nuevo director no puede ser null.");

        // Debe estar en la lista
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }

        if (!encontrado)
            throw new IllegalStateException("El director debe ser un profesor del departamento.");

        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }
}
```


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta
A continuación se muestra la versión del ejercicio usando **`List`** en vez de arrays primitivos, junto con la explicación sobre qué código se simplifica y cómo evitar problemas de encapsulación al devolver la colección interna. La respuesta mantiene el estilo impersonal indicado y la extensión pedida.

***

## ¿Qué simplifica `List` respecto al array?

El uso de `List<Profesor>` permite delegar al propio contenedor dinámico tareas como gestionar la capacidad, desplazar elementos o evitar índices manuales. En comparación con la versión basada en arrays, desaparecen secciones de código que antes eran obligatorias: ya no es necesario verificar espacio libre manualmente, compactar el array al eliminar un profesor, ni manipular posiciones internas. Gracias a estos métodos estándar, el código se vuelve más claro, más breve y menos propenso a errores.

Otra ventaja es que `List` ofrece operaciones directas como `add()`, `remove()`, `size()` o `contains()`, que simplifican enormemente el tratamiento de la colección. Por ello, la implementación se centra únicamente en mantener la invariante de clase (que el director debe pertenecer a la lista), sin preocuparse por la gestión de bajo nivel del almacenamiento, como ocurría con arrays de tamaño fijo.

***

## Problema al devolver la lista interna directamente

Si existiera un método que devolviese la lista completa de profesores, devolver directamente la colección interna comprometería la encapsulación. Quien recibiera la lista tendría la capacidad de modificarla (añadir, eliminar o reordenar profesores) sin pasar por los métodos de la clase `Departamento`. Eso podría **romper la invariante**, permitiendo eliminar al director o insertar valores no válidos.

Para evitarlo, se recomienda devolver una **copia inmodificable** mediante `Collections.unmodifiableList(lista)` o bien devolver una **copia defensiva** con `new ArrayList<>(lista)`. Ambas alternativas preservan la integridad del estado interno del objeto, impidiendo que la estructura sea modificada desde el exterior fuera de los métodos controlados.

***

## Código usando `List<Profesor>` (composición débil)

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank())
            throw new IllegalArgumentException("El nombre no puede ser vacío.");
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

final class Departamento {
    private final List<Profesor> profesores = new ArrayList<>(50);
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null)
            throw new IllegalArgumentException("El director no puede ser null.");

        profesores.add(directorInicial);
        this.director = directorInicial;
    }

    public int getNumeroProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos);
    }

    public void añadirProfesor(Profesor p) {
        if (p == null)
            throw new IllegalArgumentException("No se puede añadir un profesor null.");

        if (profesores.size() >= 50)
            throw new IllegalStateException("No caben más profesores en el departamento.");

        profesores.add(p);
    }

    public void eliminarProfesor(int pos) {
        Profesor aEliminar = profesores.get(pos);

        if (aEliminar == director)
            throw new IllegalStateException("No se puede eliminar al director del departamento.");

        profesores.remove(pos);
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null)
            throw new IllegalArgumentException("El director no puede ser null.");

        if (!profesores.contains(nuevoDirector))
            throw new IllegalStateException("El director debe ser un profesor del departamento.");

        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }

    // Método opcional seguro si se pidieran todos los profesores:
    public List<Profesor> getProfesoresSeguros() {
        // Opción A: copia inmodificable
        return Collections.unmodifiableList(profesores);

        // Opción B: copia defensiva
        // return new ArrayList<>(profesores);
    }
}
```


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta
A continuación se muestra un ejemplo completo de **composición recursiva**, siguiendo tu estilo habitual: explicación impersonal, objeto inmutable, composición recursiva madre → hija, y un `main` que forma una pequeña familia desde abuela hasta nieto.

***

## Explicación de la composición recursiva

Una **composición recursiva** ocurre cuando una clase contiene referencias a objetos *del mismo tipo*, creando una estructura potencialmente en cadena o en árbol. Este patrón aparece cuando una entidad forma parte de otra igual, como ocurre con las excepciones que pueden encerrar su “causa”. En estos casos, el objeto compuesto contiene otro objeto de su mismo tipo, lo que permite modelar relaciones como parentescos, árboles genealógicos o estructuras jerárquicas.

En una clase inmutable, como la propuesta, la referencia recursiva también debe ser inmutable: una vez asignada la madre de una persona, no puede modificarse. Esto evita inconsistencias en el modelo y facilita el razonamiento, ya que la estructura permanece fija y segura a lo largo del tiempo.

***

## Código: clase `Persona` inmutable con composición recursiva

```java
final class Persona {
    private final String nombre;
    private final Persona madre; // composición recursiva

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank())
            throw new IllegalArgumentException("El nombre no puede ser vacío.");

        this.nombre = nombre;
        this.madre = madre;  // puede ser null en el caso de la anciana de la familia
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }
}

public class Main {
    public static void main(String[] args) {
        // Abuela (sin madre conocida)
        Persona abuela = new Persona("María", null);

        // Madre, cuya madre es la abuela
        Persona madre = new Persona("Lucía", abuela);

        // Nieto, cuya madre es Lucía
        Persona nieto = new Persona("Carlos", madre);

        // Ejemplo de uso
        System.out.println("Nieto: " + nieto.getNombre());
        System.out.println("Madre del nieto: " + nieto.getMadre().getNombre());
        System.out.println("Abuela del nieto: " + nieto.getMadre().getMadre().getNombre());
    }
}
```

Este ejemplo ilustra cómo una estructura puede encadenarse recursivamente, con cada persona apuntando hacia su madre, y cómo pueden recorrerse los niveles de parentesco sucesivamente.

***

## Ejemplos clásicos de **composiciones recursivas**

1.  **Excepciones en Java**  
    Cada excepción puede contener otra excepción como “causa”, formando una cadena recursiva de fallos anidados.

2.  **Árboles y nodos**  
    Un `Nodo` contiene sus nodos hijos, que a su vez contienen otros hijos, estructura clásica en árboles sintácticos, directorios de archivos o árboles binarios.

3.  **Listas enlazadas**  
    Cada nodo contiene un elemento y una referencia al siguiente nodo de la lista, que es otro nodo del mismo tipo.

4.  **Estructuras jerárquicas**  
    Como empleados con jefes que a su vez tienen jefes, o menús con submenús.


## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta
Las relaciones de composición **bidireccionales** aparecen cuando dos clases se componen mutuamente y cada una mantiene una referencia a la otra. En una relación de este tipo, cada objeto conoce a su correspondiente “compañero”, creando así una conexión en ambas direcciones. Aunque esto permite navegar desde cualquiera de los dos objetos (por ejemplo, desde un profesor hasta su departamento y viceversa), también introduce mayor complejidad, ya que es necesario garantizar que ambas referencias se mantengan coherentes. Además, este tipo de relaciones incrementan el acoplamiento entre las clases, por lo que deben utilizarse con cuidado dentro del diseño del sistema.

Para implementar una composición bidireccional en un ejemplo de `Profesor` y `Departamento`, sería necesario que el profesor mantuviera un atributo que apunte a un departamento, y simultáneamente que el departamento mantuviera una colección de profesores. Esto implica proporcionar métodos que aseguren que, al asignar un profesor a un departamento, el profesor quede registrado correctamente en la lista del departamento y, al mismo tiempo, el departamento quede asignado dentro del objeto profesor. De igual manera, al eliminar o mover profesores entre departamentos, ambos lados de la relación deben actualizarse de manera consistente, evitando referencias desactualizadas.

Un enfoque habitual consiste en centralizar la lógica de enlace en el propio departamento, de modo que una operación como `addProfesor()` añada el profesor a su lista y, además, llame internamente a un método del profesor para establecer la referencia al departamento. Con esta estrategia se asegura que no existan estados incoherentes, ya que la actualización se realiza en un único punto controlado. No obstante, este mecanismo requiere disciplina en el uso de los métodos adecuados, evitando manipulaciones directas de atributos que rompan la simetría.

Un ejemplo ilustrativo podría escribirse así:

```java
class Profesor {
    private String nombre;
    private Departamento departamento;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    void setDepartamento(Departamento d) {
        this.departamento = d;
    }
}

class Departamento {
    private String nombre;
    private List<Profesor> profesores = new ArrayList<>();

    public Departamento(String nombre) {
        this.nombre = nombre;
    }

    void addProfesor(Profesor p) {
        profesores.add(p);
        p.setDepartamento(this); // Enlace bidireccional
    }
}
```
