<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

Un ejemplo clásico de estructura de datos no genérica que permite almacenar cualquier tipo de dato consiste en usar, en Java, un array de tipo `Object`. Dado que todas las clases heredan implícitamente de `Object`, un array de este tipo puede contener referencias a objetos de clases distintas en una misma estructura. Esta técnica se utilizó ampliamente antes de la introducción de la genericidad en Java.

Este enfoque permite construir, por ejemplo, una lista dinámica sencilla sobre un array primitivo de referencias (`Object[]`). La estructura no conoce el tipo real de los elementos que almacena, limitándose a tratarlos como `Object`. La responsabilidad de recordar el tipo concreto de cada elemento y realizar las conversiones adecuadas recae completamente en quien use la estructura.

```java
public class ListaSimple {
    private Object[] datos;
    private int size;

    public ListaSimple(int capacidad) {
        datos = new Object[capacidad];
        size = 0;
    }

    public void add(Object elemento) {
        datos[size++] = elemento;
    }

    public Object get(int indice) {
        return datos[indice];
    }
}
```

El principal inconveniente de esta solución es la pérdida de seguridad de tipos, ya que al recuperar un elemento es necesario realizar un *casting* explícito. Si el tipo esperado no coincide con el tipo real del objeto almacenado, el error solo se detectará en tiempo de ejecución mediante una excepción `ClassCastException`. Este problema es uno de los motivos fundamentales por los que surge la genericidad, que traslada el control de tipos al compilador y elimina estos riesgos en el uso de estructuras de datos reutilizables.


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta

La **programación genérica** es un paradigma que permite definir algoritmos y estructuras de datos de forma independiente del tipo concreto de datos con el que operan. El objetivo principal es escribir código reutilizable y seguro, de manera que el mismo componente pueda trabajar con distintos tipos sin duplicar su implementación. En lenguajes como Java, esta idea se materializa mediante los *genéricos*, que permiten parametrizar clases y métodos con tipos que se concretan en el momento de su uso.

Desde el punto de vista conceptual, la programación genérica busca separar **el qué se hace** (la lógica de la estructura o del algoritmo) de **con qué tipo se hace**. A diferencia de soluciones anteriores, el tipo concreto se mantiene y se comprueba en tiempo de compilación, lo que evita conversiones explícitas y reduce errores. Esto supone una evolución clara respecto a técnicas más antiguas basadas en tipos generales.

El ejemplo anterior, basado en `Object` en Java (o en `void*` en C), **no se considera programación genérica propiamente dicha**, sino una aproximación previa a ella. Aunque permite almacenar cualquier tipo de dato, la estructura no es consciente del tipo real de los elementos y no existe verificación de tipos en tiempo de compilación. La genericidad es solo aparente y se apoya en conversiones forzadas.

Por tanto, ese enfoque puede calificarse como una forma de *polimorfismo débil o manual*, pero no como programación genérica real. La auténtica programación genérica aparece cuando el lenguaje proporciona mecanismos explícitos para parametrizar tipos y garantiza la corrección de los mismos antes de ejecutar el programa, como ocurre con los genéricos introducidos en Java a partir de la versión 5.


## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta

El principal problema al emplear `void*` en C o `Object` en Java para crear estructuras de datos supuestamente genéricas es la **ausencia de chequeo de tipos en tiempo de compilación**. La estructura no conserva información sobre el tipo real de los elementos almacenados, por lo que el compilador no puede verificar si los usos posteriores de esos datos son correctos. Como consecuencia, muchos errores potenciales no se detectan hasta el momento de la ejecución.

Al recuperar un elemento de la estructura, es necesario realizar una conversión explícita al tipo esperado mediante *casting*. Si el tipo asumido por el programador no coincide con el tipo real del objeto almacenado, el error no se manifiesta hasta que el programa se ejecuta, dando lugar a fallos como `ClassCastException` en Java o comportamientos indefinidos en C. Esto dificulta la depuración y reduce la fiabilidad del software.

Otro problema importante es que el uso de tipos generales debilita la expresividad del código. El uso constante de conversiones de tipo añade ruido innecesario y hace que el código sea más difícil de leer y mantener. Además, la documentación implícita que aporta el tipo de una variable desaparece, obligando a conocer el funcionamiento interno de la estructura para usarla correctamente.

Por último, este enfoque facilita errores lógicos difíciles de detectar, como insertar elementos de tipos inconsistentes en la misma estructura sin intención. Al no existir restricciones de tipo, se pierde la protección que ofrece el sistema de tipos del lenguaje. Precisamente para resolver estos problemas surgen los mecanismos de programación genérica, que permiten mantener flexibilidad sin renunciar a un chequeo de tipos estricto en tiempo de compilación.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta

Los **parámetros de tipo** son un mecanismo de la programación genérica que permite definir clases, interfaces o métodos utilizando tipos “variables”, que se especifican más adelante cuando el componente se utiliza. En lugar de trabajar con un tipo general como `Object`, se introduce un identificador de tipo (por ejemplo `T`) que representa un tipo concreto aún desconocido en el momento de escribir la definición. De esta forma, el mismo código puede reutilizarse con distintos tipos manteniendo la información de tipo.

Este mecanismo permite que el compilador conozca y controle el tipo real de los datos con los que se trabaja, aun cuando la implementación sea genérica. Al instanciar la clase o invocar el método, el parámetro de tipo se sustituye por un tipo concreto (por ejemplo `Integer`, `String`, etc.), y todas las operaciones quedan chequeadas estáticamente. Esto supone una mejora clara frente al uso de `Object`, donde esa información se pierde.

En Java, los parámetros de tipo se declaran entre los símbolos `< >` y se utilizan como si fueran tipos normales dentro del código. Por ejemplo, una lista genérica puede declararse con un parámetro `T` que representa el tipo de los elementos almacenados, garantizando que solo se insertan y se recuperan valores de ese tipo, sin necesidad de conversiones explícitas.

```java
public class Lista<T> {
    private T[] datos;

    public T get(int i) {
        return datos[i];
    }

    public void add(T elemento) {
        datos[i] = elemento;
    }
}
```

En resumen, los parámetros de tipo permiten expresar la genericidad de forma explícita y segura, desplazando la comprobación de tipos al compilador. Esto aumenta la robustez del código, mejora su legibilidad y elimina una gran parte de los errores asociados a las aproximaciones tradicionales basadas en `void*` o `Object`.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

En Java, la programación genérica se implementa mediante **generics**, que permiten parametrizar clases y colecciones con un tipo concreto. Un ejemplo típico es el uso de `ArrayList<String>`, que indica explícitamente que la lista solo admitirá elementos de tipo `String`. Gracias a esto, el compilador impide insertar objetos de otro tipo y garantiza que, al recorrer la lista, cada elemento obtenido es un `String` sin necesidad de conversiones explícitas.

```java
import java.util.ArrayList;

public class EjemploJava {
    public static void main(String[] args) {
        ArrayList<String> lista = new ArrayList<>();

        lista.add("Hola");
        lista.add("Programación");
        lista.add("Genérica");

        for (String s : lista) {
            System.out.println(s.toUpperCase());
        }
    }
}
```

En este ejemplo, el recorrido de la lista se realiza con total seguridad de tipos: cada elemento es tratado directamente como `String`. Cualquier intento de insertar, por ejemplo, un `Integer` produciría un error de compilación. Esto demuestra cómo los genéricos en Java trasladan el control de tipos al compilador, eliminando errores que antes solo se detectaban en tiempo de ejecución.

En C++, la programación genérica se lleva a cabo mediante **templates**, que permiten definir clases y funciones parametrizadas por tipos. La biblioteca estándar proporciona contenedores genéricos como `std::vector`, que pueden instanciarse con un tipo concreto, en este caso `std::string`. Al igual que en Java, el tipo se fija en tiempo de compilación y se mantiene durante todo el uso del contenedor.

```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> v;

    v.push_back("Hola");
    v.push_back("Programación");
    v.push_back("Genérica");

    for (const std::string& s : v) {
        std::cout << s << std::endl;
    }
}
```

Aquí, `std::vector<std::string>` solo admite objetos de tipo `std::string`, y cada elemento recuperado durante el recorrido tiene ese tipo de forma explícita y segura. Cualquier intento de insertar un tipo distinto generaría un error en compilación. Tanto en Java como en C++, estos mecanismos representan ejemplos plenos de programación genérica, ya que combinan reutilización de código con un chequeo de tipos estricto.


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

Cuando se instancia una clase que tiene **parámetros de tipo**, el compilador adapta la definición genérica para que funcione con un tipo concreto. El objetivo es que, aunque la clase se haya escrito de forma abstracta respecto al tipo, su uso final sea coherente y seguro. Sin embargo, **Java y C++ no realizan este proceso de la misma manera**, y esa diferencia afecta tanto al funcionamiento interno como a las capacidades del sistema de tipos en tiempo de ejecución.

En **Java**, el compilador aplica un proceso llamado **type erasure** (borrado de tipos). Durante la compilación, se comprueba que el uso de los tipos genéricos es correcto, pero una vez superada esa fase, los parámetros de tipo se eliminan. Todos los tipos genéricos se sustituyen por su límite superior (normalmente `Object`), y se insertan conversiones de tipo implícitas donde sea necesario. Como consecuencia, en tiempo de ejecución no existe información sobre el tipo genérico concreto, y todas las instancias de una clase genérica comparten la misma representación en memoria.

Este diseño permite mantener compatibilidad con versiones antiguas de Java que no soportaban generics, pero tiene limitaciones importantes. Por ejemplo, no es posible conocer el tipo genérico real en tiempo de ejecución ni crear directamente arrays de tipos genéricos. En resumen, en Java la genericidad es principalmente un mecanismo de **chequeo en tiempo de compilación**, no de diferenciación en tiempo de ejecución.

En **C++**, en cambio, el compilador utiliza un mecanismo conocido como **instanciación de plantillas**. Cada vez que se usa una plantilla con un tipo concreto, el compilador genera una versión específica del código para ese tipo. Por ejemplo, `vector<int>` y `vector<string>` producen implementaciones distintas, completamente separadas. Esto implica que el tipo genérico no se borra, sino que se convierte en código concreto durante la compilación, permitiendo mayor flexibilidad y optimización, a costa de un mayor tiempo de compilación y mayor tamaño del binario.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta

Se puede definir una clase genérica `Par` en Java utilizando **dos parámetros de tipo**, de forma que permita almacenar dos valores potencialmente distintos entre sí. Cada parámetro de tipo representa un tipo concreto que se fijará en el momento de usar la clase. La clase no necesita conocer nada sobre esos tipos más allá de que serán consistentes durante su uso, lo que permite reutilizarla en multitud de contextos distintos con total seguridad de tipos.

La clase `Par` puede incluir un constructor que inicialice ambos valores y métodos *getter* para acceder a cada uno de ellos. Al emplear parámetros de tipo en lugar de `Object`, el compilador garantiza que los valores devueltos por los *getters* son del tipo correcto, sin necesidad de conversiones explícitas y sin riesgo de errores en tiempo de ejecución relacionados con el *casting*.

```java
public class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
}
```

Un ejemplo de uso típico consiste en emplear esta clase como tipo de retorno de una función. Por ejemplo, se puede definir un método que calcule la media y la desviación típica de un array de `double` y devuelva ambos resultados en un objeto `Par<Double, Double>`. De esta forma se agrupan dos valores relacionados sin perder claridad ni seguridad de tipos.

```java
public static Par<Double, Double> estadisticas(double[] datos) {
    double suma = 0.0;
    for (double d : datos) {
        suma += d;
    }
    double media = suma / datos.length;

    double sumaCuadrados = 0.0;
    for (double d : datos) {
        sumaCuadrados += Math.pow(d - media, 2);
    }
    double desviacion = Math.sqrt(sumaCuadrados / datos.length);

    return new Par<>(media, desviacion);
}

// Uso
double[] valores = {1.0, 2.0, 3.0, 4.0};
Par<Double, Double> res = estadisticas(valores);
System.out.println("Media: " + res.getPrimero());
System.out.println("Desviación típica: " + res.getSegundo());
```

En este uso, el compilador conoce en todo momento que ambos valores del `Par` son de tipo `Double`, y cualquier uso incorrecto sería detectado en compilación. Esto ilustra cómo los parámetros de tipo permiten expresar con claridad la intención del código y evitar errores que, con enfoques basados en `Object`, solo aparecerían en tiempo de ejecución.


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta

En Java es posible declarar **parámetros de tipo a nivel de método**, lo que permite que la genericidad se aplique solo a una operación concreta sin necesidad de que la clase completa sea genérica. Un método genérico declara sus parámetros de tipo antes del tipo de retorno y puede trabajar con ellos de forma segura. Esto resulta especialmente útil para funciones utilitarias que deben operar sobre distintos tipos manteniendo consistencia y control de tipos.

Si el método `seleccionaUno` se define usando `Object`, puede aceptar cualquier par de objetos, incluso de tipos distintos. Además, el tipo devuelto es `Object`, lo que obliga a realizar *downcasting* al tipo esperado en el código cliente, transfiriendo los errores al tiempo de ejecución. Tampoco existe ninguna garantía de que ambos objetos pasados al método sean del mismo tipo.

```java
import java.util.Random;

public static Object seleccionaUno(Object a, Object b) {
    Random r = new Random();
    return r.nextBoolean() ? a : b;
}

// Uso
String s = (String) seleccionaUno("Hola", "Adiós");
```

En cambio, al definir el método con un **parámetro de tipo**, se indica explícitamente que ambos argumentos deben ser del mismo tipo `T`, y ese mismo tipo es el que se devuelve. El compilador fuerza esa restricción y elimina la necesidad de conversiones explícitas. Cualquier intento de llamar al método con objetos de tipos distintos produce un error de compilación.

```java
import java.util.Random;

public static <T> T seleccionaUno(T a, T b) {
    Random r = new Random();
    return r.nextBoolean() ? a : b;
}

// Uso
String s = seleccionaUno("Hola", "Adiós");
```

En términos de comparación, el método genérico evita el *downcasting* porque el tipo de retorno es conocido en compilación, y además impone que ambos parámetros sean del mismo tipo, mejorando la corrección del programa antes de su ejecución. Esta combinación de seguridad y expresividad es una de las principales ventajas de los métodos genéricos frente a soluciones basadas en `Object`.


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

Sí, en Java es posible **establecer restricciones sobre los parámetros de tipo**, lo que se conoce como *bounded type parameters*. Esto permite indicar que un parámetro genérico debe ser una subclase de una clase concreta o implementar una interfaz determinada. De este modo, el tipo genérico no es completamente arbitrario y se pueden usar con seguridad los métodos definidos en esa superclase o interfaz. Para el caso de los números, Java proporciona la clase abstracta `Number`, de la que heredan `Integer`, `Double`, `Float`, etc.

Una primera solución, sin usar parámetros de tipo, consiste en definir un `Punto` cuyas coordenadas sean de tipo `Number`. Esto permite aceptar cualquier tipo numérico, pero el tipo concreto se pierde y es necesario convertir a `double` para operar, asumiendo que todos los números pueden representarse así. El chequeo de tipos es limitado, ya que no se distingue si el punto trabaja internamente con enteros, reales u otro subtipo de `Number`.

```java
public class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(Punto otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```

Una segunda solución más estricta consiste en usar **generics acotados**, indicando que el parámetro de tipo `T` debe extender `Number`. De esta forma, se mantiene la información del tipo concreto (`Integer`, `Double`, etc.) durante la compilación, y el compilador garantiza que ambos puntos usan exactamente el mismo tipo de coordenadas. El código es más expresivo y el chequeo de tipos es más fuerte.

```java
public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```

En relación con el **type erasure**, en ambos casos la información genérica se elimina tras la compilación. En la primera solución el tipo ya es explícitamente `Number`, mientras que en la segunda, tras el borrado de tipos, el parámetro `T` se sustituye por su límite superior. Por tanto, el tipo final generado por el compilador es también `Number`, lo que confirma que los genéricos en Java refuerzan el chequeo en tiempo de compilación, pero no introducen nuevos tipos en tiempo de ejecución.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

Ambas soluciones permiten reutilizar la clase `Punto` para distintos tipos numéricos, pero **no refuerzan el chequeo de tipos de la misma manera**. En la solución sin genéricos, al declarar las coordenadas como `Number`, el compilador permite libremente combinar distintos subtipos de `Number`. Es perfectamente válido crear un punto con una coordenada entera (`Integer`) y la otra real (`Double`), ya que ambas cumplen el contrato de ser `Number`. Esta flexibilidad, aunque cómoda, puede ocultar errores de diseño si se pretendía trabajar con un único tipo numérico homogéneo.

En la solución con genéricos acotados (`<T extends Number>`), el chequeo de tipos se ve claramente reforzado. Al instanciar un `Punto<T>`, el tipo `T` queda fijado a un subtipo concreto de `Number`, por ejemplo `Integer` o `Double`. Como consecuencia, **no es posible** crear un punto con una coordenada entera y otra real dentro de la misma instancia, ni tampoco calcular distancias entre puntos con distintos tipos de coordenadas. Cualquier intento de hacerlo sería rechazado en tiempo de compilación, lo que impone una coherencia estricta en el uso del tipo.

En cuanto al tipo devuelto por los métodos de acceso, la diferencia es significativa. En la solución sin genéricos, el método `getX` devuelve un `Number`, lo que obliga a tratar el resultado de forma genérica o a realizar conversiones explícitas si se necesita un subtipo concreto. El tipo exacto del número no forma parte de la interfaz pública de la clase desde el punto de vista del compilador.

En cambio, en la solución con genéricos, `getX` devuelve el tipo `T`, es decir, el subtipo concreto de `Number` con el que se instanció el `Punto`. Esto proporciona mayor precisión y expresividad al código cliente, ya que el compilador conoce exactamente el tipo devuelto y puede verificar su uso sin necesidad de *casting*. Así, los genéricos no solo mejoran la seguridad, sino también la claridad semántica del diseño.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta



## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

Para reforzar el chequeo de tipos y evitar el uso de `instanceof` y *downcasting*, se puede recurrir a una técnica conocida como **polimorfismo acotado recursivo** (*F-bounded polymorphism*). La idea consiste en hacer que la propia interfaz `Punto` sea genérica y que el parámetro de tipo represente el subtipo concreto de `Punto` con el que se puede calcular la distancia. De este modo, el compilador garantiza que la distancia siempre se calcula entre puntos del mismo tipo.

La interfaz se define con un parámetro de tipo que extiende a la propia interfaz. Así, el método `distanciaA` solo acepta puntos del mismo subtipo concreto. Esta restricción se expresa directamente en la signatura del método, lo que elimina por completo la necesidad de comprobaciones dinámicas del tipo en tiempo de ejecución.

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
```

Con esta definición, cada implementación concreta fija su propio tipo como parámetro genérico. En el caso de `Punto2D`, el método `distanciaA` solo puede recibir otro `Punto2D`, y el compilador impide cualquier combinación incorrecta. El código resulta más limpio y seguro, ya que se trabaja directamente con el tipo correcto sin conversiones forzadas.

```java
public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        double dx = x - p.x;
        double dy = y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

De forma análoga, `Punto3D` implementaría `Punto<Punto3D>` y calcularía la distancia usando tres coordenadas. Con este diseño, el refuerzo del chequeo de tipos es máximo: no es posible calcular la distancia entre un `Punto2D` y un `Punto3D`, y cualquier intento se detecta en tiempo de compilación. La genericidad se utiliza aquí no para abstraer datos, sino para **expresar relaciones de tipo más precisas** y eliminar errores estructurales del diseño.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Un **wildcard** (`?`) en Java representa un *tipo desconocido* dentro de un tipo genérico. Se utiliza cuando no interesa fijar un tipo concreto, sino expresar una relación de **covarianza** o **contravarianza** de forma controlada. Los wildcards permiten relajar la invariancia por defecto de los genéricos en Java, manteniendo al mismo tiempo la seguridad de tipos que impone el compilador.

La forma `List<? extends T>` indica que la lista contiene elementos de un tipo **desconocido que es T o un subtipo de T**. Esta construcción se usa cuando la lista se va a **leer** (consumir valores) pero no a modificar, ya que no se puede garantizar qué subtipo concreto contiene. Es el caso típico de algoritmos que procesan datos sin alterarlos, como sumar números. En términos prácticos, se puede leer elementos como `T`, pero no insertar nuevos valores (salvo `null`).

```java
public static double sumar(List<? extends Number> lista) {
    double suma = 0.0;
    for (Number n : lista) {
        suma += n.doubleValue();
    }
    return suma;
}
```

Por otro lado, `List<? super T>` indica que la lista es de un tipo **desconocido que es T o un supertipo de T**. Esta forma se emplea cuando la lista se va a **escribir** (producir valores), ya que es seguro insertar objetos de tipo `T`. Sin embargo, al recuperar elementos, solo se puede asumir que son de tipo `Object`, ya que el compilador no conoce el subtipo concreto almacenado. Este patrón es habitual en métodos que rellenan o amplían colecciones.

```java
public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
    lista.add(3);
}
```

En resumen, `? extends T` se utiliza cuando una estructura **produce** objetos de tipo `T` (lectura), mientras que `? super T` se usa cuando la estructura **consume** objetos de tipo `T` (escritura). Esta idea se resume habitualmente con la regla *PECS* (*Producer Extends, Consumer Super*), que permite recuperar covarianza y contravarianza de forma explícita y segura en Java.

