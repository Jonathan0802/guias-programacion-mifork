<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta

El **polimorfismo** es un concepto de la programación orientada a objetos que permite tratar objetos de distintas clases como si fueran del mismo tipo base, normalmente una clase padre. Gracias a ello, una misma referencia puede apuntar a objetos diferentes y estos pueden comportarse de manera distinta ante la misma llamada a un método. Su utilidad principal es aumentar la flexibilidad y reutilización del código, ya que permite escribir programas más genéricos y desacoplados del tipo concreto de los objetos con los que trabajan.

En Java, el polimorfismo se apoya especialmente en la **herencia** y en el uso de referencias del tipo de la clase padre. Cuando un método se invoca a través de una referencia polimórfica, Java decide en tiempo de ejecución qué versión del método se debe ejecutar, en función del tipo real del objeto. Esto permite añadir nuevas clases que hereden de una existente sin necesidad de modificar el código que ya las utiliza.

La **sobreescritura de métodos** consiste en definir en una clase hija un método con la misma firma (mismo nombre y parámetros) que un método heredado de la clase padre, proporcionando una implementación diferente. De este modo, cada clase hija puede adaptar el comportamiento del método a sus propias necesidades. La sobreescritura es esencial para el polimorfismo, ya que es lo que permite que un mismo mensaje (llamada a un método) produzca distintos comportamientos según el objeto que lo recibe.

```java
class Animal {
    void sonido() {
        System.out.println("El animal hace un sonido");
    }
}

class Perro extends Animal {
    @Override
    void sonido() {
        System.out.println("El perro ladra");
    }
}
```


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La **ligadura dinámica** o **enlace tardío** es el mecanismo por el cual la decisión de qué versión de un método se ejecuta no se toma en tiempo de compilación, sino en **tiempo de ejecución**, en función del tipo real del objeto al que apunta una referencia. Es decir, aunque una variable esté declarada como de un tipo base (clase padre), el método que se ejecuta será el correspondiente a la clase concreta del objeto almacenado. Este comportamiento contrasta con el **enlace estático**, donde la decisión se toma durante la compilación y no depende del objeto concreto.

La ligadura dinámica está directamente relacionada con el **polimorfismo**, ya que es el mecanismo que lo hace posible en la práctica. El polimorfismo permite usar una referencia genérica para trabajar con objetos de distintas clases relacionadas por herencia, pero sin la ligadura dinámica todos los métodos llamados se resolverían como si el objeto fuera siempre del tipo de la referencia. Por tanto, la sobreescritura de métodos combinada con la ligadura dinámica permite que una misma llamada produzca comportamientos distintos según el objeto real.

En **C++**, la ligadura dinámica **no es automática**: solo se produce cuando los métodos de la clase base se declaran como `virtual`. Si no se indica explícitamente, el lenguaje utiliza enlace estático por defecto. Esto obliga al programador a decidir de forma consciente qué métodos deben ser polimórficos. En **Java**, en cambio, la ligadura dinámica es el comportamiento por defecto para los métodos de instancia, salvo que se indique lo contrario con palabras clave como `final` o `static`. Por ello, en Java no es necesario marcar explícitamente los métodos para que participen en el polimorfismo.

En **Python**, el enlace dinámico está aún más presente, ya que el lenguaje es dinámico y todo se resuelve en tiempo de ejecución. No existen declaraciones de tipo estrictas como en Java o C++, y las llamadas a métodos dependen únicamente de si el objeto tiene o no un método con ese nombre. De este modo, el polimorfismo y la ligadura dinámica son naturales en Python, incluso sin herencia explícita, lo que se conoce como *duck typing*.



## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

A continuación se muestra un ejemplo sencillo en Java que ilustra el uso del **polimorfismo** mediante herencia y sobreescritura de métodos. Se define una clase base `Soldado` con un método `saludar`, y dos subclases: `Zapador` y `Artillero`. En el caso de `Zapador`, el método `saludar` se **sobreescribe**, sustituyendo completamente el comportamiento heredado de la clase padre.

El polimorfismo se pone en práctica al crear un array de referencias de tipo `Soldado` que contiene objetos de distintas clases hijas. Aunque todas las referencias del array son del tipo `Soldado`, en tiempo de ejecución Java invoca la versión correcta del método `saludar` según la clase real del objeto almacenado en cada posición. Esto demuestra la ligadura dinámica y permite tratar de forma uniforme a objetos diferentes.

Este ejemplo muestra cómo el uso de referencias del tipo base facilita escribir código genérico y extensible. El código que recorre el array no necesita conocer si el objeto es un `Zapador` o un `Artillero`; simplemente llama al método `saludar`, y cada objeto responde de acuerdo con su propia implementación. Así, el polimorfismo permite añadir nuevas clases de soldados sin modificar el código existente que los utiliza.

```java
class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de manera general.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        System.out.println("El zapador saluda mientras prepara explosivos.");
    }
}

class Artillero extends Soldado {
    // No se sobreescribe el método saludar
}

public class PruebaPolimorfismo {
    public static void main(String[] args) {
        Soldado[] soldados = new Soldado[2];
        soldados[0] = new Zapador();
        soldados[1] = new Artillero();

        for (Soldado s : soldados) {
            s.saludar();
        }
    }
}
```



## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Sí, al **sobreescribir un método** es posible invocar el método de la clase base y trabajar a partir de su comportamiento. Esto se utiliza cuando no se desea reemplazar completamente la funcionalidad heredada, sino **extenderla o modificarla ligeramente**. De este modo, la clase hija reutiliza el código de la clase padre y añade su propio comportamiento adicional, evitando duplicaciones y manteniendo una mejor organización del código.

En Java, esta invocación se realiza mediante la palabra clave **`super`**, que permite acceder a los miembros (métodos o atributos) de la clase padre. Al llamar a `super.nombreMetodo()`, se ejecuta explícitamente la versión del método definida en la superclase, incluso aunque haya sido sobreescrito en la subclase. Esta llamada suele colocarse al inicio o al final del método sobrescrito, según se quiera ejecutar primero el comportamiento base o el específico.

En el siguiente ejemplo, la clase `Zapador` no sustituye por completo el saludo del soldado base, sino que primero ejecuta el saludo general y, a continuación, añade un mensaje propio. Esto sigue siendo un caso de sobreescritura, pero con reutilización del comportamiento original. La palabra clave utilizada para invocar al método de la clase base es **`super`**.

```java
class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de manera general.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        super.saludar();
        System.out.println("ZAPADOR A SUS ÓRDENES");
    }
}
```


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

Al **sobreescribir un método** en Java existen varias restricciones importantes relativas a su firma. Los **tipos y el número de parámetros deben ser exactamente los mismos** que en el método de la clase base; de lo contrario, no se considera sobreescritura. En cuanto al **tipo de retorno**, este debe ser el mismo o un subtipo del tipo original, lo que se conoce como *retorno covariante*. Además, no se puede reducir la visibilidad del método (por ejemplo, pasar de `public` a `protected`), y no se pueden lanzar excepciones más generales que las declaradas en el método original.

La **sobreescritura (*overriding*)** y la **sobrecarga (*overloading*)** son conceptos distintos aunque a veces se confundan. La sobreescritura ocurre entre una clase base y una clase hija, y sirve para **modificar el comportamiento heredado**, estando directamente relacionada con el polimorfismo y la ligadura dinámica. La sobrecarga, en cambio, consiste en definir varios métodos con el mismo nombre pero **diferentes parámetros** dentro de una misma clase (o entre una clase y su subclase), y se resuelve en tiempo de compilación. La sobrecarga no es polimórfica en el sentido clásico de la herencia.

La anotación **`@Override`** se utiliza para indicar explícitamente que un método pretende sobreescribir otro definido en una superclase. Su función principal es ayudar al compilador a detectar errores: si la firma no coincide exactamente, el compilador generará un error en lugar de permitir un método nuevo por accidente. Esto evita muchos fallos comunes, como errores tipográficos o cambios en la firma del método base.

El uso de `@Override` es **altamente recomendable** porque mejora la legibilidad del código, deja clara la intención del programador y añade una capa extra de seguridad durante la compilación. Aunque no es obligatorio desde el punto de vista del lenguaje, su uso sistemático se considera una buena práctica en Java y es habitual en código profesional y académico.



## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

Sí, al estudiar Java se empieza a **emplear el polimorfismo desde fases muy tempranas**, aunque al principio no siempre se sea consciente de ello. Desde el momento en que se trabaja con clases que heredan de `Object` (todas las clases en Java), y se redefinen métodos como `toString` o `equals`, ya se está haciendo uso del polimorfismo. Estos métodos están definidos en la clase base `Object` y son invocados habitualmente a través de referencias genéricas, por ejemplo cuando se imprime un objeto o se compara con otro.

Al **sobreescribir `toString`**, se está modificando el comportamiento de un método heredado para que responda de forma específica según la clase concreta del objeto. Cuando una referencia de tipo `Object` apunta a un objeto de una clase propia y se invoca `toString`, Java decide en tiempo de ejecución qué versión del método ejecutar. Ese es exactamente el mecanismo del polimorfismo apoyado en la ligadura dinámica, aunque su uso sea tan cotidiano que pase desapercibido.

Lo mismo ocurre con la **sobreescritura de `equals`**, donde se redefine la forma en la que dos objetos se consideran iguales. Métodos de bibliotecas estándar, como los de colecciones (`contains`, `remove`, etc.), trabajan con referencias de tipo `Object`, pero dependen del comportamiento polimórfico de `equals` para funcionar correctamente con objetos de cualquier clase. Esto demuestra que el polimorfismo no es solo un concepto avanzado, sino una base fundamental del diseño del propio lenguaje.

Por tanto, puede afirmarse que en Java se utiliza el polimorfismo **antes incluso de estudiarlo formalmente**. La sobreescritura de métodos heredados y su invocación a través de referencias del tipo base son prácticas habituales desde los primeros programas, y constituyen ejemplos claros de polimorfismo aplicado de manera natural e integrada en el lenguaje.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta

Una **clase abstracta** es una clase que sirve como modelo o base para otras clases, pero que no está pensada para ser instanciada directamente. Su función principal es definir una estructura común y un comportamiento parcial que deberán compartir sus subclases. Puede contener atributos, métodos ya implementados y también métodos sin implementar. Se utiliza cuando se desea expresar que todos los objetos de un cierto tipo “son un” determinado concepto, pero que ese concepto por sí solo es incompleto.

Un **método abstracto** es un método que se declara sin implementación, es decir, sin cuerpo. Indica que las clases hijas están obligadas a proporcionar su propia versión del método. Al declarar un método como abstracto, se está imponiendo un contrato: cualquier clase concreta que herede de esa clase abstracta deberá implementar dicho método. Como consecuencia, **no es posible crear instancias de una clase abstracta**, ya que tendría métodos sin comportamiento definido.

En Java, la palabra clave **`abstract`** debe colocarse en la declaración de la clase y también en la declaración del método abstracto. En el siguiente ejemplo, `Soldado` se redefine como clase abstracta y se le añade el método abstracto `atacar`. Cada tipo de soldado está obligado a implementar su propia forma de atacar, mientras que el método `saludar` puede seguir teniendo una implementación común heredada.

```java
abstract class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de manera general.");
    }

    abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    void atacar() {
        System.out.println("El zapador coloca y detona explosivos.");
    }
}

class Artillero extends Soldado {
    @Override
    void atacar() {
        System.out.println("El artillero dispara la artillería pesada.");
    }
}
```


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta

La palabra clave **`final`** en Java permite **restringir la herencia y la modificación del comportamiento**. Cuando se aplica a un **método**, indica que dicho método **no puede ser sobreescrito** por las clases hijas. Cuando se aplica a una **clase**, impide que esa clase sea heredada por cualquier otra. De este modo, `final` se utiliza para fijar definitivamente un comportamiento o una estructura que no debe modificarse.

La relación con el **polimorfismo** es directa: un método marcado como `final` **no puede participar en polimorfismo mediante sobreescritura**, ya que no puede redefinirse en las subclases. Al invocar un método `final`, el comportamiento está completamente determinado por la clase donde se define, independientemente del tipo real del objeto. En el caso de una clase `final`, el polimorfismo basado en herencia queda completamente bloqueado, ya que no pueden existir subclases que aporten comportamientos alternativos.

El uso de `final` suele responder a razones de **seguridad, coherencia del diseño o eficiencia**. Impedir la herencia evita usos no previstos de una clase, y declarar métodos como `final` garantiza que ciertas operaciones se comporten siempre de la misma forma. Además, el compilador puede aplicar ciertas optimizaciones cuando sabe que un método no será sobreescrito, aunque este no suele ser el motivo principal de su uso.

Un ejemplo muy conocido en la **API estándar de Java** es la clase **`String`**, que es `final`. Esto significa que no se puede heredar de `String`, garantizando que su comportamiento sea inmutable y seguro en todo el sistema. Otros ejemplos son clases como `Integer`, `Math` o `System`. En todos estos casos, el diseño deliberado como clases `final` evita modificaciones peligrosas y refuerza la fiabilidad del lenguaje y de sus bibliotecas estándar.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

En Java, las **interfaces** son un mecanismo para definir un **contrato** que especifica qué métodos debe proporcionar una clase, sin indicar cómo deben implementarse. Una interfaz declara métodos (y constantes) que representan comportamientos que una clase puede ofrecer, pero no contiene estado ni implementación tradicional. Su objetivo principal es definir capacidades comunes que pueden ser compartidas por clases que no están relacionadas por herencia.

Las interfaces **se parecen a las clases abstractas**, pero no son equivalentes. Mientras que una clase abstracta puede contener atributos, métodos implementados y métodos abstractos, una interfaz se centra únicamente en la definición del comportamiento. Tradicionalmente, todos los métodos de una interfaz eran abstractos y públicos por defecto, aunque en versiones modernas de Java pueden existir métodos `default` y `static`. Aun así, la interfaz sigue representando una especificación más estricta y desacoplada que una clase abstracta.

Una diferencia clave es que **una clase puede implementar más de una interfaz**, algo que no es posible con las clases abstractas, ya que Java solo permite heredar de una única clase. Gracias a esto, las interfaces permiten una forma controlada de *herencia múltiple*, resolviendo uno de los problemas clásicos de la programación orientada a objetos. Una clase puede, por tanto, comprometerse a cumplir varios contratos distintos sin heredar implementación.

Desde el punto de vista del polimorfismo, las interfaces son fundamentales, ya que permiten trabajar con referencias del tipo de la interfaz sin depender de la clase concreta. Cualquier objeto cuya clase implemente la interfaz puede ser tratado de forma uniforme, reforzando la flexibilidad, la extensibilidad y el diseño desacoplado del programa.



## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

Una forma habitual de aplicar polimorfismo es definir un **tipo abstracto común** que represente una idea general y delegar en las subclases el comportamiento concreto. En este caso, `Punto` representa el concepto genérico de punto, pero no se define cómo se calcula la distancia, ya que esa operación depende de la dimensionalidad. Por ello, el método `calcularDistanciaA` se declara como **abstracto**, obligando a cada subtipo a proporcionar su propia implementación.

Cada subclase (`Punto2D` y `Punto3D`) implementa el cálculo de la distancia correspondiente, pero además verifica que el punto recibido sea del **mismo subtipo**. Para ello se emplea `instanceof` y *downcasting*. Este enfoque permite detectar usos incorrectos en tiempo de ejecución, ya que no tendría sentido calcular la distancia entre un punto 2D y uno 3D. Si el tipo no es compatible, se lanza una excepción.

Gracias a este diseño, es posible crear una clase `Linea` que trabaje únicamente con referencias de tipo `Punto`, sin conocer si son 2D o 3D. La clase `Linea` delega completamente el cálculo de la longitud en el método polimórfico `calcularDistanciaA`, obteniendo así un comportamiento correcto sin depender de la dimensionalidad concreta. Esto es un ejemplo claro de **polimorfismo basado en clases abstractas**.

```java
abstract class Punto {
    abstract double calcularDistanciaA(Punto otro);
}
```

```java
class Punto2D extends Punto {
    double x, y;

    Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Punto incompatible");
        }
        Punto2D p = (Punto2D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

```java
class Punto3D extends Punto {
    double x, y, z;

    Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Punto incompatible");
        }
        Punto3D p = (Punto3D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        double dz = this.z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}
```

```java
class Linea {
    private Punto a;
    private Punto b;

    Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    double longitud() {
        return a.calcularDistanciaA(b);
    }
}
```


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta

La **herencia de interfaces** en Java consiste en que una interfaz puede **extender a otra interfaz**, heredando sus métodos y pudiendo añadir nuevos. A diferencia de las clases, las interfaces no heredan implementación (salvo métodos `default`), sino únicamente **obligaciones** que las clases que las implementen deberán cumplir. Este mecanismo permite organizar comportamientos de forma jerárquica y reutilizar definiciones de contratos sin imponer una estructura concreta de clases.

En Java **sí existe herencia múltiple de interfaces**. Una interfaz puede extender **una o varias interfaces a la vez**, separándolas por comas. Esto es posible porque no hay conflicto de estado ni de implementación, ya que las interfaces no contienen atributos de instancia ni constructores. Esta característica permite modelar de forma flexible capacidades transversales, algo que no está permitido con clases, ya que Java no admite herencia múltiple de clases.

A continuación se muestra un ejemplo sencillo. La interfaz `Fichero` define un contrato básico que permite leer el contenido de un fichero como una cadena de texto. Posteriormente, la interfaz `FicheroEscribible` **extiende** a `Fichero` y añade nuevas operaciones relacionadas con la escritura y eliminación. Cualquier clase que implemente `FicheroEscribible` estará obligada a implementar **todos** los métodos definidos en ambas interfaces.

```java
interface Fichero {
    String leerContenido();
}
```

```java
interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}
```

Este diseño permite trabajar de forma polimórfica con referencias del tipo `Fichero` o `FicheroEscribible`, según el nivel de funcionalidad requerido. Además, facilita la evolución del sistema, ya que se pueden crear nuevas interfaces que extiendan a las existentes sin modificar el código ya escrito, reforzando así el desacoplamiento y la reutilización.

