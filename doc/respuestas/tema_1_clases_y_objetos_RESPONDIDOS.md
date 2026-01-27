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


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

