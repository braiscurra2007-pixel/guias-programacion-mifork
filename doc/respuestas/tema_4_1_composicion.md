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

En C, la composición de estructuras permite construir tipos de datos complejos a partir de otros más simples. Por ejemplo, se puede definir un `struct Punto` con dos coordenadas y un `struct Linea` que contiene dos puntos, representando así una línea en el plano. Esta técnica es fundamental para modelar relaciones "tiene-un" en lenguajes sin orientación a objetos.

Un ejemplo sería:

```c
typedef struct {
	double x;
	double y;
} Punto;

typedef struct {
	Punto inicio;
	Punto fin;
} Linea;

double distancia(Punto a, Punto b) {
	double dx = a.x - b.x;
	double dy = a.y - b.y;
	return sqrt(dx * dx + dy * dy);
}

double longitudLinea(Linea l) {
	return distancia(l.inicio, l.fin);
}
```

Este enfoque permite reutilizar estructuras y funciones para manipular datos geométricos de manera modular, aunque no se dispone de encapsulamiento ni de métodos asociados a los datos como en la POO.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

En orientación a objetos con Java, la composición se implementa creando clases que contienen referencias a otras clases. Se puede definir una clase `Punto` con atributos privados e inmutables para las coordenadas, y una clase `Linea` que contiene dos puntos, también inmutables. Esto garantiza que, una vez creados, ni los puntos ni la línea puedan modificarse, mejorando la seguridad y robustez del código respecto a C.

Un ejemplo sería:

```java
public final class Punto {
	private final double x;
	private final double y;

	public Punto(double x, double y) {
		this.x = x;
		this.y = y;
	}

	public double distanciaA(Punto otro) {
		double dx = this.x - otro.x;
		double dy = this.y - otro.y;
		return Math.sqrt(dx * dx + dy * dy);
	}
}

public final class Linea {
	private final Punto inicio;
	private final Punto fin;

	public Linea(Punto inicio, Punto fin) {
		this.inicio = inicio;
		this.fin = fin;
	}

	public double longitud() {
		return inicio.distanciaA(fin);
	}
}
```

Gracias a la encapsulación y la inmutabilidad, se evita la modificación accidental de los datos, lo que representa una mejora significativa respecto a la aproximación en C.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad en la composición describe cuántas instancias de una clase pueden estar asociadas a otra. Es una propiedad fundamental en el modelado de relaciones entre objetos, permitiendo especificar si una entidad contiene una, varias o ninguna instancia de otra entidad.

En el ejemplo de `Linea` y `Punto`, la multiplicidad de `Linea` a `Punto` es 2, ya que cada línea está compuesta exactamente por dos puntos. En sentido inverso, de `Punto` a `Linea`, la multiplicidad es 0..n, porque un punto puede no pertenecer a ninguna línea, o puede estar presente en varias líneas distintas.

Esta notación ayuda a clarificar las restricciones y relaciones en el diseño de sistemas orientados a objetos, facilitando la comprensión y el mantenimiento del código.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

La composición fuerte implica que el ciclo de vida de los objetos contenidos está completamente ligado al del objeto contenedor. Si el contenedor se destruye, también lo hacen los objetos que contiene. En cambio, la composición débil, también llamada agregación o asociación, permite que los objetos contenidos tengan un ciclo de vida independiente del contenedor.

En la composición fuerte, la relación es de "todo-parte" inseparable, mientras que en la débil, los objetos pueden existir por sí mismos y ser compartidos entre varios contenedores. Por ello, la agregación se asocia a la composición débil y la composición propiamente dicha a la fuerte.

Esta distinción es importante para gestionar correctamente la memoria y las dependencias entre objetos, especialmente en lenguajes con gestión manual de memoria como C++.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

Cuando una clase utiliza otra solo como parámetro, valor de retorno, variable local o mediante la creación temporal de objetos dentro de un método, se habla de dependencia y no de composición. La dependencia indica que una clase necesita a otra para realizar alguna operación, pero no mantiene una relación estructural permanente.

La composición, en cambio, implica que una clase contiene referencias a instancias de otra clase como parte de su estado interno, estableciendo una relación más fuerte y duradera. La distinción es relevante para entender el acoplamiento entre clases y la organización del código.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

Para modelar la relación entre `Linea` y `Punto` como composición fuerte en Java, los puntos pueden ser creados dentro de la propia clase `Linea`, de modo que solo existen como parte de la línea. En la composición débil, los puntos se crean fuera y se pasan a la línea, permitiendo que existan independientemente y puedan ser compartidos.

Ejemplo de composición fuerte:

```java
public class Linea {
	private final Punto inicio;
	private final Punto fin;

	public Linea(double x1, double y1, double x2, double y2) {
		this.inicio = new Punto(x1, y1);
		this.fin = new Punto(x2, y2);
	}
	// ...
}
```

Ejemplo de composición débil:

```java
public class Linea {
	private final Punto inicio;
	private final Punto fin;

	public Linea(Punto inicio, Punto fin) {
		this.inicio = inicio;
		this.fin = fin;
	}
	// ...
}
```

En la composición fuerte, los puntos no existen fuera de la línea; en la débil, pueden ser compartidos o utilizados en otros contextos.


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En Java, la destrucción de objetos no se realiza de forma explícita como en C o C++. En la composición fuerte, cuando el objeto contenedor (por ejemplo, `Linea`) deja de ser referenciado y es recolectado por el recolector de basura, también lo serán los objetos que solo están referenciados por él (como los `Punto`).

No se observa una destrucción explícita porque Java gestiona la memoria automáticamente mediante la recolección de basura. Cuando no existen referencias a un objeto, este es eliminado de la memoria sin intervención directa del programador, simplificando la gestión del ciclo de vida de los objetos.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

En una composición débil, el departamento mantiene una lista de profesores y una referencia a su director, que debe ser uno de los profesores. Se emplea un array privado para almacenar hasta 50 profesores, pero se oculta la implementación para mantener la encapsulación. Se proporcionan métodos para añadir y eliminar profesores, obtener el número de profesores y acceder a un profesor por posición. El director solo puede ser cambiado por otro profesor del departamento, y nunca puede quedar el departamento sin director.

Si se intenta eliminar al director o dejar el departamento sin director, se lanza una excepción para mantener la invariante de clase. Este diseño asegura que la relación entre el departamento y sus profesores es de composición débil, ya que los profesores pueden existir fuera del departamento.

Ejemplo de implementación:

```java
public class Profesor {
	private final String nombre;
	public Profesor(String nombre) { this.nombre = nombre; }
	public String getNombre() { return nombre; }
}

public class Departamento {
	private final Profesor[] profesores = new Profesor[50];
	private int numProfesores = 0;
	private Profesor director;

	public Departamento(Profesor director) {
		anadirProfesor(director);
		this.director = director;
	}

	public void anadirProfesor(Profesor p) {
		if (numProfesores >= 50) throw new IllegalStateException("Máximo alcanzado");
		profesores[numProfesores++] = p;
	}

	public void eliminarProfesor(int pos) {
		if (pos < 0 || pos >= numProfesores) throw new IndexOutOfBoundsException();
		if (profesores[pos] == director) throw new IllegalStateException("No se puede eliminar al director");
		for (int i = pos; i < numProfesores - 1; i++) {
			profesores[i] = profesores[i + 1];
		}
		profesores[--numProfesores] = null;
	}

	public int getNumProfesores() { return numProfesores; }
	public Profesor getProfesor(int pos) {
		if (pos < 0 || pos >= numProfesores) throw new IndexOutOfBoundsException();
		return profesores[pos];
	}

	public Profesor getDirector() { return director; }
	public void cambiarDirector(Profesor nuevo) {
		boolean encontrado = false;
		for (int i = 0; i < numProfesores; i++) {
			if (profesores[i] == nuevo) { encontrado = true; break; }
		}
		if (!encontrado) throw new IllegalArgumentException("El director debe ser profesor del departamento");
		director = nuevo;
	}
}
```


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

Al emplear `List` en vez de arrays primitivos, se simplifica la gestión de la colección de profesores, eliminando la necesidad de gestionar manualmente el tamaño y los desplazamientos al añadir o eliminar elementos. `List` proporciona métodos como `add`, `remove` y `size`, facilitando la implementación y reduciendo la posibilidad de errores.

Ejemplo de implementación:

```java
import java.util.*;

public class Departamento {
	private final List<Profesor> profesores = new ArrayList<>();
	private Profesor director;

	public Departamento(Profesor director) {
		anadirProfesor(director);
		this.director = director;
	}

	public void anadirProfesor(Profesor p) { profesores.add(p); }
	public void eliminarProfesor(int pos) {
		Profesor p = profesores.get(pos);
		if (p == director) throw new IllegalStateException("No se puede eliminar al director");
		profesores.remove(pos);
	}
	public int getNumProfesores() { return profesores.size(); }
	public Profesor getProfesor(int pos) { return profesores.get(pos); }
	public Profesor getDirector() { return director; }
	public void cambiarDirector(Profesor nuevo) {
		if (!profesores.contains(nuevo)) throw new IllegalArgumentException("El director debe ser profesor del departamento");
		director = nuevo;
	}
}
```

Si se devolviera directamente la lista interna con un método como `getProfesores()`, se perdería el control sobre la encapsulación, ya que el usuario podría modificar la lista desde fuera. Para evitarlo, se puede devolver una copia de la lista o una vista inmodificable usando `Collections.unmodifiableList(profesores)`.


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

La composición recursiva se da cuando una clase contiene una referencia a otra instancia de la misma clase, permitiendo modelar relaciones jerárquicas o de árbol. Un ejemplo clásico es una persona que tiene como atributo a su madre, que también es una persona. Para garantizar la inmutabilidad, los atributos se declaran como finales y no existen métodos para modificarlos tras la creación.

Ejemplo en Java:

```java
public final class Persona {
	private final String nombre;
	private final Persona madre;

	public Persona(String nombre, Persona madre) {
		this.nombre = nombre;
		this.madre = madre;
	}

	public String getNombre() { return nombre; }
	public Persona getMadre() { return madre; }
}

public class Main {
	public static void main(String[] args) {
		Persona abuela = new Persona("Ana", null);
		Persona madre = new Persona("Beatriz", abuela);
		Persona hijo = new Persona("Carlos", madre);
		System.out.println(hijo.getNombre() + ", madre: " + hijo.getMadre().getNombre() + ", abuela: " + hijo.getMadre().getMadre().getNombre());
	}
}
```

Otros ejemplos de composiciones recursivas incluyen árboles binarios, listas enlazadas y estructuras de carpetas en sistemas de archivos.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Las relaciones de composición bidireccionales se producen cuando dos clases mantienen referencias mutuas, es decir, cada objeto conoce al otro. En el caso de `Profesor` y `Departamento`, esto implicaría que cada profesor tendría una referencia a su departamento y el departamento mantendría la lista de profesores.

Para implementarlo, la clase `Profesor` debería tener un atributo para el departamento y métodos para establecerlo, mientras que `Departamento` seguiría gestionando la lista de profesores. Es importante evitar inconsistencias, asegurando que al añadir o eliminar un profesor, ambas referencias se actualicen correctamente para mantener la integridad de la relación.
