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

### Un puntero a función es una variable que almacena la dirección de memoria donde residen las instrucciones de una rutina ejecutable, en lugar de almacenar un valor de datos estándar. Esto permite invocar dinámicamente diferentes funciones en tiempo de ejecución, pasarlas como argumentos a otras rutinas o almacenarlas en estructuras de datos, constituyendo un mecanismo fundamental en C para lograr flexibilidad en la arquitectura del código (como la implementación de retrollamadas o callbacks).

Al declarar un puntero a función, resulta imperativo especificar la firma exacta de la función a la que apuntará, incluyendo su tipo de retorno y los tipos de sus parámetros. Esto garantiza que la comprobación estática de tipos del compilador opere correctamente. Tras asignar la dirección de la función al puntero, su invocación se realiza de manera transparente empleando la misma sintaxis que una llamada convencional.

#include <stdio.h>
#include <ctype.h>

/* Función que modifica la cadena en el mismo espacio de memoria */
void convertirMayusculas(char *cadena) {
    while (*cadena) {
        *cadena = toupper((unsigned char)*cadena);
        cadena++;
    }
}

int main() {
    char texto[] = "hola mundo";
    
    /* Declaración del puntero a función e inicialización */
    void (*aMayusculas)(char *) = convertirMayusculas;

    /* Invocación a través del puntero */
    aMayusculas(texto);
    
    printf("%s\n", texto);

    return 0;
}


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Una función lambda, frecuentemente denominada función anónima, es una subrutina definida sin un identificador o nombre explícito. A diferencia de las funciones tradicionales que requieren declaración formal en el ámbito global o en el cuerpo de una clase, las lambdas se pueden construir directamente en el lugar donde serán requeridas, evaluándose como literales que pueden ser asignados a variables o inyectados como argumentos.

Estas expresiones constituyen un pilar en lenguajes que incorporan elementos del paradigma funcional, propiciando un código más declarativo y conciso. Internamente, encapsulan un fragmento de comportamiento para su posterior ejecución, aportando una sintaxis ligera orientada a minimizar el código accesorio requerido para su declaración formal.

// Java
import java.util.function.Function;

public class Principal {
    public static void main(String[] args) {
        // Declaración de la lambda tipada mediante interfaz funcional
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();
        
        String textoJava = "hola mundo";
        System.out.println(aMayusculas.apply(textoJava));
    }
}



## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### El paradigma funcional es un estilo de programación declarativa que trata la computación como la evaluación de funciones matemáticas, evitando rigurosamente la mutación del estado y los datos modificables. Su enfoque primordial se centra en "qué" se debe resolver frente al "cómo" imperativo, promoviendo el empleo de funciones puras, donde el resultado computado depende de manera exclusiva de los argumentos de entrada y no se desencadenan efectos colaterales sobre el resto del sistema. 


Se clasifica como multiparadigma a lenguajes como Java (a partir de la versión 8) debido a que no imponen un aislamiento estricto en un único estilo de codificación. Aunque la arquitectura nuclear del lenguaje preserva su naturaleza fuertemente orientada a objetos, se incorporan herramientas de la rama funcional (expresiones lambda, evaluación perezosa, inmutabilidad de colecciones). Esta convergencia posibilita aplicar la estrategia más adecuada según la complejidad del dominio, enlazando la robustez de la encapsulación orientada a objetos con la expresividad matemática.

Que las funciones ostenten la categoría de "ciudadanos de primera clase" indica que el entorno de ejecución las manipula con la misma libertad otorgada a cualquier otro tipo de dato fundamental (como un entero o la referencia a un objeto). En consecuencia, es factible asignar rutinas completas a variables, suministrarlas como parámetros a otras rutinas, retornarlas como resultado de un método o custodiarlas en estructuras de datos, dotando al sistema de una alta componibilidad lógica.


## 4. Explica la sintaxis básica de una función lambda en Java.

### La sintaxis base para constituir una función lambda en Java se segmenta en tres componentes primordiales: una lista de parámetros formalizados entre paréntesis, el operador flecha (->) que actúa como separador lógico, y el cuerpo contenedor de la expresión o conjunto de sentencias a evaluar. Esta configuración se fundamenta en la inferencia de tipos por parte del compilador para suprimir código predecible.

En la definición de los parámetros, se emplean paréntesis vacíos () si la función no requiere datos de entrada. En el supuesto de contar con un único parámetro cuyo tipo es deducible por el contexto de compilación, se permite la omisión tanto de los paréntesis como de la declaración estricta del tipo. Ante la presencia de múltiples parámetros, se separan mediante comas, preservando la facultad de omitir sus tipos si la interfaz funcional de destino ofrece suficiente información al compilador.

Respecto al cuerpo de la lambda, su morfología varía en función de la complejidad estructural requerida. Si la operación se resuelve en una única expresión matemática o llamada a método, no se precisan llaves {} y el resultado computado se retorna de forma implícita sin exigir la instrucción return. Inversamente, ante un flujo que requiere bloques multilínea o bifurcaciones condicionales, se exige el encuadre explícito entre llaves y, en caso de generar un resultado, el uso estricto de la directiva return.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### // JavaScript
function transformar(texto, funcionTransformadora) {
    return funcionTransformadora(texto);
}

const aMayusculas = (cadena) => cadena.toUpperCase();
console.log(transformar("hola mundo", aMayusculas));

// Java
import java.util.function.Function;

public class Principal {
    
    // Método de orden superior que acepta comportamiento inyectado
    public static String transformar(String texto, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();
        System.out.println(transformar("hola mundo", aMayusculas));
    }
}


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### // JavaScript
// Se define la lógica de inversión sin asignación intermedia
console.log(transformar("hola mundo", cadena => cadena.split('').reverse().join('')));

// Java
public class Principal {
    public static String transformar(String texto, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(texto);
    }

    public static void main(String[] args) {
        // Se construye y se pasa la función lambda en un solo paso
        System.out.println(transformar("hola mundo", 
            cadena -> new StringBuilder(cadena).reverse().toString()
        ));
    }
}


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### En el ámbito del paradigma funcional, se designa como cierre léxico o closure a la capacidad de una función lambda para retener visibilidad sobre el ecosistema de variables presente en su bloque de definición circundante. La función engloba no solo su bloque de instrucciones, sino también referencias (o copias) del estado externo preexistente, logrando "capturar" información circundante en el momento preciso de su creación.

Para garantizar la integridad del entorno concurrente en el lenguaje Java, la sintaxis exige que las variables locales interceptadas mediante closure posean la restricción de ser "efectivamente finales" (effectively final). Esto dictamina que, aunque se omita la palabra clave final en la declaración, el valor de la variable capturada por la lambda no debe sufrir ninguna alteración en su registro de memoria posterior a su asignación originaria; de lo contrario, el compilador bloqueará la operación.

Gracias a los cierres, el comportamiento anónimo adopta la cualidad de mantener estado contextual prescindiendo de una declaración formal de clase o atributos constructores. En el siguiente escenario de código, la rutina de transformación asimila una variable exógena para anexar un prefijo estático a toda cadena de entrada procesada.

import java.util.function.Function;

public class Principal {
    public static String transformar(String texto, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(texto);
    }

    public static void main(String[] args) {
        // Variable local del entorno exterior. Se considera "effectively final".
        String prefijo = "LOG EXECUTADO: ";

        // La lambda hace uso del closure capturando la variable 'prefijo'
        Function<String, String> añadirPrefijo = cadena -> prefijo + cadena;

        System.out.println(transformar("hola mundo", añadirPrefijo));
    }
}


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### La discrepancia central que separa al modelo funcional de las lambdas respecto a los punteros de C subyace en la concepción del estado encapsulado. En el estándar C, un puntero a función consiste inexorablemente en una dirección de memoria plana apuntando al vector de instrucciones inmutables del binario; no existe mecanismo inherente para "recordar" las variables colindantes donde se efectuó la vinculación. Toda contextualización requiere un esfuerzo explícito del desarrollador mediante el tránsito forzoso de estructuras complejas (típicamente empleando punteros opacos void*).

Por la parte antagónica, la estructura lambda despliega un empaquetamiento conjunto de código procedural y del estado del entorno (closure). En sistemas tipados orientados a objetos como Java, el paso de una lambda deviene en la compilación oculta e instanciación de un objeto bajo una clase anónima, donde el contexto atrapado se mapea internamente como campos de dicho objeto virtual. Por lo tanto, mientras que el puntero representa simple ejecución referenciada, la lambda proyecta un fragmento de ejecución auto-contextualizado.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### El flujo de información en la programación de orden superior trasciende la simple inyección de funciones y faculta la emisión de las mismas como producto de retorno. Este comportamiento origina el concepto de "fábricas de funciones": subrutinas especializadas que configuran el estado inicial de un bloque lógico inyectando parámetros estáticos para retornar rutinas configuradas listas para diferir su ejecución.

En la mecánica mostrada a continuación, al invocar crearDescuento, la rutina receptora procesa un parámetro numérico con el porcentaje a disminuir. Sin embargo, no elabora el cálculo, sino que ensambla y retorna una evaluación lambda. Aquí el fenómeno del closure cobra vital relevancia: cuando la pila de ejecución de crearDescuento se destruye, la variable local que contiene el porcentaje persiste resguardada en el cierre léxico de la lambda devuelta.

Posteriormente, las rutinas generadas se asignan a variables distintas (descuento10 y descuento25). Estas operan de forma independiente al ser sometidas a una cantidad de entrada; cada una recupera intacto de su propio closure el porcentaje exacto con el que fue pre-parametrizada para efectuar el decremento matemático pertinente.

Java
import java.util.function.Function;

public class Principal {
    
    // Rutina de orden superior que devuelve una subrutina
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // Closure: el entorno devuelto captura el parámetro 'porcentaje'
        return cantidad -> cantidad - (cantidad * (porcentaje / 100.0));
    }

    public static void main(String[] args) {
        // Creación de las funciones pre-configuradas mediante la fábrica
        Function<Double, Double> descuento10 = crearDescuento(10.0);
        Function<Double, Double> descuento25 = crearDescuento(25.0);

        double precioBase = 150.0;

        // Ejecución delegada a los comportamientos instanciados
        System.out.println("Precio con el 10% descontado: " + descuento10.apply(precioBase));
        System.out.println("Precio con el 25% descontado: " + descuento25.apply(precioBase));
    }
}


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### En arquitecturas fundamentadas sobre sistemas de tipado estático riguroso, carece de sentido la noción abstracta de una "función libre" que escapa a la validación de compilación. Para absorber adecuadamente la semántica de las lambdas asegurando la verificación de tipos, la plataforma Java introduce el mecanismo de la "interfaz funcional". Una interfaz de este perfil actúa como el contrato subyacente que impone el molde morfológico sobre cualquier asignación de una expresión lambda anónima.

El axioma técnico innegociable que debe ostentar una interfaz para cualificar como funcional radica en poseer, de forma unívoca, un solo método abstracto sin implementar. Esta firma única constituye el destino exacto que la representación lambda satisfará estructuralmente en cantidad de parámetros recibidos y valor computado retornado. Siempre que la regla del método único se preserve intacta, el entorno del compilador podrá establecer correspondencias sin ambigüedades.

Adicionalmente, se recomienda acompañar la definición superior con el decorador @FunctionalInterface. Su rol estriba en notificar al motor de compilación para que emita alertas semánticas en el instante en que erróneamente se agreguen métodos abstractos suplementarios. Es relevante especificar que las normativas de estas interfaces toleran la adición paralela de un volumen ilimitado de métodos estáticos o rutinas implementadas por defecto (default methods), manteniendo el umbral del método abstracto en estricta unidad.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### // Etiquetado opcional pero recomendado por seguridad estática
@FunctionalInterface
public interface Transformador {
    // Única firma abstracta presente
    String transformar(String entrada);
}

// Escenario de instanciación ilustrativo
class Utilizacion {
    public static void main(String[] args) {
        // La lambda coincide lógicamente con el método 'transformar'
        Transformador enmascarar = texto -> texto.replaceAll(".", "*");
        System.out.println(enmascarar.transformar("Secreto"));
    }
}


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### @FunctionalInterface
public interface Transformador<T, R> {
    // 'T' representa el tipo originario, 'R' representa el tipo resultivo
    R transformar(T entrada);
}

public class Principal {
    public static void main(String[] args) {
        // Especialización de los generics en el punto de uso
        Transformador<Double, Integer> redondeoFuncional = numero -> (int) Math.round(numero);

        Double fraccionario = 8.65;
        Integer resolucion = redondeoFuncional.transformar(fraccionario);

        System.out.println("Base decimal: " + fraccionario);
        System.out.println("Entero aproximado: " + resolucion);
    }
}


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Function<T, R>: Ingiere un parámetro del segmento de datos T emitiendo como salida una correlación de la variante R. Resulta análoga al modelo abstracto recientemente generado.

Consumer<T>: Absorbe un argumento adscrito al grupo T exento de toda obligación de retorno (void). Su campo de acción se dirige frecuentemente a operaciones terminales con efectos laterales en los límites del sistema (v.gr., guardados en base de datos o emisiones a consola).

Supplier<T>: Deniega la asimilación de cualquier tipo de argumento de entrada mientras suministra en exclusiva instancias conformes al tipo T. Destaca su empleo como agente fabricador aplazado.

Predicate<T>: Procesa la entrada estructurada T resolviéndose en un valor booleano puro. Opera de facto como pilar evaluador en operaciones de segmentación y filtrado.

UnaryOperator<T>: Constituye una reducción morfológica de la rama Function, operando bajo el axioma inquebrantable de que el punto de partida procesado y la respuesta arrojada residen idénticamente en el tipo dimensional T.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### import java.util.Arrays;
import java.util.List;

public class Principal {
    public static void main(String[] args) {
        List<Integer> secuenciaNumerica = Arrays.asList(-8, 12, -4, 9, 0, 22);

        // La responsabilidad iterativa recae sobre el ecosistema de la lista
        secuenciaNumerica.forEach(valor -> {
            if (valor > 0) {
                System.out.println("Localizado factor positivo: " + valor);
            }
        });
    }
}

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### La formalización acotada de las firmas abstractas en métodos como forEach recurre a la directiva genérica <? super T> con el propósito de infundir resiliencia en ecosistemas con herencias de objetos estratificadas. El fundamento lógico detrás de esta instrucción reposa en el acrónimo PECS (Producer Extends, Consumer Super). La regla establece analíticamente que ante componentes encargados de extraer o producir entidades, se exige el operador extends para asumir flexibilidad de subtipos; inversamente, cuando un componente precisa consumir entidades, se emplea la acotación super posibilitando tolerar consumidores diseñados en jerarquías primarias (contravarianza).

Al dictaminar que una colección poblada por elementos Integer (T) será despachada iterativamente mediante Consumer<? super T>, se autoriza la recepción de cualquier consumidor que sea capaz de lidiar no solamente con un riguroso Integer, sino con abstracciones superiores como lo son Number o Object. Prescindir de la acotación relacional y exigir un Consumer<T> exacto derivaría fatalmente en el rechazo en tiempo de compilación frente a un método analítico que tuviese la potestad y compatibilidad comprobada de trabajar sobre la clase madre pura.

Trasladando la regla de seguridad PECS al escenario planteado por la función modular transformar, su reescritura técnica se alejaría del esquema determinista Function<T, R>. La estructura inviolable implicaría declararla bajo Function<? super T, ? extends R>. Con esto se afirma que la tarea inyectable tiene permiso de consumir un objeto tan basal o más alto jerárquicamente que T, y que la emisión funcional producida por su computación es libre de enmarcarse en el nodo idéntico de R o descender válidamente hacia cualquiera de sus derivados adyacentes.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### // JavaScript
class PersonaJS {
    constructor(nombre) {
        this.nombre = nombre;
    }
    saludar() {
        console.log("Saludos desde JS, usuario: " + this.nombre);
    }
}

let individuoJS = new PersonaJS("Elena");
// Preservación indispensable del contexto de instanciación
let accionSaludarJS = individuoJS.saludar.bind(individuoJS);
accionSaludarJS();

// Java
class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Saludos desde Java, usuario: " + this.nombre);
    }
}

public class Principal {
    public static void main(String[] args) {
        Persona individuoJava = new Persona("Marcos");

        // Captura representativa del método de instancia en la interfaz Runnable
        Runnable accionSaludarJava = individuoJava::saludar;

        // Ejecución propagada
        accionSaludarJava.run();
    }
}


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### El analizador semántico en Java clasifica las estructuras posicionales originadas por el operador binario (::) fragmentándolas en cuatro taxonomías estructurales. La identificación del tipo radica en la fuente invocable (si deviene de matriz estática o si requiere la inyección instanciada) sumado al modelo funcional de compatibilidad exigido para alojar la operación.

Dicha categorización establece un marco estricto que rige cómo se suministran y ordenan los parámetros implícitos:

Referencia a método estático (Clase::metodoEstatico): El bloque procesador recibe los argumentos directamente a la entidad de la clase sin contextualización de instancia previa.

Referencia a constructor (Clase::new): Configura delegación a modo de fábrica de construcción; el ensamblaje asimila las entradas y resuelve en una nueva entidad de la memoria.

Referencia a método en instancia concreta (instancia::metodo): La evaluación queda anclada intrínsecamente a un objeto fijo definido prematuramente, transcurriendo los argumentos funcionales únicamente como parámetros regulares al mismo.

Referencia a método de instancia arbitraria (Clase::metodo): La resolución asume implícitamente que el factor de inicio en el parámetro será la instancia objeto sobre la que se ejecutará el comportamiento; los consecuentes conformarán su argumentario funcional.

Java
import java.util.function.Function;
import java.util.function.Supplier;
import java.util.function.Consumer;

class BloqueReferencial {
    // 1. Matriz estática
    public static void volcarConsola(String txt) { System.out.println(txt); }
    // 3 y 4. Acción anclada a instanciación
    public void registrarEstado() { System.out.println("Estado capturado."); }
}

public class Principal {
    public static void main(String[] args) {
        // [Tipo 1] Invocación Estática delegada en Consumer
        Consumer<String> refEstatica = BloqueReferencial::volcarConsola;

        // [Tipo 2] Invocación al Constructor delegada en Supplier
        Supplier<BloqueReferencial> refFabrica = BloqueReferencial::new;

        // [Tipo 3] Invocación vinculada a Instancia Concreta
        BloqueReferencial entidadViva = refFabrica.get();
        Runnable refConcreta = entidadViva::registrarEstado;

        // [Tipo 4] Invocación vinculada a Instancia Arbitraria de Tipo
        Function<String, String> refArbitraria = String::toLowerCase;
    }
}


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

class Individuo {
    private String nombre;
    private int edad;

    public Individuo(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }

    @Override
    public String toString() { return nombre + " [" + edad + "]"; }
}

public class Principal {
    public static void main(String[] args) {
        List<Individuo> registro = Arrays.asList(
            new Individuo("Xavier", 42),
            new Individuo("Alicia", 28),
            new Individuo("Bernardo", 42)
        );

        // ENFOQUE 1: Procedimiento evaluativo mediante expresión Lambda manual
        Collections.sort(registro, (agenteA, agenteB) -> {
            int divergenciaNumerica = Integer.compare(agenteA.getEdad(), agenteB.getEdad());
            if (divergenciaNumerica == 0) {
                return agenteA.getNombre().compareTo(agenteB.getNombre());
            }
            return divergenciaNumerica;
        });
        System.out.println("Criterio procesado manualmente: " + registro);

        // Perturbación del vector para test funcional consecutivo
        Collections.shuffle(registro);

        // ENFOQUE 2: Procedimiento fluente empleando enlazado funcional Comparator
        Collections.sort(registro, Comparator
                .comparingInt(Individuo::getEdad)
                .thenComparing(Individuo::getNombre)
        );
        System.out.println("Criterio procesado estandarizado: " + registro);
    }
}
```</T,></T></T></T></T></T></T></T,></T,></Double,></String,>
