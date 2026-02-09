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
	Las cuatro características esenciales de la programación orientada a objetos son encapsulamiento, abstracción, herencia y polimorfismo. Estas propiedades permiten organizar el software de forma más modular y mantener una estructura que refleja mejor la lógica del problema. Aunque en C no existe orientación a objetos, puede verse como una evolución que integra datos y funciones en unidades coherentes llamadas clases. 🧩

	El encapsulamiento consiste en reunir datos y métodos dentro de una clase, controlando qué partes son accesibles desde el exterior. De esta forma, se evita que el código externo modifique directamente información interna, lo cual reduce errores y dependencias innecesarias. En contraste, en C cualquier módulo puede acceder a variables globales o funciones si conoce sus declaraciones, por lo que esta protección no existe de manera nativa. 🔐

	La abstracción permite ocultar los detalles internos del funcionamiento de una clase, exponiendo únicamente lo necesario para usarla. Esto permite trabajar con objetos sin conocer su implementación completa, de forma similar a cómo se usa una función en C sin ver necesariamente su código. En Java, esta abstracción se formaliza mediante clases, métodos e incluso palabras clave como abstract o interface, que ayudan a centrarse en lo que un objeto hace y no en cómo lo hace. 🎭

	La herencia posibilita crear nuevas clases basadas en otras ya existentes, lo que fomenta la reutilización y evita duplicar código. Gracias a ella pueden construirse jerarquías y especializar comportamientos. Por último, el polimorfismo permite que distintos objetos respondan de manera diferente a un mismo método, según su tipo real. Esta capacidad ofrece gran flexibilidad al diseño, ya que permite escribir código más genérico y adaptable sin conocer todos los tipos específicos que se usarán. 🔄

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta
	Cuatro lenguajes muy populares que permiten la programación orientada a objetos son Java, C++, Python y C#. Estos lenguajes incorporan de forma nativa los conceptos fundamentales de la POO —como clases, objetos, herencia y polimorfismo—, lo que facilita diseñar programas más organizados y modulares. 🌟

	Java es uno de los lenguajes más representativos del paradigma orientado a objetos, ya que prácticamente todo en él gira alrededor de clases y objetos. C++, aunque procede de C y permite programación estructurada, incluye un sistema completo de clases que habilita trabajar con POO cuando se desea. Por otro lado, Python destaca por ofrecer POO de manera sencilla y flexible, permitiendo crear clases con muy poca sintaxis. 🐍

	Finalmente, C# es un lenguaje moderno diseñado por Microsoft que adopta la POO como eje central, con características avanzadas y un diseño similar al de Java. Estos lenguajes son ampliamente utilizados en la industria, lo que los convierte en buenas opciones para aprender y practicar conceptos de orientación a objetos. 💻

	También destacan otros como Rust.

	Estos lenguajes se pueden diferenciar, entre otras cosas, por que algunos tienen "Garbage Colector" y otros no.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta
	La programación estructurada es un paradigma anterior a la POO que organiza el código en bloques lógicos y controlados mediante estructuras como secuencias, condiciones (if/else) y bucles (for, while). Su objetivo principal es evitar el uso excesivo de goto, lo que permite escribir programas más legibles y fáciles de seguir. En este enfoque, el flujo del programa se controla de forma clara y lineal, lo que reduce errores y facilita comprender qué ocurre en cada parte del código. 📘

	Este paradigma también promueve dividir el programa en funciones, aunque dichas funciones no están ligadas a datos como en la POO. En lenguajes como C —que seguramente te resulta familiar— este estilo es el más habitual: los datos se almacenan en variables o estructuras, y las operaciones sobre ellos se realizan mediante funciones independientes. La programación estructurada supuso un gran avance respecto a los programas desordenados y difíciles de mantener de épocas anteriores. 🔧

	La **programación modular**, por su parte, puede entenderse como una evolución natural de la programación estructurada. Su finalidad es dividir un programa grande en módulos independientes, cada uno con una responsabilidad clara. Un módulo suele estar formado por un conjunto de funciones y datos relacionados, lo que permite desarrollar, probar y modificar partes del programa sin afectar al resto. Este enfoque mejora enormemente la mantenibilidad y la reutilización del código. 📦

	En C, la modularidad se consigue mediante la separación en archivos *.c* y *.h*, algo que ya habrás visto: cada módulo define sus funciones y variables internas, mientras que los archivos de cabecera exponen únicamente lo necesario para que otros módulos puedan interactuar con él. En cierto modo, la programación modular sienta las bases de lo que luego será la orientación a objetos, pues ambas buscan organizar el código en unidades coherentes y bien definidas. 🧩

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta
	Un objeto en programación orientada a objetos se define principalmente por tres elementos fundamentales: estado, comportamiento e identidad. Estos elementos permiten que un objeto represente algo del mundo real de forma coherente dentro de un programa. Comprenderlos es clave para entender cómo funciona la POO en lenguajes como Java. 🔍

	El estado de un objeto está formado por sus atributos o variables internas, que almacenan la información que lo describe. Por ejemplo, un objeto Coche podría tener estado representado por su color, su velocidad o la cantidad de combustible. Cada objeto mantiene sus propios valores, lo que permite que existan múltiples instancias con características diferentes. 🚗

	El comportamiento es el conjunto de acciones que un objeto puede realizar, y se expresa a través de métodos. Siguiendo el ejemplo del coche, comportamientos típicos serían acelerar(), frenar() o girar(). Estos métodos permiten manipular o consultar el estado del objeto, estableciendo cómo interactúa con otros elementos del programa. ⚙️

	Finalmente, la identidad distingue a un objeto de otro, incluso cuando tienen el mismo estado y el mismo comportamiento. En Java, esta identidad está asociada a la ubicación del objeto en memoria, lo que garantiza que dos objetos iguales en contenido sigan siendo entidades diferentes. Esta propiedad permite manejar instancias independientes sin confundirse entre sí. 🆔

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
	Una clase es una plantilla o modelo a partir del cual se crean objetos en programación orientada a objetos. Define qué atributos (estado) y qué métodos (comportamiento) tendrán los objetos creados a partir de ella. Puede imaginarse como un plano o esquema que describe cómo será un cierto tipo de entidad dentro del programa. 🧩

	Una clase **no es lo mismo** que un objeto. La clase es el diseño, mientras que el objeto es un ejemplar concreto construido a partir de ese diseño. Siguiendo una analogía muy común, la clase sería como el plano de una casa, y el objeto sería una casa real construida siguiendo ese plano. Aunque haya múltiples casas basadas en el mismo plano, cada una existe de manera independiente. 🏠

	El término **instancia** se refiere precisamente a cada uno de esos objetos concretos creados a partir de una clase. Instanciar significa crear un objeto real en memoria utilizando la definición establecida por la clase. En Java, por ejemplo, esto se hace mediante la palabra clave `new`, que crea una instancia con su propio estado inicial. 🌱

	No todos los lenguajes orientados a objetos manejan el concepto de clase de la misma manera. Lenguajes como Java, C++ o C# están basados en clases, pero otros como JavaScript utilizan un modelo basado en **prototipos**, donde los objetos se crean a partir de otros objetos sin necesidad de una clase formal. Esto demuestra que la orientación a objetos puede aplicarse bajo distintos enfoques según el lenguaje. 🌐

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta
	En la mayoría de lenguajes orientados a objetos, los objetos se almacenan en memoria dinámica, es decir, en el heap. Esta zona de memoria permite crear objetos cuyo tamaño y duración no están determinados de antemano, algo imprescindible en la programación orientada a objetos. Cuando un objeto se crea, se reserva espacio en esta región y se devuelve una referencia que permite acceder a él desde el programa. 📦

	No todos los lenguajes almacenan los objetos de la misma manera. En Java, por ejemplo, **todos** los objetos se crean en el heap y solo se almacenan referencias en la pila (*stack*). En C++, sin embargo, es posible crear objetos tanto en la pila como en el heap, dependiendo de cómo se declaren. Esto muestra que la gestión de la memoria y la ubicación de los objetos no es uniforme entre lenguajes, sino que responde al modelo de ejecución de cada uno. ⚙️

	La **recolección de basura** (*garbage collection*) es un mecanismo automático que tienen algunos lenguajes, como Java o Python, para liberar memoria que ya no está siendo utilizada. El sistema identifica objetos a los que el programa ya no tiene referencias y los elimina del heap, recuperando ese espacio de forma segura. Gracias a este proceso, el programador no necesita liberar manualmente la memoria, lo que reduce errores como fugas de memoria o accesos a memoria ya liberada. 🗑️

	En otros lenguajes, como C o C++, la memoria no se libera automáticamente; el programador debe hacerlo usando instrucciones específicas como `free()` o `delete`. La ausencia de un recolector de basura ofrece más control, pero también aumenta el riesgo de errores de gestión de memoria. Por eso, la recolección de basura se considera una característica clave en lenguajes modernos que priorizan la seguridad y simplicidad del desarrollo. 🔍

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta
	Un método es una función definida dentro de una clase y representa una acción que los objetos de esa clase pueden realizar. Es la forma en que se expresa el comportamiento de un objeto dentro de la programación orientada a objetos. Cuando un objeto necesita ejecutar una operación, se invoca uno de sus métodos, que puede consultar o modificar el estado del propio objeto. ⚙️

	La **sobrecarga de métodos** es una característica que permite definir varios métodos con el mismo nombre dentro de una misma clase, siempre que cada uno tenga una lista de parámetros diferente. Esto ofrece flexibilidad al programar, ya que permite utilizar el mismo nombre de método para acciones conceptualmente similares, pero adaptadas a distintos tipos o cantidades de datos. 🔄

Por ejemplo, en Java se pueden tener dos métodos llamados `sumar()`, uno que reciba dos enteros y otro que reciba tres. Aunque compartan nombre, el compilador distingue cuál debe ejecutarse analizando los parámetros utilizados en la llamada. Este mecanismo facilita escribir código más claro y más natural para el programador. ✨

La sobrecarga no altera el comportamiento del polimorfismo ni tiene relación con herencia; simplemente proporciona múltiples versiones de un mismo método dentro de la misma clase. Es una herramienta muy común en lenguajes orientados a objetos como Java, C++ o C#, aunque su sintaxis y reglas pueden variar ligeramente entre ellos. 🧩

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta
	Un ejemplo mínimo de una clase en Java puede mostrar cómo se definen atributos y métodos dentro de la propia clase. En este caso, la clase Punto contendrá dos atributos llamados x e y, ambos con visibilidad por defecto, y un método que calcule la distancia del punto al origen utilizando la fórmula matemática de la distancia euclidiana. 📐

	Además del código de la clase, resulta útil mostrar un pequeño ejemplo de uso, donde se crea una instancia de `Punto` y se llama a su método `calculaDistanciaAOrigen`. Esto permite ver cómo se aplican en la práctica los conceptos de clase, objeto e invocación de métodos. 🧩

📝 Clase Punto en Java	
	public class Punto {
    int x;  // visibilidad por defecto
    int y;  // visibilidad por defecto

    // Método que calcula la distancia a (0,0)
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
▶️ Ejemplo de uso
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;

        double distancia = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + distancia);
    }
}


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta
	En un programa Java, el punto de entrada es el método main, definido exactamente con la firma public static void main(String[] args). Es el método que la JVM invoca cuando se ejecuta una aplicación, y actúa como el inicio del flujo del programa. Sin este método, Java no sabría dónde comenzar la ejecución. ▶️

	La palabra clave **`static`** indica que un método o atributo pertenece a la *clase* y no a un *objeto concreto*. Esto significa que puede usarse sin necesidad de crear una instancia previa. En el caso del método `main`, es necesario que sea estático porque la JVM lo ejecuta antes de que exista ningún objeto de la clase, por lo que no puede depender de una instancia. 📌

	`static` **no se utiliza solo para el método `main`**. También se emplea para declarar variables de clase (compartidas por todos los objetos), métodos auxiliares que no dependen del estado interno de un objeto, o incluso bloques estáticos que se ejecutan al cargar la clase. Su uso debe ser cuidadoso, ya que demasiado código estático puede romper la idea de encapsular comportamiento dentro de objetos. 🧩

	La combinación `static final` se utiliza principalmente para definir **constantes**, es decir, valores que pertenecen a la clase y que no pueden modificarse. Por ejemplo, `static final double PI = 3.14159;`. En este caso, `static` hace que la constante sea accesible sin crear objetos, y `final` impide que se cambie su valor. Esto permite crear valores globales seguros, claros y coherentes dentro del diseño del programa. 🔒

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta
	Para compilar un programa Java desde la línea de comandos se utiliza el comando javac, seguido del nombre del archivo que contiene el código fuente. Una vez compilado, la ejecución se realiza con el comando java, indicando el nombre de la clase que contiene el método main sin la extensión .class. Este proceso permite transformar el código escrito por el programador en un formato que la máquina virtual podrá interpretar. ▶️

Por ejemplo, si tienes un archivo llamado `Main.java`, el proceso sería:
	javac Main.java
	java Main

	Una pregunta habitual es si *Java es compilado*. La respuesta es que Java es **compilado e interpretado a la vez**. Primero, el compilador `javac` transforma el código fuente `.java` en *bytecode*, un formato intermedio independiente del sistema operativo. Luego, ese bytecode es ejecutado por la **Máquina Virtual de Java (JVM)**, que actúa como intérprete y entorno de ejecución. Este diseño hace que Java sea muy portable entre diferentes sistemas. 🌍

	La **máquina virtual** es un programa que simula un ordenador abstracto capaz de ejecutar bytecode. Gracias a esto, el mismo programa Java puede funcionar en Windows, Linux, macOS o cualquier otro sistema que tenga instalada una JVM. En lugar de generar código específico para cada plataforma, se genera un formato universal que todas las JVM pueden entender. 🧠

	El **bytecode** es ese lenguaje intermedio generado por `javac`, más sencillo que el código de máquina real y diseñado para ser ejecutado por la JVM. Cada clase compilada se guarda en un archivo con extensión `.class`, el cual contiene exclusivamente bytecode. Estos archivos son los que realmente ejecuta la JVM cuando se lanza un programa Java. Por eso, en la ejecución solo se indica el nombre de la clase, no el archivo. 📦

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta
	En Java, new es el operador que reserva memoria en el heap y crea un objeto, devolviendo una referencia a él. Además de reservar memoria, new invoca al constructor de la clase para inicializar el estado del objeto. A diferencia de C/C++, donde puede usarse malloc o constructores en C++ con distinta semántica, en Java siempre que se desea un objeto se utiliza new (salvo contadas excepciones internas como interning de String, que no aplica aquí). 🧱

	Un **constructor** es un método especial de la clase que **no tiene tipo de retorno** (ni siquiera `void`) y cuyo nombre coincide exactamente con el de la clase. Su finalidad es **inicializar los atributos** cuando se crea una instancia. Si no se define ningún constructor, el compilador genera un **constructor por defecto** sin parámetros; en cuanto se define uno propio con parámetros, conviene declarar también el vacío si se necesita. Durante la inicialización es común usar `this` para distinguir entre parámetros y atributos. 🛠️

	A continuación se muestra un ejemplo de clase `Empleado` con tres atributos (`dni`, `nombre`, `apellidos`) y un **constructor** que los inicializa. Se incluye además un constructor vacío opcional (por si se requiere crear el objeto sin datos y rellenarlo después) y un método simple para mostrar el estado. 📄

📝 Ejemplo de clase Empleado con constructor:

	public class Empleado {
    // Atributos (visibilidad por defecto para brevedad; en práctica suele usarse 'private')
    String dni;
    String nombre;
    String apellidos;

    // Constructor por defecto (opcional, útil si se quiere instanciar sin datos)
    public Empleado() {
        // Podría establecer valores por defecto si se desea
    }

    // Constructor que inicializa todos los atributos
    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }

    // Método de utilidad (opcional)
    public String descripcion() {
        return "Empleado[dni=" + dni + ", nombre=" + nombre + ", apellidos=" + apellidos + "]";
    }
}


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta
	La referencia this en Java apunta al objeto actual sobre el que se está ejecutando un método o constructor. Sirve para acceder a los atributos y métodos de esa instancia y, muy especialmente, para desambiguar cuando los nombres de los parámetros coinciden con los de los atributos (por ejemplo, en un constructor). También puede usarse para encadenar constructores o pasar la referencia del objeto actual a otros métodos. 🔎

	No se llama exactamente igual ni funciona igual en todos los lenguajes. En **Java, C++ y C#** existe `this` con un propósito muy similar: referirse a la instancia actual. En **Python**, en cambio, se utiliza el parámetro **`self`** (que no es palabra clave, sino un parámetro convencional) para acceder al objeto actual dentro de métodos. En **JavaScript**, `this` existe, pero su **vinculación depende del contexto de invocación** (puede cambiar según cómo se llame la función), lo que lo hace más sutil que en Java. 🌐

A continuación se muestra un ejemplo de uso de `this` en la clase `Punto`, tanto en el **constructor** (para desambiguar parámetros y atributos) como en un **método** que devuelve una copia traducida, ilustrando cómo `this` referencia los campos de la instancia actual. 🧩

📝 Ejemplo: uso de this en Punto (Java)
	public class Punto {
    int x;  // visibilidad por defecto, como pediste en el ejemplo anterior
    int y;

    // Constructor que desambigua con 'this'
    public Punto(int x, int y) {
        this.x = x;  // 'this.x' = atributo; 'x' = parámetro
        this.y = y;
    }

    // Método que calcula la distancia al origen
    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
        // 'this.' es opcional aquí, pero hace explícito que se usan atributos de la instancia
    }

    // Método que crea y devuelve un nuevo Punto trasladado respecto a 'this'
    Punto trasladar(int dx, int dy) {
        return new Punto(this.x + dx, this.y + dy);
    }
}

// Ejemplo de uso
class Main {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        System.out.println("Distancia: " + p.calculaDistanciaAOrigen()); // 5.0
        Punto q = p.trasladar(2, -1); // q es (5, 3)
        System.out.println("Distancia de q: " + q.calculaDistanciaAOrigen());
    }
}
``


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta
	Se añadirá a la clase Punto un método llamado distanciaA(Punto otro) que calcule la distancia entre el objeto actual (this) y el punto recibido como parámetro. Para ello, se toma la diferencia en cada coordenada (dx y dy) y se aplica la fórmula de la distancia euclidiana: √(dx² + dy²). 📐

	Se incluye además una pequeña comprobación de *null* para evitar errores si se invoca con un parámetro nulo, lanzando una `IllegalArgumentException`. Aunque `this.` es opcional al acceder a `x` e `y`, se mantiene para dejar claro que se usan los atributos del objeto actual. 🧩

📝 Clase Punto con distanciaA
	public class Punto {
    int x;  // visibilidad por defecto
    int y;

    public Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Nuevo método: distancia entre 'this' y el punto 'otro'
    double distanciaA(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto proporcionado no puede ser null");
        }
        int dx = this.x - otro.x;
        int dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
        // Alternativa equivalente y estable numéricamente:
        // return Math.hypot(this.x - otro.x, this.y - otro.y);
    }
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta
	En Java, los parámetros siempre se pasan por valor, pero es importante entender qué significa ese “valor”. En el caso de un objeto como Punto, lo que se pasa es una copia de la referencia, no una copia del objeto. Esto implica que, si dentro del método se modifican los atributos del objeto recibido, esos cambios afectan al objeto original, porque tanto dentro como fuera del método se está apuntando al mismo objeto real en memoria. 🔍

	Por ejemplo, si en un método haces `p.x = 10;`, y `p` es un parámetro de tipo `Punto`, el cambio se reflejará fuera del método. Lo que **no** se puede hacer es cambiar la referencia original (por ejemplo, asignando `p = new Punto(0,0)`), ya que al tratarse de una copia de la referencia, esa reasignación solo cambia la variable interna del método, no la que está fuera. 🧩

Cuando el parámetro es un tipo **primitivo** como `int`, el comportamiento es distinto: se pasa una **copia del valor** y no hay referencia involucrada. Esto significa que cualquier cambio que se haga dentro del método sobre ese entero **no afecta** al valor original. Es completamente equivalente a C en este sentido: modificar el parámetro dentro de la función no altera la variable que se pasó en la llamada. 🔢

En resumen: al pasar un objeto se copia la *referencia*, por lo que sí se pueden modificar sus atributos desde dentro del método; pero al pasar un tipo primitivo como `int`, solo se copia el valor, y los cambios dentro de la función no tienen efecto fuera. Esta distinción es fundamental para entender cómo se comportan los métodos en Java y evitar confusiones con el modelo de ejecución. ✔️

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta
	El método toString() en Java es la representación textual “amigable” de un objeto. Todas las clases lo heredan de java.lang.Object, y suele sobrescribirse para devolver información útil del estado del objeto, por ejemplo cuando se imprime con System.out.println(obj) o al concatenar con cadenas. Si no se sobrescribe, se mostrará algo poco informativo (el nombre de la clase y un hash). ✨

	En otros lenguajes existe un concepto muy similar: en **C#** se usa `ToString()`; en **Python** se emplea `__str__` (y `__repr__` para una representación más “oficial”/depurable); en **JavaScript** hay un método `toString()` en los prototipos; y en **C++** no hay un método universal, pero se acostumbra a sobrecargar el operador de inserción `<<` para `std::ostream` o a proporcionar funciones como `to_string`. La idea general es ofrecer una vista legible del objeto para depuración, logging y salidas de usuario. 🌐

Como buenas prácticas, conviene que `toString()` sea **claro, conciso y estable**, que no tenga efectos secundarios ni realice cómputos costosos. Además, suele anotarse con `@Override` para dejar claro que se está sobrescribiendo el método heredado. A continuación se muestra un ejemplo para la clase `Punto` con atributos `x` e `y`. 📌

📝 Ejemplo de toString() en Punto (Java)

	public class Punto {
    int x; // visibilidad por defecto
    int y; // visibilidad por defecto

    public Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    double distanciaA(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto proporcionado no puede ser null");
        }
        int dx = this.x - otro.x;
        int dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    @Override
    public String toString() {
        return "Punto(x=" + x + ", y=" + y + ")";
        // Alternativa:
        // return String.format("Punto(x=%d, y=%d)", x, y);
    }
}

// Ejemplo de uso
class Main {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        System.out.println(p); // Imprime: Punto(x=3, y=4)
    }
}


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

### Respuesta
	Una clase puede parecerse a un struct de C, sobre todo si se piensa únicamente en que ambos permiten agrupar varios datos bajo un mismo tipo. De hecho, en la superficie, una clase sencilla sin métodos se parece mucho a un struct con varios campos. Sin embargo, la orientación a objetos añade una serie de capacidades adicionales que los struct en C no poseen de forma nativa. 🧩

	El `struct` de C **no tiene métodos**, mientras que una clase no solo contiene datos, sino también funciones asociadas (*métodos*) que actúan sobre esos mismos datos. En una clase, datos y comportamiento van juntos; en un `struct`, el comportamiento se implementa como funciones separadas, sin vínculo formal con el tipo. Además, los `struct` no tienen **encapsulamiento**, es decir, no pueden declarar atributos privados, protegidos o públicos: todo es siempre público por defecto. 🔧

Tampoco existe en C la noción de **constructores**, que permiten inicializar un objeto de forma controlada en el momento de su creación. Un `struct` solo puede ser inicializado mediante inicializadores simples o asignaciones posteriores, pero sin un mecanismo centralizado equivalente al constructor de una clase. Otro aspecto ausente es la **herencia**, característica esencial en muchos lenguajes orientados a objetos y completamente inexistente en C. 🧱

Por último, a un `struct` en C también le falta la idea de **instancia** tal como se entiende en la POO. Aunque técnicamente cada variable de tipo `struct` es una “instancia”, el lenguaje no ofrece un modelo formal de objetos: no hay `this`, no hay métodos asociados al tipo, no hay polimorfismo ni un sistema de tipos dinámicos. En resumen, un `struct` proporciona solo los datos; una clase proporciona datos, comportamiento y un conjunto completo de mecanismos para construir objetos reales dentro del paradigma orientado a objetos. 🚀

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
	En C es posible “emular” parcialmente una clase definiendo un struct que almacene los datos y escribiendo funciones externas que operen sobre dicho struct. No existe una vinculación formal entre datos y funciones como en Java, pero sí puede lograrse un comportamiento similar pasando la dirección del struct a las funciones, para que estas puedan acceder o modificar sus campos. De este modo, se separan los datos (en el struct) del comportamiento (en funciones sueltas), algo que en Java se integraría naturalmente en una clase. 🔧

	En esta aproximación, la función que calcula la distancia al origen recibe un puntero al `struct Punto`. Así puede acceder a los campos `x` e `y` usando `p->x` y `p->y`. Esto es conceptualmente parecido al uso de `this` en Java, ya que el puntero pasado a la función indica sobre qué objeto concreto debe operar la función. En C, sin embargo, ese “objeto actual” no se pasa automáticamente: hay que enviarlo explícitamente como parámetro. 📌

La diferencia clave es que en Java el parámetro `this` existe siempre y se gestiona automáticamente por el lenguaje, sin que el programador lo pase manualmente. En C, en cambio, es responsabilidad del programador incluir en cada función un argumento que reciba el puntero al `struct`. Es decir, el equivalente de `this` es simplemente el primer parámetro de la función, por ejemplo `Punto* p`. 🧩

📝 Ejemplo: “emulación” de la clase Punto en C

	#include <stdio.h>
#include <math.h>

// Definición del "tipo"
typedef struct {
    int x;
    int y;
} Punto;

// "Método" que calcula la distancia al origen
double calculaDistanciaAOrigen(Punto *p) {
    return sqrt(p->x * p->x + p->y * p->y);
}

int main() {
    Punto p;
    p.x = 3;
    p.y = 4;

    double d = calculaDistanciaAOrigen(&p);
    printf("Distancia al origen: %f\n", d);

    return 0;
}

🔍 ¿Qué pasó con this?

En Java, this siempre está disponible dentro de métodos y apunta automáticamente al objeto actual.
En C, no existe this, y se reemplaza pasándolo manualmente:

	calculaDistanciaAOrigen(&p);

Dentro de la función, ese parámetro (por convención el primero) actúa como la referencia al “objeto actual”, es decir:

p->x   // equivalente conceptual a this.x


