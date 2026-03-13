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

La programación orientada a objetos se basa en cuatro características fundamentales que permiten organizar el código de manera más modular y reutilizable. La primera es la abstracción, que consiste en representar conceptos complejos del mundo real mediante clases y objetos, enfocándose en los aspectos esenciales y ocultando detalles innecesarios. La segunda es el encapsulamiento, que implica agrupar datos y las operaciones que los manipulan dentro de una unidad, protegiendo el estado interno de un objeto y permitiendo el acceso controlado a través de métodos públicos.

La tercera característica es la herencia, que permite crear nuevas clases basadas en clases existentes, heredando sus propiedades y comportamientos, lo que facilita la reutilización de código y la creación de jerarquías. Finalmente, el polimorfismo permite que objetos de diferentes clases respondan de manera diferente a la misma operación, proporcionando flexibilidad en el diseño del software. Estas características, aunque novedosas para quienes provienen de lenguajes procedurales como C, ofrecen una forma estructurada de modelar problemas complejos.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Entre los lenguajes de programación que soportan la orientación a objetos se encuentran Java, C++, Python y C#. Java es ampliamente utilizado en aplicaciones empresariales y móviles debido a su portabilidad. C++ extiende el lenguaje C con características orientadas a objetos, manteniendo la eficiencia de bajo nivel.

Python ofrece una sintaxis sencilla y es popular en ciencia de datos y desarrollo web. C# se emplea principalmente en el ecosistema de Microsoft para aplicaciones de escritorio y web. Estos lenguajes permiten implementar las características básicas de la POO, adaptándose a diferentes necesidades de desarrollo.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

La programación estructurada es un paradigma que organiza el código en estructuras de control como secuencias, selecciones e iteraciones, evitando el uso indiscriminado de saltos como goto. Este enfoque, común en lenguajes como C, facilita la escritura de programas más legibles y mantenibles al dividir el código en funciones o procedimientos que realizan tareas específicas.

La programación modular va un paso más allá al dividir el programa en módulos independientes que encapsulan funcionalidad relacionada, permitiendo la reutilización y el desarrollo paralelo. Cada módulo puede tener su propia interfaz, lo que reduce la complejidad y facilita las pruebas. Aunque estos paradigmas no incluyen conceptos como clases y objetos, proporcionan una base sólida para entender la evolución hacia la POO.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

Un objeto en programación orientada a objetos se define por tres elementos principales: el estado, el comportamiento y la identidad. El estado representa los datos que el objeto contiene, almacenados en atributos o variables de instancia. El comportamiento se refiere a las acciones que el objeto puede realizar, implementadas a través de métodos.

La identidad es lo que distingue a un objeto de otros, incluso si tienen el mismo estado y comportamiento. Estos elementos permiten modelar entidades del mundo real de manera más intuitiva que en lenguajes procedurales, donde los datos y las funciones están separados.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Una clase es una plantilla o blueprint que define la estructura y el comportamiento de objetos, especificando atributos y métodos. No es lo mismo que un objeto, ya que la clase es la definición abstracta, mientras que el objeto es una entidad concreta creada a partir de ella. Una instancia es un objeto específico creado de una clase, con sus propios valores para los atributos.

No todos los lenguajes orientados a objetos manejan el concepto de clase de la misma manera; por ejemplo, algunos como JavaScript utilizan prototipos en lugar de clases tradicionales. Sin embargo, el concepto de clase es fundamental en lenguajes como Java y C++.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta

Los objetos en lenguajes orientados a objetos se almacenan típicamente en el heap, una región de memoria dinámica gestionada por el sistema. Esto difiere de las variables locales en la pila, como en C, donde la memoria se asigna y libera manualmente. No es igual en todos los lenguajes; algunos permiten más control sobre la asignación de memoria.

La recolección de basura es un mecanismo automático que libera la memoria ocupada por objetos que ya no se utilizan, evitando fugas de memoria comunes en lenguajes procedurales. Este proceso es manejado por el entorno de ejecución, como la JVM en Java, simplificando la gestión de memoria para el programador.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta

Un método es una función definida dentro de una clase que opera sobre los datos del objeto o realiza acciones relacionadas. A diferencia de las funciones en C, los métodos están asociados a instancias de clases y pueden acceder a sus atributos. La sobrecarga de métodos permite definir múltiples métodos con el mismo nombre pero con diferentes parámetros, lo que facilita la flexibilidad en el código.

Este concepto no existe en lenguajes puramente procedurales, donde las funciones se distinguen únicamente por su nombre. La sobrecarga se resuelve en tiempo de compilación basándose en los tipos y número de argumentos proporcionados.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

Se presenta un ejemplo de clase en Java que ilustra los conceptos básicos de clases y objetos. La clase Punto define dos atributos para las coordenadas y un método para calcular la distancia al origen.

```java
class Punto {
    double x;
    double y;
    
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

Para utilizar esta clase, se crea una instancia y se invoca el método.

```java
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;
        double distancia = p.calculaDistanciaAOrigen();
        System.out.println("Distancia: " + distancia);
    }
}
```

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

El punto de entrada en un programa Java es el método main, definido como public static void main(String[] args). Este método es donde comienza la ejecución del programa. La palabra clave static indica que el método pertenece a la clase en sí, no a una instancia específica, permitiendo su invocación sin crear un objeto.

No se emplea únicamente para el método main; static se utiliza para variables y métodos de clase que se comparten entre todas las instancias. Cuando se combina con final, como en static final, define constantes de clase que no pueden modificarse, proporcionando valores inmutables compartidos.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

Para compilar un programa Java, se utiliza el comando javac seguido del nombre del archivo fuente, como javac Main.java. Esto genera archivos .class con bytecode. Para ejecutar, se usa java seguido del nombre de la clase principal, como java Main.

Java es un lenguaje compilado, pero a bytecode intermedio en lugar de código máquina nativo. La máquina virtual Java (JVM) interpreta y ejecuta este bytecode, proporcionando portabilidad entre plataformas. Los archivos .class contienen el bytecode compilado, listo para ser ejecutado por la JVM.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

La palabra clave new en Java se utiliza para crear una nueva instancia de una clase, asignando memoria en el heap y llamando al constructor. Un constructor es un método especial que inicializa el objeto recién creado, con el mismo nombre que la clase y sin tipo de retorno.

En el ejemplo de la clase Empleado, se define un constructor que recibe parámetros para inicializar los atributos.

```java
class Empleado {
    String dni;
    String nombre;
    String apellidos;
    
    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
```

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? ¿Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

La referencia this en Java apunta al objeto actual sobre el que se está ejecutando el método, permitiendo acceder a sus atributos y métodos. No se llama igual en todos los lenguajes; en C++ se utiliza this, pero en Python se usa self.

En la clase Punto, this se emplea para distinguir entre parámetros y atributos con el mismo nombre.

```java
class Punto {
    double x;
    double y;
    
    void setCoordenadas(double x, double y) {
        this.x = x;
        this.y = y;
    }
}
```

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

Se añade el método distanciaA a la clase Punto para calcular la distancia euclidiana entre dos puntos.

```java
class Punto {
    double x;
    double y;
    
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
    
    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Este método utiliza this para referirse al punto actual y calcula la distancia al punto pasado como parámetro.

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta

En Java, los objetos como Punto se pasan por referencia, lo que significa que el método recibe una copia de la referencia al objeto original. Si se modifica un atributo del objeto dentro del método, los cambios afectan al objeto fuera del método, ya que apuntan al mismo lugar en memoria.

Sin embargo, para tipos primitivos como int, el paso es por valor, creando una copia independiente. Modificar el int dentro del método no afecta al valor original fuera de él, similar al comportamiento en C para variables escalares.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

El método toString() en Java proporciona una representación en cadena de texto del objeto, útil para depuración e impresión. Se invoca automáticamente en ciertas situaciones, como al concatenar con strings. Existe en otros lenguajes orientados a objetos, como C# con ToString().

En la clase Punto, se puede sobrescribir toString() para mostrar las coordenadas.

```java
class Punto {
    double x;
    double y;
    
    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
}
```

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

### Respuesta

Un struct en C es similar a una clase en que agrupa datos relacionados, pero le falta la capacidad de incluir funciones o métodos. En una clase, además de atributos, se definen comportamientos a través de métodos, permitiendo encapsular lógica junto con los datos.

Además, las instancias de clases tienen identidad única y pueden participar en herencia y polimorfismo, características ausentes en structs. Los structs en C son meras agrupaciones de datos sin comportamiento asociado.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

Para emular una clase Punto en C usando struct, se define el struct con los atributos y una función separada que recibe un puntero al struct como parámetro, simulando this.

```c
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

double calculaDistanciaAOrigen(Punto* p) {
    return sqrt(p->x * p->x + p->y * p->y);
}
```

En este enfoque, this se reemplaza por el puntero explícito pasado a la función, requiriendo que el programador gestione manualmente las referencias.
