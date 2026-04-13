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
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### La herencia es un pilar de la orientación a objetos que permite crear una nueva clase a partir de una existente. Se establece una relación semántica denominada "es-un", lo que implica que la subclase es una especialización de la superclase. Esta relación conlleva dos consecuencias fundamentales: la herencia de estado y comportamiento, por la cual la subclase recibe los atributos y métodos de la superclase, y la compatibilidad de tipos, que permite tratar a un objeto de la subclase como si fuera de la superclase.

La compatibilidad de tipos (polimorfismo) es especialmente potente, ya que permite que una referencia de un tipo superior apunte a instancias de cualquier tipo derivado. Esto facilita el procesamiento uniforme de colecciones de objetos heterogéneos, siempre que compartan un ancestro común en la jerarquía.

public class Soldado {
    private String nombre;

    public Soldado(String nombre) { this.nombre = nombre; }
    public void saludar() { System.out.println("Soldado " + nombre + " presente."); }
    public String getNombre() { return nombre; }
}

public class Artillero extends Soldado {
    private int cohetes;
    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }
    public int getCohetes() { return cohetes; }
}

public class Zapador extends Soldado {
    private int minas;
    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }
    public int getMinas() { return minas; }
}

// Ejemplo de uso y compatibilidad de tipos
public class Main {
    public static void main(String[] args) {
        Soldado[] peloton = {
            new Artillero("Lopez", 10),
            new Zapador("Gomez", 5)
        };
        for (Soldado s : peloton) {
            s.saludar(); // Todos saludan por ser Soldados
        }
    }
}


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Cuando se instancia un objeto de una subclase, se produce una reacción en cadena de constructores. El orden de ejecución es siempre de arriba hacia abajo en la jerarquía: primero se ejecuta el constructor de la superclase y, una vez finalizado este, se ejecuta el de la subclase. Esto garantiza que la parte del objeto que corresponde a la base esté correctamente inicializada antes de que la subclase intente añadir o modificar su estado específico.

La palabra reservada super se utiliza dentro del constructor de la subclase para invocar explícitamente a un constructor de la superclase. Si la clase base no posee un constructor sin parámetros (constructor por defecto) o este no es visible, el programador está obligado a realizar una llamada explícita a super(...) como primera instrucción del constructor de la subclase, pasando los argumentos necesarios. De no hacerlo, el compilador de Java generará un error de compilación.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Es un error común pensar que los atributos privados de la superclase no existen en la subclase. En realidad, cuando se crea una instancia de una subclase (como Zapador), el objeto en memoria reserva espacio para todos los atributos definidos en la jerarquía, incluyendo los privados de Soldado. El objeto es una entidad única que contiene tanto el estado de la base como el de la extensión.

Sin embargo, que existan en memoria no significa que sean accesibles desde el código de la subclase. El principio de encapsulación se mantiene: los métodos de Zapador no pueden acceder directamente a la variable nombre si esta es privada en Soldado. Para interactuar con dicho estado, la subclase debe emplear los métodos públicos o protegidos que la superclase haya proporcionado (como un "getter" o el propio constructor mediante super).

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### La compatibilidad de tipos dota al código de una gran capacidad de extensión. Al programar funciones o bucles que operan sobre la clase base (Soldado), el código se vuelve "agnóstico" a las implementaciones concretas. Esto permite añadir nuevos tipos de soldados en el futuro sin necesidad de modificar, recompilar o incluso conocer la lógica que gestiona el conjunto de soldados.

public class Explorador extends Soldado {
    public Explorador(String nombre) { super(nombre); }
    // No hace falta tocar el bucle 'for' del ejemplo anterior
}


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### En Java, una referencia del supertipo puede apuntar a un objeto del subtipo (esto se conoce como upcasting y es automático). Sin embargo, a través de esa referencia solo se pueden invocar los métodos que estén definidos en el supertipo. Aunque el objeto real sea un Artillero, si la referencia es de tipo Soldado, no se podrá llamar a getCohetes() directamente, ya que el compilador solo garantiza que el objeto tiene las capacidades de un Soldado.

Para acceder a las funcionalidades específicas del subtipo, se debe realizar un downcasting (conversión explícita). Dado que esta operación es arriesgada, se emplea el operador instanceof para verificar el tipo real del objeto en tiempo de ejecución. Esto evita excepciones de tipo ClassCastException si el objeto no resulta ser del tipo esperado.

for (Soldado s : peloton) {
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // Downcasting seguro
        System.out.println("Munición: " + a.getCohetes());
    }
}

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### El modificador de acceso protected es un punto intermedio entre public y private. Un atributo o método marcado como protected es accesible por cualquier clase que se encuentre en el mismo paquete y, lo más importante, por cualquier subclase, independientemente del paquete en el que se encuentre. Esto permite que las subclases hereden capacidades o datos que no se desean exponer al resto del mundo (clientes de la clase), pero que son necesarios para la especialización.

En Java, se implementa anteponiendo la palabra protected a la declaración del miembro. En el caso del Zapador, si el nombre en Soldado fuera protegido, el método ponerMina() podría utilizar ese nombre directamente para imprimir un mensaje personalizado, manteniendo el atributo oculto para clases que no formen parte de la jerarquía o el paquete.

public class Soldado {
    protected String nombre;
    // ...
}

public class Zapador extends Soldado {
    public void ponerMina() {
        System.out.println(nombre + " está colocando una mina."); // Acceso directo por ser protected
    }
}



## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### En la orientación a objetos, no todos los lenguajes imponen una raíz común. Por ejemplo, en C++, es posible crear jerarquías totalmente independientes entre sí. Sin embargo, en Java, se ha optado por un modelo de jerarquía única: todas las clases heredan, de forma directa o indirecta, de la clase java.lang.Object.

Si al definir una clase no se especifica una cláusula extends, Java hace que esa clase herede automáticamente de Object. Esto garantiza que todos los objetos en Java compartan comportamientos mínimos y universales, como la capacidad de compararse (equals), convertirse a cadena de texto (toString) o proporcionar su identidad de memoria (hashCode).


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### La herencia múltiple es la capacidad de una clase de heredar estado y comportamiento de más de una superclase simultáneamente. Aunque algunos lenguajes como C++ la permiten, este concepto introduce problemas de ambigüedad técnica conocidos como el "problema del diamante" (cuando dos superclases tienen métodos con la misma firma y la subclase no sabe cuál elegir).

En Java, no existe la herencia múltiple de clases. Una clase solo puede extender a una única superclase (extends). Para suplir la necesidad de que un objeto cumpla con múltiples roles o tipos, Java ofrece las interfaces, que permiten una forma de herencia múltiple de tipos (pero no de estado), evitando así las colisiones y ambigüedades de la herencia múltiple tradicional.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Las excepciones en Java son objetos y, como tales, pueden extenderse para crear tipos de error específicos del dominio de la aplicación. Al heredar de RuntimeException, se crea una excepción no controlada (unchecked), lo que significa que el programador no está obligado a declararla en el throws ni a capturarla con un bloque try-catch, simplificando el flujo en errores lógicos.

Una excepción personalizada puede contener atributos adicionales mediante composición (como un objeto Usuario). Además, es una buena práctica incluir un constructor que acepte un objeto Throwable para permitir el "encadenamiento de excepciones", manteniendo la causa original del error.

public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario u) {
        super("No se encontró al usuario: " + u.getNombre());
        this.usuario = u;
    }

    public UsuarioNoEncontradoException(Usuario u, Throwable causa) {
        super("Error al buscar usuario: " + u.getNombre(), causa);
        this.usuario = u;
    }
}


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### No se debe emplear la herencia exclusivamente para reutilizar código porque la herencia establece un vínculo extremadamente fuerte y rígido. Si se hereda de una clase solo para usar sus funciones, se está obligando a que la nueva clase sea compatible con el tipo de la anterior, lo cual puede no tener sentido semántico.

Esta práctica genera jerarquías artificiales y frágiles. Si la superclase cambia su comportamiento en versiones futuras, todas las subclases que heredaron de ella solo por "conveniencia" pueden verse afectadas o romper su lógica interna, incluso si conceptualmente no tenían una relación de parentesco real.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Este principio de diseño sugiere que es preferible construir objetos complejos combinando objetos más simples (composición) que extendiendo clases (herencia). La composición es mucho más flexible porque las relaciones se pueden cambiar en tiempo de ejecución (cambiando una referencia), mientras que la herencia es estática y se define en tiempo de compilación.

Además, la composición reduce el acoplamiento. El objeto contenedor solo conoce la interfaz pública de los objetos que contiene, lo que permite modificar los componentes internos sin que el sistema colapse, facilitando además las pruebas unitarias al poder sustituir componentes reales por simulados (objetos mock).


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Se dice que la herencia rompe la encapsulación porque el diseño de una subclase a menudo depende de los detalles internos de implementación de su superclase. Al heredar, el programador de la subclase suele asumir que ciertos métodos de la superclase funcionan de una forma específica o se llaman entre sí en un orden concreto.

Si el autor de la superclase decide cambiar esos detalles internos en una actualización (por ejemplo, optimizando un método o cambiando qué métodos privados invoca), la subclase puede dejar de funcionar correctamente aunque la interfaz pública de la superclase siga siendo la misma. Por tanto, la subclase "sabe demasiado" sobre la estructura interna de su padre, lo que viola el aislamiento total que pretende la encapsulación.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### public class Persona {
    protected String dni, nombre;
}
public class Estudiante extends Persona { 
    /* ... dni y nombre heredados ... */ 
}
public class Trabajador extends Persona { 
    /* ... dni y nombre heredados ... */ 
}


public class DatosPersonales {
    private String dni, nombre;
    public DatosPersonales(String dni, String nombre) { ... }
}
public class Estudiante {
    private DatosPersonales datos;
    public Estudiante(DatosPersonales datos) { this.datos = datos; }
}
public class Trabajador {
    private DatosPersonales datos;
    public Trabajador(DatosPersonales datos) { this.datos = datos; }
}

