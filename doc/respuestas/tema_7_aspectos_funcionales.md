<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta

Un **puntero a una función** es una variable que almacena la dirección de una función, en lugar de la dirección de un dato. En C, las funciones también ocupan una posición en memoria, por lo que pueden referenciarse e invocarse indirectamente mediante punteros. Este mecanismo permite pasar funciones como parámetros, almacenarlas en estructuras o seleccionar dinámicamente qué función ejecutar, algo habitual en diseños modulares o en la implementación de callbacks. Aunque se usa en programación estructurada, conceptualmente se relaciona con el paso de comportamiento, idea que más adelante aparece de forma más evolucionada en la programación funcional y en Java con interfaces funcionales y expresiones lambda.

La declaración de un puntero a función debe coincidir exactamente con la firma de la función a la que apunta: tipo de retorno y tipos de los parámetros. En este caso, se considera una función que recibe una cadena de caracteres (`char *`) y devuelve esa misma cadena transformada a mayúsculas. En C es habitual modificar la cadena “in situ”, recorriendo carácter a carácter y aplicando una transformación, lo que evita asignaciones dinámicas adicionales y simplifica la gestión de memoria.

A continuación se muestra un ejemplo completo en C. Se define la función `convertirAMayusculas`, se crea un puntero local a dicha función llamado `aMayusculas` y se invoca usando el puntero. Obsérvese que la llamada mediante el puntero utiliza la misma sintaxis que una llamada directa, lo que hace transparente su uso una vez declarado.

```c
#include <stdio.h>
#include <ctype.h>

char* convertirAMayusculas(char* cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = (char) toupper(cadena[i]);
    }
    return cadena;
}

int main() {
    char texto[] = "Hola mundo";

    // Puntero a función
    char* (*aMayusculas)(char*);
    aMayusculas = convertirAMayusculas;

    // Invocación mediante el puntero
    char* resultado = aMayusculas(texto);

    printf("%s\n", resultado);
    return 0;
}
```

Este uso de punteros a funciones resulta especialmente relevante para entender la transición hacia estilos más declarativos, ya que introduce la idea de tratar las funciones como valores. Aunque en C se hace de forma explícita y con una sintaxis más compleja, el concepto es fundamental para comprender posteriormente mecanismos más expresivos en Java, donde el comportamiento puede encapsularse y pasarse de forma más segura y legible.


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una **función lambda** es una función anónima, es decir, una función que no tiene nombre y que se define directamente allí donde se necesita. Su principal objetivo es permitir expresar comportamiento de forma concisa y tratarlo como un valor que puede asignarse a variables, pasarse como argumento o devolver como resultado. Conceptualmente, una función lambda representa la misma idea que un puntero a función en C, pero con una sintaxis más simple y segura, evitando detalles de bajo nivel como direcciones de memoria y tipos explícitos complejos.

En **JavaScript**, las funciones son ciudadanos de primera clase, por lo que es posible asignar directamente una función lambda (arrow function) a una variable local. El siguiente ejemplo define una función que recibe una cadena y devuelve otra en mayúsculas, y la almacena en la variable `aMayusculas`. El método `toUpperCase` ya forma parte de las cadenas, por lo que simplifica la implementación respecto a C.

```javascript
let aMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

let resultado = aMayusculas("Hola mundo");
console.log(resultado);
```

En **Java**, las funciones lambda se introdujeron para facilitar un estilo más funcional, pero siempre están asociadas a un **tipo funcional**, es decir, una interfaz con un único método abstracto. En este caso se utiliza `Function<String, String>`, que representa una función que recibe un `String` y devuelve un `String`. La variable local `aMayusculas` actúa como referencia a la función lambda, de forma análoga al puntero a función en C, pero con comprobación de tipos en tiempo de compilación.

```java
import java.util.function.Function;

public class Ejemplo {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        String resultado = aMayusculas.apply("Hola mundo");
        System.out.println(resultado);
    }
}
```

El uso de funciones lambda permite separar el *qué se hace* del *cómo se usa*, favoreciendo un código más expresivo y modular. Para alguien con experiencia en C y Java orientado a objetos, pueden verse como una evolución natural del uso de punteros a función y del polimorfismo, donde el comportamiento se encapsula de manera más directa y menos ceremonial.


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El **paradigma funcional** es un estilo de programación que se basa en la evaluación de funciones y en la composición de estas para construir programas, priorizando *qué se quiere calcular* frente a *cómo se ejecuta paso a paso*. En este paradigma se fomenta el uso de funciones puras, es decir, funciones que para una misma entrada producen siempre la misma salida y no dependen ni modifican un estado externo. Esto contrasta con el enfoque imperativo típico de C o del Java clásico, donde el flujo del programa y el cambio de estado mediante asignaciones son elementos centrales.

Se denomina a lenguajes como **Java 8 multi‑paradigma** porque, aun siendo fundamentalmente orientados a objetos, incorporan características propias de otros paradigmas, en este caso del funcional. Java sigue basándose en clases, objetos, herencia y polimorfismo, pero desde Java 8 permite trabajar con funciones lambda, referencias a métodos y APIs diseñadas con un enfoque declarativo, como Stream. Esto significa que en un mismo lenguaje pueden combinarse varios mecanismos para resolver un problema, eligiendo el paradigma más adecuado en cada situación.

Decir que las funciones son **“ciudadanos de primera clase”** implica que las funciones se tratan como cualquier otro valor del lenguaje. Esto supone que pueden almacenarse en variables, pasarse como argumentos a otras funciones, devolverse como resultado y componerse entre sí. En C esta idea aparece de forma limitada mediante punteros a función, mientras que en Java se materializa a través de interfaces funcionales y lambdas, con mayor seguridad de tipos y una sintaxis más expresiva.

Este concepto es clave para entender el paradigma funcional, ya que permite abstraer el comportamiento y desacoplarlo de las estructuras de datos. Desde una perspectiva procedente de la programación estructurada y orientada a objetos, puede interpretarse como una evolución natural del polimorfismo: en lugar de seleccionar comportamientos mediante jerarquías de clases, se seleccionan directamente mediante funciones, facilitando un código más flexible, reutilizable y declarativo.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

La **sintaxis básica de una función lambda en Java** se introdujo a partir de Java 8 y permite definir de forma compacta la implementación de una interfaz funcional. Una expresión lambda consta de tres partes fundamentales: la lista de parámetros, el operador `->` (llamado *flecha*) y el cuerpo de la función. Esta sintaxis elimina la necesidad de declarar explícitamente una clase o un método con nombre, reduciendo notablemente el código necesario para expresar un comportamiento concreto.

La forma general es `(parámetros) -> expresión` o `(parámetros) -> { bloque de instrucciones }`. Si el cuerpo de la lambda es una sola expresión, su resultado se devuelve de manera implícita, sin usar la palabra clave `return`. En cambio, si el cuerpo contiene varias instrucciones, debe encerrarse entre llaves `{}` y utilizar `return` cuando sea necesario. El tipo de los parámetros puede omitirse si el compilador puede inferirlo, lo que suele ocurrir cuando la lambda se asigna a una interfaz funcional conocida.

```java
Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();
```

En este ejemplo, `cadena` es el parámetro de entrada, `->` separa los parámetros del cuerpo, y `cadena.toUpperCase()` es la expresión que se ejecuta. El tipo `Function<String, String>` indica que la lambda recibe un `String` y devuelve otro `String`, por lo que no es necesario especificar tipos en el parámetro. Conceptualmente, esta lambda actúa como una implementación directa del método abstracto de la interfaz funcional.

Desde el punto de vista de alguien con experiencia en Java orientado a objetos, una función lambda puede entenderse como una forma abreviada de crear una clase anónima con un único método. La diferencia principal es que la sintaxis es más clara y enfatiza el comportamiento en lugar de la estructura, lo que facilita la adopción de un estilo más funcional sin abandonar el modelo de tipos y la seguridad característica de Java.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Recibir una función como parámetro implica tratar el comportamiento como un valor que puede pasarse a otros métodos para que estos lo ejecuten internamente. Esta idea es fundamental en el paradigma funcional y permite escribir código más flexible y reutilizable, ya que un mismo método puede aplicar distintas transformaciones sin modificarse. Conceptualmente, es una generalización del uso de punteros a función en C, pero con una sintaxis más clara y, en el caso de Java, con control de tipos en tiempo de compilación.

En **JavaScript**, al ser las funciones ciudadanos de primera clase, un método puede recibir directamente una función y llamarla como si fuera cualquier otra variable. El método `transformar` recibe una cadena y una función transformadora, y simplemente invoca dicha función con la cadena como argumento. La variable local `aMayusculas` referencia una función lambda que se pasa al método.

```javascript
function transformar(texto, transformadora) {
    return transformadora(texto);
}

let aMayusculas = cadena => cadena.toUpperCase();

let resultado = transformar("Hola mundo", aMayusculas);
console.log(resultado);
```

En **Java**, el mismo concepto se implementa utilizando interfaces funcionales. El método `transformar` recibe un `String` y un objeto de tipo `Function<String, String>`, que representa la operación a aplicar. Desde el interior del método, la función se ejecuta mediante el método `apply`. La lambda asignada a `aMayusculas` actúa como implementación concreta del comportamiento pasado como parámetro.

```java
import java.util.function.Function;

public class Ejemplo {

    public static String transformar(String texto, Function<String, String> transformadora) {
        return transformadora.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        String resultado = transformar("Hola mundo", aMayusculas);
        System.out.println(resultado);
    }
}
```

Este estilo de programación permite desacoplar los datos del comportamiento que se aplica sobre ellos, favoreciendo soluciones más genéricas y declarativas. Para alguien con experiencia previa en Java orientado a objetos, puede interpretarse como una alternativa al uso de jerarquías o clases anónimas, donde el polimorfismo se expresa directamente mediante el paso de funciones en lugar de objetos completos.


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

Invocar un método pasando una **función lambda directamente en la llamada** refuerza la idea de que el comportamiento puede definirse “en el momento de uso”, sin necesidad de almacenarlo previamente en una variable. Este estilo es característico del paradigma funcional y favorece un código más conciso, ya que la transformación se expresa justo donde se aplica. El método `transformar` permanece inmutable y reutilizable, mientras que la lógica concreta se define dinámicamente.

En **JavaScript**, esta técnica resulta natural, ya que las funciones pueden declararse inline sin restricciones adicionales. En el siguiente ejemplo, se pasa directamente a `transformar` una función lambda que invierte la cadena recibida. La inversión se realiza separando la cadena en caracteres, invirtiendo el array resultante y volviéndolo a unir.

```javascript
function transformar(texto, transformadora) {
    return transformadora(texto);
}

let resultado = transformar("Hola mundo", cadena =>
    cadena.split("").reverse().join("")
);

console.log(resultado);
```

En **Java**, el enfoque es conceptualmente el mismo, aunque la lambda debe ajustarse al tipo funcional esperado. El método `transformar` recibe un `Function<String, String>`, y la lambda que invierte la cadena se define directamente en la llamada, utilizando clases estándar como `StringBuilder` para realizar la inversión de forma eficiente.

```java
import java.util.function.Function;

public class Ejemplo {

    public static String transformar(String texto, Function<String, String> transformadora) {
        return transformadora.apply(texto);
    }

    public static void main(String[] args) {
        String resultado = transformar("Hola mundo",
                cadena -> new StringBuilder(cadena).reverse().toString()
        );

        System.out.println(resultado);
    }
}
```

Este uso directo de lambdas permite expresar claramente la intención del código sin introducir identificadores adicionales ni estructuras auxiliares. Desde una perspectiva orientada a objetos, puede verse como una forma de polimorfismo puntual, donde el comportamiento concreto se define exactamente en el punto de llamada, aumentando la legibilidad y reduciendo el código ceremonial.


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un **cierre** o *closure* es una función que, además de su propio cuerpo, **captura y conserva el acceso a variables del contexto donde fue definida**, incluso cuando se ejecuta fuera de ese contexto. En el caso de las funciones lambda, esto significa que pueden utilizar variables locales externas sin que estas formen parte explícita de sus parámetros. Este concepto es fundamental en el paradigma funcional, ya que permite combinar datos y comportamiento de forma flexible y expresiva.

En **Java**, las lambdas pueden acceder a variables locales siempre que estas sean **efectivamente finales**, es decir, que no se modifiquen después de su inicialización. Aunque no sea obligatorio marcarlas con la palabra clave `final`, el compilador impone esta restricción para evitar problemas de consistencia y concurrencia. De este modo, la lambda “cierra” sobre el valor de la variable en el momento de su definición, no sobre su ubicación en memoria como ocurriría con punteros en C.

A continuación se modifica el ejemplo anterior incorporando una nueva función lambda que concatena a la cadena de entrada un sufijo definido en una variable local externa. La lambda accede directamente a dicha variable, demostrando el comportamiento de cierre.

```java
import java.util.function.Function;

public class Ejemplo {

    public static String transformar(String texto, Function<String, String> transformadora) {
        return transformadora.apply(texto);
    }

    public static void main(String[] args) {
        String sufijo = " - procesado";

        String resultado = transformar("Hola mundo",
                cadena -> cadena + sufijo
        );

        System.out.println(resultado);
    }
}
```

En este caso, la variable `sufijo` no pertenece a la función lambda, pero puede utilizarse dentro de ella gracias al mecanismo de cierre. Este comportamiento resulta especialmente útil para parametrizar funciones sin aumentar la lista de argumentos, y representa un paso importante hacia un estilo más declarativo. Para alguien con experiencia en programación estructurada, puede entenderse como una forma controlada y segura de “recordar” contexto, integrada de manera natural en el diseño de Java moderno.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

Una **función lambda** y un **puntero a función en C** comparten la idea básica de permitir que el comportamiento se trate como un valor, pero difieren profundamente en su nivel de abstracción, seguridad y capacidades. Un puntero a función en C es simplemente una dirección de memoria que apunta a código ejecutable, sin información adicional sobre contexto o estado. La función llamada a través de ese puntero solo puede operar con los parámetros que se le pasan explícitamente, y el compilador no introduce restricciones más allá de la coincidencia estricta de tipos en la firma.

Por el contrario, una función lambda es una **abstracción de alto nivel** integrada en el sistema de tipos del lenguaje. En Java, una lambda no es solo código, sino una instancia de una interfaz funcional, lo que implica comprobación de tipos, integración con el modelo de objetos y mejor legibilidad. Además, la sintaxis es más expresiva y evita detalles de bajo nivel como la manipulación explícita de punteros o la complejidad sintáctica asociada a ellos en C.

Una diferencia clave es la existencia de **cierres (closures)**. Las funciones lambda pueden capturar variables del contexto donde fueron definidas, conservando ese estado para su uso posterior. Esto no ocurre con los punteros a función en C, que no capturan contexto por sí mismos; para simular este comportamiento es necesario recurrir a estructuras adicionales y pasar manualmente punteros a datos, lo que aumenta la complejidad y el riesgo de errores de memoria.

En resumen, mientras que los punteros a función en C representan una herramienta potente pero de bajo nivel, las funciones lambda ofrecen una forma más segura, expresiva y composable de trabajar con comportamiento. Desde una perspectiva evolutiva, puede entenderse que las lambdas generalizan y refinan la idea de los punteros a función, integrándola de forma natural en lenguajes modernos y facilitando la adopción del paradigma funcional sin renunciar a la seguridad y estructura del lenguaje.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

Devolver funciones consiste en crear una función cuyo resultado es otra función, lo que permite **configurar comportamiento de forma dinámica**. En el paradigma funcional, esta técnica se utiliza para generar funciones especializadas a partir de parámetros iniciales. En Java, esto se logra devolviendo una interfaz funcional, como `Function<Double, Double>`, y es posible gracias al uso de funciones lambda y cierres.

En el ejemplo propuesto, la función `crearDescuento` recibe un porcentaje y devuelve una función “descuento”. Esta función resultante recibe una cantidad y devuelve el valor con el porcentaje descontado. El porcentaje no se pasa como parámetro a la función descuento, sino que queda **capturado** en el momento de su creación. Desde el punto de vista conceptual, `crearDescuento` fabrica funciones con un comportamiento parcialmente definido.

```java
import java.util.function.Function;

public class Ejemplo {

    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return cantidad -> cantidad * (1 - porcentaje);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(0.10);
        Function<Double, Double> descuento25 = crearDescuento(0.25);

        double precio = 100.0;

        System.out.println(descuento10.apply(precio)); // 90.0
        System.out.println(descuento25.apply(precio)); // 75.0
    }
}
```

En este caso se crean dos funciones de descuento distintas, cada una con su propio porcentaje capturado. Ambas funciones comparten la misma estructura, pero su comportamiento difiere según el valor encerrado en el cierre. La variable `porcentaje` pertenece al contexto de `crearDescuento`, pero sigue estando accesible cuando se ejecuta la lambda devuelta.

La **closure** se produce porque cada función lambda mantiene una referencia al valor de `porcentaje` existente en el momento de su creación. Aunque la ejecución de la lambda ocurre posteriormente y fuera del cuerpo de `crearDescuento`, el valor permanece disponible y constante. Este mecanismo permite crear funciones altamente reutilizables y configurables, y representa una diferencia esencial frente a enfoques más tradicionales donde el estado debe pasarse explícitamente en cada llamada.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

En Java, una **interfaz funcional** es una interfaz que define un **único método abstracto**, y cuyo propósito es representar el tipo de una función lambda. Dado que Java es un lenguaje con comprobación estática de tipos, toda función lambda debe asociarse a un tipo bien definido, y ese tipo es precisamente una interfaz funcional. De este modo, la lambda actúa como una implementación concreta del único método abstracto de la interfaz, sin necesidad de crear explícitamente una clase.

El **requisito fundamental** para que una interfaz sea funcional es que contenga **exactamente un método abstracto**. No obstante, puede incluir otros elementos sin dejar de ser funcional, como métodos `default`, métodos `static` o métodos heredados de `Object` (`toString`, `equals`, etc.). Estos métodos no cuentan como abstractos a efectos de la definición, ya que no obligan a la lambda a proporcionar una implementación adicional.

Para reforzar esta intención, Java proporciona la anotación `@FunctionalInterface`. Su uso no es obligatorio, pero resulta altamente recomendable, ya que permite al compilador verificar que la interfaz cumple realmente los requisitos de una interfaz funcional. Si se añade un segundo método abstracto por error, el compilador lo detectará inmediatamente. Esta anotación cumple una función similar a las comprobaciones de coherencia que se realizan manualmente en C, pero de forma automática y segura.

Las interfaces funcionales son la base sobre la que se construyen las funciones lambda y muchas APIs modernas de Java, como `Function`, `Predicate`, `Consumer` o `Supplier`. Gracias a ellas, se puede integrar el paradigma funcional dentro del modelo orientado a objetos de Java, manteniendo el tipado estático y permitiendo que el comportamiento se trate como un valor, de forma clara, segura y expresiva.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

Una **interfaz funcional definida a mano** permite describir explícitamente el tipo de una función lambda, adaptándolo a un dominio concreto del problema. En este caso, se trata de representar una operación que transforma una cadena de texto en otra, algo que conceptualmente ya existe en `Function<String, String>`, pero que puede expresarse de forma más semántica mediante una interfaz propia. Esto mejora la legibilidad del código y hace más clara la intención del comportamiento que se espera.

Para que una interfaz sea funcional, debe cumplir el requisito de tener **un único método abstracto**. Ese método define la firma de la función que podrán implementar las lambdas asociadas a dicha interfaz. De forma opcional, puede añadirse la anotación `@FunctionalInterface`, que no cambia el comportamiento, pero garantiza en tiempo de compilación que la interfaz no rompe este requisito.

A continuación se define la interfaz funcional `Transformador`, cuya responsabilidad es convertir una cadena (`String`) en otra. Esta interfaz puede usarse después para recibir lambdas, devolverlas o almacenarlas, exactamente igual que las interfaces funcionales estándar de Java.

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}
```

Con esta definición, cualquier función lambda que reciba un `String` y devuelva un `String` podrá asignarse a una referencia de tipo `Transformador`. Esta técnica ilustra cómo Java integra el paradigma funcional dentro de su modelo orientado a objetos, permitiendo crear tipos de función personalizados sin perder el control del tipado estático ni la claridad del diseño.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Para hacer la interfaz funcional **más genérica**, se pueden emplear **genéricos** en Java, de forma que el tipo de entrada y el tipo de salida no queden fijados a `String`. Esto permite definir transformadores reutilizables para distintos tipos de datos, manteniendo la comprobación estática de tipos en tiempo de compilación. Desde el punto de vista conceptual, se está generalizando la idea de “función” para que opere sobre cualquier par de tipos.

La interfaz funcional genérica `Transformador<T, R>` define un único método abstracto que recibe un valor de tipo `T` y devuelve un valor de tipo `R`. El uso de genéricos no altera los requisitos de una interfaz funcional, ya que sigue existiendo un solo método abstracto. La anotación `@FunctionalInterface` sigue siendo válida y continúa garantizando que la interfaz cumple las condiciones necesarias para ser usada con expresiones lambda.

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}
```

Una vez definida la interfaz genérica, es posible crear transformadores concretos mediante funciones lambda. En el siguiente ejemplo se define un transformador que convierte un `Double` en un `Integer` redondeando su valor. La correspondencia entre los tipos genéricos y la implementación de la lambda se verifica automáticamente por el compilador.

```java
public class Ejemplo {

    public static void main(String[] args) {
        Transformador<Double, Integer> redondear =
                numero -> (int) Math.round(numero);

        Integer resultado = redondear.transformar(3.6);
        System.out.println(resultado); // 4
    }
}
```

Este enfoque combina las ventajas del paradigma funcional con el sistema de tipos de Java, permitiendo definir comportamientos genéricos, reutilizables y seguros. Para alguien con experiencia previa en Java orientado a objetos, puede interpretarse como una extensión natural del uso de interfaces y genéricos, donde ahora no solo se parametrizan tipos de datos, sino también transformaciones entre ellos.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

En Java, además de permitir definir **interfaces funcionales propias**, el lenguaje incluye un **conjunto amplio de interfaces funcionales predefinidas** en el paquete `java.util.function`. Estas interfaces cubren los casos de uso más habituales al trabajar con funciones lambda y programación funcional, evitando la necesidad de redefinir interfaces genéricas como `Transformador<T, R>`. Todas ellas cumplen el requisito de tener un único método abstracto y están diseñadas para integrarse de forma natural con las APIs modernas de Java, como *Stream*.

La interfaz más general es **`Function<T, R>`**, que representa una función que recibe un valor de tipo `T` y devuelve un valor de tipo `R`. A partir de ella se definen variantes especializadas según el sentido de la operación: **`UnaryOperator<T>`** cuando entrada y salida son del mismo tipo, y **`BinaryOperator<T>`** cuando la función combina dos valores del mismo tipo para producir uno del mismo tipo. Estas interfaces se utilizan de forma intensiva en transformaciones y composiciones de datos.

Otro grupo importante son las interfaces destinadas a funciones que **no devuelven un valor**, como **`Consumer<T>`**, que representa una operación que recibe un dato y realiza una acción (por ejemplo, mostrarlo por pantalla), o **`BiConsumer<T, U>`**, que actúa sobre dos parámetros. En el extremo opuesto se encuentra **`Supplier<T>`**, que no recibe parámetros y simplemente devuelve un valor, siendo útil para la creación diferida de objetos o valores.

Finalmente, están las interfaces cuyo resultado es booleano, como **`Predicate<T>`** y **`BiPredicate<T, U>`**, empleadas para expresar condiciones lógicas, especialmente en filtrados. Además de estas versiones genéricas, Java proporciona numerosas variantes especializadas para tipos primitivos (`IntFunction`, `DoublePredicate`, `LongConsumer`, etc.), con el objetivo de evitar el *boxing* y mejorar el rendimiento. En conjunto, este conjunto de interfaces funcionales estándar permite cubrir la mayoría de necesidades funcionales en Java sin definir nuevos tipos, reforzando su carácter de lenguaje multi‑paradigma.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método **`forEach`** de la interfaz `Iterable` (y, por tanto, de `List`) permite recorrer una colección aplicando una función a cada uno de sus elementos. Desde un punto de vista funcional, representa una alternativa declarativa al bucle `for` tradicional, ya que desplaza el énfasis desde el control explícito de la iteración hacia la definición del comportamiento que se quiere ejecutar para cada elemento. Este método recibe como parámetro un `Consumer<T>`, es decir, una función que acepta un valor pero no devuelve resultado.

En lugar de gestionar índices o variables auxiliares, con `forEach` se expresa directamente la acción que debe realizarse con cada elemento de la lista. Esto encaja con el estilo funcional, donde las colecciones “saben” cómo recorrerse y el programador se limita a proporcionar la operación a aplicar. Para quienes provienen de C o Java imperativo, puede verse como una abstracción del bucle `for`, donde el recorrido queda oculto y solo se expone el comportamiento.

A continuación se muestra un ejemplo en Java donde se recorre una lista de `Integer` y se muestra un mensaje únicamente si el número es positivo. La condición se expresa dentro de la función lambda pasada a `forEach`, manteniendo el recorrido separado de la lógica de decisión.

```java
import java.util.Arrays;
import java.util.List;

public class Ejemplo {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(3, -2, 0, 5, -7);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("Número positivo: " + n);
            }
        });
    }
}
```

Este enfoque mejora la legibilidad al centrar el código en la intención y no en la mecánica del bucle. Además, prepara el terreno para operaciones funcionales más avanzadas, como filtrados o transformaciones con `Stream`, donde el estilo declarativo resulta aún más expresivo y potente que la iteración tradicional.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

La firma de `forEach` utiliza `Consumer<? super T>` en lugar de `Consumer<T>` para **aumentar la flexibilidad de tipos** sin perder seguridad. La interfaz `Consumer` se usa únicamente para *consumir* elementos de la colección, es decir, para recibir valores de tipo `T` y operar con ellos, pero nunca para producir valores de ese tipo. Al permitir `? super T`, se acepta no solo un `Consumer<T>`, sino también un consumidor de cualquier supertipo de `T` (por ejemplo, `Consumer<Number>` para una `List<Integer>`), lo que amplía las posibilidades de reutilización del código sin introducir errores de tipo.

Este criterio se resume en la regla conocida como **PECS** (*Producer Extends, Consumer Super*). PECS indica que cuando un parámetro genérico **produce** valores de tipo `T`, debe declararse con `? extends T`, y cuando **consume** valores de tipo `T`, debe declararse con `? super T`. En el caso de `forEach`, la colección produce elementos de tipo `T`, pero la función pasada como argumento los consume, por lo que se utiliza correctamente `Consumer<? super T>`. De esta manera se mantiene el principio de sustitución sin forzar tipos innecesariamente restrictivos.

Este mismo razonamiento puede aplicarse para mejorar la definición del método `transformar`. Si el método recibe un valor de tipo `T` y aplica una función que consume un `T` y produce un `R`, la firma más flexible para la función transformadora no es `Function<T, R>`, sino `Function<? super T, ? extends R>`. Con ello se permite, por ejemplo, usar una función que acepte un supertipo de `T` o que devuelva un subtipo de `R`, aumentando la generalidad del método.

```java
public static <T, R> R transformar(
        T valor,
        Function<? super T, ? extends R> transformadora
) {
    return transformadora.apply(valor);
}
```

El uso de PECS permite diseñar APIs genéricas más robustas y expresivas, algo especialmente importante en programación funcional, donde las funciones se pasan como valores. Para quien proviene de Java imperativo o de C, esta regla proporciona un criterio sistemático para decidir entre `extends` y `super`, evitando errores sutiles y mejorando la reutilización del código sin comprometer la seguridad de tipos.


## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

Las **referencias a métodos** permiten tratar un método existente como una función, sin necesidad de ejecutarlo en el momento de obtener la referencia. En esencia, se extrae el comportamiento de un objeto o de una clase y se almacena en una variable, para poder invocarlo más tarde. Este mecanismo es una extensión natural del uso de funciones como valores y resulta especialmente útil en estilos funcionales, donde el comportamiento se pasa y reutiliza con facilidad.

En **JavaScript**, los métodos de un objeto son funciones y pueden referenciarse directamente. Al obtener la referencia a un método de un objeto concreto, dicha función mantiene el acceso al objeto mediante `this` cuando se invoca correctamente. En el siguiente ejemplo se define una clase `Persona` con un método `saludar`, se crea una instancia y se obtiene una referencia a su método para ejecutarlo posteriormente.

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, me llamo " + this.nombre);
    }
}

// Código principal
let persona = new Persona("Ana");
let referenciaSaludar = persona.saludar.bind(persona);

referenciaSaludar();
```

En **Java**, las referencias a métodos forman parte del lenguaje desde Java 8 y se integran con las interfaces funcionales. A diferencia de JavaScript, la referencia a un método de instancia se obtiene de forma explícita indicando el objeto y el método mediante la sintaxis `objeto::metodo`. Esta referencia se puede almacenar en una variable cuyo tipo sea una interfaz funcional compatible, como `Runnable` si el método no recibe parámetros ni devuelve valor.

```java
public class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, me llamo " + nombre);
    }
}
```

```java
public class Ejemplo {
    public static void main(String[] args) {
        Persona persona = new Persona("Ana");

        Runnable referenciaSaludar = persona::saludar;
        referenciaSaludar.run();
    }
}
```

En ambos lenguajes, la referencia a métodos permite desacoplar la invocación del comportamiento de su definición concreta. En JavaScript se apoya en el modelo dinámico de funciones, mientras que en Java se integra en el sistema de tipos mediante interfaces funcionales, reforzando la seguridad y claridad del código sin renunciar a un estilo más funcional y expresivo.


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

En Java existen **cuatro tipos principales de referencias a métodos**, todas ellas introducidas en Java 8 como una forma concisa de reutilizar métodos existentes allí donde se espera una función lambda. Una referencia a método es, conceptualmente, una abreviatura de una lambda que simplemente invoca un método ya definido. Todas ellas deben ser compatibles con una **interfaz funcional**, ya que la referencia no es un tipo por sí misma, sino una forma de proporcionar una implementación de un único método abstracto.

El primer tipo es la **referencia a un método estático**, cuya forma general es `Clase::metodoEstatico`. Se utiliza cuando el método no depende de ninguna instancia concreta. Por ejemplo, puede referenciarse un método estático que convierte una cadena a mayúsculas, siempre que su firma sea compatible con la interfaz funcional utilizada.

```java
Function<String, String> aMayusculas = String::toUpperCase;
```

El segundo tipo es la **referencia a un método de instancia de un objeto concreto**, con la forma `instancia::metodo`. En este caso, la referencia queda ligada a ese objeto específico, y cuando se invoca la función se ejecuta el método sobre dicha instancia. Es el caso típico cuando se quiere reutilizar un método sin parámetros o con parámetros fijos implícitos en el estado del objeto.

```java
Persona persona = new Persona("Ana");
Runnable saludar = persona::saludar;
```

El tercer tipo es la **referencia a un método de instancia sobre cualquier instancia de una clase**, cuya forma es `Clase::metodo`. Aquí el objeto sobre el que se ejecuta el método se recibe implícitamente como primer parámetro de la función. Este tipo de referencia suele aparecer en operaciones sobre colecciones, donde cada elemento actúa como la instancia sobre la que se invoca el método.

```java
Function<String, Integer> longitud = String::length;
```

Por último, existe la **referencia a constructor**, con la forma `Clase::new`. Este tipo permite tratar un constructor como una función que crea objetos, y se utiliza habitualmente junto con interfaces funcionales que no devuelven `void`, como `Supplier` o `Function`. De este modo, la creación de objetos puede integrarse en un estilo funcional y declarativo.

```java
Supplier<Persona> crearPersona = () -> new Persona("Ana");
// equivalente a:
Supplier<Persona> crearPersonaRef = Persona::new;
```

En conjunto, estos cuatro tipos de referencias a métodos proporcionan una forma clara y tipada de reutilizar código existente, reduciendo el uso de lambdas innecesarias y facilitando la adopción de un estilo funcional dentro del modelo orientado a objetos de Java.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

El método `Collections.sort` permite ordenar listas recibiendo un **comparador**, que define el criterio de ordenación entre dos elementos. Con la llegada de las expresiones lambda, este comparador puede expresarse de forma mucho más compacta que con clases anónimas, facilitando un estilo declarativo. En este caso, se desea ordenar objetos `Persona` primero por edad y, en caso de igualdad, por orden alfabético del nombre.

A continuación se muestra la clase `Persona`, que contiene los atributos `nombre` y `edad`, junto con sus métodos de acceso. Esta definición es común a ambos ejemplos de ordenación y resulta coherente con un diseño orientado a objetos clásico en Java.

```java
public class Persona {
    private String nombre;
    private int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() {
        return nombre;
    }

    public int getEdad() {
        return edad;
    }
}
```

La **primera versión** utiliza una expresión lambda que implementa manualmente la lógica de comparación. Dentro de la lambda se comparan primero las edades y, solo si son iguales, se comparan los nombres usando el orden natural de las cadenas. Esta forma resulta explícita y cercana a cómo se habría implementado un `Comparator` de manera tradicional.

```java
import java.util.Collections;
import java.util.List;

Collections.sort(personas, (p1, p2) -> {
    int comparacionEdad = Integer.compare(p1.getEdad(), p2.getEdad());
    if (comparacionEdad != 0) {
        return comparacionEdad;
    }
    return p1.getNombre().compareTo(p2.getNombre());
});
```

La **segunda versión** emplea la clase `Comparator` y sus métodos auxiliares, lo que da lugar a un código más expresivo y legible. Con `Comparator.comparing` se define el primer criterio de ordenación y con `thenComparing` se encadena el segundo criterio. Esta aproximación es más declarativa y aprovecha mejor las utilidades funcionales añadidas a Java 8.

```java
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

Collections.sort(
    personas,
    Comparator.comparing(Persona::getEdad)
              .thenComparing(Persona::getNombre)
);
```

Ambas versiones son correctas y funcionalmente equivalentes, pero la segunda resalta mejor la intención del código y reduce el riesgo de errores. Este tipo de construcciones muestra cómo Java combina la orientación a objetos con elementos funcionales para expresar de forma clara operaciones comunes como la ordenación de colecciones.

