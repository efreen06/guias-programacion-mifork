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

### public class ContenedorUniversal {
    private Object[] elementos;
    private int tamaño;

    public ContenedorUniversal(int capacidad) {
        elementos = new Object[capacidad];
        tamaño = 0;
    }

    public void añadir(Object elemento) {
        if (tamaño < elementos.length) {
            elementos[tamaño++] = elemento;
        }
    }

    public Object obtener(int indice) {
        return elementos[indice];
    }
}

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### La programación genérica es un paradigma de diseño de software que permite definir algoritmos y estructuras de datos de manera independiente a los tipos de datos sobre los que van a operar. El objetivo principal consiste en maximizar la reutilización del código, escribiendo la lógica una sola vez y permitiendo que se adapte automáticamente a diferentes tipos bajo el respaldo y control del compilador.

El ejemplo anterior, basado en el uso sistemático de Object o void*, representa una forma rudimentaria de lograr esta generalización. Aunque cumple funcionalmente con el propósito de almacenar distintos tipos de datos en una misma estructura sin duplicar el código, carece de los mecanismos modernos asociados formalmente al término "programación genérica" en la actualidad.

Específicamente, esas aproximaciones clásicas delegan en el programador toda la responsabilidad de llevar un control riguroso de los tipos almacenados. Por lo tanto, aunque conceptualmente buscan la abstracción, no se consideran ejemplos genuinos de programación genérica moderna, puesto que esta última implica que el propio lenguaje ofrezca herramientas para garantizar la coherencia y seguridad de los tipos tratados.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### El principal problema de emplear Object en Java o void* en C para diseñar estructuras genéricas reside en la pérdida total del chequeo de tipos durante la fase de compilación. Cuando un elemento se inserta en un contenedor de esta naturaleza, el compilador ignora o "olvida" su tipo original, asumiendo exclusivamente que se trata del tipo universal abstracto.

Como consecuencia de esta pérdida de información, al extraer un elemento es estrictamente necesario realizar un downcasting (conversión forzada hacia un tipo más específico) para poder acceder a los métodos y propiedades originales del dato. Esta operación introduce una vulnerabilidad crítica, ya que el compilador carece de mecanismos para verificar si dicha conversión manual es semánticamente válida o correcta.

Si se asume erróneamente el tipo de un elemento recuperado y se ejecuta un cast hacia un tipo incompatible, el programa no presentará fallos de compilación, sino que experimentará un error fatal en tiempo de ejecución (como una excepción ClassCastException en Java). Este comportamiento traslada los errores desde una fase temprana de detección hacia la fase de ejecución, complicando el mantenimiento y la estabilidad del código.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Los parámetros de tipo constituyen el mecanismo fundamental introducido en lenguajes modernos orientados a objetos para solventar las deficiencias de seguridad inherentes al uso de tipos universales como Object. Operan conceptualmente como variables, pero en lugar de representar valores numéricos o cadenas en tiempo de ejecución, representan tipos de datos que serán concretados en tiempo de compilación.

Al declarar una clase, interfaz o método, se introducen identificadores encerrados entre símbolos de menor y mayor (típicamente letras mayúsculas como <T>, <E> o <K, V>) que funcionan como marcadores de posición temporales. Cuando se decide instanciar dicha estructura, se sustituye ese parámetro abstracto por un tipo concreto, como String o una clase definida por el usuario, informando al compilador sobre la naturaleza exacta de los datos que fluirán por el código.

Gracias a la implementación de los parámetros de tipo, el compilador adquiere la capacidad de ejecutar un análisis estático exhaustivo. Este proceso garantiza automáticamente que todas las operaciones de inserción correspondan al tipo declarado y elimina por completo la necesidad de realizar conversiones explícitas o casts al recuperar la información, haciendo que las interacciones sean seguras.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### #include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> lista;
    lista.push_back("Programación");
    lista.push_back("Genérica");

    for(const std::string& elemento : lista) {
        // Se puede llamar a .length() sin conversiones
        std::cout << elemento.length() << std::endl;
    }
    return 0;
}



import java.util.ArrayList;
import java.util.List;

public class Principal {
    public static void main(String[] args) {
        List<String> lista = new ArrayList<>();
        lista.add("Programación");
        lista.add("Genérica");

        for(String elemento : lista) {
            // Acceso seguro a métodos de String
            System.out.println(elemento.length());
        }
    }
}


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### El tratamiento que aplican los compiladores a la programación genérica presenta diferencias arquitectónicas profundas entre C++ y Java. Cada lenguaje adopta una estrategia distinta para gestionar los parámetros de tipo y generar el código que finalmente será ejecutado por la máquina.

En C++, el proceso se denomina instanciación de plantillas. Cuando el compilador detecta el uso de una plantilla (como un vector) con un tipo concreto (por ejemplo, vector<int> y vector<string>), procede a escribir y compilar código fuente completamente independiente para cada tipo involucrado. En tiempo de ejecución existirán clases separadas en memoria; esta técnica maximiza el rendimiento al evitar conversiones en ejecución, aunque puede aumentar considerablemente el tamaño del archivo ejecutable (fenómeno conocido como code bloat).

Por el contrario, Java implementa una técnica denominada type erasure (borrado de tipos). Durante el proceso de compilación, toda la información relativa a los parámetros de tipo se elimina, reemplazando las referencias genéricas por la clase Object (o por un límite superior establecido). Adicionalmente, el compilador inyecta automáticamente de forma invisible los casts necesarios. Esto permite que exista una única clase compilada en la Máquina Virtual de Java para todas las variantes instanciadas, priorizando el ahorro de memoria y asegurando la retrocompatibilidad estricta con versiones antiguas de Java previas a la introducción de generics.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### public class Par<T, U> {
    private final T primero;
    private final U segundo;

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


public class CalculadoraEstadistica {
    public static Par<Double, Double> calcularMediaYDesviacion(double[] datos) {
        double media = 0.0;
        // Lógica matemática para calcular la media...
        
        double desviacion = 0.0;
        // Lógica matemática para calcular la desviación...

        return new Par<>(media, desviacion);
    }
}


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### import java.util.Random;

public class Selector {
    private static final Random random = new Random();

    // Versión utilizando Object
    public static Object seleccionaUnoObject(Object a, Object b) {
        return random.nextBoolean() ? a : b;
    }

    // Versión utilizando método genérico
    public static <T> T seleccionaUnoGenerico(T a, T b) {
        return random.nextBoolean() ? a : b;
    }
}


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Es completamente posible restringir los tipos que puede adoptar un parámetro genérico utilizando la sintaxis de límites superiores o bounded type parameters. Empleando la palabra reservada extends seguida de una clase (por ejemplo, <T extends Number>), se instruye al compilador para que acepte únicamente instancias de esa clase o de sus descendientes, lo que habilita la invocación segura de los métodos definidos en la clase límite.

Para ilustrar el desarrollo de un Punto con coordenadas puramente numéricas, la primera solución elude el uso de generics y se apoya en el polimorfismo tradicional. Se utiliza directamente la superclase Number para tipar las coordenadas, admitiendo cualquier subclase numérica derivada, pero sacrificando el conocimiento del tipo específico.




## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Al evaluar las restricciones de homogeneidad, se evidencia una diferencia notoria en el rigor con el que cada solución gestiona los datos internos. La aproximación basada en el tipo genérico polimórfico Number carece de mecanismos para forzar uniformidad. Por tanto, esta solución permite crear inadvertidamente un punto híbrido, insertando un objeto Integer para la coordenada X y un objeto Double para la coordenada Y sin provocar objeciones por parte del compilador.

En contraposición, la solución estructurada con el parámetro de clase <T extends Number> implementa un refuerzo estricto en el chequeo de tipos. Cuando un desarrollador instancía el objeto (por ejemplo, declarando new PuntoGenerico<Integer>(...)), el tipo queda fijado para toda la estructura. En consecuencia, el compilador rechazará la instanciación si las coordenadas suministradas en los argumentos del constructor presentan discrepancias, impidiendo que una coordenada sea de tipo entero y la otra de tipo real.

Respecto al retorno de la información, las implicaciones son igualmente divergentes. El método getX() de la versión desprovista de generics devuelve en todo momento una referencia de tipo Number; para operar matemáticamente o acceder a particularidades del número introducido, el programador estará obligado a realizar una conversión de tipos explícita. Por el contrario, en la versión genérica, getX() devuelve el tipo preciso utilizado al crear el objeto (como un Integer), garantizando seguridad y eliminando la necesidad de conversiones en el código cliente.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.


### Para evitar el uso de validaciones de tipo en tiempo de ejecución (mediante instanceof) y su subsecuente downcasting, se recurre a un patrón de diseño avanzado de generics a veces denominado "F-Bounded Polymorphism". Esta técnica consiste en parametrizar la propia interfaz con el tipo de la clase hija que va a implementarla, logrando un vínculo de tipos seguro a nivel de firma de métodos.

El primer paso consiste en transformar la interfaz original. Se introduce un parámetro de tipo T que se restringe a subtipos de la propia interfaz Punto<T>. Posteriormente, se utiliza este parámetro T para definir rígidamente el argumento que espera el método distanciaA.

Java
public interface Punto<T extends Punto<T>> { 
    public double distanciaA(T p); 
} 
Una vez asegurada la interfaz, las clases de implementación concretas se declaran implementando Punto y proporcionando su propia identidad de clase como parámetro de tipo. Este simple cambio instruye al compilador para que exija matemáticamente que el cálculo en Punto2D se realice contra otro Punto2D, y el de Punto3D contra otro Punto3D.

Java
public class Punto2D implements Punto<Punto2D> { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto2D p) { 
        // El compilador garantiza el tipo, eliminando el instanceof y el cast
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2)); 
    } 
} 

public class Punto3D implements Punto<Punto3D> { 
     private final double x, y, z;
     public Punto3D(double x, double y, double z) {
         this.x = x; this.y = y; this.z = z;
     }

     @Override
     public double distanciaA(Punto3D p) {
         return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2) + Math.pow(z - p.z, 2));
     }
}


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### A pesar de que existe una relación jerárquica clara donde String es una subclase de Object, en el ecosistema de Java una lista genérica parametrizada como List<String> no se considera un subtipo de List<Object>. Esta decisión de diseño es intencionada para preservar la seguridad de los tipos: si el lenguaje permitiera esta asignación, se podría introducir erróneamente un número entero en la referencia apuntada por List<Object>, corrompiendo la lista de cadenas original.

Por el contrario, los arreglos primitivos en Java poseen una naturaleza distinta y String[] sí es considerado un subtipo legítimo de Object[]. Este comportamiento se definió históricamente en las versiones iniciales del lenguaje para permitir cierta flexibilidad en métodos utilitarios antes de que existieran los generics. El problema inherente a esta concesión es que permite asignaciones inválidas que el compilador aprueba pasivamente, pero que resultarán en la interrupción abrupta de la aplicación (generando una excepción ArrayStoreException) si se intenta almacenar un tipo incompatible en memoria durante la ejecución.

Estos comportamientos contrapuestos establecen los conceptos de varianza de tipos. Un tipo es invariante si carece de relaciones de subtipado genéricas a pesar de la herencia de sus parámetros (el comportamiento seguro de las colecciones genéricas en Java). Un tipo es covariante si respeta el sentido de la herencia de sus componentes, permitiendo asignar un conjunto de hijos a una referencia de padres (el modelo de los arreglos). Finalmente, un tipo es contravariante si invierte el sentido de la herencia, permitiendo asignar una estructura genérica basada en la superclase a una referencia que espera la subclase.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Un wildcard o comodín, representado en el código mediante el símbolo de interrogación (?), es un elemento de la sintaxis de los generics en Java que representa un tipo de dato indeterminado. Su función principal consiste en flexibilizar las rígidas normas de la invarianza en las declaraciones de variables o argumentos, permitiendo a los desarrolladores recuperar controladamente comportamientos covariantes o contravariantes según las necesidades operativas de una función.

La distinción entre sus dos vertientes radica en la dirección de la limitación del tipo genérico. La declaración List<? extends T> establece un "límite superior", representando cualquier clase que descienda de T. Esta modalidad se emplea exclusivamente en contextos de extracción de información (solo lectura), garantizando la recuperación segura del dato, pero imposibilitando inserciones por desconocerse la clase exacta de la colección. Inversamente, List<? super T> establece un "límite inferior", denotando cualquier superclase en la jerarquía por encima de T. Esta variante se destina a contextos de inserción de datos (escritura), donde se certifica que la colección puede acomodar al menos elementos del tipo T.

A continuación se ejemplifica el uso adecuado de ambas sintaxis en función del principio de lectura frente a escritura. El primer caso aplica covarianza para procesar de forma unificada listas de cualquier subtipo derivado de números, mientras que el segundo recurre a la contravarianza para inyectar enteros con seguridad en listas que puedan representarlos adecuadamente.

Java
import java.util.List;

public class OperacionesListas {
    
    // (i) Uso de límite superior para recuperar datos con seguridad (Covarianza)
    public static double sumarNumeros(List<? extends Number> lista) {
        double total = 0.0;
        for (Number n : lista) {
            total += n.doubleValue(); // Operación exclusiva de lectura
        }
        return total;
    }

    // (ii) Uso de límite inferior para insertar datos con seguridad (Contravarianza)
    public static void añadirEnteros(List<? super Integer> lista) {
        lista.add(10); // Operación segura de escritura
        lista.add(25);
    }
}
