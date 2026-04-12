<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 4.2 HERENCIA

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

En orientación a objetos, la **herencia** es un mecanismo que permite definir una nueva clase a partir de otra ya existente, estableciendo una relación conceptual del tipo **“A es‑un B”**. Esta relación indica que la subclase representa una especialización de la superclase, no algo distinto o no relacionado. Por ejemplo, si se afirma que un `Artillero` es‑un `Soldado`, se está expresando que todo lo que conceptualmente es válido para un soldado también lo es para un artillero. Esto va más allá de reutilizar código: implica una relación semántica fuerte entre tipos.

La primera implicación importante de la herencia es la **compatibilidad de tipos**. Siempre que una clase `A` hereda de una clase `B`, cualquier objeto de tipo `A` puede utilizarse donde se espere un objeto de tipo `B`. En Java, esto permite tratar de forma uniforme a objetos de diferentes subclases usando referencias de la superclase. Gracias a esta propiedad, se pueden almacenar distintos tipos de soldados en una misma estructura (como un array de `Soldado`) y operar sobre ellos sin conocer su tipo concreto, algo que no es posible en programación estructurada en C/C++ sin orientación a objetos.

La segunda implicación es la **herencia de estado y comportamiento**. La subclase hereda los atributos y métodos de la superclase, respetando las normas de visibilidad (por ejemplo, los atributos privados no son accesibles directamente). De este modo, tanto `Artillero` como `Zapador` heredan el atributo `nombre` y el método `saludar()`, evitando duplicación de código. Cada subclase puede además añadir su propio estado y comportamiento específico, como el número de cohetes o de minas, sin afectar a la definición general de lo que significa ser un soldado.

### Ejemplo en Java

```java
public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}
```

```java
public class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}
```

```java
public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}
```

```java
public class Prueba {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Soldado("Carlos");
        ejercito[1] = new Artillero("Luis", 5);
        ejercito[2] = new Zapador("Ana", 3);

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```

Este último fragmento muestra la compatibilidad de tipos: aunque los objetos reales son de clases distintas, todos pueden tratarse como `Soldado` y responder al mensaje `saludar()`, demostrando el efecto práctico de la herencia.



## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Al crear un objeto de una clase concreta que hereda de otra, **siempre se ejecutan dos (o más) constructores**, uno por cada nivel de la jerarquía de herencia. En el ejemplo de `Artillero` o `Zapador`, primero se ejecuta el constructor de la **clase base `Soldado`** y, a continuación, el constructor de la **subclase concreta**. Este orden está fijado por el lenguaje y garantiza que el estado heredado se inicialice antes de que la subclase añada su propio estado. Por tanto, al crear un `new Artillero(...)`, se ejecuta primero el constructor de `Soldado` y después el de `Artillero`.

La palabra clave `super` dentro de un constructor sirve para **invocar explícitamente un constructor de la clase base**. Mediante `super(...)` se pasan los valores necesarios para inicializar la parte heredada del objeto. Conceptualmente, equivale a decir que la subclase delega en la superclase la responsabilidad de construir su propio estado. Esta llamada debe ser siempre la **primera instrucción** del constructor, ya que la inicialización de la superclase no puede depender de código ejecutado posteriormente en la subclase.

Si la clase base dispone de un **constructor sin parámetros y accesible**, el compilador de Java inserta automáticamente una llamada implícita a `super()` cuando no se escribe nada. Sin embargo, si la clase base **no tiene un constructor sin parámetros visible** (por ejemplo, solo define constructores con argumentos), **es obligatorio llamar a `super(...)` de forma explícita**. De no hacerlo, el código no compila, porque Java no sabe cómo inicializar correctamente la parte heredada del objeto. Esto obliga a la subclase a decidir explícitamente qué constructor de la superclase debe usarse.

En resumen, en cualquier creación de un objeto con herencia participan siempre los constructores de toda la cadena de clases, ejecutándose desde la clase más general hasta la más específica. `super` es el mecanismo que conecta esos constructores y asegura una inicialización correcta del objeto completo. Cuando no existe un constructor base sin parámetros accesible, el uso explícito de `super` deja de ser opcional y pasa a ser una exigencia del diseño de la jerarquía.


## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Sí, los **atributos privados de la superclase forman parte de la instancia de la subclase en memoria**. Cuando se crea un objeto de una subclase como `Artillero` o `Zapador`, ese objeto contiene físicamente la parte correspondiente a `Soldado` y, dentro de ella, todos sus atributos, incluidos los privados como `nombre`. En memoria no existen “objetos separados” para la superclase y la subclase, sino un único objeto cuyo diseño incluye toda la cadena de herencia.

Ahora bien, que esos atributos **formen parte del objeto** no implica que **puedan usarse directamente desde el código de la subclase**. La palabra clave `private` no tiene efecto sobre la memoria, sino sobre la **visibilidad desde el código fuente**. Por este motivo, aunque un `Artillero` tenga en memoria el atributo `nombre` heredado de `Soldado`, el compilador no permite acceder a él directamente mediante `this.nombre`, ya que ese atributo solo es visible dentro de la propia clase `Soldado`.

La forma correcta de interactuar con ese estado heredado es a través de los **métodos públicos o protegidos** que exponga la superclase. En el ejemplo, tanto `Artillero` como `Zapador` pueden llamar al método `saludar()`, que sí tiene acceso interno a `nombre`, aunque ellas mismas no puedan leerlo ni modificarlo directamente. Esto refuerza la encapsulación: la subclase reutiliza el estado, pero sin romper las reglas de acceso definidas por la clase base.

Por ejemplo, el siguiente código **no es válido**, aunque el atributo exista en memoria, porque viola la visibilidad:

```java
public class Artillero extends Soldado {
    public void imprimirNombre() {
        // System.out.println(nombre); // ERROR: nombre es private en Soldado
        saludar(); // Correcto: usa un método heredado
    }
}
```

En conclusión, una instancia de una subclase contiene completamente los atributos privados de su superclase, pero la herencia **no elimina la encapsulación**: el acceso sigue estando controlado estrictamente por las reglas de visibilidad del lenguaje, no por la estructura en memoria.


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La **compatibilidad de tipos** entre una superclase y sus subclases tiene una consecuencia directa muy importante en términos de **extensibilidad del código**. Significa que es posible añadir nuevos tipos concretos derivados sin necesidad de modificar el código que ya trabaja con la superclase. El sistema está diseñado para operar con la abstracción general (`Soldado`), no con los detalles de cada subtipo, lo que reduce el acoplamiento y facilita la evolución del programa.

Gracias a esta propiedad, el código que maneja colecciones de soldados no necesita conocer cuántos tipos existen ni cuáles se añadirán en el futuro. Mientras todos los nuevos tipos respeten la relación “A es‑un B” y hereden de `Soldado`, serán automáticamente compatibles. Este enfoque es una base del principio de **abierto/cerrado**: el código está abierto a ser ampliado (añadiendo nuevas subclases) pero cerrado a ser modificado (no es necesario cambiar el código existente).

Para ilustrarlo, se puede añadir un nuevo tipo de soldado, por ejemplo un `Sanitario`, que también hereda de `Soldado` y añade su propio estado específico, como el número de botiquines. No es necesario tocar el código que recorre el array de soldados y les pide que saluden, porque dicho código solo depende del tipo base `Soldado`, que sigue siendo el mismo.

```java
public class Sanitario extends Soldado {
    private int botiquines;

    public Sanitario(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
}
```

El código que hace que todos los soldados saluden **no se modifica en absoluto**:

```java
Soldado[] ejercito = new Soldado[4];
ejercito[0] = new Soldado("Carlos");
ejercito[1] = new Artillero("Luis", 5);
ejercito[2] = new Zapador("Ana", 3);
ejercito[3] = new Sanitario("Marta", 2);

for (Soldado s : ejercito) {
    s.saludar();
}
```

En conclusión, la compatibilidad de tipos permite que el sistema crezca de forma ordenada y segura, añadiendo nuevas funcionalidades mediante nuevas subclases sin riesgo de romper el código existente. Esta es una de las ventajas clave de la herencia frente a enfoques no orientados a objetos.



## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

Sí, en Java es perfectamente posible que una **referencia del supertipo apunte a objetos reales de un subtipo**. Esto es una consecuencia directa de la compatibilidad de tipos derivada de la herencia: si `Artillero` es‑un `Soldado`, entonces una referencia de tipo `Soldado` puede almacenar un objeto `Artillero`. Esta situación es la más habitual cuando se trabaja con arrays, listas o parámetros de métodos definidos usando el tipo base, permitiendo tratar de forma uniforme a objetos de diferentes clases concretas.

Sin embargo, cuando se utiliza una **referencia del supertipo**, solo se pueden invocar los **métodos que estén definidos en ese supertipo** (o heredados por él). Aunque el objeto real sea, por ejemplo, un `Artillero`, el compilador solo permite llamar a métodos conocidos por la clase `Soldado`. Por este motivo, no se puede invocar directamente `getCohetes()` usando una referencia `Soldado`, ya que ese método no forma parte del contrato público del supertipo.

El **upcasting** consiste en tratar un objeto de un subtipo como si fuera del supertipo, y en Java se realiza de forma implícita y segura. Es lo que ocurre al hacer `Soldado s = new Artillero(...)`. El **downcasting**, en cambio, consiste en convertir una referencia del supertipo en una referencia del subtipo, y requiere una conversión explícita. Este tipo de conversión es potencialmente peligrosa, porque solo es válida si el objeto real es realmente de ese subtipo. Para evitar errores en tiempo de ejecución, se utiliza el operador `instanceof`, que permite comprobar el tipo real del objeto antes de realizar el `cast`.

Un recorrido típico del array de soldados con comprobación de tipo podría ser el siguiente:

```java
for (Soldado s : ejercito) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // downcasting seguro
        System.out.println("Cohetes disponibles: " + a.getCohetes());
    }
}
```

En este ejemplo se observa cómo se combina la generalidad del supertipo con el acceso controlado a comportamientos específicos de las subclases. El uso de `instanceof` garantiza que el `downcasting` solo se realice cuando es seguro, manteniendo la flexibilidad del diseño sin comprometer la seguridad del programa.



## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El acceso **protegido** forma parte de los mecanismos de ocultación de información en orientación a objetos y se sitúa entre el acceso `private` y el acceso `public`. Un atributo o método protegido **no es accesible desde cualquier clase**, pero **sí lo es desde las subclases**, incluso aunque se encuentren en otro paquete. De este modo, se permite que las clases derivadas reutilicen o amplíen el comportamiento interno de la superclase sin exponer ese estado o comportamiento al resto del programa.

En Java, el acceso protegido se implementa mediante la palabra clave `protected`. A diferencia de `private`, que limita el acceso exclusivamente a la propia clase, `protected` concede visibilidad a las subclases, preservando al mismo tiempo la encapsulación frente a clases no relacionadas. Esto resulta especialmente útil cuando la superclase define un estado que conceptualmente pertenece a todas las subclases, pero que no debería ser accesible libremente desde fuera de la jerarquía.

Aplicado al ejemplo de `Soldado`, convertir el atributo `nombre` en protegido permite que las subclases lo utilicen directamente cuando tenga sentido desde el punto de vista del diseño. Por ejemplo, un `Zapador` puede necesitar referirse al nombre del soldado al realizar una acción específica, como colocar una mina, sin romper la relación de herencia ni duplicar información. Aun así, el atributo sigue sin ser público, por lo que otras clases externas no pueden acceder a él.

Un posible uso sería el siguiente:

```java
public class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}
```

```java
public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerMina() {
        System.out.println("El zapador " + nombre + " ha colocado una mina");
        minas--;
    }

    public int getMinas() {
        return minas;
    }
}
```

En conclusión, el acceso protegido permite un equilibrio entre reutilización y encapsulación: las subclases pueden acceder directamente a información relevante de la superclase, mientras que el resto del sistema sigue estando aislado de los detalles internos de la jerarquía.



## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

En orientación a objetos, **no todos los lenguajes definen necesariamente una única clase base común para todos los objetos**, aunque muchos sí lo hacen. La existencia de una clase raíz permite proporcionar un conjunto mínimo de operaciones y comportamientos comunes (como comparación, conversión a texto o gestión básica de identidad) que estarán disponibles para cualquier objeto del sistema. Sin embargo, esta decisión depende del diseño del lenguaje y de su modelo de tipos, por lo que **no es una característica universal**.

En algunos lenguajes orientados a objetos más flexibles o de tipado menos estricto, puede no existir una clase base explícita para todos los objetos, o bien esa clase puede no ser visible o relevante para el programador. En otros casos, como en ciertos lenguajes que mezclan paradigmas, solo los tipos definidos como “objetos” forman parte de una jerarquía común, quedando fuera tipos primitivos u otras construcciones básicas del lenguaje.

En **Java**, sí existe claramente una **clase base común para todos los objetos: `java.lang.Object`**. Toda clase definida en Java hereda directa o indirectamente de `Object`, incluso aunque no se indique explícitamente con `extends`. Esto implica que cualquier objeto en Java dispone, al menos, de los métodos definidos en esa clase, como `toString()`, `equals()` o `hashCode()`. Esta herencia implícita garantiza un comportamiento mínimo común y una gran uniformidad en el modelo de objetos del lenguaje.

Como consecuencia práctica, cualquier referencia a un objeto en Java puede tratarse, en última instancia, como una referencia de tipo `Object`. Este diseño simplifica la interoperabilidad de tipos y refuerza la coherencia del sistema, aunque también introduce diferencias claras entre los tipos primitivos (como `int` o `double`) y los objetos, aun cuando Java proporcione mecanismos como las clases envoltorio para reducir esa distancia.



## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La **herencia múltiple** es una característica de algunos lenguajes orientados a objetos que permite que una clase **herede directamente de más de una clase base al mismo tiempo**. Es decir, una clase puede ser una especialización de varias clases distintas, reutilizando su estado y su comportamiento. Conceptualmente, esto permitiría expresar relaciones del tipo “A es‑un B y es‑un C”, lo que puede resultar atractivo en determinados modelos, pero también introduce ambigüedades y complejidad, especialmente cuando varias clases base definen métodos o atributos con el mismo nombre.

No todos los lenguajes orientados a objetos admiten herencia múltiple de clases. En aquellos que sí lo hacen (como C++), el programador debe enfrentarse a problemas como el **diamante de la herencia**, donde una misma clase base puede aparecer varias veces en la jerarquía, generando dudas sobre qué versión de un atributo o método debe utilizarse. Esto complica tanto el diseño como la comprensión del código, y requiere reglas adicionales para resolver conflictos.

En **Java no existe herencia múltiple de clases**. Una clase solo puede extender directamente a **una única clase base** mediante `extends`. Esta decisión de diseño elimina de raíz los problemas de ambigüedad asociados a la herencia múltiple y simplifica el modelo de herencia, haciéndolo más predecible y seguro, especialmente para programadores que se están iniciando en la orientación a objetos.

No obstante, Java ofrece una alternativa parcial mediante las **interfaces**. Una clase puede implementar múltiples interfaces, heredando así varios contratos de comportamiento (métodos) sin heredar estado. De este modo se obtiene parte de la flexibilidad de la herencia múltiple, pero evitando la mayoría de sus problemas. Conceptualmente, Java separa claramente la reutilización de comportamiento común (herencia de clases) de la definición de capacidades múltiples (implementación de interfaces).



## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta

En los lenguajes orientados a objetos, las **excepciones son objetos** y, como tales, forman parte de una jerarquía de clases. Esto permite definir **excepciones personalizadas** que representan errores propios del dominio del problema, en lugar de utilizar únicamente las excepciones genéricas del lenguaje. Al modelar las excepciones como clases, se pueden añadir atributos, métodos y constructores que aporten información contextual adicional sobre el error ocurrido.

En Java, una excepción será **no controlada (unchecked)** cuando herede de `RuntimeException` o de alguna de sus subclases. Este tipo de excepciones no obliga a ser declarado en la firma del método ni a ser capturado explícitamente, lo que resulta adecuado para errores de lógica o situaciones excepcionales que normalmente no pueden resolverse localmente. La decisión de que una excepción sea controlada o no controlada forma parte del diseño de la API.

Al ser objetos, las excepciones pueden **componerse con otros objetos**, aplicando el mismo principio que ya se conoce de la composición. En este caso, la excepción puede contener una referencia a un `Usuario`, permitiendo identificar con precisión qué entidad provocó el problema. Además, Java permite encadenar excepciones mediante una **causa subyacente**, de forma que se conserve la información del error original y se facilite el diagnóstico.

A continuación se muestra un ejemplo de una excepción personalizada no controlada, compuesta con un `Usuario` y con constructores sobrecargados para permitir incluir opcionalmente la causa:

```java
public class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java
public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

Este diseño permite lanzar una excepción rica en información, manteniendo la trazabilidad del error y aplicando de forma coherente los principios de orientación a objetos vistos previamente, como la encapsulación y la composición.



## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

Se afirma que **no se debe usar herencia únicamente para reutilizar código** porque la herencia introduce una **relación semántica fuerte** entre clases: la relación “A es‑un B”. Si esta relación no es conceptualmente cierta, el diseño se vuelve frágil y engañoso, aunque técnicamente funcione. Reutilizar métodos o atributos no justifica por sí solo que una clase sea considerada una especialización de otra; la herencia no expresa “A reutiliza a B”, sino “A es un tipo de B”.

Uno de los principales problemas de usar herencia solo por reutilización es el **acoplamiento excesivo**. La subclase queda fuertemente ligada a la implementación interna de la superclase, incluso a detalles que no forman parte de su intención original. Cambios en la superclase pueden afectar de forma inesperada a todas las subclases, dificultando el mantenimiento y la evolución del código. Esto es especialmente problemático cuando la relación entre clases no representa una jerarquía natural del dominio.

La **composición**, en cambio, permite reutilizar comportamiento sin imponer una relación de herencia. En lugar de “ser” otra cosa, una clase **tiene** otra cosa y delega en ella parte de su comportamiento. Esto reduce el acoplamiento, aumenta la flexibilidad y permite cambiar implementaciones de forma más segura. Por este motivo, en diseño orientado a objetos se suele recomendar “**priorizar composición frente a herencia**”, reservando la herencia para los casos en los que exista una relación clara y estable de especialización.

En conclusión, la herencia debe usarse cuando se quiera modelar una relación conceptual sólida del tipo “es‑un”, no como un simple mecanismo de reutilización técnica. Utilizar herencia sin ese fundamento conduce a diseños rígidos y difíciles de mantener, mientras que la composición ofrece una alternativa más segura y adaptable cuando el objetivo principal es compartir código o comportamiento.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta

Se recomienda **favorecer la composición frente a la herencia** porque la composición produce diseños más **flexibles, desacoplados y fáciles de mantener**. Mientras que la herencia establece una relación fija y fuerte entre clases (“A es‑un B”), la composición establece una relación más débil y dinámica (“A tiene‑un B”). Esto permite reutilizar comportamiento sin comprometer el significado conceptual de las clases ni forzar jerarquías artificiales.

Uno de los motivos principales es que la herencia **acopla fuertemente la subclase a la implementación de la superclase**. La subclase depende no solo de la interfaz pública, sino también de decisiones internas de diseño que pueden cambiar con el tiempo. Si la superclase evoluciona, las subclases pueden verse afectadas de forma inesperada, incluso aunque conceptualmente no debería cambiar su comportamiento. La composición, al basarse en delegación explícita, limita el impacto de estos cambios.

Además, la composición **facilita la extensión del comportamiento en tiempo de ejecución**. Una clase puede componerse con distintos objetos para modificar o ampliar su funcionalidad sin necesidad de crear nuevas subclases. En cambio, la herencia fija la combinación de comportamientos en tiempo de compilación, lo que puede provocar jerarquías grandes y difíciles de entender cuando se intenta cubrir muchas variantes funcionales.

En conclusión, favorecer la composición frente a la herencia conduce a diseños más robustos y adaptables. La herencia sigue siendo una herramienta valiosa cuando existe una relación clara y estable de especialización, pero la composición debe ser la primera opción cuando el objetivo es reutilizar funcionalidad o combinar comportamientos sin imponer relaciones rígidas entre los tipos.



## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta

Cuando se dice que la **herencia rompe la encapsulación**, no significa que el lenguaje deje de aplicar las reglas de visibilidad (`private`, `protected`, etc.), sino que la **subclase queda expuesta a los detalles internos de la superclase** de una forma más profunda de lo deseable. La encapsulación busca que una clase oculte su implementación interna y exponga solo un conjunto de operaciones bien definidas. Sin embargo, al heredar, la subclase pasa a depender no solo de la interfaz pública, sino también de decisiones internas y supuestos de diseño de la clase base.

En particular, la herencia hace que la subclase esté **íntimamente ligada al comportamiento interno de la superclase**, incluso a aspectos que no estaban pensados para ser utilizados o modificados. Métodos `protected`, campos protegidos o llamadas internas entre métodos heredados forman parte de un “contrato implícito” que no siempre está documentado. Si la superclase cambia su implementación interna (por ejemplo, el orden en que llama a ciertos métodos o cómo mantiene su estado), la subclase puede dejar de funcionar correctamente, aunque la interfaz pública no haya cambiado.

Este problema no aparece con la misma intensidad en la composición. Cuando una clase está compuesta con otra, solo depende de su **interfaz pública explícita**, no de su estructura interna. La clase que contiene delega comportamiento, pero no hereda decisiones internas ni asume cómo está implementado el otro objeto. De este modo, la encapsulación se preserva mejor, ya que cada clase mantiene un control claro sobre su propio estado y comportamiento.

En resumen, se dice que la herencia rompe la encapsulación porque **expone a las subclases a la implementación interna de la superclase**, creando dependencias frágiles y difíciles de mantener. Por este motivo, la herencia debe usarse con cuidado y solo cuando exista una relación clara y estable de especialización, mientras que la composición ofrece una alternativa más segura cuando se busca reutilizar funcionalidad sin comprometer el aislamiento entre clases.



## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

Una misma realidad del dominio puede modelarse de distintas formas en orientación a objetos, y la elección entre **herencia** y **composición** tiene implicaciones de diseño importantes. En este caso, `Estudiante` y `Trabajador` comparten datos comunes (DNI y nombre), lo que podría invitar a extraer una superclase común. El enfoque por herencia expresa claramente que ambos **son** un tipo de `Persona`, estableciendo una relación “es‑un” y centralizando los datos compartidos en un único punto.

Mediante herencia, la reutilización de código es directa: `Estudiante` y `Trabajador` heredan automáticamente los atributos y métodos de `Persona`. Este diseño es adecuado cuando la jerarquía representa una relación conceptual fuerte y estable. Sin embargo, también introduce un acoplamiento rígido, ya que cualquier cambio en `Persona` afecta a todas las subclases, incluso si no resulta relevante para ellas.

La alternativa mediante **composición** separa explícitamente los datos comunes en una clase independiente (`DatosPersonales`). En este enfoque, `Estudiante` y `Trabajador` **no son** `DatosPersonales`, sino que **tienen** unos datos personales. Este diseño suele ser más flexible, ya que evita jerarquías forzadas, facilita la reutilización de los datos en otros contextos y reduce el acoplamiento entre clases.

A continuación se muestran ambos modelos para contrastar las dos aproximaciones, observando cómo expresan relaciones conceptuales distintas aun resolviendo el mismo problema.

***

### Modelo usando herencia

```java
public class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}
```

```java
public class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}
```

```java
public class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}
```

***

### Modelo usando composición

```java
public class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java
public class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}
```

```java
public class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}
```

Este ejemplo muestra claramente cómo la herencia y la composición no son intercambiables: cada enfoque expresa una relación distinta y debe elegirse en función del significado del modelo, no solo por la posibilidad de reutilizar código.

