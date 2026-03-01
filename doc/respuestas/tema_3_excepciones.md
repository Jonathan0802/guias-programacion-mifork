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

A falta de excepciones en C, una forma clásica de señalar errores consiste en devolver **un código de estado** y entregar el valor correcto mediante **parámetro de salida**. Así, la función no imprime nada ni decide cómo informar al usuario; simplemente comunica si hubo error (por ejemplo, `0` = OK, `-1` = argumento inválido). El llamador comprueba el código y, *desde fuera de `raiz`*, decide cómo notificar. Esta opción es simple, portable y explícita.

```c
#include <stdio.h>
#include <math.h>

/* Devuelve 0 si OK, -1 si x < 0. El resultado se escribe en *out */
int raiz(double x, double *out) {
    if (x < 0.0) {
        return -1; // error: dominio inválido
    }
    *out = sqrt(x);
    return 0;
}

int main(void) {
    double x = -9.0, r;
    int status = raiz(x, &r);
    if (status == 0) {
        printf("raiz(%.2f) = %.4f\n", x, r);
    } else {
        // La función no imprime; se informa aquí (fuera de raiz)
        fprintf(stderr, "Error: la raíz cuadrada no está definida para x < 0 (x=%.2f)\n", x);
    }
    return 0;
}
```

Otra alternativa habitual en C es emplear **valores sentinela** junto con **`errno`** (definido en `<errno.h>`). La función devuelve un `double` (el resultado) y, en caso de error de dominio, devuelve `NAN` y establece `errno` a un código apropiado (por ejemplo, `EDOM`). El llamador comprueba `errno` (preferiblemente reini­ciándolo antes de la llamada) o verifica `isnan(...)` para detectar el fallo y, de nuevo, informa *desde fuera* de la función. Esta aproximación imita el estilo de la biblioteca estándar para errores de dominio matemático.

```c
#include <stdio.h>
#include <math.h>
#include <errno.h>
#include <fenv.h> // opcional si se quieren flags de excepciones de coma flotante

double raiz(double x) {
    if (x < 0.0) {
        errno = EDOM;     // dominio matemático inválido
        return NAN;       // valor sentinela para señalar fallo en double
    }
    errno = 0;            // (opcional) no estrictamente necesario si el llamador lo limpia
    return sqrt(x);
}

int main(void) {
    errno = 0;            // limpiar antes de la llamada
    double x = -16.0;
    double r = raiz(x);

    if (errno == 0 && !isnan(r)) {
        printf("raiz(%.2f) = %.4f\n", x, r);
    } else {
        // La función no imprime; se informa aquí (fuera de raiz)
        if (errno == EDOM) {
            fprintf(stderr, "Error: dominio inválido (x=%.2f). No existe raíz real.\n", x);
        } else {
            fprintf(stderr, "Error: operación fallida para x=%.2f\n", x);
        }
    }
    return 0;
}
```

Ambos enfoques mantienen la responsabilidad de **informar al usuario en el llamador**, tal y como se pide. La primera opción (código de estado + parámetro de salida) evita ambigüedades con valores especiales y es fácil de testear; la segunda (retorno `double` + `errno` y/o `NAN`) encaja bien cuando se desea una interfaz más parecida a la de funciones matemáticas estándar y permite encadenar llamadas en expresiones, a costa de requerir disciplina al comprobar `errno` o `isnan`. En proyectos mayores también es común generalizar con **enumeraciones de error** o **estructuras** que empaquetan `{status, value}`.



## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una **excepción** es un mecanismo de control de flujo que permite señalar y manejar condiciones anómalas (errores o situaciones excepcionales) separando el **código normal** del **código de gestión de errores**. En lugar de devolver códigos de error como en C, una función “lanza” (throw) una señal que interrumpe el flujo habitual y busca un manejador adecuado (catch) más arriba en la pila de llamadas. Esto posibilita que el código que detecta el problema no tenga que decidir cómo informarlo ni resolverlo en el mismo punto.

El objetivo al **implementar** funciones es expresar de forma clara cuándo una precondición falla o aparece una condición imposible de resolver localmente (p. ej., argumento inválido, recurso no disponible), confiando en que el llamador o una capa superior adopte la política de recuperación. En Java, además, las excepciones fomentan contratos más explícitos: las **checked exceptions** obligan al desarrollador a declarar y/o manejar ciertos fallos previsibles, mientras que las **unchecked** (subclases de `RuntimeException`) se reservan para errores de programación o situaciones no recuperables donde no tiene sentido forzar manejo en cada llamada.

Al **llamar** funciones, el objetivo es centralizar el manejo de errores donde se tiene **contexto** para reaccionar (volver a pedir datos, registrar y continuar, abortar una operación, liberar recursos, etc.). Así, el código que usa la función se enfoca en el caso “feliz” y delega la respuesta a fallos a bloques `try/catch/finally`, lo que reduce el “ruido” de comprobaciones repetitivas como sucede con los códigos de retorno en C. En conjunto, las excepciones proporcionan propagación automática, separación de responsabilidades y claridad en la intención, a costa de requerir diseño cuidadoso para no usarlas como control de flujo ordinario.



## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

Una forma idiomática en Java consiste en validar el argumento y **lanzar una excepción** si no se cumple la precondición (número negativo). De este modo, el método `raiz` se concentra en su responsabilidad (calcular la raíz cuando procede) y delega la política de manejo de errores al código **externo** que lo invoque. En este ejemplo se utiliza `IllegalArgumentException` (no comprobada) porque un argumento negativo suele considerarse **error de programación** del llamador; alternativamente, podría definirse una excepción comprobada si se quisiera forzar el manejo en cada llamada.

A la hora de **controlar desde fuera**, el `main` envuelve la llamada en un bloque `try/catch` y decide cómo informar al usuario (mostrar un mensaje, registrar el error, volver a pedir datos, etc.). El bloque `finally` se puede usar para liberar recursos o mostrar mensajes finales si hiciera falta (aquí no es necesario, pero se incluye como recordatorio del patrón). El ejemplo también muestra un caso correcto y otro que dispara la excepción.

```java
public class Calculadora {

    /**
     * Calcula la raíz cuadrada de x si x >= 0.
     * @param x valor de entrada
     * @return sqrt(x)
     * @throws IllegalArgumentException si x < 0
     */
    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException(
                "La raíz cuadrada no está definida para números negativos: x=" + x
            );
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        double a = 9.0;
        double b = -16.0;

        // Caso correcto
        try {
            double r1 = Calculadora.raiz(a);
            System.out.println("raiz(" + a + ") = " + r1);
        } catch (IllegalArgumentException e) {
            // El método no informa: se informa desde aquí (fuera de raiz)
            System.err.println("Error con a: " + e.getMessage());
        }

        // Caso que provoca error
        try {
            double r2 = Calculadora.raiz(b);
            System.out.println("raiz(" + b + ") = " + r2);
        } catch (IllegalArgumentException e) {
            // Control del error desde fuera del método
            System.err.println("Error con b: " + e.getMessage());
        } finally {
            // (Opcional) Acciones de cierre/limpieza si fueran necesarias
            // System.out.println("Fin de la operación.");
        }
    }
}
```

Si se prefiriera **forzar** al llamador a manejar la situación (similar a declarar un contrato explícito), podría definirse una excepción comprobada (por ejemplo, `class RaizNegativaException extends Exception`) y declarar `throws RaizNegativaException` en `raiz`. Eso obligaría a capturarla o propagarla. En cambio, con `IllegalArgumentException` se indica que el error es de uso del método y que no siempre merece manejo local en cada sitio, manteniendo el código de caso “feliz” más limpio y el manejo centralizado donde exista contexto para reaccionar.



## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

**Lanzar** una excepción consiste en interrumpir el flujo normal de ejecución en el punto donde se detecta una condición anómala, creando un objeto de tipo excepción y “arrojándolo” hacia el entorno de ejecución (`throw`). En el ejemplo, `Calculadora.raiz(double x)` valida `x` y, si es negativo, **lanza** `IllegalArgumentException`. La función no decide cómo informar ni cómo recuperarse; solo declara el incumplimiento de la precondición. Ejemplo:

```java
public static double raiz(double x) {
    if (x < 0) throw new IllegalArgumentException("x negativo: " + x);
    return Math.sqrt(x);
}
```

**Controlar** o **capturar** una excepción significa rodear la llamada potencialmente fallida con un bloque `try` y uno o varios `catch` que indiquen cómo reaccionar si ocurre ese tipo de error. El código en `catch` se ejecuta únicamente cuando se ha lanzado una excepción compatible con el tipo capturado; si no hay error, se omite. Opcionalmente, `finally` se ejecuta siempre (haya o no excepción) para liberar recursos o realizar acciones de cierre. Ejemplo de control desde fuera del método:

```java
try {
    double r = Calculadora.raiz(-16.0);
    System.out.println("Resultado = " + r);
} catch (IllegalArgumentException e) {
    System.err.println("Error: " + e.getMessage()); // decisión externa
} finally {
    // limpieza opcional
}
```

Que una excepción se **propague** significa que, si la función que la detecta no la captura, la excepción “sube” a la función llamadora; si tampoco allí se captura, continúa ascendiendo por la **pila de llamadas** buscando un manejador adecuado. Durante esta propagación ocurre el **desapilado** (stack unwinding): se van abandonando los marcos de pila de las funciones intermedias, ejecutando los bloques `finally` pertinentes y liberando implícitamente sus variables locales. Ninguna de esas funciones intermedias reanuda la ejecución “después del punto del error”; su ejecución termina al comenzar la propagación, y solo el primer `catch` que encaje con el tipo de la excepción recupera el control.

Las funciones que **no** controlan la excepción **no se reanudan** de vuelta en el punto donde fallaron; su ejecución queda interrumpida definitivamente. El flujo continúa en el `catch` más cercano y compatible; si no existe ningún manejador en toda la cadena hasta `main`, el programa termina con un error en tiempo de ejecución y un *stack trace*. En el ejemplo, si `main` no tuviera el `try/catch`, la `IllegalArgumentException` lanzada en `raiz` se propagaría hasta el hilo principal, se imprimiría el rastro de pila y la aplicación finalizaría. Este comportamiento contrasta con C: en Java, la gestión se centraliza donde hay contexto para decidir (el `try/catch` externo), y la limpieza garantizada se realiza con `finally` (o con *try-with-resources* cuando hay recursos autocerrables).



## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

La **propagación natural** de excepciones en Java aporta una separación mucho más clara entre la lógica principal y la gestión de errores. En C, cada función debe comprobar manualmente códigos de retorno y decidir cómo reaccionar o retransmitir el error. Esto produce código repetitivo y disperso, en el que las comprobaciones pueden ocultar la intención real del algoritmo. En cambio, en Java, si una función no puede resolver el problema, simplemente lanza la excepción y deja que un nivel superior decida; así, el código “feliz” permanece limpio y la política de tratamiento del error queda centralizada donde existe contexto para actuar.

Otra ventaja significativa es que la propagación automática realiza **stack unwinding**, es decir, libera correctamente los marcos de la pila y ejecuta cualquier bloque `finally` pendiente. En C, este proceso es manual: si ocurre un error en mitad de una función, el programador debe asegurarse explícitamente de liberar memoria, cerrar archivos o revertir estados antes de devolver el control al llamador. Con excepciones, esa limpieza está garantizada siempre que se use `finally` o estructuras como *try-with-resources*. Por tanto, disminuye la probabilidad de fugas de memoria, descriptores o estados inconsistentes.

Además, el diseño con excepciones permite que la **decisión sobre cómo manejar el fallo** se ubique en el nivel adecuado. Una función como `raiz` no tiene por qué saber si el programa debe mostrar un mensaje, registrar un error, volver a pedir datos o abortar la operación; simplemente declara que se ha violado una precondición. Esto favorece un estilo modular y reduce el acoplamiento entre funciones. En C, en cambio, cada función debe decidir si propaga el error codificándolo, lo gestiona localmente o imprime directamente un mensaje, lo que suele mezclar responsabilidades.

Finalmente, la propagación facilita el **flujo de control coherente ante errores graves**. Si varias funciones se llaman en cadena, y una de ellas lanza una excepción, el control llega automáticamente al primer bloque `catch` adecuado sin necesidad de comprobar manualmente cada paso intermedio. Esto reduce la complejidad lógica acumulativa que sí aparece en C, donde la propagación explícita de códigos de error puede convertirse en un patrón tedioso y propenso a omisiones. En conjunto, la propagación natural de excepciones ofrece claridad, seguridad y una gestión de errores más robusta que las soluciones tradicionales basadas en valores de retorno.



## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

En los lenguajes orientados a objetos como Java, las excepciones **son objetos**, es decir, instancias de clases que heredan de `Throwable`. Esto permite que cada excepción transporte no solo un mensaje descriptivo, sino también información adicional encapsulada: la traza de pila, causas encadenadas (`cause`), e incluso campos específicos definidos por el programador. De este modo, una excepción no es solo una señal de error, sino una unidad completa de información que describe el problema de forma estructurada.

La encapsulación aporta ventajas claras: el código que lanza la excepción no necesita revelar detalles internos sobre cómo se detectó el fallo; simplemente crea un objeto que representa esa situación anómala. Por su parte, el llamador puede inspeccionar únicamente los métodos públicos del objeto de excepción (`getMessage()`, `getCause()`, etc.) sin conocer los mecanismos internos que han llevado al error. Además, al ser objetos, pueden clasificarse jerárquicamente: excepciones más generales arriba y casos concretos abajo, permitiendo capturar de forma precisa o amplia según convenga.

El hecho de que las excepciones sean clases posibilita **crear excepciones personalizadas**, adaptadas al dominio del programa. Por ejemplo, si se implementa un sistema matemático, podría definirse una excepción propia como `RaizNegativaException`, que describa explícitamente la violación de una precondición numérica. Esto mejora la claridad de la API, permite distinguir errores diferentes mediante tipos concretos y facilita que otras partes de la aplicación reaccionen de manera diferenciada según el tipo de excepción recibida.

Un ejemplo básico en Java sería:

```java
class RaizNegativaException extends Exception {
    public RaizNegativaException(double x) {
        super("No existe raíz real para x negativo: " + x);
    }
}
```

Este tipo personalizado permite que el método `raiz` declare un contrato explícito:

```java
public static double raiz(double x) throws RaizNegativaException {
    if (x < 0) throw new RaizNegativaException(x);
    return Math.sqrt(x);
}
```

Así, se obtiene un diseño más expresivo, modular y alineado con los principios de orientación a objetos, donde los errores dejan de ser simples códigos numéricos para convertirse en entidades con semántica propia.



## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

En comparación con C, donde un error suele representarse mediante un **código numérico** o un valor especial, un objeto excepción en Java encapsula de forma automática información mucho más rica y estructurada. La pieza esencial es el **stack trace**, una representación interna de la pila de llamadas en el momento exacto en que se produjo el error. Este rastro permite conocer no solo *qué* falló, sino *dónde* y *cómo* se llegó a ese punto, ofreciendo una visión completa del camino de ejecución.

El **stack trace** contiene cada función (método) que estaba activa, el fichero fuente asociado y la línea exacta donde se lanzó la excepción. Esta información resulta especialmente valiosa cuando se llega a un manejador externo, ya que facilita reconstruir la secuencia de llamadas sin necesidad de imprimir manualmente cada paso, a diferencia del patrón habitual en C. Además, viene acompañado por el **mensaje descriptivo** proporcionado al crear la excepción —por ejemplo, `"No existe raíz real para x negativo"`— que resume de forma clara el motivo del fallo.

Otra característica relevante es la posibilidad de disponer de una **causa encadenada** (`getCause()`), que permite asociar una excepción con otra más interna que provocó el fallo original. Esto habilita diagnósticos más precisos, especialmente en sistemas donde intervienen varias capas (capa lógica, capa de acceso a datos, comunicaciones, etc.). En C, este tipo de encadenamiento debe implementarse manualmente, si es que se hace, mientras que en Java forma parte natural del modelo de excepciones.

En conjunto, disponer de un objeto excepción con **stack trace**, **mensaje** y **causa interna** proporciona un diagnóstico mucho más eficaz y coherente. En cuanto el manejador recibe el objeto, dispone de todos los datos necesarios para decidir cómo reaccionar, registrar el error o presentarlo al usuario, sin tener que reconstruir el contexto a mano. Esta encapsulación de información representa una mejora clara en legibilidad, mantenibilidad y depuración respecto a los mecanismos tradicionales de señalización de errores en C.



## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

Sí, en Java se pueden incluir **varios bloques `catch`** detrás de un mismo bloque `try`. Esto permite manejar **diferentes tipos de excepciones** de forma diferenciada, lo cual es útil cuando dentro del `try` pueden producirse fallos de distinta naturaleza. Cada `catch` actúa como un filtro por tipo: si la excepción lanzada coincide con el tipo declarado (o es una subclase), ese bloque es candidato para capturarla.

Sin embargo, **solo se ejecuta un único bloque `catch`**, el primero cuyo tipo sea compatible con la excepción lanzada. Una vez encontrado ese manejador, los demás `catch` se ignoran y el flujo continúa después del último `catch` o dentro del `finally` si existe. Esto evita ambigüedades y mantiene un comportamiento predecible. Por esta razón, debe colocarse primero el `catch` más específico y después los más generales; si se hace lo contrario, los específicos quedarían inaccesibles.

Este diseño permite organizar el manejo de errores de forma más expresiva. Por ejemplo, en un cálculo matemático podría capturarse una excepción personalizada `RaizNegativaException` y, en otro `catch`, capturar otras excepciones como `NumberFormatException` si se convierte texto en número. Así, cada `catch` gestiona un tipo de error distinto, pero solo aquel que corresponde exactamente al problema real llegará a ejecutarse.

Un ejemplo típico sería:

```java
try {
    double r = Calculadora.raiz(x);
    System.out.println("Resultado = " + r);
} catch (RaizNegativaException e) {
    System.err.println("Error específico: " + e.getMessage());
} catch (IllegalArgumentException e) {
    System.err.println("Error genérico de argumento: " + e.getMessage());
} catch (Exception e) {
    System.err.println("Otro error inesperado.");
}
```

En este caso, **solo uno** de los bloques se ejecutará, concretamente el primero cuyo tipo coincida con la excepción lanzada.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

Sí, aunque las excepciones rompen el flujo normal, Java garantiza la ejecución del bloque **`finally`** siempre que se abandone el `try`, haya o no excepción, incluso si hay `return` dentro del `try` o del `catch`. Esto permite asegurar la **liberación de recursos** (cerrar ficheros, conexiones, desbloquear *locks*, etc.) antes de que la excepción continúe propagándose. Las únicas excepciones a esta garantía son situaciones anómalas como `System.exit()`, apagado de la JVM o errores fatales (p. ej., `OutOfMemoryError` severo).

Cuando se **captura** la excepción, `finally` sigue siendo el lugar idóneo para limpieza determinista. El `catch` decide la política (registrar, transformar, recuperar) y el `finally` realiza las tareas que **siempre** deben ocurrir. En el ejemplo siguiente se captura una excepción, se informa, y se cierra el recurso en `finally`:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class EjemploConCatchFinally {
    public static void main(String[] args) {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader("datos.txt"));
            String linea = br.readLine();
            System.out.println("Primera línea: " + linea);
        } catch (IOException e) {
            // Manejo/registro del error (política de la capa llamadora)
            System.err.println("Error de E/S: " + e.getMessage());
        } finally {
            // Limpieza garantizada
            if (br != null) {
                try { br.close(); } catch (IOException ignore) {}
            }
        }
    }
}
```

También puede necesitarse **no capturar** (para que la excepción se **propague**) pero **aun así** garantizar la limpieza. En ese caso, se usa `try`–`finally` sin `catch`. La excepción se re-lanza automáticamente tras ejecutar el `finally`, por lo que el manejador ocurrirá más arriba en la pila:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class EjemploSinCatchConFinally {
    public static void main(String[] args) throws IOException {
        // Se declara throws para permitir la propagación al llamador
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader("datos.txt"));
            String linea = br.readLine();
            System.out.println("Primera línea: " + linea);
            // Si ocurre IOException aquí, no se captura localmente
        } finally {
            // Se ejecuta tanto si hubo excepción como si no
            if (br != null) {
                try { br.close(); } catch (IOException ignore) {}
            }
        }
        // Si hubo excepción, no se llega aquí: se propagó tras el finally
    }
}
```

En código moderno, conviene considerar **try-with-resources** para recursos autocerrables (`AutoCloseable`), que cierra automáticamente sin necesidad de `finally`. No obstante, cuando el ejercicio pide explícitamente `finally`, los patrones anteriores muestran cómo **garantizar** el cierre tanto si se **captura** como si se **deja propagar** la excepción.



## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

Sí, en Java el bloque **`finally` puede aparecer sin ningún bloque `catch`**, siempre que exista un bloque `try` asociado. En ese caso, el propósito del `finally` es garantizar que cierto código —normalmente relacionado con limpieza o liberación de recursos— se ejecute **siempre**, ocurra o no ocurra una excepción dentro del `try`. Este patrón se usa cuando no interesa manejar la excepción localmente, sino **dejar que se propague**, pero aun así asegurar cierta acción final antes de abandonar el método.

El bloque `finally` se **ejecuta siempre** tanto si no hay errores como si se lanza una excepción durante la ejecución del `try`. Incluso si la excepción no es capturada ahí mismo, Java ejecuta el `finally` antes de continuar con la propagación hacia niveles superiores en la pila. Gracias a este comportamiento se garantiza que los recursos queden correctamente cerrados o liberados, evitando fugas y estados inconsistentes que serían fáciles de introducir en un diseño basado únicamente en retornos manuales como sucede en C.

Un punto importante es que el `finally` se ejecuta **incluso cuando dentro del `try` hay un `return`**. El flujo de retorno se “pospone”: Java ejecuta primero el contenido del `finally` y, tras terminarlo, realiza el `return`. Esto significa que el `finally` tiene prioridad sobre la salida inmediata del método, lo que refuerza la garantía de ejecución sin importar el camino seguido dentro del `try`.

Un ejemplo ilustrativo sería:

```java
public static int ejemploConReturn() {
    try {
        System.out.println("Dentro del try");
        return 42;              // El return se suspende temporalmente
    } finally {
        System.out.println("Ejecutando finally");
    }
    // Nunca se llega aquí
}
```

En este caso, aunque el `try` devuelve un valor, **primero** se ejecuta el `finally`. De igual forma, si dentro del `try` se produjera una excepción no controlada, el `finally` se ejecutaría igualmente antes de que la excepción se propagase. Todo esto convierte a `finally` en una herramienta esencial para garantizar comportamientos deterministas al abandonar un bloque de código, independientemente de la ruta seguida para salir de él.



## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

En Java se distingue entre **excepciones controladas (checked)** y **no controladas (unchecked)**. Las controladas son aquellas que **el compilador obliga a declarar o capturar**, porque representan situaciones excepcionales *previsibles* y potencialmente recuperables (fallos de E/S, problemas de red, ausencia de un fichero…). Las no controladas son subclases de **`RuntimeException`**, y se usan para errores de programación o situaciones de las que difícilmente se puede recuperar localmente (índices fuera de rango, errores lógicos, precondiciones violadas…). Su manejo **no es obligatorio** y suele reservarse para fallos del propio código.

La clase **`RuntimeException`** actúa como raíz de las excepciones no controladas. Si una excepción hereda de ella, Java no exige ni declarar `throws` ni capturarla. Esto sirve para expresar errores cuya responsabilidad recae en el desarrollador que llama al método, como pasar `null` donde no se debe o divisiones por cero lógicas. En cambio, si se crea una excepción personalizada que herede de `Exception` directamente (sin pasar por `RuntimeException`), entonces es controlada y obliga a quien use el método a manejarla o declararla.

A continuación se muestran **ejemplos típicos**, incluyendo excepciones de uso general que nosotros mismos podríamos emplear o extender:

**Ejemplos de excepciones controladas (checked):**

*   `IOException` (p. ej., lectura/escritura de ficheros).
*   `FileNotFoundException` (intentar abrir un fichero inexistente).
*   `SQLException` (errores al trabajar con bases de datos).
*   Cualquier excepción personalizada creada como:
    ```java
    class RaizNegativaException extends Exception { ... }
    ```

**Ejemplos de excepciones no controladas (unchecked):**

*   `NullPointerException` (acceder a un objeto nulo).
*   `IllegalArgumentException` (argumento inválido pasado a un método).
*   `ArithmeticException` (división por cero, desbordamientos aritméticos).
*   Excepción personalizada como:
    ```java
    class ValorInesperadoException extends RuntimeException { ... }
    ```

Finalmente, se muestran **dos listas** con situaciones donde suele preferirse cada tipo:

### Casos donde se prefiere una excepción **controlada** (checked)

1.  Fallos de entrada/salida que el llamador puede recuperar (reintentar lectura).
2.  Operaciones con recursos externos donde es razonable que el código cliente decida qué hacer.
3.  Errores de negocio para los que existe una lógica de recuperación posible.
4.  Validaciones donde se quiere *forzar* al llamador a pensar en cómo gestionar el fallo.

### Casos donde se prefiere una excepción **no controlada** (unchecked)

1.  Violación de precondiciones del método (argumentos incorrectos).
2.  Errores de programación que deben corregirse, no manejarse (`NullPointerException`).
3.  Situaciones donde capturar la excepción generaría código ruidoso sin aportar recuperación real.
4.  Problemas lógicos internos del programa que no son responsabilidad del usuario de la API.

Si quieres, puedo elaborar excepciones personalizadas para tu clase `Calculadora` y mostrar cómo se diseñan tanto controladas como no controladas.


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

En Java, la palabra clave **`throws`** se utiliza en la **declaración de un método** para indicar que dicho método *puede lanzar* una o varias excepciones controladas. Al hacerlo, el método no asume la responsabilidad de manejarlas, sino que delega en el **código llamador** la obligación de capturarlas o volver a declararlas. De este modo, la firma del método expresa un **contrato explícito**: “quien me llame debe estar preparado para tratar esta situación excepcional”. Esto hace que el compilador pueda verificar estáticamente que las excepciones controladas no quedan sin tratar en ningún camino del programa.

El uso de `throws` constituye una **alternativa a capturar** una excepción controlada cuando el método en cuestión no tiene suficiente información o responsabilidad para decidir cómo reaccionar ante el error. Por ejemplo, una función que calcula una raíz cuadrada puede detectar que el número es negativo, pero quizá no sepa si el programa debe pedir otro número, registrar un error o abortar la operación. En esos casos, declarar `throws` permite dejar que un nivel superior —más cercano a la lógica de la aplicación— adopte la decisión adecuada sin obligar al método a manejar algo para lo que no tiene contexto.

Además, `throws` evita introducir bloques `try-catch` innecesarios, lo que mantiene el código más claro y centrado en su propósito principal. En vez de interrumpir el flujo del método con gestión de errores, este simplemente señala formalmente que puede fallar. La gestión se pospone para el punto donde realmente se puede actuar. Es importante recordar que esta obligación solo aplica a **excepciones controladas**; las no controladas (`RuntimeException`) no requieren ni captura ni declaración en `throws`, ya que representan fallos de programación o errores no recuperables.

Por ejemplo:

```java
public static double raiz(double x) throws RaizNegativaException {
    if (x < 0) {
        throw new RaizNegativaException(x);
    }
    return Math.sqrt(x);
}
```

Aquí, el método declara `throws` porque **no captura** la excepción; será el llamador quien decida cómo manejarla. Este patrón refleja precisamente el motivo por el que `throws` es una alternativa directa a capturar una excepción controlada: permite delegar la responsabilidad sin romper las reglas del compilador y manteniendo un diseño limpio y modular.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

Una forma de expresar que un método **no quiere manejar localmente** la ausencia del fichero es declarar `throws` en su firma y usar un `try`–`finally` **sin `catch`** para garantizar el cierre del recurso. En este patrón, cualquier `IOException` (incluida `FileNotFoundException`) **se propaga** al llamador, pero el `finally` asegura la liberación del `BufferedReader` aunque se produzca la excepción en mitad del `try`.

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class LectorFicheros {

    /**
     * Abre un fichero y devuelve su primera línea.
     * No maneja localmente si el fichero no existe: lo declara con throws para propagarlo.
     */
    public static String leerPrimeraLinea(String ruta) throws IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(ruta)); // puede lanzar FileNotFoundException (checked)
            return br.readLine();                          // puede lanzar IOException
        } finally {
            // Se ejecuta siempre: haya éxito, haya return o haya excepción
            if (br != null) {
                try { br.close(); } catch (IOException ignore) {}
            }
        }
        // Nota: si hubo excepción, no se llega aquí; la excepción se propaga tras ejecutar el finally.
    }

    public static void main(String[] args) {
        // Aquí se decide la política de manejo (capturar, registrar, reintentar, etc.)
        try {
            String linea = leerPrimeraLinea("datos.txt");
            System.out.println("Primera línea: " + linea);
        } catch (IOException e) {
            System.err.println("No se pudo leer el fichero: " + e.getMessage());
        }
    }
}
```

En este diseño, la **firma del método** (`throws IOException`) actúa como contrato explícito: la función puede fallar por razones de E/S y **delega** en el llamador decidir qué hacer. Aun así, el `finally` garantiza la limpieza determinista del recurso incluso si se lanzó una excepción o hubo un `return` temprano dentro del `try`. Si se prefiriera forzar únicamente la propagación de “fichero inexistente”, podría declararse `throws FileNotFoundException`, aunque en la práctica suele emplearse `IOException` para cubrir el conjunto de errores de E/S relacionados.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Sí, técnicamente en Java se pueden poner **excepciones no controladas** (subclases de `RuntimeException`) en la cláusula `throws`, aunque **no es obligatorio** hacerlo. El compilador no exige declarar excepciones no controladas porque representan errores de programación o condiciones que no requieren manejo forzado. Por tanto, incluirlas en `throws` es válido, pero en la práctica se usa solo en situaciones muy específicas donde se quiere dejar explícito algo sobre el contrato del método.

En cuanto al llamador, **no está obligado** a rodear con `try-catch` una excepción no controlada aunque aparezca en `throws`. La semántica no cambia: sigue sin ser obligatorio capturarla. El llamador puede capturarla si considera útil reaccionar ante ese error, pero normalmente no se hace porque las excepciones no controladas representan problemas de los que no suele recuperarse la aplicación, como violaciones de precondiciones (`IllegalArgumentException`) o errores lógicos internos (`NullPointerException`). Por tanto, obligar al llamador a manejar algo así no tendría sentido en la mayoría de casos.

¿Para qué sirve entonces poner una excepción no controlada en `throws`? Su utilidad principal es **documentar la intención** del método: advertir al desarrollador de que puede lanzarse una cierta excepción no controlada y que forma parte del “contrato informal” de uso. Se usa a veces en APIs para indicar que un método puede fallar de una forma concreta aunque no sea obligatorio capturarla. Esto puede ayudar a comprender costes, riesgos o precondiciones del método sin necesidad de leer toda su implementación. Sin embargo, es menos común porque muchas herramientas y programadores asumen que las `RuntimeException` pueden aparecer en cualquier momento sin avisar.

Un ejemplo válido pero poco habitual sería:

```java
public void procesarDatos(String ruta) throws NullPointerException, IllegalStateException {
    if (ruta == null) {
        throw new NullPointerException("La ruta no puede ser null.");
    }
    // ...
}
```

Aquí, el uso de `throws` no implica obligación de captura, sino que **sirve de advertencia explícita**, y el código llamador decide libremente si quiere reaccionar o no:

```java
try {
    procesarDatos(null);
} catch (NullPointerException e) {
    System.err.println("Ruta inválida: " + e.getMessage());
}
```

En resumen:

*   **Sí se pueden** incluir excepciones no controladas en `throws`.
*   **No es obligatorio** capturarlas, aunque aparezcan en `throws`.
*   Su presencia en `throws` cumple una función **documental**, no de control obligatorio.

Si quieres, puedo darte ejemplos comparando un método con `throws RuntimeException` frente a uno sin `throws`, para ver cómo cambia la claridad del contrato pero no la obligación de captura.



## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

En Java, las **excepciones controladas (checked)** se recomiendan cuando una operación puede fallar por causas **externas, previsibles y potencialmente recuperables**, y cuando tiene sentido obligar al llamador a decidir qué hacer con ese fallo. Un ejemplo típico es `IOException`, que representa errores de entrada/salida: el programa podría reintentar, notificar al usuario, usar un fichero alternativo o abortar la operación. En estos casos, disponer de una excepción controlada fuerza a que el llamador **piense explícitamente** cómo manejar ese error, lo que encaja con la idea de que la recuperación es posible y deseable en ciertos contextos.

Por el contrario, las **excepciones no controladas (unchecked)**, derivadas de `RuntimeException`, se recomiendan cuando el fallo **no es recuperable** o representa un **error de programación**. Ejemplos típicos son `NullPointerException`, `IndexOutOfBoundsException` o `IllegalArgumentException`. Estas situaciones indican violaciones de precondiciones o errores lógicos que deben ser corregidos en el código, no gestionados en tiempo de ejecución mediante `try-catch`. En estos casos sería contraproducente obligar al programador a capturar la excepción, dado que la recuperación no tiene sentido y el fallo debe resolverse modificando el código que lo provoca.

En cuanto a otros lenguajes, **no todos proporcionan ambas opciones**. Lenguajes como **Python, C#, C++ o JavaScript** **no tienen excepciones controladas**, sino que todas son equivalentes a las "no controladas" de Java. De hecho, Java es uno de los pocos lenguajes populares que mantiene esta distinción. En estos lenguajes, lanzar una excepción es siempre opcional en términos de captura, y el programador decide si quiere manejarla en niveles superiores. Este enfoque evita la verbosidad asociada a las excepciones controladas, pero también pierde la capacidad de “forzar” al llamador a reaccionar a fallos previsibles.

En los lenguajes donde sólo existe **una única categoría de excepción**, lo habitual es el enfoque equivalente a las **excepciones no controladas** de Java. Es decir, el lenguaje permite lanzar excepciones libremente, pero no obliga al llamador a capturarlas ni declararlas. La responsabilidad recae en el programador, que decide cuándo envolver en `try-catch` y cuándo permitir la propagación. Este modelo, más flexible pero menos estricto, es el dominante fuera de Java, lo que explica por qué muchos desarrolladores consideran las excepciones controladas como una característica útil en ciertos dominios pero demasiado intrusiva en otros.

Si quieres, puedo darte una tabla comparativa entre distintos lenguajes sobre cómo tratan las excepciones, o ejemplos adicionales de cuándo elegir cada tipo en tus propios proyectos.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Sí, **tiene sentido lanzar una nueva excepción dentro de un `catch`** cuando el objetivo es *traducir* el fallo a un tipo más significativo para la capa actual, o *añadir contexto* que se perdería si solo se registrara. Este patrón se conoce como **exception translation**: se captura una excepción de bajo nivel (p. ej., `IOException`) y se lanza otra más alineada con el dominio (p. ej., `ServicioNoDisponibleException`). Es importante **encadenar la causa** (`new ...Exception(msg, causa)`) para no perder el rastro original. También puede usarse para **cambiar el tipo** (de checked a unchecked) cuando la API pública no quiere forzar manejo, pero necesita conservar el detalle técnico en `getCause()`.

Por otro lado, **se puede relanzar la misma excepción capturada** (rethrow) cuando el bloque `catch` *no puede* resolver el problema pero **sí aporta valor antes de propagarlo**: registrar con contexto adicional, medir tiempos, actualizar métricas, o realizar acciones compensatorias. En Java, se re-lanza con `throw e;` y el **stack trace original se conserva** (no usar `throw new` salvo que se quiera cambiar el tipo o añadir contexto explícitamente). Tiene sentido re-lanzar la misma excepción cuando **la política de recuperación reside en un nivel superior**, pero esta capa intermedia quiere dejar trazabilidad o asegurar tareas de limpieza (además de usar `finally` o *try-with-resources*).

### 1) Lanzar **otra** excepción dentro de `catch` (traducción / añadir contexto)

```java
// Excepción de dominio (checked)
class ServicioNoDisponibleException extends Exception {
    public ServicioNoDisponibleException(String msg, Throwable cause) { super(msg, cause); }
}

public class ClienteRemoto {
    public String descargarConfig(String url) throws ServicioNoDisponibleException {
        try {
            // ... código que puede lanzar IOException
            throw new java.io.IOException("Timeout al conectar"); // ejemplo
        } catch (java.io.IOException e) {
            // Se traduce a una excepción de dominio y se encadena la causa
            throw new ServicioNoDisponibleException(
                "No se pudo obtener la configuración desde " + url, e
            );
        }
    }
}
```

Este patrón hace que las capas superiores no dependan de detalles de bajo nivel (E/S, red), y reciban un **tipo semántico**. El rastro original sigue disponible mediante `getCause()`, preservando la depuración. También puede usarse para **cambiar de checked a unchecked**: capturar `SQLException` y lanzar `IllegalStateException` con causa, si la API no quiere obligar a `try-catch` en cada uso.

### 2) **Relanzar la misma excepción** tras registrar o anotar

```java
import java.util.logging.Logger;

public class Procesador {
    private static final Logger LOG = Logger.getLogger(Procesador.class.getName());

    public void procesar(String ruta) throws java.io.IOException {
        java.io.BufferedReader br = null;
        try {
            br = new java.io.BufferedReader(new java.io.FileReader(ruta));
            // ... lógica que puede lanzar IOException
            throw new java.io.IOException("Lectura interrumpida"); // ejemplo
        } catch (java.io.IOException e) {
            // Se añade contexto (logging, métricas) y se relanza la MISMA excepción
            LOG.severe(() -> "Fallo leyendo '" + ruta + "': " + e.getMessage());
            throw e; // conserva el stack trace original
        } finally {
            if (br != null) {
                try { br.close(); } catch (java.io.IOException ignore) {}
            }
        }
    }
}
```

Aquí el método **no cambia el tipo** (la capa superior ya espera `IOException`), pero añade **contexto útil** para diagnóstico y asegura la **limpieza determinista** en `finally`. Este enfoque es recomendable cuando no hay una recuperación local razonable, pero sí hay tareas transversales (registro, auditoría, contadores, *circuit breakers*) que deben ejecutarse justo antes de permitir que la excepción **se propague** al manejador con contexto suficiente.



## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

Una excepción es la **“causa”** de otra cuando se encadena el fallo original al lanzar una nueva excepción de nivel superior, preservando el contexto técnico. En Java, esto se realiza pasando la excepción capturada como **`cause`** al constructor de la nueva excepción (`new MiExcepcion(msg, causa)`) o usando `initCause`. Este patrón, conocido como **encadenamiento de excepciones** (*exception chaining*), permite traducir un error de bajo nivel (p. ej., E/S) a uno de **alto nivel de dominio** (p. ej., “configuración no disponible”), sin perder la traza ni los detalles necesarios para el diagnóstico.

En la práctica, se captura la excepción técnica donde ocurre (red, disco, formato), se añade **semántica de negocio** lanzando otra excepción más significativa para la capa actual, y se **encadena** la original como causa. Así, las capas superiores no dependen de tipos de bajo nivel, pero si necesitan investigar, el **stack trace** conserva la ruta completa. Cuando una excepción encadenada se imprime (por ejemplo, con `printStackTrace()` o registrada por el runtime), aparece explícitamente la sección **`Caused by:`** con la clase, mensaje y traza de la causa, potencialmente con **varias causas** anidadas si hay más niveles de traducción.

Ejemplo en Java encapsulando una `IOException` en una excepción de dominio personalizada:

```java
// Excepción de alto nivel (dominio)
class ConfigNoDisponibleException extends Exception {
    public ConfigNoDisponibleException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

class ConfigService {
    public String cargarConfig(String ruta) throws ConfigNoDisponibleException {
        try (java.io.BufferedReader br = new java.io.BufferedReader(new java.io.FileReader(ruta))) {
            String linea = br.readLine(); // puede lanzar IOException
            if (linea == null) {
                throw new java.io.IOException("El fichero está vacío");
            }
            return linea;
        } catch (java.io.IOException e) {
            // Traducción + encadenamiento de la causa original
            throw new ConfigNoDisponibleException("No se pudo cargar la configuración: " + ruta, e);
        }
    }
}

public class App {
    public static void main(String[] args) {
        try {
            String cfg = new ConfigService().cargarConfig("config.txt");
            System.out.println("Config: " + cfg);
        } catch (ConfigNoDisponibleException e) {
            // Al imprimir, se verá la cadena de causas con "Caused by:"
            e.printStackTrace();
        }
    }
}
```

Al ejecutar este código cuando el fichero falta o hay un error de lectura, el volcado de la pila mostrará algo como: `ConfigNoDisponibleException: No se pudo cargar la configuración: config.txt` seguido de **`Caused by: java.io.FileNotFoundException: ...`** (o la `IOException` correspondiente), con sus líneas y métodos. Esto facilita diagnosticar rápidamente la **intención de alto nivel** (configuración no disponible) y el **origen técnico exacto** (fallo de E/S), manteniendo separación de capas y buena trazabilidad.


