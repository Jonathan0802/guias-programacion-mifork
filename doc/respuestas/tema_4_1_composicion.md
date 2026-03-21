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

A la hora de modelar “composición” en C (relación *tiene-un*), se emplean `struct` anidados: una estructura contiene como campos a otras estructuras. En este caso, una **línea** puede representarse como “tiene-dos” **puntos**, y cada punto se define por sus dos coordenadas `x` e `y`. Esta composición permite reutilizar el tipo `Punto` en múltiples estructuras (línea, polígono, etc.) sin duplicar definiciones, algo muy similar a cómo en Java se encapsulan objetos como atributos de otros objetos, pero aquí con tipos compuestos de C.

Para calcular la **distancia entre dos puntos** se aplica la fórmula euclídea clásica, usando `sqrt` y `pow` (o `hypot` cuando se prefiera estabilidad numérica). La **longitud de una línea** se corresponde exactamente con la distancia entre sus dos puntos extremos. A continuación se muestra un ejemplo autocontenido en C que define las estructuras, implementa las funciones `distancia_puntos` y `longitud_linea`, e ilustra un uso básico en `main`.

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto a;  // extremo 1
    Punto b;  // extremo 2
} Linea;

double distancia_puntos(Punto p1, Punto p2) {
    // Se puede usar hypot(dx, dy) para mejor estabilidad numérica:
    // return hypot(p2.x - p1.x, p2.y - p1.y);
    double dx = p2.x - p1.x;
    double dy = p2.y - p1.y;
    return sqrt(dx*dx + dy*dy);
}

double longitud_linea(Linea l) {
    return distancia_puntos(l.a, l.b);
}

int main(void) {
    Punto p1 = {0.0, 0.0};
    Punto p2 = {3.0, 4.0};
    Linea l = {p1, p2};

    double d = distancia_puntos(p1, p2);
    double L = longitud_linea(l);

    printf("Distancia p1-p2 = %.2f\n", d);  // 5.00
    printf("Longitud linea  = %.2f\n", L);  // 5.00
    return 0;
}
```

Este patrón de composición con `struct` es directo de trasladar a otros problemas: por ejemplo, un **segmento** tiene dos puntos, un **rectángulo** tiene dos puntos opuestos, un **polígono** tiene “varios” puntos (usando un array dinámico y su tamaño), y una **polilínea** podría tener un array de segmentos o un array de puntos consecutivos. La idea central es mantener tipos pequeños y reutilizables (`Punto`) y construir tipos más complejos (`Linea`) agregándolos, de forma similar al principio de “composición sobre herencia” que más tarde se ve en POO.



## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

A partir del diseño en C, la **composición** en Java se expresa haciendo que la clase `Linea` contenga dos objetos `Punto`. Para mejorar frente a C mediante **ocultación de información** (encapsulación) e **inmutabilidad**, se definen campos `private final` y no se exponen *setters*. De este modo, una vez creado un `Punto`, sus coordenadas no cambian; y una vez creada una `Linea`, sus extremos quedan fijados. La inmutabilidad simplifica el razonamiento y evita estados incoherentes, lo que resulta especialmente útil al pasar referencias por métodos u operar en entornos concurrentes.

En la clase `Punto` se implementa un método de instancia `distanciaA(Punto otro)` que aplica la distancia euclídea. Como `Punto` es inmutable, puede devolverse sin copias defensivas y usarse con seguridad dentro de `Linea`. En la clase `Linea`, los extremos se modelan como dos campos `Punto` inmutables; su método `longitud()` delega en `distanciaA`. No se ofrecen modificadores posteriores; la única forma de definir una línea es en el constructor, lo cual respeta el requisito “no queremos que se modifique de qué a qué puntos va la línea”.

Se incluye a continuación un ejemplo completo. Nótese el uso de campos `private final`, la ausencia de *setters* y la validación de argumentos en constructores (para evitar `null`). Los *getters* exponen lecturas seguras, pues `Punto` es inmutable.

```java
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    /** Devuelve la coordenada X. */
    public double getX() { return x; }

    /** Devuelve la coordenada Y. */
    public double getY() { return y; }

    /** Distancia euclídea hasta otro punto. */
    public double distanciaA(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto de destino no puede ser null");
        }
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}
```

```java
public final class Linea {
    private final Punto a;  // extremo 1 (inmutable)
    private final Punto b;  // extremo 2 (inmutable)

    public Linea(Punto a, Punto b) {
        if (a == null || b == null) {
            throw new IllegalArgumentException("Los extremos de la línea no pueden ser null");
        }
        this.a = a;
        this.b = b;
    }

    /** Devuelve el extremo A. Seguro porque Punto es inmutable. */
    public Punto getA() { return a; }

    /** Devuelve el extremo B. Seguro porque Punto es inmutable. */
    public Punto getB() { return b; }

    /** Longitud de la línea: distancia entre sus extremos. */
    public double longitud() {
        return a.distanciaA(b);
    }

    @Override
    public String toString() {
        return "Linea[" + a + " -> " + b + "]";
    }
}
```

```java
public class Demo {
    public static void main(String[] args) {
        Punto p1 = new Punto(0.0, 0.0);
        Punto p2 = new Punto(3.0, 4.0);
        Linea l = new Linea(p1, p2);

        System.out.println("Distancia p1 a p2: " + p1.distanciaA(p2)); // 5.0
        System.out.println("Longitud de la línea: " + l.longitud());   // 5.0

        // No existen setters: p1, p2 y l son efectivamente inmutables.
    }
}
```

Como alternativa moderna, podría emplearse `record` para `Punto` (si se permitiese cambiar el formato), lo que haría la inmutabilidad aún más explícita. No obstante, al requerirse clases, la combinación de `private final` + ausencia de *setters* + validaciones en constructores ofrece ya una garantía sólida de inmutabilidad y una composición clara: **una línea tiene-dos puntos**.



## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La **multiplicidad** en composición indica cuántas instancias de una clase están vinculadas, de forma *fija y obligatoria*, a una instancia de otra clase dentro de una relación de composición. Como la composición implica propiedad fuerte (“*A tiene-un B*”), la multiplicidad determina cuántos objetos componentes forman parte del objeto compuesto, y además establece que dichos componentes no pueden existir de manera independiente una vez creada la relación. En diagramas UML suele expresarse mediante rangos como `1`, `0..1`, `1..*`, `2`, etc., colocados en los extremos de la asociación.

En el ejemplo de `Linea` y `Punto`, una `Linea` **necesita exactamente dos puntos** para existir. Esto significa que la multiplicidad desde `Linea` hacia `Punto` es **2**, o expresado en notación UML:  
**`Linea` → `Punto` : 2**.  
No se trata de “cero, uno o varios”, sino exactamente dos, porque sin ellos la línea no estaría definida. Además, dado el carácter inmutable del diseño, estos dos puntos permanecen constantes durante toda la vida del objeto.

En la dirección opuesta, un `Punto` **puede no pertenecer a ninguna línea**, o puede utilizarse en varias líneas distintas dentro del sistema, ya que su existencia no depende de la línea. Por tanto, la multiplicidad desde `Punto` hacia `Linea` es **0..**\* (cero o muchas). Expresado en notación UML:  
**`Punto` → `Linea` : 0..**\*.  
La composición se da únicamente desde `Linea` hacia sus dos puntos; el punto no “posee” a la línea ni depende de ella.

Por tanto, la relación completa de multiplicidades es:

*   **De `Linea` a `Punto`: 2**
*   **De `Punto` a `Linea`: 0..**\*

Esto refleja adecuadamente la estructura del ejemplo: una línea tiene dos puntos, mientras que un punto puede participar en varias líneas sin estar compuesto por ellas.



## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

La distinción entre **composición fuerte** y **composición débil** describe el grado de dependencia entre un objeto “contenedor” y los objetos que contiene. En una **composición fuerte**, el objeto compuesto es dueño exclusivo de sus componentes y controla completamente su ciclo de vida. Esto significa que cuando el objeto contenedor deja de existir, sus partes también desaparecen necesariamente. Los componentes no tienen sentido por sí mismos fuera del todo, ni pueden estar compartidos entre múltiples contenedores. Esta relación suele representarse técnicamente como **composición** en UML, identificada por un rombo negro.

En la **composición débil**, también llamada **agregación**, la relación indica que un objeto “tiene” o “utiliza” a otros, pero no es su propietario exclusivo. Los componentes pueden existir independientemente, pueden compartirse entre varios objetos agregadores y su ciclo de vida no está estrictamente ligado al del objeto contenedor. Si el contenedor desaparece, los objetos agregados pueden seguir existiendo sin problema. En UML se representa mediante un rombo blanco, reflejando ese vínculo más flexible.

En cuanto a la consecuencia directa sobre el **ciclo de vida**, la composición fuerte implica una dependencia total: destruir el todo implica destruir sus partes. En cambio, con composición débil o agregación, la vida de los componentes es independiente; se crean y destruyen según otras reglas del sistema, y no necesariamente ligadas al objeto que los usa o agrupa.

Por convención, cuando se habla de **“asociación o agregación”** se está aludiendo a la **composición débil**, mientras que cuando se menciona específicamente **“composición”**, se refiere a la **composición fuerte**. De este modo, el término “composición” a secas suele reservarse para relaciones de posesión estricta y dependencia de ciclo de vida.



## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

Cuando una clase **solo utiliza** a otra de forma puntual —ya sea recibiéndola como parámetro, devolviéndola desde un método, creándola temporalmente con `new` dentro de un método o empleándola como variable local— no se considera que exista composición, sino una relación de **dependencia**. La característica principal es que la clase no “posee” al objeto ni mantiene una relación estructural estable con él; simplemente lo usa en un contexto limitado, normalmente durante la ejecución de un método.

La dependencia indica por tanto una relación débil y momentánea, donde la clase necesita conocer a la otra para realizar alguna operación, pero no forma parte de su estado interno. Esto implica que la vida del objeto dependiente **no está ligada** al ciclo de vida del objeto que lo usa: puede existir antes, después o independientemente, y no queda almacenado como atributo privado. Por ejemplo, un método que recibe un `Punto` para calcular una distancia no establece composición; únicamente depende de ese tipo para realizar su tarea.

En cambio, la **composición** requiere que un objeto forme parte del estado interno de otro, típicamente como campos privados, y que su ciclo de vida esté fuertemente asociado al del objeto contenedor. Si se elimina el objeto contenedor, también deben considerarse eliminadas sus partes internas. Este vínculo no aparece en el simple uso de parámetros, variables locales o instancias pasajeras.

Por tanto, cuando una clase emplea a otra en métodos sin integrarla en su estructura permanente, se habla de **dependencia**, no de composición. Se trata de la forma más ligera de relación entre clases, adecuada para tareas puntuales o colaboraciones momentáneas, sin implicar propiedad ni responsabilidad sobre el ciclo de vida del objeto utilizado.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

Para ilustrar la diferencia entre **composición fuerte** y **composición débil (agregación)**, puede mantenerse el mismo `Punto` inmutable y cambiar únicamente la política de propiedad en `Linea`. En **composición fuerte**, la línea es **dueña exclusiva** de sus puntos: crea **copias defensivas** en el constructor y también en los *getters*, de modo que nadie externo comparte las referencias internas. Así, cuando la línea deja de existir, sus puntos internos también quedan inalcanzables y serán recolectados por el GC (su ciclo de vida está ligado al de `Linea`). En **composición débil**, la línea **solo referencia** puntos externos que pueden ser compartidos por otras líneas y vivir antes o después; al desaparecer la línea, esos puntos pueden seguir existiendo por tener otras referencias (ciclo de vida independiente).

> Se conserva `Punto` como tipo **inmutable**, tal como se viene trabajando (campos `private final`, sin *setters*). La diferencia crucial está en cómo `Linea` gestiona la propiedad de esos puntos.

### Composición fuerte (propiedad exclusiva; ciclo de vida ligado)

```java
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    public double getX() { return x; }
    public double getY() { return y; }

    public double distanciaA(Punto otro) {
        if (otro == null) throw new IllegalArgumentException("Punto destino null");
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx*dx + dy*dy);
    }

    @Override public String toString() { return "Punto(" + x + ", " + y + ")"; }
}
```

```java
/** Composición fuerte: LineaFuerte es dueña exclusiva de sus puntos. */
public final class LineaFuerte {
    private final Punto a; // copias internas (no compartidas)
    private final Punto b;

    public LineaFuerte(Punto a, Punto b) {
        if (a == null || b == null) {
            throw new IllegalArgumentException("Extremos null");
        }
        // Copias defensivas: nadie fuera comparte estas instancias internas.
        this.a = new Punto(a.getX(), a.getY());
        this.b = new Punto(b.getX(), b.getY());
    }

    // Getters devuelven copias para no exponer las referencias internas.
    public Punto getA() { return new Punto(a.getX(), a.getY()); }
    public Punto getB() { return new Punto(b.getX(), b.getY()); }

    public double longitud() { return a.distanciaA(b); }

    @Override public String toString() { return "LineaFuerte[" + a + " -> " + b + "]"; }
}
```

### Composición débil / Agregación (referencias compartibles; ciclo de vida independiente)

```java
/** Composición débil (agregación): LineaDebil solo referencia puntos externos. */
public final class LineaDebil {
    private final Punto a; // referencias compartibles
    private final Punto b;

    public LineaDebil(Punto a, Punto b) {
        if (a == null || b == null) {
            throw new IllegalArgumentException("Extremos null");
        }
        // Sin copias: se guardan las referencias proporcionadas (posible compartición).
        this.a = a;
        this.b = b;
    }

    // Se retornan las mismas referencias (Punto es inmutable, por lo que es seguro).
    public Punto getA() { return a; }
    public Punto getB() { return b; }

    public double longitud() { return a.distanciaA(b); }

    @Override public String toString() { return "LineaDebil[" + a + " -> " + b + "]"; }
}
```

En la **fuerte**, los puntos internos son **distintos objetos** (copiados) y **no se exponen**; por ello, si `LineaFuerte` queda inalcanzable, también lo quedan sus `Punto` internos. En la **débil**, los puntos pueden estar **compartidos** por varias líneas; si una `LineaDebil` desaparece, los `Punto` pueden seguir vivos referenciados desde otros sitios (por ejemplo, otras líneas o variables locales). Este contraste muestra cómo la **propiedad y el encapsulamiento** determinan el **ciclo de vida** de los objetos en una relación de composición frente a agregación.



## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En una **composición fuerte** en Java, el contenedor *no destruye explícitamente* los objetos que contiene porque en Java **no existe destrucción manual de objetos**. El programador no libera memoria ni destruye instancias; en su lugar, el trabajo lo realiza automáticamente el **Garbage Collector (GC)**. Por ello, aunque conceptualmente se afirma que “las partes mueren con el todo”, en la práctica la clase contenedora (como `Linea`) no necesita —ni puede— invocar ninguna operación de destrucción sobre sus `Punto`. Simplemente, cuando el objeto contenedor deja de ser alcanzable, también dejan de serlo sus componentes privados en una composición fuerte.

La razón técnica es que Java utiliza un modelo de **memoria administrada**, donde la vida útil de un objeto depende únicamente de si existe alguna referencia alcanzable hacia él desde un *root set* (como variables locales activas, campos estáticos, etc.). En una composición fuerte, los componentes (los `Punto`) suelen estar encapsulados como campos privados y no se exponen referencias externas; por ello, cuando la instancia de `Linea` ya no es accesible, tampoco lo son sus puntos internos. Al volverse inalcanzables, el GC está autorizado a reclamarlos en una de sus pasadas, eliminándolos de forma automática sin intervención del programador.

Este comportamiento implica que “destruir” un objeto en Java no significa ejecutar un acto explícito, sino simplemente **romper todas las referencias hacia él**. En una composición fuerte eso ocurre naturalmente cuando desaparece el contenedor, porque era el único poseedor de sus componentes. En cambio, en una composición débil o agregación, los objetos internos podrían seguir vivos si están referenciados por otras clases o variables externas.

Por tanto, en Java la afirmación “los componentes mueren con el contenedor” se interpreta en términos de **alcanzabilidad** y no de destrucción explícita. El contenedor no destruye nada directamente: simplemente deja de existir, y sus componentes quedan sin referencias externas, permitiendo que el Garbage Collector los elimine cuando corresponda.



## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

En una **composición débil (agregación)** el departamento *usa* profesores pero no es su dueño exclusivo: los objetos `Profesor` pueden existir fuera del `Departamento`, compartirse con otros contenedores y sobrevivirle. Para cumplir la invariante pedida, se impone que **siempre exista un director** y que **el director forme parte de la lista de profesores**. El diseño encapsula el detalle de almacenamiento (un `Profesor[]` de hasta 50 plazas) y expone solo operaciones de alto nivel: añadir al final, eliminar por posición, consultar el número de profesores y obtener un profesor por su posición; además, permite **cambiar el director** únicamente si el nuevo director **ya pertenece** al departamento. Si alguna operación violase la invariante (por ejemplo, eliminar al director, o cambiar el director a alguien que no está en la lista), se lanzan **excepciones**.

La implementación evita revelar el array interno: no se expone ningún getter del array, no se permite modificar posiciones arbitrarias, y se realizan **copias defensivas** únicamente a nivel de construcción de la estructura (copiando desde el array de entrada, si se proporciona). Para evitar estados confusos, se controla la **capacidad máxima (50)**, se prohíben `null` y se evitan **duplicados** de profesores según `equals`. Esta es la típica situación de agregación: el `Departamento` mantiene **referencias** a `Profesor` y no controla su ciclo de vida; si el departamento desapareciera, los profesores podrían seguir existiendo porque pueden estar referenciados en otros lugares.

### Código

```java
// Clase de dominio sencilla; puede compartirse entre departamentos (agregación).
public final class Profesor {
    private final String id;     // identificador único (DNI, código interno, etc.)
    private final String nombre;

    public Profesor(String id, String nombre) {
        if (id == null || id.isBlank()) {
            throw new IllegalArgumentException("id de profesor inválido");
        }
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("nombre de profesor inválido");
        }
        this.id = id;
        this.nombre = nombre;
    }

    public String getId()     { return id; }
    public String getNombre() { return nombre; }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Profesor)) return false;
        Profesor p = (Profesor) o;
        return id.equals(p.id); // igualdad por id
    }

    @Override
    public int hashCode() { return id.hashCode(); }

    @Override
    public String toString() {
        return "Profesor{id='" + id + "', nombre='" + nombre + "'}";
    }
}
```

```java
import java.util.Objects;

/**
 * Agregación (composición débil) entre Departamento y sus Profesores.
 * - Capacidad máxima: 50.
 * - El director es un Profesor del propio departamento y siempre debe existir.
 * - No se expone el array interno; se ofrecen operaciones controladas.
 */
public final class Departamento {
    private static final int CAPACIDAD_MAX = 50;

    private final Profesor[] profesores = new Profesor[CAPACIDAD_MAX];
    private int size = 0;

    private Profesor director;

    /**
     * Crea un departamento con un director obligatorio y, opcionalmente,
     * una lista inicial de profesores. El director se asegura como miembro.
     */
    public Departamento(Profesor directorInicial, Profesor[] listaInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El departamento debe tener director desde el inicio");
        }
        // Añadir primero al director para garantizar la invariante.
        addInterno(directorInicial); // no puede fallar por duplicado; la lista está vacía
        this.director = directorInicial;

        // Añadir el resto (si se suministra), evitando nulos y duplicados.
        if (listaInicial != null) {
            for (Profesor p : listaInicial) {
                if (p == null) {
                    throw new IllegalArgumentException("La lista inicial no puede contener null");
                }
                if (!contiene(p)) {
                    addInterno(p);
                }
            }
        }
    }

    /**
     * Sobrecarga práctica: solo con director desde el inicio.
     */
    public Departamento(Profesor directorInicial) {
        this(directorInicial, null);
    }

    /** Número de profesores actualmente en el departamento. */
    public int getNumeroProfesores() {
        return size;
    }

    /**
     * Obtiene el profesor por posición [0..size-1].
     * No se expone ninguna estructura de almacenamiento.
     */
    public Profesor getProfesor(int posicion) {
        validarPosicion(posicion);
        return profesores[posicion];
    }

    /** Devuelve el director actual. */
    public Profesor getDirector() {
        return director;
    }

    /**
     * Añade un profesor al final de la lista.
     * - No admite null.
     * - No admite duplicados (según equals).
     * - Respeta la capacidad máxima (50).
     */
    public void addProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("Profesor null");
        }
        if (contiene(p)) {
            throw new IllegalArgumentException("El profesor ya existe en el departamento: " + p);
        }
        addInterno(p);
    }

    /**
     * Elimina el profesor en la posición dada, desplazando el resto a la izquierda.
     * - No permite eliminar al director (invariante).
     */
    public void removeProfesorAt(int posicion) {
        validarPosicion(posicion);
        Profesor aEliminar = profesores[posicion];
        if (aEliminar.equals(director)) {
            throw new IllegalStateException("No se puede eliminar al director; cámbielo primero por otro profesor del departamento");
        }
        // Compactar el array
        for (int i = posicion; i < size - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[size - 1] = null;
        size--;
    }

    /**
     * Cambia el director por otro profesor que ya pertenezca al departamento.
     * - No admite null.
     * - Debe existir previamente en la lista (invariante).
     */
    public void cambiarDirector(Profesor nuevoDirector) {
        Objects.requireNonNull(nuevoDirector, "nuevoDirector null");
        if (!contiene(nuevoDirector)) {
            throw new IllegalArgumentException("El nuevo director debe pertenecer al departamento");
        }
        this.director = nuevoDirector;
    }

    // ----------------- auxiliares de encapsulación -----------------

    private void addInterno(Profesor p) {
        if (size >= CAPACIDAD_MAX) {
            throw new IllegalStateException("Capacidad máxima alcanzada (" + CAPACIDAD_MAX + ")");
        }
        profesores[size++] = p;
    }

    private boolean contiene(Profesor p) {
        for (int i = 0; i < size; i++) {
            if (profesores[i].equals(p)) return true;
        }
        return false;
    }

    private void validarPosicion(int pos) {
        if (pos < 0 || pos >= size) {
            throw new IndexOutOfBoundsException("Posición fuera de rango: " + pos);
        }
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder("Departamento{director=")
                .append(director).append(", profesores=[");
        for (int i = 0; i < size; i++) {
            sb.append(profesores[i]);
            if (i < size - 1) sb.append(", ");
        }
        sb.append("]}");
        return sb.toString();
    }
}
```

```java
public class Demo {
    public static void main(String[] args) {
        Profesor p1 = new Profesor("P-001", "Ada Lovelace");
        Profesor p2 = new Profesor("P-002", "Alan Turing");
        Profesor p3 = new Profesor("P-003", "Grace Hopper");

        // Siempre con director desde el inicio:
        Departamento d = new Departamento(p1, new Profesor[]{p2}); // p1 será director; p2 se añade

        System.out.println("Num profesores: " + d.getNumeroProfesores()); // 2
        System.out.println("Director: " + d.getDirector());               // p1
        System.out.println("Profesor[1]: " + d.getProfesor(1));           // p2

        // Añadir al final
        d.addProfesor(p3);
        System.out.println("Num profesores (tras añadir): " + d.getNumeroProfesores()); // 3

        // Cambiar director a alguien ya incluido
        d.cambiarDirector(p2);
        System.out.println("Director (tras cambio): " + d.getDirector()); // p2

        // Intento de eliminar al director -> excepción
        try {
            d.removeProfesorAt(1); // si p2 está en esa posición, fallará
        } catch (Exception e) {
            System.out.println("Esperado: " + e.getMessage());
        }

        // Eliminar a otro profesor (no director)
        d.removeProfesorAt(2); // elimina p3 si está en pos 2
        System.out.println("Num profesores (tras eliminar): " + d.getNumeroProfesores()); // 2
    }
}
```

Este diseño satisface los requisitos: **composición débil** (el departamento mantiene referencias a profesores), **invariante** preservada (el director siempre pertenece a la lista y no se puede eliminar; solo puede cambiarse por otro ya existente), **encapsulación** del almacenamiento (no se expone el array interno), y operaciones de **añadir al final**, **eliminar por posición**, **consultar tamaño** y **acceder por posición** dentro del rango.



## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

Al sustituir el array primitivo por `List<Profesor>`, se simplifica notablemente la gestión interna: no es necesario manejar una **capacidad fija**, ni **desplazar elementos** manualmente al eliminar, ni **llevar un contador** `size`. La validación de **rango** la realiza la propia `List` (lanzando `IndexOutOfBoundsException`), y operaciones como añadir al final o eliminar por posición quedan expresadas con `add(p)` y `remove(pos)`. Se mantiene, eso sí, la **invariante**: siempre debe existir director y el director **debe** pertenecer a la lista; además, se evita `null` y se siguen prohibiendo duplicados (según `equals`). En **agregación** (composición débil), se guardan **referencias** a `Profesor` sin “poseer” su ciclo de vida.

Si en lugar de `getProfesor(int pos)` se ofreciera un método que devolviera **todos** los profesores, **no** debería retornarse la lista interna directamente, ya que se rompería la **encapsulación** y podrían **violarse invariantes** (por ejemplo, eliminar al director con `list.remove(...)` desde fuera o inyectar `null`). La solución pasa por devolver una **copia defensiva** (`new ArrayList<>(profesores)`) o una **vista no modificable** (`Collections.unmodifiableList(profesores)`). La copia es más segura si se teme la exposición indirecta a futuras mutaciones; la vista no modificable es eficiente y suficiente si `Profesor` es inmutable y solo preocupa bloquear modificaciones externas.

```java
import java.util.*;

public final class Profesor {
    private final String id;
    private final String nombre;

    public Profesor(String id, String nombre) {
        if (id == null || id.isBlank()) throw new IllegalArgumentException("id inválido");
        if (nombre == null || nombre.isBlank()) throw new IllegalArgumentException("nombre inválido");
        this.id = id;
        this.nombre = nombre;
    }
    public String getId() { return id; }
    public String getNombre() { return nombre; }

    @Override public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Profesor)) return false;
        Profesor p = (Profesor) o;
        return id.equals(p.id);
    }
    @Override public int hashCode() { return id.hashCode(); }
    @Override public String toString() { return "Profesor{id='" + id + "', nombre='" + nombre + "'}"; }
}
```

```java
import java.util.*;

public final class Departamento {
    // Capacidad lógica: ya no es necesaria; si se desea, se puede mantener como regla de negocio.
    private static final int CAPACIDAD_MAX = 50;

    private final List<Profesor> profesores = new ArrayList<>(CAPACIDAD_MAX);
    private Profesor director;

    public Departamento(Profesor directorInicial, List<Profesor> listaInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe existir director desde el inicio");
        }
        // Añadir director primero
        addInterno(directorInicial);
        this.director = directorInicial;

        if (listaInicial != null) {
            for (Profesor p : listaInicial) {
                if (p == null) throw new IllegalArgumentException("La lista inicial no puede contener null");
                if (!profesores.contains(p)) addInterno(p);
            }
        }
    }

    public Departamento(Profesor directorInicial) {
        this(directorInicial, null);
    }

    /** Número de profesores. */
    public int getNumeroProfesores() {
        return profesores.size();
    }

    /** Acceso por posición (la List valida rango). */
    public Profesor getProfesor(int pos) {
        return profesores.get(pos);
    }

    /** Alternativa segura: obtener todos sin exponer la lista interna. */
    public List<Profesor> getProfesores() {
        // Opción A (vista no modificable):
        return Collections.unmodifiableList(profesores);
        // Opción B (copia defensiva):
        // return new ArrayList<>(profesores);
    }

    public Profesor getDirector() {
        return director;
    }

    /** Añade al final (sin null, sin duplicados, respetando capacidad lógica). */
    public void addProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("Profesor null");
        if (profesores.contains(p)) throw new IllegalArgumentException("Duplicado: " + p);
        if (profesores.size() >= CAPACIDAD_MAX) {
            throw new IllegalStateException("Capacidad máxima alcanzada (" + CAPACIDAD_MAX + ")");
        }
        profesores.add(p);
    }

    /** Elimina por posición; no permite eliminar al director. */
    public void removeProfesorAt(int pos) {
        Profesor aEliminar = profesores.get(pos); // valida rango
        if (aEliminar.equals(director)) {
            throw new IllegalStateException("No se puede eliminar al director; cámbielo primero");
        }
        profesores.remove(pos);
    }

    /** Cambia el director a otro profesor ya existente en el departamento. */
    public void cambiarDirector(Profesor nuevoDirector) {
        Objects.requireNonNull(nuevoDirector, "nuevoDirector null");
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El nuevo director debe pertenecer al departamento");
        }
        this.director = nuevoDirector;
    }

    // ---------------- auxiliares ----------------
    private void addInterno(Profesor p) {
        if (profesores.size() >= CAPACIDAD_MAX) {
            throw new IllegalStateException("Capacidad máxima alcanzada (" + CAPACIDAD_MAX + ")");
        }
        profesores.add(p);
    }

    @Override public String toString() {
        return "Departamento{director=" + director + ", profesores=" + profesores + "}";
    }
}
```

```java
public class Demo {
    public static void main(String[] args) {
        Profesor p1 = new Profesor("P-001", "Ada Lovelace");
        Profesor p2 = new Profesor("P-002", "Alan Turing");
        Profesor p3 = new Profesor("P-003", "Grace Hopper");

        Departamento d = new Departamento(p1, List.of(p2)); // director obligatorio + lista inicial

        d.addProfesor(p3);
        System.out.println("Total: " + d.getNumeroProfesores()); // 3
        System.out.println("Director: " + d.getDirector());      // p1

        d.cambiarDirector(p2);
        System.out.println("Director tras cambio: " + d.getDirector()); // p2

        // Eliminación segura (no director)
        d.removeProfesorAt(2); // elimina p3 si está en pos 2

        // Iteración segura sin exponer estructura
        for (Profesor p : d.getProfesores()) {
            System.out.println(p);
        }
    }
}
```

**¿Qué se ha ahorrado?** La contabilidad manual de `size`, el control de desplazamiento al eliminar, la gestión de índices internos y gran parte de la validación de rango. Se mantiene la lógica de negocio (invariante del director, no nulos, no duplicados, límite máximo opcional). **¿Cómo devolver todos los profesores?** Nunca retornando la lista interna; usar **copia defensiva** o **vista no modificable** para preservar la encapsulación y evitar que código externo viole la invariante del departamento.



## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

Las **composiciones recursivas** aparecen cuando una clase contiene una referencia a otra instancia **del mismo tipo**. En Java, se puede modelar una `Persona` inmutable cuya **madre** es otra `Persona`. La inmutabilidad se garantiza con campos `private final` y sin *setters*; así, una vez creada la persona, ni su nombre ni su madre cambian. Para el ancestro raíz (por ejemplo, la abuela sin madre registrada) se permite `null` en el campo `madre`, lo que cierra la cadena recursiva de forma natural.

```java
public final class Persona {
    private final String nombre;
    private final Persona madre; // puede ser null para el ancestro raíz

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("nombre inválido");
        }
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() { return nombre; }
    public Persona getMadre() { return madre; }

    @Override
    public String toString() {
        return "Persona{nombre='" + nombre + "', madre=" + (madre != null ? madre.nombre : "null") + "}";
    }
}
```

A continuación, se muestra un `main` que crea una pequeña **línea materna**: abuela → madre → nieto, y recorre la cadena desde el nieto hasta la abuela. Obsérvese que el recorrido puede hacerse de forma iterativa hasta encontrar `null`. Se evita cualquier “destrucción” manual porque, como ya se ha visto, el ciclo de vida en Java está gestionado por el *garbage collector* y la inmutabilidad simplifica el razonamiento sobre el estado.

```java
public class Demo {
    public static void main(String[] args) {
        Persona abuela = new Persona("Carmen", null);
        Persona madre  = new Persona("Laura",  abuela);
        Persona nieto  = new Persona("Diego",  madre);

        System.out.println("Nieto: " + nieto.getNombre());
        System.out.println("Madre del nieto: " + nieto.getMadre().getNombre());
        System.out.println("Abuela del nieto: " + nieto.getMadre().getMadre().getNombre());

        imprimirLineaMaterna(nieto); // Diego <- Laura <- Carmen
    }

    private static void imprimirLineaMaterna(Persona p) {
        System.out.print("Línea materna: ");
        Persona actual = p;
        boolean primero = true;
        while (actual != null) {
            if (!primero) System.out.print(" <- ");
            System.out.print(actual.getNombre());
            primero = false;
            actual = actual.getMadre();
        }
        System.out.println();
    }
}
```

Entre **ejemplos clásicos** de composiciones recursivas se encuentran: (1) **listas enlazadas** (`Nodo` con un `Nodo next`), (2) **árboles** (p. ej., `Nodo` con `Nodo left/right`), (3) el **patrón Composite** en jerarquías como **directorios/archivos** o **nodos DOM** (un `Elemento` con `List<Elemento> hijos`), (4) **árboles sintácticos (AST)** donde cada nodo contiene subnodos, y (5) **organigramas** con `Empleado` que referencia a su `manager` (otro `Empleado`). En todos los casos, la relación se construye enlazando objetos del mismo tipo, permitiendo estructuras jerárquicas o lineales que se navegan recursiva o iterativamente.


## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Las **relaciones de composición bidireccionales** son aquellas en las que ambos extremos mantienen una referencia el uno al otro: el contenedor conoce a sus componentes y, simultáneamente, cada componente conoce a su contenedor. En términos de modelado, esto crea una **invariante de consistencia** que obliga a mantener sincronizadas ambas direcciones en cada operación (crear, añadir, quitar, mover). Aunque en UML puede representarse como agregación/composición con navegabilidad en ambos sentidos, en código el reto principal no es el diagrama, sino **garantizar las actualizaciones atómicas** para que no haya estados intermedios incoherentes (por ejemplo, un `Profesor` que afirma pertenecer a un `Departamento` que no lo incluye en su lista).

Para implementarlo en el ejemplo de **`Profesor` y `Departamento`**, se introduciría en `Profesor` un campo (normalmente `private`) que referencie a su `Departamento` actual (o `null` si no pertenece a ninguno) y se **centralizarían las operaciones de enlace/desenlace** en la clase `Departamento` (no en `Profesor`). De este modo, `Departamento.addProfesor(p)` añade a `p` en la lista **y** establece `p.departamento = this`; `removeProfesorAt(i)` lo quita **y** deja `p.departamento = null`; `cambiarDirector(...)` sigue verificando que el nuevo director **ya** pertenece al departamento. No se expondrían *setters* públicos en `Profesor` para su `Departamento`; a lo sumo, un *getter* de solo lectura. Así se evita que código externo “rompa” la bidireccionalidad. En cuanto al **ciclo de vida**, al ser una **agregación (composición débil)**, el `Departamento` no “destruye” profesores; si desaparece, los `Profesor` pueden seguir existiendo si hay otras referencias. La bidireccionalidad **no impide** la recolección de basura en Java (el GC maneja ciclos), pero sí exige cuidar la **consistencia** y evitar exponer colecciones modificables.

Un esbozo (con `List`) ilustra la mecánica. Nótese: (1) `Profesor` tiene un `departamento` con *setter* **package-private** (o `private` + método amigo) para que **solo** `Departamento` pueda modificarlo; (2) toda mutación pasa por `Departamento`; (3) se preservan las invariantes del director.

```java
// Archivo Profesor.java
public final class Profesor {
    private final String id;
    private final String nombre;
    private Departamento departamento; // bidireccional: referencia inversa (agregación)

    public Profesor(String id, String nombre) {
        if (id == null || id.isBlank()) throw new IllegalArgumentException("id inválido");
        if (nombre == null || nombre.isBlank()) throw new IllegalArgumentException("nombre inválido");
        this.id = id;
        this.nombre = nombre;
    }

    public String getId() { return id; }
    public String getNombre() { return nombre; }
    public Departamento getDepartamento() { return departamento; }

    // Setter NO público: restringir a paquete o a Departamento (mismo paquete).
    void setDepartamento(Departamento dpto) {
        this.departamento = dpto;
    }

    @Override public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Profesor)) return false;
        return id.equals(((Profesor) o).id);
    }
    @Override public int hashCode() { return id.hashCode(); }
}
```

```java
// Archivo Departamento.java
import java.util.*;

public final class Departamento {
    private static final int CAPACIDAD_MAX = 50;

    private final List<Profesor> profesores = new ArrayList<>(CAPACIDAD_MAX);
    private Profesor director;

    public Departamento(Profesor directorInicial, List<Profesor> inicial) {
        if (directorInicial == null) throw new IllegalArgumentException("Director obligatorio");
        addInterno(directorInicial);        // enlaza ambos lados
        this.director = directorInicial;

        if (inicial != null) {
            for (Profesor p : inicial) {
                if (p == null) throw new IllegalArgumentException("Profesor null en lista inicial");
                if (!profesores.contains(p)) addInterno(p);
            }
        }
    }

    public Departamento(Profesor directorInicial) { this(directorInicial, null); }

    public int getNumeroProfesores() { return profesores.size(); }
    public Profesor getProfesor(int pos) { return profesores.get(pos); }
    public List<Profesor> getProfesores() { return Collections.unmodifiableList(profesores); }
    public Profesor getDirector() { return director; }

    public void addProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("Profesor null");
        if (profesores.size() >= CAPACIDAD_MAX) throw new IllegalStateException("Capacidad máxima");
        if (profesores.contains(p)) throw new IllegalArgumentException("Duplicado");

        // Si p ya pertenece a otro departamento, o se rechaza o se “mueve”.
        Departamento actual = p.getDepartamento();
        if (actual != null && actual != this) {
            // Política de negocio: o lanzar excepción, o moverlo limpiamente.
            // Aquí: mover (primero desenlazar del anterior).
            actual.removeProfesor(p); // desenlaza ambos lados en el otro departamento
        }

        profesores.add(p);
        p.setDepartamento(this); // enlaza la inversa
    }

    public void removeProfesorAt(int pos) {
        Profesor aEliminar = profesores.get(pos); // valida rango
        if (aEliminar.equals(director)) {
            throw new IllegalStateException("No se puede eliminar al director; cámbielo primero");
        }
        profesores.remove(pos);
        aEliminar.setDepartamento(null); // desenlaza inversa
    }

    // Variante de eliminación por identidad (útil para movimientos entre departamentos)
    void removeProfesor(Profesor p) {
        int idx = profesores.indexOf(p);
        if (idx >= 0) {
            if (p.equals(director)) throw new IllegalStateException("No se puede eliminar al director");
            profesores.remove(idx);
            p.setDepartamento(null);
        }
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        Objects.requireNonNull(nuevoDirector, "nuevoDirector null");
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe pertenecer al departamento");
        }
        this.director = nuevoDirector;
    }

    private void addInterno(Profesor p) {
        if (profesores.size() >= CAPACIDAD_MAX) throw new IllegalStateException("Capacidad máxima");
        profesores.add(p);
        p.setDepartamento(this);
    }
}
```

Con este patrón se obtiene una relación **bidireccional coherente**: cualquier alta/baja en `Departamento` actualiza la referencia inversa en `Profesor`, y cualquier “traslado” entre departamentos desenlaza primero de uno y enlaza en el otro, manteniendo la invariante del **director**. Se conserva la **agregación** (composición débil): `Profesor` puede existir fuera o cambiar de `Departamento`, y no se expone la lista interna ni *setters* peligrosos que permitirían romper la consistencia de la relación.

