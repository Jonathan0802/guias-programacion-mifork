<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

La encapsulación en POO busca agrupar en una misma entidad (la clase) tanto los datos como las operaciones que se realizan sobre ellos, controlando el acceso desde el exterior. Su objetivo principal es proteger el estado interno del objeto, de manera que solo pueda modificarse mediante métodos definidos por la propia clase. Por su parte, la ocultación de información consiste en impedir que ciertos detalles internos sean visibles o accesibles desde fuera, de modo que otros módulos del programa no dependan de la implementación concreta. Esto permite crear clases donde el funcionamiento interno queda aislado, y solo se expone una interfaz segura y controlada.

Una ventaja importante de ocultar la información es que facilita la seguridad y coherencia de los datos, ya que evita que cualquier parte del código modifique atributos de forma arbitraria. También favorece la modularidad, al permitir que la implementación interna pueda cambiar sin afectar al resto del programa siempre que la interfaz pública permanezca igual. Por último, mejora la legibilidad y mantenibilidad, al obligar a interactuar con los objetos a través de métodos claros y documentados, evitando dependencias innecesarias sobre detalles internos.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La interfaz pública de una clase u objeto se entiende como el conjunto de métodos y atributos que se ponen a disposición del exterior mediante el modificador public. Esta interfaz actúa como el “contrato” que explica cómo puede interactuarse con el objeto sin necesidad de conocer su funcionamiento interno. En otras palabras, la interfaz pública define qué se puede hacer con un objeto, mientras que la implementación interna determina cómo lo hace. Gracias a esto, el usuario de la clase solo necesita conocer los métodos públicos para utilizarla correctamente.

Esta idea se relaciona directamente con la ocultación de información, ya que aquello que no forma parte de la interfaz pública debe quedar protegido mediante niveles de acceso como private o protected. De esta forma, los detalles internos —como atributos o métodos auxiliares— no son visibles desde el exterior, evitando modificaciones indebidas y reduciendo dependencias. El resultado es un diseño más seguro, modular y flexible, donde los cambios internos de la clase no afectan a quienes la utilizan, siempre que la interfaz pública permanezca constante.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

La interfaz pública de una clase debe diseñarse con cuidado porque constituye el conjunto de operaciones mediante las cuales el exterior interactúa con el objeto. Una vez que otros módulos del programa dependen de esos métodos, cualquier cambio puede afectar al funcionamiento del sistema completo. Por ello, la interfaz pública funciona como un contrato estable, que idealmente no debería modificarse con frecuencia. Un diseño poco meditado puede provocar dependencias innecesarias, falta de claridad o incluso permitir usos incorrectos de la clase.

Modificar la interfaz pública no suele ser sencillo, especialmente en proyectos medianos o grandes, porque implica revisar y actualizar todo el código que utilice esa clase. Cambiar un nombre de método, su firma o eliminar una operación puede romper el funcionamiento de múltiples componentes. Por esta razón, se recomienda exponer solo lo esencial y mantener privados los elementos que no sean necesarios desde el exterior, facilitando cambios internos sin repercusiones en el resto del programa.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Las invariantes de clase se entienden como condiciones lógicas que deben cumplirse siempre para que un objeto se considere válido durante toda su vida útil. Estas condiciones suelen referirse al estado interno de la clase, como rangos permitidos para atributos, relaciones entre valores o restricciones que garantizan que el objeto funciona de manera coherente. Las invariantes deben cumplirse después de construir el objeto y tras la ejecución de cualquier método público que pueda modificar el estado, asegurando así consistencia y fiabilidad en el comportamiento.

La ocultación de información ayuda a mantener las invariantes porque impide que el exterior modifique directamente los atributos de la clase. Al obligar a que cualquier cambio pase por métodos controlados, se puede comprobar que las condiciones necesarias se mantienen en todo momento. Esto evita que partes externas del programa introduzcan estados inválidos, ya sea por error o desconocimiento. De este modo, la clase mantiene el control absoluto sobre su propio estado interno, reduciendo posibilidades de fallos y facilitando la depuración y el mantenimiento del código.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

public class Punto {
    // Estado interno oculto (no accesible desde fuera)
    private double x;
    private double y;

    // Interfaz pública: constructores y métodos que definen el "contrato" de uso
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        // Aquí podrían comprobarse invariantes si hubiese restricciones
    }

    // Getters: exponen lectura controlada sin permitir modificación directa
    public double getX() { return x; }
    public double getY() { return y; }

    // Setters (opcional): si se permiten cambios, pasan por puntos de control
    public void setX(double x) { this.x = x; }
    public void setY(double y) { this.y = y; }

    // Método de comportamiento público
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Método estático de utilidad (no depende de una instancia concreta)
    public static double distanciaEntre(Punto a, Punto b) {
        double dx = a.x - b.x;
        double dy = a.y - b.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

En esta clase, la interfaz pública está formada por todo lo declarado como public: el constructor Punto(double, double), los getters (getX, getY), los setters (si se decide exponerlos), el método calcularDistanciaAOrigen y el método estático distanciaEntre. Esa interfaz define qué operaciones pueden realizarse desde fuera con un Punto. En cambio, los atributos x e y permanecen private, de modo que su representación interna queda oculta y solo puede modificarse a través de métodos controlados. Si en el futuro se necesitara validar rangos o mantener invariantes (por ejemplo, restringir valores), bastaría con introducir comprobaciones en constructor y setters sin romper a los usuarios que dependen de la interfaz existente.

Respecto a los modificadores de acceso: public significa que el elemento (clase, método o campo) es visible desde cualquier otro código que pueda acceder al paquete/clase, convirtiéndose en parte de la API de la clase. Por su parte, private restringe el acceso exclusivamente a la propia clase, impidiendo que código externo lea o escriba directamente el estado interno. Esta separación favorece la encapsulación y la ocultación de información: se controla cómo se usa el objeto, se protegen las invariantes y se posibilitan cambios internos sin afectar a quienes utilizan la clase.



## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java, los modificadores public y private pueden aplicarse a distintos elementos del código para controlar su visibilidad, pero no todos admiten los mismos niveles de acceso. En el caso de las clases de nivel superior, únicamente pueden ser public o tener el acceso por defecto (sin palabra clave). Es decir, una clase externa nunca puede ser private, porque debe ser accesible al menos desde su propio paquete. En cambio, las clases internas (dentro de otra clase) sí pueden declararse tanto public como private, ya que forman parte del interior de una clase y sus reglas de visibilidad permiten más control.

Respecto a los atributos, métodos y constructores, los modificadores public y private son totalmente válidos. Declararlos private permite ocultar el estado interno y obligar a que todas las interacciones pasen por la interfaz pública; declararlos public expone sus operaciones para que puedan ser utilizadas desde otras clases. Esta posibilidad permite definir con precisión qué forma parte de la API pública y qué pertenece exclusivamente al funcionamiento interno.

Por último, también es posible aplicar estos modificadores a clases internas estáticas, interfaces internas, así como a métodos estáticos y atributos estáticos. La elección del nivel de acceso influye directamente en la encapsulación, porque determina qué partes de la implementación quedan protegidas frente al exterior. Diseñar correctamente estos accesos permite mantener un código más modular, seguro y fácil de mantener, ya que evita dependencias accidentales sobre detalles internos.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

En POO, además de la visibilidad pública y privada, suelen existir otros niveles intermedios que permiten un control más fino sobre quién puede acceder a los atributos y métodos. Estos niveles adicionales ayudan a modular mejor el código y a decidir qué partes deben estar expuestas, cuáles deben permanecer ocultas y cuáles deben ser accesibles solo desde ciertos contextos. En general, los lenguajes orientados a objetos incorporan mecanismos para equilibrar encapsulación y flexibilidad, permitiendo distintos grados de visibilidad según el diseño requerido.
En Java existen cuatro niveles de visibilidad:

-public: accesible desde cualquier parte.
-package-private (por defecto, sin escribir modificador): accesible únicamente desde clases del mismo paquete.
-protected: accesible desde el mismo paquete y también desde subclases, incluso si están en paquetes distintos.
-private: accesible solo dentro de la misma clase.
-Esta variedad permite estructurar el código en paquetes y jerarquías de herencia, definiendo qué debe ser parte de la API pública y qué debe quedar reservado para extensiones o uso interno.

En otros lenguajes de POO también aparecen niveles similares, aunque con variaciones. Por ejemplo, en C++ existen public, protected y private, pero sin un equivalente directo a package-private, ya que el lenguaje no organiza el código en paquetes, sino en módulos mediante archivos y namespaces. En lenguajes como Python, la encapsulación es principalmente convencional: no existen private o protected estrictos, pero los nombres con guiones bajos (_atributo) se interpretan como elementos internos y la doble barra baja (__atributo) activa name mangling para desalentar accesos externos. Aun así, el mecanismo es menos restrictivo y se basa más en buenas prácticas que en prohibiciones formales del compilador.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

Los miembros de instancia marcados como private están ocultos para otras clases, pero no para otras instancias de la misma clase. En Java, el modificador private restringe el acceso al ámbito de la propia clase: cualquier código escrito dentro de esa clase puede acceder a los campos privados de cualquier objeto de ese mismo tipo, incluido el parámetro otro. Por el contrario, el código escrito en otra clase (aunque esté en el mismo paquete) no puede leer ni escribir esos campos privados directamente.

El siguiente ejemplo ilustra este comportamiento. Aunque x e y sean private, el método calcularDistanciaAPunto(Punto otro) puede acceder a otro.x y otro.y porque se encuentra dentro de la clase Punto. Si se intentara leer p.x desde otra clase externa, el compilador lo prohibiría.

public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Puede acceder a 'otro.x' y 'otro.y' porque el código está dentro de la clase Punto
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;  // acceso válido: mismo tipo y misma clase
        double dy = this.y - otro.y;  // acceso válido: mismo tipo y misma clase
        return Math.sqrt(dx * dx + dy * dy);
    }
}

En resumen: la ocultación con private opera a nivel de clase, no a nivel de instancia. Por ello, (a) sí están ocultos para otras clases; (b) no están ocultos para otras instancias cuando el acceso ocurre dentro del propio código de la clase. Esto permite implementar métodos que comparan o combinan estados de dos objetos del mismo tipo sin romper la encapsulación frente al exterior.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

Los métodos getter y setter son funciones que permiten acceder y modificar atributos privados de una clase de forma controlada. En lugar de exponer directamente los campos internos, se utilizan estos métodos para mantener la encapsulación y garantizar que cualquier lectura o cambio de datos pase por un punto de control definido por la propia clase. De este modo, el estado interno permanece oculto, pero sigue siendo accesible mediante operaciones seguras.

Un getter es un método cuyo propósito es devolver el valor de un atributo privado. Suele nombrarse siguiendo la convención getAtributo(), y permite consultar el estado sin otorgar capacidad de modificación. En cambio, un setter es un método utilizado para establecer o modificar el valor de un atributo, normalmente siguiendo la convención setAtributo(valor). Estos métodos permiten introducir comprobaciones o restricciones, como validar rangos o mantener invariantes de clase antes de aceptar un cambio.

Su uso es especialmente útil cuando se desea controlar la coherencia de los datos. Por ejemplo, si un atributo no puede tomar valores negativos, el setter puede rechazar entradas inválidas y así proteger al objeto. Con este patrón, la interfaz pública se mantiene clara y segura, y la implementación interna puede evolucionar sin afectar al código que utiliza la clase.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

No, cuando en POO se afirma que la ocultación de información mejora la “seguridad”, no se hace referencia a evitar ataques externos ni a impedir que el programa sea “hackeado”. En este contexto, el término se utiliza de forma interna al diseño del software, y describe la capacidad de un programa para evitar estados inválidos, errores lógicos y usos incorrectos de los objetos por parte de otros módulos del propio programa. Es una seguridad de tipo lógico y estructural, no una seguridad informática relacionada con amenazas externas o vulnerabilidades.

La ocultación de información mejora esta seguridad interna porque impide que el código exterior modifique directamente los atributos, lo que evita daños accidentales al estado del objeto. Al forzar que todos los cambios pasen por métodos controlados, es posible validar valores, mantener invariantes de clase y asegurar que el objeto se utilice siempre de la forma correcta. Esto reduce la aparición de errores difíciles de rastrear y favorece que la clase conserve coherencia durante toda su vida útil.
En resumen, la “seguridad” aportada por la encapsulación se orienta a diseñar sistemas más robustos, predecibles y fáciles de mantener, protegiéndolos de fallos internos causados por el propio desarrollador o por otros componentes del software. No tiene relación con la seguridad frente a ataques externos, que pertenece a otras áreas como la ciberseguridad, el control de accesos o la defensa contra vulnerabilidades.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

Un **miembro de instancia** es un atributo o método cuyo valor pertenece a **cada objeto individual** creado a partir de una clase. Cada instancia mantiene su propia copia de estos miembros, por lo que modificar el valor en un objeto no afecta al resto. Por ejemplo, en una clase `Punto`, los atributos `x` e `y` suelen ser miembros de instancia: cada punto tiene sus propias coordenadas, independientes de los demás. Su ciclo de vida está asociado al objeto concreto, de manera que existen mientras la instancia exista.

En cambio, un **miembro de clase** (declarado con la palabra clave `static`) es compartido por **todas las instancias** de la clase. Solo existe una única copia del miembro, independientemente del número de objetos creados. Esto se utiliza para valores comunes, contadores, constantes o métodos que no dependen del estado individual de un objeto. Por ejemplo, un método `distanciaEntre(Punto a, Punto b)` puede ser estático porque su cálculo no requiere acceder a datos de un objeto particular, sino únicamente a los parámetros recibidos.

Respecto a la ocultación, los **miembros de clase también pueden ocultarse** aplicando los mismos modificadores de acceso que a los miembros de instancia, como `private`, `protected` o `public`. Un miembro de clase `private` será accesible únicamente desde el código de la propia clase, incluso si es estático. Esto permite encapsular detalles globales que deben permanecer protegidos, como contadores internos o utilidades que no deberían formar parte de la interfaz pública. Por tanto, el hecho de ser `static` no elimina la posibilidad de aplicar encapsulación: el control de visibilidad funciona del mismo modo.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Sí, tiene sentido que los **constructores sean privados**, aunque solo en situaciones específicas del diseño. Un constructor privado impide crear objetos desde fuera de la propia clase, lo que permite controlar estrictamente *cómo* y *cuándo* se crean las instancias. Esto resulta útil cuando no se desea que cualquiera pueda generar objetos libremente, ya sea para mantener invariantes, limitar el número de instancias o evitar configuraciones incorrectas.

Un caso típico es el **patrón Singleton**, donde solo puede existir una única instancia de una clase. Al declarar el constructor como privado, se obliga a que la instancia se obtenga mediante un método público controlado. También es habitual usar constructores privados en clases que solo ofrecen **métodos estáticos** (por ejemplo, clases de utilidades), evitando así creaciones innecesarias de objetos. En otras situaciones, los constructores privados pueden combinarse con **métodos fábrica** para ofrecer formas más flexibles de construcción, sin exponer cómo se crean internamente las instancias.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

En Java, los **miembros de clase** se indican utilizando la palabra clave `static`. Esto significa que el atributo o método pertenece a la **clase en sí**, no a cada objeto individual. Por ello, todos los objetos comparten el mismo valor, y se puede acceder a ellos incluso sin crear instancias. Esta característica permite almacenar información común, como contadores o valores globales relacionados con todas las instancias de la clase. En este caso, resulta útil para registrar los valores máximos de `x` e `y` establecidos en todos los puntos creados.

Para lograr esto en la clase `Punto`, pueden añadirse dos atributos estáticos y privados que almacenen los máximos globales, junto con métodos que actualicen esos valores cada vez que se asignan nuevas coordenadas. Los constructores y los *setters* serían los encargados de modificar esos máximos, ya que son los puntos donde cambian las coordenadas internas del objeto. De esta manera, se mantiene la encapsulación: los máximos existen a nivel de clase, pero solo pueden modificarse mediante la lógica interna definida por la propia clase.

Aquí tienes un ejemplo integrado en la clase `Punto`:

```java
public class Punto {
    // Miembros de instancia (propios de cada objeto)
    private double x;
    private double y;

    // Miembros de clase (compartidos por todos los objetos)
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximos(x, y);
    }

    public double getX() { return x; }
    public double getY() { return y; }

    public void setX(double x) {
        this.x = x;
        actualizarMaximos(x, this.y);
    }

    public void setY(double y) {
        this.y = y;
        actualizarMaximos(this.x, y);
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    private static void actualizarMaximos(double x, double y) {
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    // Getters de clase para acceder a los máximos
    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
}
```

En este ejemplo, `maxX` y `maxY` son **miembros de clase**, por lo que su valor es compartido por todas las instancias de `Punto`. Cada vez que se crea o modifica un punto, los métodos internos actualizan estos valores. Gracias al uso de `private` se mantiene la ocultación de información y se asegura que solo la propia clase controla cómo y cuándo cambian dichos máximos. Esto ilustra bien cómo los miembros de clase pueden usarse junto a la encapsulación de manera coherente.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta

Un **método factoría** suele declararse `static` para poder crear instancias **sin** requerir un objeto previo. En este caso, tiene sentido que la factoría reciba `double` y redondee cada coordenada al entero más cercano antes de construir el `Punto`. Se puede utilizar `Math.round(...)`, que devuelve `long`; luego se convierte a `double` para llamar al constructor existente.

```java
public static Punto desdeCoordenadasRedondeadas(double x, double y) {
    double rx = (double) Math.round(x);
    double ry = (double) Math.round(y);
    return new Punto(rx, ry);
}
```

Sí, se ha utilizado `static` porque el propósito es **proveer una forma alternativa de construcción** directamente desde la clase, manteniendo la encapsulación del proceso de redondeo. Si se desea un comportamiento de redondeo distinto (por ejemplo, *ties-to-even*), puede sustituirse `Math.round` por `Math.rint`, que devuelve un `double` ya enterizado según esa regla.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

Para cambiar la **representación interna** sin alterar la **interfaz pública**, puede sustituirse el par de `double` por un array privado de dos posiciones. La idea es mantener exactamente los mismos **constructores** y **métodos públicos** (`getX`, `getY`, `setX`, `setY`, `calcularDistanciaAOrigen`, `calcularDistanciaAPunto`, y la utilidad estática `distanciaEntre`) para que ningún código cliente deba modificarse. Se refuerza así la encapsulación: el exterior sigue viendo el mismo “contrato”, pero la implementación interna puede evolucionar libremente.

Para evitar “números mágicos”, conviene definir **constantes de índice** (`IDX_X`, `IDX_Y`) y centralizar el almacenamiento en un único campo `double[]`. No se expone el array al exterior (no hay *getters* del array ni referencias directas), con lo que se preserva la ocultación de información. Si en el futuro se quisiera cambiar de nuevo la representación (por ejemplo, a una clase `Vector2D`), bastaría con adaptar esta implementación manteniendo la interfaz pública.

```java
public class Punto {
    // Representación interna cambiada a array
    private static final int IDX_X = 0;
    private static final int IDX_Y = 1;
    private final double[] coords;

    // Interfaz pública conservada
    public Punto(double x, double y) {
        this.coords = new double[2];
        this.coords[IDX_X] = x;
        this.coords[IDX_Y] = y;
    }

    public double getX() { return coords[IDX_X]; }
    public double getY() { return coords[IDX_Y]; }

    public void setX(double x) { coords[IDX_X] = x; }
    public void setY(double y) { coords[IDX_Y] = y; }

    public double calcularDistanciaAOrigen() {
        double x = coords[IDX_X], y = coords[IDX_Y];
        return Math.sqrt(x * x + y * y);
    }

    // Método que accede a los privados de otra instancia de la misma clase (válido en Java)
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coords[IDX_X] - otro.coords[IDX_X];
        double dy = this.coords[IDX_Y] - otro.coords[IDX_Y];
        return Math.sqrt(dx * dx + dy * dy);
    }

    // Semejante a la versión previa, sin depender de la representación externa
    public static double distanciaEntre(Punto a, Punto b) {
        double dx = a.coords[IDX_X] - b.coords[IDX_X];
        double dy = a.coords[IDX_Y] - b.coords[IDX_Y];
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

Declarar un atributo público no es equivalente a ofrecer un *getter* y un *setter* públicos. Cuando un atributo es público, cualquier clase puede leerlo y modificarlo directamente, sin ningún tipo de control. En cambio, los métodos *getter* y *setter* permiten mantener la **encapsulación**, ya que todos los accesos pasan por puntos de control definidos por la propia clase. Esto permite añadir validaciones, mantener invariantes, registrar cambios o modificar la implementación interna sin afectar al código que usa la clase. Por tanto, incluso si ambos métodos son públicos, mantener el atributo como `private` sigue siendo ventajoso.

La convención más extendida en lenguajes orientados a objetos como Java es que **los atributos deben ser privados**. Esta práctica se conoce como *encapsulation-first* y forma parte del diseño orientado a objetos desde sus orígenes. La idea es que la clase exponga únicamente la interfaz necesaria mediante métodos públicos, mientras que sus detalles internos permanezcan ocultos. Incluso en casos donde no se añada lógica adicional en los *setters* o *getters*, preservar la privacidad permite introducir dicha lógica en el futuro sin romper a los usuarios de la clase.

Además, esta decisión está directamente relacionada con las **invariantes de clase**, es decir, las condiciones lógicas que deben cumplirse para que un objeto sea válido en todo momento. Si los atributos fueran públicos, cualquier parte del programa podría modificarlos libremente, lo que haría imposible garantizar que las invariantes se cumplan permanentemente. En cambio, mantener los atributos como privados y exigir que todas las modificaciones pasen por métodos específicos permite verificar, corregir o rechazar valores que violen dichas condiciones, manteniendo la coherencia del objeto.

En conjunto, evitar atributos públicos y optar por *getters*/*setters* privados protege la estructura interna, permite evolucionar la clase sin impactos externos y asegura el mantenimiento de las invariantes. Por ello, es considerada una práctica esencial en un buen diseño orientado a objetos.



## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase **inmutable** es aquella cuyo estado **no puede cambiar** una vez creada la instancia. Esto implica que todos sus atributos deben establecerse en el constructor y no modificarse después. Para lograrlo, los atributos suelen declararse `private` y `final`, y no se proporcionan métodos que alteren su valor. En una clase inmutable, cualquier operación que conceptualmente “modifique” un objeto devuelve **una nueva instancia**, dejando la original intacta. Este diseño evita que existan varios puntos del programa alterando el mismo objeto de forma inesperada.

Un **método modificador** es cualquier método que **cambia el estado interno del objeto**. No importa si su nombre incluye “set” o no: si altera atributos, se considera modificador. Por ello, aunque muchos métodos modificadores son *setters*, no todos los modificadores tienen por qué serlo. Por ejemplo, un método `mover(double dx, double dy)` que actualice `x` e `y` en un `Punto` sería un método modificador, aunque no sea un setter tradicional. La clave está en si afecta al estado interno, no en la nomenclatura empleada.

Las clases inmutables tienen ventajas importantes. Una de ellas es que **el estado siempre es coherente**, ya que no puede alterarse después de la construcción, lo que elimina muchos errores asociados a estados cambiantes. También facilitan la depuración y el razonamiento, porque los objetos se comportan como valores constantes: si dos módulos comparten una misma instancia, ninguno puede provocar efectos secundarios en el otro. Además, las clases inmutables son **seguras en entornos concurrentes** sin necesidad de sincronización adicional, pues no existe riesgo de que dos hilos modifiquen simultáneamente el mismo objeto. Este conjunto de propiedades hace que la inmutabilidad sea una estrategia muy valorada en el diseño robusto de software orientado a objetos.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No, no es recomendable incluir métodos *setter* siempre ni como una simple convención. Su presencia debe justificarse por las **necesidades reales del diseño**, no por costumbre. Incluir *setters* de forma automática tiende a debilitar la encapsulación, ya que convierte en modificable cualquier atributo, incluso aquellos que deberían permanecer estables para mantener la coherencia del objeto. Por este motivo, un *setter* solo debe aparecer cuando realmente sea necesario permitir la modificación de un valor desde fuera de la clase.

Además, la existencia de *setters* indiscriminados puede comprometer las **invariantes de clase**. Si otros módulos pueden cambiar libremente atributos, se vuelve más difícil garantizar que el objeto se mantenga siempre en un estado válido. En muchos casos, es preferible diseñar métodos más seguros y específicos que expresen mejor la intención, en lugar de exponer directamente el cambio de un atributo. Por ejemplo, un método `mover(dx, dy)` en vez de un *setter* genérico para `x` o `y`, porque encapsula la lógica necesaria para mantener coherencia.

La tendencia moderna, especialmente en diseño orientado a objetos y en lenguajes influidos por prácticas funcionales, favorece limitar al máximo el número de *setters* e incluso optar por clases **inmutables** cuando sea posible. Esto contribuye a reducir errores, evitar efectos secundarios y permitir un diseño más claro y robusto. Por tanto, los *setters* deben añadirse con moderación, justificadamente y siempre considerando el impacto en la encapsulación y en las invariantes del objeto.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

La clase `String` en Java es **inmutable**, lo que significa que una vez creada una cadena, su contenido no puede modificarse. Cada operación que conceptualmente “cambia” una cadena, como concatenar, reemplazar o recortar, en realidad crea **un nuevo objeto `String`**. Esta decisión de diseño aporta ventajas como simplicidad, seguridad y facilidad para usar cadenas como claves en estructuras de datos, pero también implica costes cuando se realizan muchas modificaciones sucesivas.

Cuando se concatenan dos cadenas mediante `+` o `concat`, no se modifica ninguna de las cadenas originales. En su lugar, se crea una **nueva instancia de `String`** que contiene el resultado. Por ejemplo, la expresión `"Hola" + " Mundo"` genera internamente otro objeto con el valor `"Hola Mundo"`. Si se repite este proceso muchas veces en un bucle, se crean múltiples objetos intermedios, lo cual resulta ineficiente tanto en tiempo como en memoria debido a la generación innecesaria de basura (*garbage*).

Si se necesita construir una cadena de forma incremental con muchas concatenaciones, la práctica recomendada es utilizar **`StringBuilder`** (o `StringBuffer` en contextos con múltiples hilos). Estas clases son **mutables**, lo que significa que pueden modificar su contenido sin crear nuevos objetos en cada operación. Al final del proceso, se convierte el resultado en un `String` mediante `toString()`. Con esto se obtiene un diseño eficiente, claro y coherente con la inmutabilidad de `String`.

En resumen, `String` es inmutable; concatenar crea nuevos objetos; y cuando se anticipan muchas concatenaciones, la opción adecuada es `StringBuilder`, lo que evita pérdidas de rendimiento y reduce la presión sobre el recolector de basura.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

En POO, la comparación entre objetos puede hacerse atendiendo a dos criterios distintos: **identidad** o **contenido**. La **identidad** se refiere a si dos referencias apuntan al **mismo objeto en memoria**, mientras que la comparación por **contenido** analiza si los valores internos de ambos objetos son equivalentes. El operador `==` en Java compara **identidad**, por lo que solo devuelve `true` cuando ambas referencias señalan exactamente al mismo objeto. Esto no suele ser lo deseado cuando se trabaja con clases que representan valores, ya que interesa saber si “son iguales” según su información interna, no si son físicamente el mismo objeto.

El método `equals` en Java existe precisamente para comparar objetos según su **contenido lógico**. Este método se hereda de `Object`, donde por defecto (si no se sobrescribe) compara **identidad**, es decir, actúa igual que `==`. Para que dos objetos se consideren iguales por su contenido, las clases deben redefinir `equals` implementando una comparación adecuada de sus atributos relevantes. Esta redefinición suele ir acompañada de la sobrescritura de `hashCode`, para mantener las reglas de funcionamiento de colecciones como `HashSet` o `HashMap`. Así, `equals` se convierte en la herramienta estándar para determinar igualdad semántica entre objetos.

En el caso concreto de las cadenas, Java implementa una sobrescritura propia del método `equals` en la clase `String`. Gracias a ello, dos cadenas pueden compararse por su **contenido textual**, independientemente de si son el mismo objeto en memoria. Por ejemplo, `"hola".equals(new String("hola"))` devuelve `true`, mientras que `"hola" == new String("hola")` devuelve `false`. Por esta razón, la forma correcta de comparar dos cadenas en Java es utilizando siempre `equals` (o `equalsIgnoreCase` si se desea ignorar mayúsculas y minúsculas) y no el operador `==`, que solo verifica identidad. En resumen, `equals` permite comparar objetos por su contenido, y su adecuada implementación es esencial para un diseño orientado a objetos coherente.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

Las **clases *wrapper*** son clases que envuelven (*to wrap*) un tipo primitivo para tratarlo como un **objeto** dentro de un lenguaje orientado a objetos. En Java, por ejemplo, existen wrappers para cada tipo primitivo: `Integer` para `int`, `Double` para `double`, `Character` para `char`, etc. El propósito principal es permitir que valores primitivos puedan usarse allí donde se requieren objetos, como en colecciones genéricas (`ArrayList<Integer>`, por ejemplo) o en ciertos contextos donde se necesita polimorfismo o métodos asociados al valor. La idea es encapsular el valor primitivo dentro de un objeto que proporciona métodos y un conjunto de comportamientos adicionales.

En Java, la conversión entre primitivo y wrapper puede hacerse de forma explícita, pero también existe un proceso automático llamado **autoboxing** y **unboxing**. El *autoboxing* convierte un primitivo en su wrapper correspondiente al asignarlo o pasarlo como parámetro donde se espera un objeto; el *unboxing* hace la operación inversa cuando se necesita un valor primitivo. Ambos mecanismos simplifican el código y permiten trabajar con tipos primitivos y objetos de forma más natural, sin necesidad de crear instancias manualmente. Aunque el proceso sea automático, sigue existiendo un coste asociado a la creación de objetos, por lo que no debe abusarse de él cuando se busque eficiencia.

Las clases wrapper aportan ventajas claras, como la posibilidad de almacenar valores primitivos en colecciones genéricas, el acceso a métodos útiles (por ejemplo, `Integer.parseInt(...)`), o el uso de valores *null* para representar ausencia de dato, algo que los primitivos no permiten. Además, los métodos de comparación, conversión y formateo proporcionan utilidades que simplifican muchas operaciones comunes. Por tanto, los wrappers actúan como un puente entre el mundo de los tipos básicos y el modelo totalmente orientado a objetos.

No todos los lenguajes orientados a objetos necesitan *wrappers*. Algunos lenguajes, especialmente los puramente orientados a objetos como **Smalltalk**, tratan absolutamente todo como un objeto, por lo que no existe la distinción entre primitivos y objetos. Otros lenguajes modernos, como **Python**, también gestionan los números, cadenas y booleanos como objetos desde el inicio, lo que elimina la necesidad de un sistema de envoltorio específico. Sin embargo, en lenguajes que distinguen primitivos por razones de eficiencia —como Java o C#— las clases wrapper siguen siendo necesarias para lograr integración plena con el modelo de objetos.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un **tipo de dato enumerado** en POO es un conjunto finito y cerrado de valores posibles, representados como identificadores con significado propio. Estos valores suelen utilizarse cuando una variable solo puede asumir un número limitado de opciones coherentes, como los días de la semana, colores predefinidos o estados de un proceso. Su finalidad es mejorar la claridad y fiabilidad del código, evitando el uso de números o cadenas sueltos que podrían inducir errores o permitir valores inválidos. Con un enumerado, el conjunto de valores queda perfectamente delimitado y controlado por el propio lenguaje o la clase.

En Java, un tipo enumerado —declarado con la palabra clave `enum`— es realmente una **clase especial**, generada automáticamente por el compilador. Internamente, cada valor del `enum` es una instancia única y final de esa clase. Esto permite que los enumerados tengan **métodos**, **atributos**, **constructores privados** y una interfaz pública controlada, igual que cualquier otra clase. Aunque su sintaxis sea más compacta, su comportamiento se basa en un modelo clásico de objetos, lo que facilita integrar lógica asociada directamente a cada valor del enumerado.

Desde el punto de vista de la **encapsulación**, los enumerados en Java presentan ventajas importantes. En primer lugar, impiden que se creen valores fuera de los definidos explícitamente, garantizando así la consistencia del dominio de valores. Además, pueden incluir métodos que encapsulen comportamientos específicos, evitando que el resto del programa tenga que usar estructuras condicionales repetitivas. También controlan su propia visibilidad: los valores no pueden modificarse ni ampliarse desde el exterior, lo que protege las invariantes del modelo. En conjunto, los `enum` permiten escribir código más seguro, expresivo y resistente a errores.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

En Java, un `enum` puede diseñarse como una **clase especial** con **atributos privados** y **constructor privado** para encapsular datos y exponer solo la interfaz necesaria. A continuación se define `Mes` con sus **doce instancias**, un atributo con el **número de días** (no bisiesto) y otro con el **ordinal (1–12)**, además de métodos para consultar si el mes pertenece a **primavera, verano, otoño o invierno** según el **hemisferio** indicado. Para los meses/estaciones se adopta el criterio **meteorológico** (meses completos): en el hemisferio norte, primavera = marzo–mayo, verano = junio–agosto, otoño = septiembre–noviembre e invierno = diciembre–febrero; en el hemisferio sur se invierten las estaciones.

Si se necesita contemplar años bisiestos, podría añadirse una sobrecarga como `getDias(boolean esBisiesto)` que devuelva 29 para **FEBRERO** cuando corresponda, manteniendo la versión simple `getDias()` para el resto de casos. En cuanto a encapsulación, los atributos se mantienen `private final` y los **únicos puntos de acceso** son los métodos públicos solicitados, de modo que la representación interna queda protegida y el “contrato” de uso es estable.

```java
public enum Mes {
    ENERO(1, 31),
    FEBRERO(2, 28),
    MARZO(3, 31),
    ABRIL(4, 30),
    MAYO(5, 31),
    JUNIO(6, 30),
    JULIO(7, 31),
    AGOSTO(8, 31),
    SEPTIEMBRE(9, 30),
    OCTUBRE(10, 31),
    NOVIEMBRE(11, 30),
    DICIEMBRE(12, 31);

    // Atributos privados (encapsulación)
    private final int numeroMes;   // 1..12
    private final int dias;        // Días en año no bisiesto

    // Constructor privado del enum
    Mes(int numeroMes, int dias) {
        this.numeroMes = numeroMes;
        this.dias = dias;
    }

    // Ordinal del mes en el año (1..12)
    public int getOrdinal() {
        return numeroMes;
    }

    // Número de días (no bisiesto)
    public int getDias() {
        return dias;
    }

    // --- Estaciones (criterio meteorológico: meses completos) ---
    public boolean esDePrimavera(boolean esHemisferioNorte) {
        return esHemisferioNorte
                ? (numeroMes >= 3 && numeroMes <= 5)   // Mar, Abr, May
                : (numeroMes >= 9 && numeroMes <= 11); // Sep, Oct, Nov
    }

    public boolean esDeVerano(boolean esHemisferioNorte) {
        return esHemisferioNorte
                ? (numeroMes >= 6 && numeroMes <= 8)    // Jun, Jul, Ago
                : (numeroMes == 12 || numeroMes <= 2);  // Dic, Ene, Feb
    }

    public boolean esDeOtoño(boolean esHemisferioNorte) {
        return esHemisferioNorte
                ? (numeroMes >= 9 && numeroMes <= 11)   // Sep, Oct, Nov
                : (numeroMes >= 3 && numeroMes <= 5);   // Mar, Abr, May
    }

    public boolean esDeInvierno(boolean esHemisferioNorte) {
        return esHemisferioNorte
                ? (numeroMes == 12 || numeroMes <= 2)   // Dic, Ene, Feb
                : (numeroMes >= 6 && numeroMes <= 8);   // Jun, Jul, Ago
    }

    // (Opcional) Manejo de bisiestos:
    // public int getDias(boolean esBisiesto) {
    //     if (this == FEBRERO && esBisiesto) return 29;
    //     return dias;
    // }
}
```

Con esta implementación, el tipo enumerado encapsula **datos y comportamiento** coherente con el dominio, expone una **interfaz clara** y permite razonar fácilmente sobre reglas de negocio (días por mes y pertenencia estacional) sin perder las ventajas de inmutabilidad y seguridad típicas de los `enum` de Java.

