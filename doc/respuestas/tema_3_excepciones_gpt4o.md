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

En C, donde no existen excepciones, el manejo de errores se realiza mediante códigos de retorno o el uso de variables globales como `errno`. A continuación, se presentan dos enfoques para manejar el error de recibir un número negativo en una función que calcula la raíz cuadrada:

**Opción 1: Usar un código de retorno**

En este enfoque, la función devuelve un valor especial, como `-1`, para indicar que ocurrió un error. El código llamador debe verificar este valor y manejar el error.

```c
#include <stdio.h>
#include <math.h>

float raiz(float numero) {
    if (numero < 0) {
        return -1; // Indica error
    }
    return sqrt(numero);
}

int main() {
    float numero = -4.0;
    float resultado = raiz(numero);

    if (resultado == -1) {
        printf("Error: no se puede calcular la raíz cuadrada de un número negativo.\n");
    } else {
        printf("La raíz cuadrada es: %.2f\n", resultado);
    }

    return 0;
}
```

**Opción 2: Usar una variable global para el estado de error**

Otra opción es utilizar una variable global para indicar si ocurrió un error. Esto permite que la función devuelva un valor válido, pero el estado de error se debe verificar por separado.

```c
#include <stdio.h>
#include <math.h>

int error = 0; // Variable global para el estado de error

float raiz(float numero) {
    if (numero < 0) {
        error = 1; // Indica error
        return 0; // Devuelve un valor por defecto
    }
    error = 0; // No hay error
    return sqrt(numero);
}

int main() {
    float numero = -4.0;
    float resultado = raiz(numero);

    if (error) {
        printf("Error: no se puede calcular la raíz cuadrada de un número negativo.\n");
    } else {
        printf("La raíz cuadrada es: %.2f\n", resultado);
    }

    return 0;
}
```

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una excepción es un mecanismo que permite manejar errores o situaciones inesperadas que ocurren durante la ejecución de un programa. En lugar de depender de códigos de retorno o variables globales, las excepciones interrumpen el flujo normal del programa y transfieren el control a un bloque de código diseñado específicamente para manejar el error.

El objetivo principal de las excepciones es mejorar la legibilidad y el mantenimiento del código. Al usar excepciones, un programador puede separar claramente la lógica principal del programa de la lógica de manejo de errores. Esto facilita la detección y corrección de errores, ya que las excepciones proporcionan información detallada sobre el problema ocurrido, como el tipo de error y el lugar donde se originó.

Además, las excepciones permiten manejar errores de manera jerárquica y estructurada. Por ejemplo, en Java, se pueden capturar diferentes tipos de excepciones en bloques `catch` separados, lo que permite tratar cada tipo de error de manera específica. Esto es especialmente útil en programas complejos donde pueden ocurrir múltiples tipos de errores.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

En Java, el ejemplo de cálculo de la raíz cuadrada puede implementarse utilizando excepciones para manejar errores. A continuación, se muestra cómo hacerlo:

```java
class Calculadora {
    public double raiz(double numero) throws IllegalArgumentException {
        if (numero < 0) {
            throw new IllegalArgumentException("No se puede calcular la raíz cuadrada de un número negativo.");
        }
        return Math.sqrt(numero);
    }
}

public class Main {
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        try {
            double resultado = calc.raiz(-4);
            System.out.println("La raíz cuadrada es: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

En este ejemplo, el método `raiz` lanza una excepción `IllegalArgumentException` si el número proporcionado es negativo. El método `main` utiliza un bloque `try-catch` para capturar y manejar esta excepción, mostrando un mensaje de error al usuario.

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

**Lanzar una excepción:** En Java, "lanzar" una excepción significa interrumpir el flujo normal del programa y transferir el control a un manejador de excepciones. Esto se logra utilizando la palabra clave `throw`, seguida de un objeto de excepción. Por ejemplo, en el método `raiz` del ejemplo anterior, se lanza una excepción `IllegalArgumentException` si el número es negativo.

**Capturar una excepción:** "Capturar" o "controlar" una excepción implica manejar el error en un bloque `catch`. Cuando se lanza una excepción, el programa busca un bloque `catch` adecuado que pueda manejar ese tipo de excepción. Si se encuentra, el control se transfiere a ese bloque, donde se puede realizar el manejo del error.

**Propagar una excepción:** Si una excepción no se captura en el método donde se lanza, se "propaga" hacia el método que lo llamó. Este proceso continúa hacia arriba en la pila de llamadas hasta que se encuentra un manejador adecuado o el programa termina abruptamente. Durante la propagación, los métodos en la pila de llamadas se "desapilan", liberando los recursos asociados.

En el ejemplo anterior, si el método `main` no tuviera un bloque `catch` para manejar la excepción `IllegalArgumentException`, esta se propagaría fuera del método `main`, causando que el programa termine con un mensaje de error que incluye la traza de la pila, indicando el lugar donde ocurrió la excepción y los métodos involucrados en la propagación.

```java
public class Main {
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        double resultado = calc.raiz(-4); // Lanza IllegalArgumentException
        System.out.println("La raíz cuadrada es: " + resultado);
    }
}
```

En este caso, el programa terminaría con un mensaje de error que incluye la traza de la pila, indicando el lugar donde ocurrió la excepción y los métodos involucrados en la propagación.

## 5. ¿Qué es **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

En C, el manejo de errores se realiza manualmente, lo que puede llevar a un código más complejo y propenso a errores. En Java, la propagación natural de excepciones a través de la pila de llamadas ofrece varias ventajas significativas:

1. **Legibilidad y mantenimiento:** Al usar excepciones, el manejo de errores se separa de la lógica principal del programa. Esto hace que el código sea más fácil de leer y mantener, ya que no es necesario verificar constantemente los códigos de retorno o estados de error.

2. **Propagación automática:** Cuando ocurre una excepción, esta se propaga automáticamente a través de la pila de llamadas hasta que se encuentra un manejador adecuado. Esto elimina la necesidad de verificar manualmente los errores en cada nivel de la pila, como se haría en C.

3. **Información detallada:** Las excepciones en Java incluyen información útil, como el tipo de error, el mensaje asociado y la traza de la pila. Esto facilita la depuración y el diagnóstico de problemas.

4. **Manejo jerárquico:** Java permite capturar diferentes tipos de excepciones en bloques `catch` separados, lo que permite un manejo más específico y estructurado de los errores.

En resumen, la propagación natural de excepciones en Java simplifica el manejo de errores, mejora la claridad del código y reduce la probabilidad de errores al delegar automáticamente el control a los manejadores adecuados.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

Las excepciones en Java suelen ser objetos. Esto tiene varias ventajas en términos de encapsulación:

1. **Encapsulación de información:** Al ser objetos, las excepciones pueden contener información detallada sobre el error, como un mensaje descriptivo, un código de error y datos adicionales relevantes. Esto permite que el manejador de excepciones acceda a toda la información necesaria para diagnosticar y resolver el problema.

2. **Jerarquía y reutilización:** Las excepciones en Java heredan de la clase base `Throwable`, lo que permite organizar las excepciones en una jerarquía. Esto facilita la reutilización de código y el manejo de errores de manera específica o genérica, según sea necesario.

3. **Creación de excepciones personalizadas:** Los programadores pueden definir sus propias clases de excepciones que extiendan las clases existentes, como `Exception` o `RuntimeException`. Esto permite encapsular lógica específica del dominio en las excepciones, mejorando la claridad y el mantenimiento del código.

Por ejemplo, se puede crear una excepción personalizada para manejar errores específicos en una aplicación:

```java
class MiExcepcion extends Exception {
    public MiExcepcion(String mensaje) {
        super(mensaje);
    }
}

public class Main {
    public static void main(String[] args) {
        try {
            throw new MiExcepcion("Error personalizado");
        } catch (MiExcepcion e) {
            System.out.println("Se capturó: " + e.getMessage());
        }
    }
}
```

En este ejemplo, la excepción personalizada `MiExcepcion` encapsula un mensaje específico, demostrando cómo las excepciones como objetos mejoran la encapsulación y la flexibilidad.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

Las excepciones en Java llevan información esencial como el tipo de error y el lugar donde se originó.

### Respuesta

En comparación con el manejo de errores en C, los objetos de excepción en Java proporcionan información esencial que es muy útil para los manejadores de errores. Esta información incluye:

1. **Tipo de excepción:** Cada excepción tiene una clase específica que indica el tipo de error ocurrido. Esto permite a los programadores manejar diferentes tipos de errores de manera diferenciada y precisa.

2. **Mensaje descriptivo:** Las excepciones suelen incluir un mensaje que describe el problema. Este mensaje puede ser personalizado al lanzar la excepción, proporcionando detalles adicionales sobre el error.

3. **Traza de la pila:** Las excepciones en Java incluyen una traza de la pila que muestra la secuencia de llamadas de método que llevaron al error. Esto es invaluable para depurar y localizar el origen del problema.

Por ejemplo, al manejar una excepción en Java, se puede acceder a esta información:

```java
try {
    throw new IllegalArgumentException("Número negativo no permitido");
} catch (IllegalArgumentException e) {
    System.out.println("Tipo de excepción: " + e.getClass().getName());
    System.out.println("Mensaje: " + e.getMessage());
    e.printStackTrace(); // Muestra la traza de la pila
}
```

En este ejemplo, el manejador de excepciones utiliza el tipo, el mensaje y la traza de la pila para diagnosticar y manejar el error de manera efectiva. Esta encapsulación de información es una de las principales ventajas de las excepciones como objetos en Java.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

En Java, es posible tener más de un bloque `catch` asociado a un bloque `try`. Esto permite manejar diferentes tipos de excepciones de manera específica. Sin embargo, solo se ejecuta un bloque `catch`, el primero que coincida con el tipo de excepción lanzada.

Por ejemplo:

```java
public class Ejemplo {
    public static void main(String[] args) {
        try {
            int resultado = 10 / 0; // Lanza ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Error: División por cero.");
        } catch (Exception e) {
            System.out.println("Error genérico: " + e.getMessage());
        }
    }
}
```

En este ejemplo, si ocurre una excepción de tipo `ArithmeticException`, se ejecutará el primer bloque `catch`. Si se lanza otro tipo de excepción, se ejecutará el segundo bloque `catch` que maneja excepciones genéricas.

Es importante tener en cuenta que los bloques `catch` deben estar ordenados de más específicos a más generales. Si un bloque `catch` genérico aparece antes que uno específico, el compilador generará un error, ya que el bloque genérico capturaría todas las excepciones, haciendo que los bloques específicos sean inaccesibles.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

El bloque `finally` se ejecuta siempre, independientemente de si ocurre o no una excepción.

### Respuesta

Para garantizar que se ejecuten ciertas acciones, como cerrar ficheros o liberar recursos, independientemente de si ocurre una excepción, Java proporciona el bloque `finally`. Este bloque se ejecuta siempre después del bloque `try`, ya sea que ocurra o no una excepción.

Ejemplo con `catch`:

```java
import java.io.*;

public class Ejemplo {
    public static void main(String[] args) {
        BufferedReader reader = null;
        try {
            reader = new BufferedReader(new FileReader("archivo.txt"));
            String linea = reader.readLine();
            System.out.println(linea);
        } catch (IOException e) {
            System.out.println("Error al leer el archivo: " + e.getMessage());
        } finally {
            try {
                if (reader != null) {
                    reader.close();
                    System.out.println("Archivo cerrado correctamente.");
                }
            } catch (IOException e) {
                System.out.println("Error al cerrar el archivo: " + e.getMessage());
            }
        }
    }
}
```

Ejemplo sin `catch`:

```java
public class EjemploSinCatch {
    public static void main(String[] args) {
        try {
            int resultado = 10 / 0; // Lanza ArithmeticException
        } finally {
            System.out.println("Este mensaje siempre se muestra.");
        }
    }
}
```

En el primer ejemplo, el bloque `finally` garantiza que el archivo se cierre correctamente, incluso si ocurre una excepción al leerlo. En el segundo ejemplo, el bloque `finally` se ejecuta incluso cuando no hay un bloque `catch`. Esto asegura que las acciones críticas se realicen siempre.

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

El bloque `finally` se ejecuta siempre, independientemente de si ocurre o no una excepción.

### Respuesta

En Java, el bloque `finally` puede existir sin un bloque `catch`. Este bloque se ejecuta siempre, independientemente de si ocurre o no una excepción dentro del bloque `try`. Incluso si hay un `return` dentro del bloque `try`, el bloque `finally` se ejecutará antes de que el método termine.

Ejemplo:

```java
public class EjemploFinally {
    public static void main(String[] args) {
        System.out.println("Resultado: " + metodoConFinally());
    }

    public static int metodoConFinally() {
        try {
            System.out.println("Dentro del bloque try.");
            return 10; // Se ejecuta el return
        } finally {
            System.out.println("Dentro del bloque finally.");
        }
    }
}
```

Salida del programa:

```
Dentro del bloque try.
Dentro del bloque finally.
Resultado: 10
```

En este ejemplo, aunque el bloque `try` contiene un `return`, el bloque `finally` se ejecutará antes de que el valor sea devuelto. Esto garantiza que cualquier limpieza o acción necesaria se realice siempre.

Sin embargo, si el programa se termina abruptamente, como con un `System.exit(0)`, el bloque `finally` no se ejecutará.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

En Java, las excepciones se dividen en dos categorías principales: **controladas** y **no controladas**.

1. **Excepciones controladas:** Estas excepciones heredan de la clase `Exception` (pero no de `RuntimeException`). El compilador obliga a manejar estas excepciones mediante un bloque `try-catch` o declararlas en la firma del método con `throws`. Ejemplos comunes incluyen:
   - `IOException`: Error al trabajar con archivos.
   - `SQLException`: Error al interactuar con bases de datos.
   - `FileNotFoundException`: Archivo no encontrado.

2. **Excepciones no controladas:** Estas excepciones heredan de `RuntimeException`. No es obligatorio manejarlas ni declararlas en la firma del método. Ejemplos comunes incluyen:
   - `NullPointerException`: Intento de acceder a un objeto nulo.
   - `ArithmeticException`: División por cero.
   - `IndexOutOfBoundsException`: Índice fuera de rango.

**`RuntimeException` y su papel:** Esta clase es la base de las excepciones no controladas. Se utiliza para errores que generalmente son causados por fallos de lógica del programador y que podrían evitarse con validaciones adecuadas.

**Cuándo usar cada tipo:**
- **Excepciones controladas:**
  - Operaciones de entrada/salida (archivos, redes).
  - Conexiones a bases de datos.
  - Errores que dependen de factores externos al programa.
- **Excepciones no controladas:**
  - Validación de argumentos (por ejemplo, `IllegalArgumentException`).
  - Errores de lógica que deberían corregirse en el código.
  - Situaciones que no deberían ocurrir si el programa está bien diseñado.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

`throws` se usa para declarar que un método puede lanzar una o más excepciones. Es alternativa a capturar una excepción controlada porque permite que el método lance la excepción y que el código llamador la maneje.

### Respuesta

En Java, la palabra clave `throws` se utiliza en la firma de un método para indicar que este puede lanzar una o más excepciones. Esto permite que las excepciones se propaguen a los métodos llamadores en lugar de ser manejadas directamente dentro del método.

Por ejemplo:

```java
import java.io.*;

public class EjemploThrows {
    public void leerArchivo(String nombreArchivo) throws IOException {
        BufferedReader reader = new BufferedReader(new FileReader(nombreArchivo));
        String linea = reader.readLine();
        System.out.println(linea);
        reader.close();
    }

    public static void main(String[] args) {
        EjemploThrows ejemplo = new EjemploThrows();
        try {
            ejemplo.leerArchivo("archivo.txt");
        } catch (IOException e) {
            System.out.println("Error al leer el archivo: " + e.getMessage());
        }
    }
}
```

En este ejemplo, el método `leerArchivo` declara `throws IOException`, lo que significa que no maneja la excepción directamente, sino que permite que el método llamador decida cómo manejarla. Esto es útil cuando el método no tiene suficiente contexto para manejar el error de manera adecuada.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

A continuación, se muestra un ejemplo de un método en Java que utiliza `throws` para declarar que puede lanzar una excepción controlada, como `IOException`, al abrir un archivo. Además, se incluye un bloque `finally` para garantizar que los recursos se liberen correctamente:

```java
import java.io.*;

public class EjemploThrowsFinally {
    public void leerArchivo(String nombreArchivo) throws IOException {
        BufferedReader reader = null;
        try {
            reader = new BufferedReader(new FileReader(nombreArchivo));
            String linea = reader.readLine();
            System.out.println(linea);
        } finally {
            if (reader != null) {
                reader.close();
                System.out.println("Archivo cerrado correctamente.");
            }
        }
    }

    public static void main(String[] args) {
        EjemploThrowsFinally ejemplo = new EjemploThrowsFinally();
        try {
            ejemplo.leerArchivo("archivo.txt");
        } catch (IOException e) {
            System.out.println("Error al leer el archivo: " + e.getMessage());
        }
    }
}
```

En este ejemplo:
- El método `leerArchivo` declara `throws IOException`, indicando que no maneja la excepción directamente.
- El bloque `finally` asegura que el archivo se cierre correctamente, incluso si ocurre una excepción durante la lectura.
- El método `main` maneja la excepción utilizando un bloque `try-catch`.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Podemos poner en `throws` excepciones no controladas. El método llamador debería poner `try-catch` en ese caso para manejar la excepción.

### Respuesta

Sí, es posible incluir excepciones no controladas, como `RuntimeException`, en la cláusula `throws` de un método. Sin embargo, no es obligatorio hacerlo, ya que el compilador no exige que estas excepciones sean declaradas ni manejadas.

Por ejemplo:

```java
public class EjemploRuntimeException {
    public void metodoPeligroso() throws RuntimeException {
        throw new RuntimeException("Error inesperado");
    }

    public static void main(String[] args) {
        EjemploRuntimeException ejemplo = new EjemploRuntimeException();
        try {
            ejemplo.metodoPeligroso();
        } catch (RuntimeException e) {
            System.out.println("Se capturó una excepción no controlada: " + e.getMessage());
        }
    }
}
```

En este caso, aunque `RuntimeException` se declara en la cláusula `throws`, no es necesario manejarla explícitamente. Esto puede ser útil para documentar que el método puede lanzar este tipo de excepción.

**¿Debería el método llamador usar `try-catch`?**
Depende del contexto. Si el llamador puede manejar la excepción de manera significativa, es recomendable usar un bloque `try-catch`. De lo contrario, la excepción puede propagarse y ser manejada en un nivel superior o causar la terminación del programa.

**Sentido de declarar excepciones no controladas:**
Declarar excepciones no controladas en `throws` puede ser útil para documentar el comportamiento del método y advertir a los desarrolladores que lo utilicen sobre posibles errores que podrían ocurrir.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

Se recomienda usar excepciones controladas para errores de entrada/salida. Se usan excepciones no controladas para errores de lógica.

### Respuesta

**Cuándo usar excepciones controladas:**
Las excepciones controladas, como `IOException`, se utilizan en situaciones donde el error es predecible y el programa puede recuperarse de manera significativa. Ejemplos:

1. Operaciones de entrada/salida (archivos, redes).
2. Conexiones a bases de datos.
3. Validación de datos externos (como un archivo de configuración).

**Cuándo usar excepciones no controladas:**
Las excepciones no controladas, como `IllegalArgumentException`, se utilizan para errores que indican fallos de lógica en el programa y que deberían corregirse en el código. Ejemplos:

1. Argumentos inválidos en un método.
2. Acceso a índices fuera de rango en una lista.
3. Intento de dividir por cero.

**¿Existen ambas opciones en todos los lenguajes?**
No todos los lenguajes distinguen entre excepciones controladas y no controladas. Por ejemplo, en Python, todas las excepciones son similares a las no controladas de Java, ya que no es obligatorio declararlas ni manejarlas. En lenguajes como C++, las excepciones también son más flexibles y no requieren declaración explícita.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Sí, tiene sentido lanzar excepciones dentro de un bloque `catch` en ciertos casos. Esto se hace cuando el manejador actual no puede resolver el problema, pero puede agregar información adicional antes de propagar la excepción.

**Relanzar la misma excepción capturada:**
Es posible relanzar la misma excepción capturada utilizando la palabra clave `throw`. Esto es útil cuando se desea que el llamador maneje la excepción, pero sin perder el contexto original.

Ejemplo:

```java
public class EjemploRelanzar {
    public static void metodo() throws Exception {
        try {
            throw new Exception("Error original");
        } catch (Exception e) {
            System.out.println("Añadiendo información adicional.");
            throw e; // Relanzar la misma excepción
        }
    }

    public static void main(String[] args) {
        try {
            metodo();
        } catch (Exception e) {
            System.out.println("Excepción capturada en main: " + e.getMessage());
        }
    }
}
```

**Lanzar una nueva excepción dentro de `catch`:**
Esto se hace cuando se desea encapsular la excepción original en una nueva excepción personalizada.

Ejemplo:

```java
class MiExcepcion extends Exception {
    public MiExcepcion(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class EjemploNuevaExcepcion {
    public static void metodo() throws MiExcepcion {
        try {
            throw new Exception("Error original");
        } catch (Exception e) {
            throw new MiExcepcion("Error encapsulado", e);
        }
    }

    public static void main(String[] args) {
        try {
            metodo();
        } catch (MiExcepcion e) {
            System.out.println("Excepción personalizada: " + e.getMessage());
            e.getCause().printStackTrace(); // Mostrar la causa original
        }
    }
}
```

En este ejemplo, la excepción original se encapsula en una nueva excepción personalizada, proporcionando más contexto sobre el error.

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

En Java, una excepción puede ser la **causa** de otra excepción. Esto se utiliza para encapsular una excepción de bajo nivel dentro de una excepción de alto nivel, proporcionando más contexto sobre el error.

**Ejemplo:**

```java
class ExcepcionAltoNivel extends Exception {
    public ExcepcionAltoNivel(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class EjemploCausa {
    public static void metodoBajoNivel() throws Exception {
        throw new Exception("Error de bajo nivel");
    }

    public static void metodoAltoNivel() throws ExcepcionAltoNivel {
        try {
            metodoBajoNivel();
        } catch (Exception e) {
            throw new ExcepcionAltoNivel("Error de alto nivel", e);
        }
    }

    public static void main(String[] args) {
        try {
            metodoAltoNivel();
        } catch (ExcepcionAltoNivel e) {
            System.out.println("Excepción capturada: " + e.getMessage());
            System.out.println("Causa: " + e.getCause().getMessage());
            e.printStackTrace();
        }
    }
}
```

**Salida del programa:**

```
Excepción capturada: Error de alto nivel
Causa: Error de bajo nivel
java.lang.Exception: Error de alto nivel
	at EjemploCausa.metodoAltoNivel(EjemploCausa.java:12)
	...
Causa: java.lang.Exception: Error de bajo nivel
	at EjemploCausa.metodoBajoNivel(EjemploCausa.java:6)
	...
```

En este ejemplo:
- El método `metodoBajoNivel` lanza una excepción de bajo nivel.
- El método `metodoAltoNivel` captura esta excepción y la encapsula en una excepción personalizada de alto nivel (`ExcepcionAltoNivel`).
- La excepción de alto nivel incluye la excepción original como su causa, lo que permite rastrear el origen del error.

Cuando una excepción tiene una causa, esta se muestra en la traza de la pila, proporcionando información detallada sobre el flujo de errores.

