<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta
A falta de excepciones en C, el error debe comunicarse mediante **convenciones** que el código llamador pueda comprobar. Una opción común es **devolver un código de estado** (0 = OK, ≠0 = error) y entregar el resultado por **parámetro de salida** (puntero). Así se separa claramente “éxito/fracaso” del valor numérico calculado, evitando ambigüedades (por ejemplo, `-1` podría ser un resultado válido en otras funciones; aquí se calcula raíz, pero la convención sigue siendo limpia y general).

```c
#include <math.h>
#include <stdio.h>

int raiz(double x, double *out_result) {       // 0 = OK; 1 = argumento inválido
    if (x < 0.0 || out_result == NULL) return 1;
    *out_result = sqrt(x);
    return 0;
}

int main(void) {
    double r;
    if (raiz(-9.0, &r) != 0) {
        printf("Error: argumento negativo.\n"); // informado fuera de la función
    } else {
        printf("Resultado: %.3f\n", r);
    }
}
```

Otra opción es usar **`errno`** (cabecera `<errno.h>`) y devolver un **valor centinela** que no confunda al llamador. Como se trabaja con `double`, el centinela natural es **`NAN`** (not-a-number), consultable con `isnan`. La función establece `errno` con un código estándar como `EDOM` (domain error) para indicar que el argumento está fuera del dominio permitido; el llamador comprueba `isnan` y/o `errno` y actúa en consecuencia. Esta vía es idiomática en C cuando hay valores IEEE 754 especiales y se desea transmitir un motivo de error sin parámetros extra.

```c
#include <math.h>
#include <errno.h>
#include <stdio.h>
#include <fenv.h>   // opcional, para control de flags de coma flotante

double raiz(double x) {           // devuelve NAN en error y setea errno
    if (x < 0.0) {
        errno = EDOM;             // argumento fuera de dominio
        return NAN;
    }
    errno = 0;                    // limpiar por claridad
    return sqrt(x);
}

int main(void) {
    errno = 0;
    double r = raiz(-9.0);
    if (isnan(r)) {
        if (errno == EDOM) {
            printf("Error: argumento negativo (fuera de dominio).\n");
        } else {
            printf("Error: cálculo inválido.\n");
        }
    } else {
        printf("Resultado: %.3f\n", r);
    }
}
```

Ambos diseños permiten **informar desde fuera** de `raiz`. El primero es explícito y no depende de estado global, además facilita encadenar múltiples resultados (más parámetros de salida). El segundo favorece una interfaz más simple (un solo retorno) y se integra con prácticas clásicas de la librería C; sin embargo, exige disciplina con `errno` y cuidar el uso de `NAN` si se combina con cálculos donde `NAN` pueda aparecer por otros motivos. En contextos de biblioteca o API pública, la elección suele inclinarse por el **código de estado + out-param** por su claridad y testabilidad.


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta
Una **excepción** es un objeto que representa una situación anómala o inesperada que interrumpe el flujo normal de ejecución de un programa. A diferencia de C, donde se suelen usar códigos de retorno y `errno`, en Java las excepciones permiten “saltar” directamente desde el punto del fallo hasta un manejador adecuado mediante `throw` y la estructura `try–catch`. Esto separa el **código del caso normal** del **código de manejo de errores**, haciendo el control de errores más explícito y menos repetitivo.

Al **implementar funciones/métodos**, las excepciones se usan para señalar condiciones que la función no puede resolver localmente o que no forman parte del flujo esperado (por ejemplo, un fichero inexistente o parámetros inválidos). En Java, si la condición es recuperable y prevista (I/O, red), suele emplearse una **checked exception** que obliga a quien llama a tratarla o declararla; si la condición indica un error de programación (precondición incumplida, estado inválido), se lanza típicamente una **unchecked exception** (`RuntimeException`) como `IllegalArgumentException` o `IllegalStateException`. Este diseño hace que la API comunique de forma clara qué puede fallar y bajo qué contrato de uso.

Al **llamar** a funciones/métodos, las excepciones permiten concentrar el manejo de fallos en un punto (`catch`) y decidir una estrategia: recuperar (reintentar, pedir otro dato), traducir/encapsular la excepción añadiendo contexto, registrar y continuar, o abortar la operación. El llamador no necesita comprobar manualmente códigos tras cada llamada como en C; en su lugar, captura tipos específicos (p. ej., `IOException` vs `NumberFormatException`) y aplica respuestas diferenciadas. Así, el código de “camino feliz” permanece limpio, mientras que el manejo de errores es explícito, estructurado y más difícil de olvidar.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta
A modo básico y alineado con Java, puede diseñarse `raiz(double x)` para **validar el argumento** y, ante un negativo, **lanzar una excepción no comprobada** (`IllegalArgumentException`). De este modo, el método comunica claramente que se incumple una precondición (el dominio de la función es `x ≥ 0`). El control “desde fuera” se hace con un `try–catch` en `main`, manteniendo limpio el flujo normal y concentrando el manejo del error en un punto.

```java
public class Calculadora {

    /**
     * Devuelve la raíz cuadrada de x si x >= 0.
     * Lanza IllegalArgumentException si x es negativo.
     */
    public static double raiz(double x) {
        if (x < 0.0) {
            throw new IllegalArgumentException("El argumento debe ser >= 0. Valor recibido: " + x);
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        // Caso correcto
        try {
            double r1 = Calculadora.raiz(9.0);
            System.out.println("Resultado (9.0): " + r1);
        } catch (IllegalArgumentException e) {
            System.out.println("Error (9.0): " + e.getMessage());
        }

        // Caso erróneo: controlado desde fuera
        try {
            double r2 = Calculadora.raiz(-9.0);
            System.out.println("Resultado (-9.0): " + r2);
        } catch (IllegalArgumentException e) {
            System.out.println("Error (-9.0): " + e.getMessage());
            // Aquí podría registrarse, pedir otro dato, o traducir a otra excepción
        }
    }
}
```

Este diseño se adecua a la diferencia conceptual entre **checked** y **unchecked**: los parámetros inválidos suelen modelarse como **unchecked** (`IllegalArgumentException`), porque reflejan un **error de programación** o de validación previa. Si se quisiera obligar a quien llama a tratar explícitamente el caso (como en operaciones de I/O), podría definirse una excepción **checked** propia (`extends Exception`) y declararla con `throws`, pero para una precondición simple de dominio numérico, el enfoque anterior es idiomático y suficiente.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta
**Lanzar** una excepción significa crear y emitir un objeto que representa una condición anómala para interrumpir el flujo normal. En el ejemplo de la raíz, `Calculadora.raiz(-9.0)` **lanza** una `IllegalArgumentException` al detectar un argumento negativo: el `throw` corta la ejecución de ese método en ese punto y comienza la búsqueda de un manejador adecuado. **Capturar** o **controlar** una excepción significa envolver el código potencialmente problemático en un `try` y proporcionar uno o varios `catch` con tipos de excepción concretos; si el tipo coincide, el control salta al bloque `catch`, donde se decide cómo reaccionar (informar, registrar, reintentar, traducir, etc.).

```java
public static double raiz(double x) {
    if (x < 0) throw new IllegalArgumentException("x debe ser >= 0");
    return Math.sqrt(x);
}

public static void main(String[] args) {
    try {
        double r = Calculadora.raiz(-9.0); // aquí se lanza
        System.out.println(r);             // no se ejecuta
    } catch (IllegalArgumentException e) { // aquí se captura
        System.out.println("Error: " + e.getMessage());
    } finally {
        System.out.println("Siempre se ejecuta (limpieza de recursos).");
    }
}
```

Que una excepción se **propague** significa que, si un método no la captura, esta “sube” a su llamador, y así sucesivamente, recorriendo la **pila de llamadas** hasta encontrar un `catch` compatible o, si no lo hay, hasta terminar el programa con un error no capturado. Durante la propagación, las funciones intermedias **no se reanudan** donde fallaron: su ejecución se **desenrolla** (stack unwinding), liberando el estado local y ejecutando, si existen, los bloques `finally` correspondientes. Por ejemplo, si `main` llama a `procesar()`, `procesar()` llama a `raiz()`, y `raiz()` lanza la excepción, entonces `procesar()` solo continuará si **captura** la excepción; si no lo hace, se aborta su ejecución y la excepción sigue subiendo hacia `main`.

Este desenrollado implica que el punto exacto posterior al `throw` **no se retoma** en los métodos que no capturan la excepción. La única forma de continuar dentro de un método tras un fallo es **capturarlo** en ese método (con `catch`) y, a partir de ahí, decidir continuar, retornar un valor alternativo o **re‑lanzar** la excepción (posiblemente envuelta) para que niveles superiores decidan. En todos los casos, los bloques `finally` asociados a `try` activos se ejecutan, lo que garantiza acciones de limpieza (cerrar ficheros, liberar recursos) incluso cuando ocurre el error. Esta semántica permite mantener el “camino feliz” limpio y el manejo de errores estructurado y predecible.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta
La **propagación natural** de excepciones en Java aporta una gran mejora respecto al estilo clásico de C basado en códigos de retorno. En C, cada función que recibe un código de error debe comprobarlo manualmente y reenviarlo a su llamador, lo que produce código repetitivo, propenso al olvido y mezclado con la lógica principal. En cambio, en Java la propagación ocurre **automáticamente**: si un método no captura una excepción, esta asciende a su llamador sin necesidad de escribir código extra, manteniendo el flujo normal limpio y reduciendo la posibilidad de ignorar errores importantes.

Además, esta propagación automática permite que el **manejador más apropiado** se encuentre a cualquier nivel de la pila, no necesariamente en el método que detectó el problema. Por ejemplo, un error de entrada/salida detectado en un método profundo puede ser tratado en la capa superior de la aplicación, donde se tiene el contexto adecuado para decidir cómo reaccionar. En C esto obliga a propagar manualmente códigos numéricos por todas las capas, obligando a cada función intermedia a comprobar y retransmitir esos códigos incluso si no tiene nada útil que hacer con ellos.

Otra ventaja clara es que la propagación incluye el **desenrollado controlado de la pila**, ejecutando automáticamente los bloques `finally` de cada nivel, lo que facilita la liberación fiable de recursos (ficheros, sockets, buffers, etc.). En C, este patrón debe implementarse manualmente con estructuras `goto`, múltiples `return`, o finales repetidos, lo que aumenta la complejidad y el riesgo de fugas de recursos. La semántica de excepciones garantiza que la limpieza se ejecuta siempre, incluso cuando se produce un fallo inesperado.

Finalmente, la propagación natural facilita un **código más expresivo y modular**. Los métodos no se ven obligados a entrelazar cálculos con comprobación de errores; cada uno puede centrarse en su responsabilidad principal, sabiendo que si ocurre un fallo, la excepción escalará hasta un punto donde sí pueda tomarse una decisión apropiada. Este enfoque produce programas más legibles, mantenibles y robustos que el estilo imperativo de manejo de errores manual propio de C.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta
Sí, en orientación a objetos las excepciones **son objetos**, porque en Java todas las excepciones derivan de la clase base `Throwable`. Esto significa que una excepción no es solo un código numérico o una señal, sino un objeto completo que puede almacenar información relevante: un mensaje descriptivo, una causa original, o incluso datos adicionales si se definen campos propios. Esta capacidad encaja perfectamente con la filosofía del diseño orientado a objetos, donde cada entidad del programa encapsula su estado y comportamiento.

Desde el punto de vista de la **encapsulación**, el hecho de que las excepciones sean objetos permite agrupar en un mismo lugar toda la información y la lógica asociada al error. Por ejemplo, una excepción puede incluir un mensaje claro, el tipo concreto del problema y la traza de llamadas que permite rastrear su origen. También puede especializarse mediante herencia, creando estructuras jerárquicas de errores que permiten capturar solo aquello que es relevante y mantener los detalles ocultos dentro del propio objeto. Esto hace el código más expresivo y modular, ya que cada excepción representa con precisión un tipo de fallo bien definido.

Gracias a que son objetos, es posible **crear excepciones personalizadas**, extendiendo `Exception` (checked) o `RuntimeException` (unchecked) según el tipo de condición a modelar. Esto permite que la API refleje con mayor exactitud las reglas del dominio de la aplicación: por ejemplo, `DniInvalidoException`, `SaldoInsuficienteException` o `TemperaturaFueraDeRangoException`. Estas clases pueden llevar su propio mensaje, su causa y cualquier otro dato relevante, lo que ayuda a comunicar mejor el error a niveles superiores del programa.

Un ejemplo básico de excepción personalizada sería el siguiente:

```java
public class DatoInvalidoException extends RuntimeException {
    public DatoInvalidoException(String mensaje) {
        super(mensaje);
    }
}
```

Este tipo de extensibilidad permite construir capas completas de manejo de errores adaptadas al problema concreto, manteniendo la encapsulación y evitando depender únicamente de tipos genéricos. Además, los mecanismos de captura (`catch`) pueden discriminar entre diferentes subtipos, lo que facilita reaccionar de manera más precisa ante distintas situaciones excepcionales sin mezclar lógica ni perder claridad en el código.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta
Un objeto excepción en Java siempre transporta **información esencial encapsulada**, mucho más rica que los simples códigos de error de C. La pieza más importante es el **mensaje descriptivo** (`String message`), que explica la causa del problema con claridad y permite comunicar al manejador qué ha fallado y con qué dato. Esto evita tener que depender de números simbólicos o constantes dispersas y facilita que cualquier parte del programa pueda interpretar el error sin conocer detalles internos de la función donde ocurrió.

Además del mensaje, toda excepción incluye automáticamente la **traza de pila** (*stack trace*), que registra la secuencia exacta de llamadas que llevó hasta el punto del error. Esta información es extremadamente valiosa porque permite localizar con precisión dónde se produjo el fallo, incluso si ocurrió en capas profundas del programa. En C, rastrear un error requiere depuración manual, registros explícitos o herramientas externas, mientras que en Java la traza se transmite de forma natural y completamente encapsulada dentro del propio objeto.

Otro elemento esencial es la **causa encadenada** (`Throwable cause`), que permite que una excepción “envuelva” a otra para no perder el motivo original. Esto resulta útil cuando un método de nivel bajo lanza una excepción y un nivel superior necesita traducirla o enriquecerla con más contexto. La cadena de causas conserva tanto el origen técnico de bajo nivel como la explicación de alto nivel, algo difícil de conseguir con códigos de error en C. Así, el manejador final recibe un objeto autosuficiente que encapsula *qué pasó*, *dónde*, *por qué* y *qué lo produjo*.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta
Sí, en Java se pueden tener **varios bloques `catch`** asociados a un mismo `try`. Cada bloque se utiliza para capturar **un tipo distinto de excepción**, normalmente de más específica a más general. Esto permite reaccionar de manera diferenciada según el tipo de problema: por ejemplo, no se maneja igual un error de formato (`NumberFormatException`) que un error de índice (`IndexOutOfBoundsException`). Esta posibilidad mejora la claridad del código y respeta el principio de que cada caso excepcional debe tratarse de la forma más apropiada a su naturaleza.

Aunque pueda haber **varios bloques `catch`**, **solo uno se ejecuta**: el primero cuyo tipo coincida con la excepción lanzada. Una vez encontrado un `catch` adecuado, los demás se ignoran y el control no vuelve a intentarse con otros tipos. Si no se encuentra ningún bloque compatible, la excepción se propaga hacia el llamador. Por este motivo, es importante ordenar los `catch` desde el tipo **más específico** hacia el **más general**, evitando que un `catch(Exception e)` intercepte antes de tiempo casos más concretos.

Un pequeño ejemplo contextualizado con la raíz ilustra este comportamiento:

```java
try {
    double r = Calculadora.raiz(-9.0);
    System.out.println("Resultado: " + r);
} catch (IllegalArgumentException e) {
    System.out.println("Argumento inválido: " + e.getMessage());
} catch (Exception e) {
    System.out.println("Otro error inesperado.");
}
```

En este ejemplo, si `raiz()` lanza una `IllegalArgumentException`, **solo** se ejecuta el primer `catch`; el segundo nunca se alcanza. Esto garantiza un manejo preciso del error, manteniendo el flujo claro y evitando reacciones ambiguas ante situaciones excepcionales.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta
Para garantizar la **liberación de recursos** aunque ocurra una excepción, Java ofrece el bloque `finally`, que se ejecuta **siempre** tras el `try` (haya o no excepción, y haya o no `catch`). Esto asegura el cierre de ficheros, sockets o la liberación de memoria nativa antes de que la excepción continúe **propagándose**. A diferencia de C, donde habría que repetir cierres en múltiples rutas de salida, `finally` centraliza la limpieza y se ejecuta incluso si hay `return` dentro del `try` o del `catch`. La única excepción práctica son eventos catastróficos como un `System.exit()` o apagado de la JVM.

Con **`try-catch-finally`**, se puede manejar la excepción localmente y, aun así, realizar la limpieza garantizada. En el ejemplo, se intenta calcular la raíz y, si el dato es inválido, se informa; pase lo que pase, el “recurso” se cierra en `finally`. Aquí se usa una clase `RecursoSimulado` para ilustrar la idea de cierre (en producción sería, por ejemplo, un `BufferedReader` o un `FileInputStream`):

```java
class RecursoSimulado implements AutoCloseable {
    private final String nombre;
    RecursoSimulado(String nombre) { this.nombre = nombre; }
    void usar() { System.out.println("Usando " + nombre); }
    @Override public void close() { System.out.println("Cerrando " + nombre); }
}

public class DemoConCatch {
    public static void main(String[] args) {
        RecursoSimulado res = new RecursoSimulado("fichero.txt");
        try {
            res.usar();
            double r = Calculadora.raiz(-9.0); // lanzará IllegalArgumentException
            System.out.println("Resultado: " + r);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage()); // manejo local
        } finally {
            res.close(); // se ejecuta SIEMPRE
        }
        System.out.println("Programa continúa...");
    }
}
```

También puede emplearse **`try-finally` sin `catch`** cuando se prefiere que la excepción **se propague** al llamador, pero garantizando la limpieza. Es útil en funciones intermedias que no tienen criterio para decidir cómo recuperarse. El patrón asegura que el recurso se cierra y, acto seguido, la excepción sigue su curso hacia arriba:

```java
public class DemoSinCatch {
    public static void main(String[] args) {
        RecursoSimulado res = new RecursoSimulado("socket");
        try {
            res.usar();
            double r = Calculadora.raiz(-9.0); // lanza; no se captura aquí
            System.out.println("Resultado: " + r); // no se ejecuta
        } finally {
            res.close(); // se ejecuta antes de propagar la excepción
        }
        // No se llega aquí si la excepción no fue capturada a un nivel superior
    }
}
```

> Nota: En Java moderno, cuando se trata de **recursos** que implementan `AutoCloseable`, se recomienda **try-with-resources** (`try (Recurso r = ...) { ... }`) porque cierra automáticamente y maneja correctamente excepciones múltiples al cerrar. Sin embargo, el objetivo aquí es mostrar explícitamente el rol de `finally` tanto con `catch` como sin él.


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta
Sí, en Java el bloque `finally` **puede ir sin `catch`**, siempre que exista un `try` asociado. Esto permite garantizar la ejecución de un código de limpieza incluso cuando no se desea manejar la excepción localmente. En este caso, si en el `try` ocurre una excepción y no hay `catch`, la excepción **se propaga hacia arriba**, pero **antes** se ejecuta el `finally`. Este patrón es habitual cuando un método que actúa como intermediario no tiene criterio para decidir cómo recuperarse, pero sí debe liberar recursos antes de delegar el error al llamador.

El bloque `finally` se **ejecuta siempre**, tanto si ocurre como si no ocurre una excepción. Su función precisamente es asegurar que se realicen acciones esenciales de cierre, liberación o limpieza independientemente del flujo del programa. Solo situaciones extremas como `System.exit()`, un corte de energía o un fallo grave de la JVM impedirían su ejecución. En condiciones normales, incluso si la instrucción dentro del `try` finaliza con excepción, con retorno o hasta con una interrupción parcial del flujo, el `finally` garantiza ejecutar su contenido.

Si dentro del `try` hay un **`return`**, el `finally` **también se ejecuta** antes de devolver el valor. Este comportamiento evita salidas prematuras que dejen recursos sin liberar. Así, el orden de ejecución es: se prepara el valor de retorno, se ejecuta `finally` y luego el método devuelve el valor. Este detalle es importante porque, si el `finally` también contiene un `return`, este **sobrescribe** al del `try`, lo cual suele ser una mala práctica porque confunde el flujo de lectura.

Un ejemplo simple ilustra estos comportamientos:

```java
public static int ejemplo(boolean caso) {
    try {
        if (caso) return 1;          // return dentro del try
        int x = 5 / 0;               // provoca ArithmeticException
        return 2;                    // no se ejecuta
    } finally {
        System.out.println("Se ejecuta SIEMPRE.");
    }
}
```

Al llamar:

```java
System.out.println(ejemplo(true));
// Imprime:
// Se ejecuta SIEMPRE.
// 1

System.out.println(ejemplo(false));
// Imprime:
// Se ejecuta SIEMPRE.
// (luego la excepción sigue propagándose)
```

En ambos casos, el bloque `finally` se ejecuta, ya sea antes de devolver el valor del `return` o antes de propagar la excepción que se produjo en el `try`. Este mecanismo garantiza una limpieza robusta y consistente en cualquier flujo de ejecución.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta
En Java, las **excepciones controladas (checked)** son aquellas que el compilador obliga a **declarar** o **capturar**, porque representan situaciones anómalas **previsibles** y normalmente **recuperables** (por ejemplo, errores de E/S o problemas al acceder a un recurso externo). Si un método puede producir una de estas excepciones y no la maneja con `try–catch`, debe declararla con `throws`. Por el contrario, las **excepciones no controladas (unchecked)** son las que **no requiere** capturar ni declarar, porque suelen indicar **errores de programación**, como violaciones de precondiciones o estados inconsistentes. Estas excepciones derivan de `RuntimeException`, que actúa como la raíz de todas las excepciones no controladas.

La clase **`RuntimeException`** desempeña un papel central al marcar una excepción como **no controlada**. Cualquier excepción que derive de ella indica que el problema se puede evitar corrigiendo el código del programador, y no mediante una recuperación explícita en tiempo de ejecución. Ejemplos típicos de estas son `NullPointerException` o `IllegalArgumentException`. En contraste, las controladas (`IOException`, `SQLException`, etc.) obligan a gestionar escenarios externos o fallos operativos que no dependen del programador. Por tanto, la diferencia clave no es técnica sino de **intención de diseño**: qué debe manejar el programador que llama y qué indica un mal uso de la API.

Es posible crear tanto **excepciones controladas** como **no controladas** personalizadas. Para controlar explícitamente una situación excepcional recuperable, se extendería `Exception`; mientras que para señalar errores lógicos del código, se extendería `RuntimeException`. Esta distinción ayuda a comunicar de forma clara el contrato de uso de un método y a mantener la coherencia en la arquitectura de la aplicación. Así, las excepciones se convierten en parte de la interfaz pública de las clases, contribuyendo a la corrección y expresividad del diseño orientado a objetos.

### Ejemplos de excepciones típicas

**Controladas (checked):**

*   `IOException`
*   `FileNotFoundException`
*   `SQLException`
*   `ClassNotFoundException`

**No controladas (unchecked):**

*   `NullPointerException`
*   `IllegalArgumentException`
*   `IndexOutOfBoundsException`
*   `ArithmeticException`

### Situaciones donde se prefiere una excepción **controlada (checked)**

*   Acceso a un fichero que podría no existir o no poder abrirse.
*   Comunicación por red donde puede fallar la conexión.
*   Operaciones con bases de datos susceptibles a errores externos.
*   Lectura/escritura de datos cuyo formato puede no cumplirse.

### Situaciones donde se prefiere una excepción **no controlada (unchecked)**

*   Un método recibe un **parámetro inválido** (ej.: `x < 0` en la raíz).
*   Se intenta acceder a una estructura fuera de rango.
*   El estado interno de un objeto no cumple sus invariantes.
*   Se viola una precondición del método por error del programador.


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta
En Java, la palabra clave **`throws`** se utiliza en la **firma de un método** para indicar que dicho método **puede producir una excepción controlada (checked)** y que **no va a manejarla internamente**. Esto comunica claramente al código llamador que la operación puede fallar en condiciones normales (por ejemplo, I/O, ficheros, bases de datos) y que es responsabilidad del llamador decidir cómo reaccionar. Así, `throws` forma parte del **contrato público** del método, igual que sus parámetros y su valor de retorno.

Se usa porque en Java las **excepciones controladas obligan al compilador** a que se tome una decisión: o bien se **captura** la excepción con un `try–catch`, o bien se **declara** con `throws`. Por tanto, `throws` es una **alternativa válida** a capturar la excepción, y se utiliza cuando el método que la detecta **no sabe** o **no debe** decidir qué hacer ante ese problema. Esto es habitual en métodos de bajo nivel que acceden a recursos externos y prefieren delegar la responsabilidad a niveles superiores donde existe más contexto para actuar.

En el fondo, `throws` permite que la **propagación natural** de la excepción continúe hacia arriba, sin bloquearla ni ocultarla. Esta decisión mejora la modularidad del código: cada método se centra en su función principal y deja que quien tiene la perspectiva global (por ejemplo, la capa de interfaz o lógica de negocio) decida si mostrar un mensaje, reintentar, registrar o traducir la excepción. Así, el mecanismo evita manejar errores prematuramente donde no se dispone de suficiente información.

Un ejemplo básico:

```java
public static double cargarDato(String ruta) throws IOException {
    // Este método no captura la IOException: la delega al llamador
    BufferedReader br = new BufferedReader(new FileReader(ruta));
    return Double.parseDouble(br.readLine());
}

public static void main(String[] args) {
    try {
        double x = cargarDato("datos.txt");
        System.out.println("Valor leído: " + x);
    } catch (IOException e) {
        System.out.println("Error de lectura: " + e.getMessage());
    }
}
```

En este ejemplo, `cargarDato()` **declara** que puede lanzar una `IOException`, lo que obliga a `main()` a **tratarla** o a su vez declararla. Así se mantiene un flujo claro y estructurado: el método de lectura se limita a leer, mientras que el manejo del fallo queda donde tiene sentido semántico.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta
Una forma idiomática de **declarar** que un método *no maneja* la excepción de fichero inexistente (ni otras de E/S) es añadir `throws` en su firma y dejar que la excepción **se propague** al llamador. Aun así, debe garantizarse el **cierre del recurso** en un `finally`. En el ejemplo siguiente, si el fichero no existe, `new FileReader(ruta)` lanzará `FileNotFoundException` (subtipo de `IOException`) y el método no la captura; el `finally` cerrará el `BufferedReader` si llegó a abrirse. Nótese que el método declara `throws IOException` para cubrir tanto la apertura como el cierre.

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Ficheros {

    /**
     * Abre un fichero y devuelve su primera línea.
     * No maneja las excepciones de E/S: las declara y deja que se propaguen.
     */
    public static String leerPrimeraLinea(String ruta) throws IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(ruta)); // puede lanzar FileNotFoundException
            return br.readLine();                           // puede lanzar IOException
        } finally {
            // Se garantiza el cierre aunque ocurra excepción o haya 'return'
            if (br != null) {
                br.close(); // puede lanzar IOException; está declarado en la firma
            }
        }
    }
}
```

Desde el punto de vista del llamador, hay dos opciones: **capturar** la excepción o **declararla** de nuevo con `throws`. En el siguiente `main`, se muestra la primera opción (manejo en el borde de la aplicación). Si se quisiera que también `main` dejase propagar la excepción (por ejemplo, en pruebas), bastaría con declararla: `public static void main(String[] args) throws IOException`.

```java
public class App {
    public static void main(String[] args) {
        try {
            String linea = Ficheros.leerPrimeraLinea("datos.txt");
            System.out.println("Primera línea: " + linea);
        } catch (IOException e) {
            System.out.println("No se pudo leer el fichero: " + e.getMessage());
        }
    }
}
```

> Nota: En Java moderno se prefiere **try-with-resources** (`try (BufferedReader br = ...) { ... }`) porque cierra automáticamente y gestiona mejor excepciones concurrentes (por ejemplo, al cerrar). Sin embargo, el ejemplo anterior ilustra explícitamente el uso de `finally` solicitado y la idea de **propagar** las excepciones de E/S con `throws`.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta
Sí, técnicamente se puede **declarar en `throws`** excepciones **no controladas** (subclases de `RuntimeException`). Sin embargo, hacerlo **no impone ninguna obligación** al llamador: el compilador **no exige** `try-catch` ni volver a declararlas. Por eso, incluir `throws RuntimeException` o similares suele ser **redundante** desde el punto de vista técnico. Aun así, puede tener valor **documental**: explicita en la firma que, bajo ciertas condiciones, el método puede fallar con un tipo concreto de *unchecked* (mejor todavía si se acompaña de `@throws` en Javadoc con la precondición que se viola).

El método llamador **no tiene por qué** poner `try-catch` para una `RuntimeException`; solo debería capturarla si **puede recuperarse** de forma sensata (p. ej., validar y pedir otro dato, elegir una alternativa segura). En la mayoría de los casos, las *unchecked* indican **errores de programación** o de contrato (parámetros inválidos, estados ilegales), y se deja que **se propaguen** hasta un manejador de alto nivel (frontera de la app) que registra y finaliza/retorna un error genérico. Capturar ampliamente `RuntimeException` en capas intermedias suele ser mala práctica porque oculta fallos y dificulta depurar; es preferible capturar tipos **específicos** solo cuando haya una acción correctiva clara.

¿Entonces qué sentido tiene declararlas en `throws`? Principalmente, **documentación de API** y comunicación del **contrato** (por ejemplo, “lanza `IllegalArgumentException` si `x < 0`”). No cambia las reglas de **sobrescritura** (las restricciones de `throws` solo aplican a *checked*), ni obliga al cliente a manejarlas. Un patrón razonable es capturar *unchecked* únicamente en la **capa frontera** para registrar y responder con seguridad:

```java
public static void main(String[] args) {
    try {
        double r = Calculadora.raiz(-9.0); // puede lanzar IllegalArgumentException (unchecked)
        System.out.println("Resultado: " + r);
    } catch (IllegalArgumentException e) { // captura específica si hay respuesta útil
        System.out.println("Dato inválido: " + e.getMessage());
    }
    // En servicios web/GUI, a menudo se captura aquí cualquier RuntimeException para loguear y degradar.
}
```


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta
En Java, las **excepciones controladas (checked)** se recomiendan cuando el fallo procede de **factores externos al programa** y es **razonable esperar** que el llamador pueda *intentarlo de otra manera* o *tomar una decisión alternativa*. Este es el caso típico de la E/S: ficheros que pueden no existir, problemas de red, acceso a bases de datos o recursos del sistema. En esas situaciones, el método no puede garantizar el éxito y quien llama **debe** tomar una decisión explícita. Por eso `IOException` y subtipos son checked: representan condiciones *previsibles* pero *no controlables* por el programador.

Por el contrario, las **excepciones no controladas (unchecked)** —subclases de `RuntimeException`— se recomiendan cuando la causa del fallo está en un **mal uso de la API** o en una **violación de precondiciones**. Es decir, cuando el problema puede y debe resolverse *corrigiendo el código*, no manejándolo en tiempo de ejecución. Ejemplos típicos son recibir argumentos inválidos (`IllegalArgumentException`), estados internos ilegales (`IllegalStateException`), índices fuera de rango (`IndexOutOfBoundsException`) o divisiones entre cero (`ArithmeticException`). Estas situaciones reflejan errores lógicos del programador, no fallos del entorno, y por ello no se obliga a capturarlas.

No todos los lenguajes ofrecen esta distinción entre **checked** y **unchecked**. De hecho, Java es uno de los pocos que la mantiene de forma estricta. Lenguajes como **C++, C#, Python, Kotlin, Go o Rust** solo tienen **excepciones no controladas** (o mecanismos equivalentes basados en errores), y la práctica habitual es dejar que los errores se propaguen sin obligación de capturarlos. Esto se debe a que el modelo de excepciones controladas, aunque expresivo, puede generar código más verboso, acoplar capas e incentivar capturas innecesarias. Por ello, en la mayoría de lenguajes modernos la opción preferida es la de excepciones **sin obligación de captura**, es decir, equivalentes al estilo de `RuntimeException`.

En resumen, Java distingue ambos mundos: checked para fallos externos recuperables y unchecked para errores de programación. Pero en la mayoría de lenguajes, donde solo existe una de las opciones, la predominante es la de **excepciones no controladas**, pues permite un flujo más limpio y menos burocrático, dejando la responsabilidad del manejo al nivel adecuado sin obligar al compilador a intervenir.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta
Sí, **tiene sentido lanzar excepciones dentro de un `catch`**, y es una práctica habitual cuando el manejador detecta que **no puede resolver** la situación localmente o cuando necesita **traducir el error** a un tipo más adecuado para la capa actual. En estos casos, el `catch` actúa como un punto de conversión: recibe una excepción de bajo nivel y lanza otra de más alto nivel, más coherente con la lógica del módulo. Esto permite encapsular detalles internos (por ejemplo, una `IOException`) y exponer una excepción más significativa para el dominio de la aplicación.

También es completamente posible **relanzar la misma excepción capturada** usando simplemente `throw;` o, en Java, `throw e;`. Esto tiene sentido cuando el método desea realizar alguna acción local (registrar, limpiar recursos adicionales, medir tiempos, etc.) pero **no quiere asumir la responsabilidad** de decidir cómo recuperarse; por ello, deja que la excepción continúe propagándose hacia niveles superiores. Con este patrón, el `catch` no “absorbe” el error, sino que lo deja seguir su curso tras realizar tareas auxiliares. En estos casos, el objetivo no es transformar la excepción, sino añadir contexto o ejecutar un paso necesario antes del reenvío.

Un ejemplo donde se **lanza otra excepción distinta** (traducción de excepción):

```java
try {
    double r = Calculadora.raiz(-9.0);
    System.out.println(r);
} catch (IllegalArgumentException e) {
    // Traducción a una excepción de nivel de negocio
    throw new RuntimeException("Error al procesar entrada del usuario", e);
}
```

Y un ejemplo donde se **relanza la misma excepción** después de registrar el error:

```java
try {
    double r = Calculadora.raiz(-9.0);
} catch (IllegalArgumentException e) {
    System.err.println("Registro: fallo de parámetro -> " + e.getMessage());
    throw e;  // se relanza la misma excepción
}
```

En resumen, lanzar dentro de un `catch` es útil para **mejorar la semántica del error**, y relanzar la misma excepción es adecuado cuando se desea realizar acciones locales sin ocultar la causa original. Ambos patrones son fundamentales para un manejo de errores estructurado y coherente.


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta
Ser la **“causa”** de otra excepción significa que una excepción de alto nivel *encapsula* a otra de bajo nivel como su **origen** (`cause`). En Java esto se logra pasando la excepción original como segundo argumento en el constructor (`new MiExcepcion(msg, causa)`) o con `initCause`. El encadenamiento preserva el detalle técnico (p. ej., un fallo de E/S) mientras se expone una excepción más significativa para el dominio (p. ej., “no se pudo cargar configuración”), manteniendo trazabilidad sin filtrar detalles de implementación hacia capas superiores.

Un ejemplo típico es **capturar** una excepción de bajo nivel (`IOException`) y **envolverla** en una excepción personalizada de negocio (`ConfiguracionException`) que expresa el problema en términos de la aplicación. Así, la capa superior puede reaccionar con un mensaje adecuado sin perder la traza que ayuda a diagnosticar. Además, el método `getCause()` permite recuperar la causa original si se requiere un manejo más fino.

```java
// Excepción de alto nivel (personalizada)
public class ConfiguracionException extends Exception {
    public ConfiguracionException(String mensaje, Throwable causa) {
        super(mensaje, causa); // conserva la causa
    }
}

// Uso: capturar y encapsular la causa
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class CargadorConfig {
    public static String cargar(String ruta) throws ConfiguracionException {
        try (BufferedReader br = new BufferedReader(new FileReader(ruta))) {
            return br.readLine(); // puede lanzar IOException
        } catch (IOException e) {
            // Se traduce a una excepción de dominio manteniendo la CAUSA
            throw new ConfiguracionException("No se pudo cargar la configuración desde: " + ruta, e);
        }
    }

    public static void main(String[] args) {
        try {
            System.out.println(cargar("config.txt"));
        } catch (ConfiguracionException e) {
            e.printStackTrace(); // Muestra la cadena de causas
            // También podría usarse: System.err.println("Causa raíz: " + e.getCause());
        }
    }
}
```

Cuando una excepción con causa se imprime (por ejemplo, con `printStackTrace()` o cuando la JVM reporta una **excepción no capturada**), se muestra la **cadena completa** con el formato estándar `Caused by: ...` para cada nivel. Es decir, aparece la traza de la excepción de alto nivel y, debajo, “**Caused by** `IOException: ...`” con su propia traza, y así sucesivamente para causas encadenadas. Esto facilita localizar la **causa raíz** sin perder el contexto semántico que proporciona la excepción de alto nivel.
