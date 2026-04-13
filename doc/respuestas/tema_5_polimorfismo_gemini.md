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

### El polimorfismo es uno de los pilares fundamentales de la programación orientada a objetos. Etimológicamente significa "múltiples formas" y, en el contexto de la programación, se refiere a la capacidad que tiene una referencia de una clase base (o superclase) de apuntar a objetos de cualquiera de sus clases derivadas y comportarse de manera diferente según el tipo exacto del objeto al que apunta en ese momento.

Su utilidad principal radica en la posibilidad de escribir código genérico, flexible y fácilmente extensible. En lugar de tener que escribir estructuras de control condicionales (como múltiples sentencias if o switch) para comprobar el tipo exacto de un objeto antes de realizar una acción, se invoca un método común y el propio objeto asume la responsabilidad de ejecutar el comportamiento que le corresponde. Esto facilita enormemente la adición de nuevos tipos en el futuro sin modificar el código que ya existe.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### La ligadura dinámica (también conocida como enlace tardío o late binding) es el proceso mediante el cual el sistema retrasa la decisión de qué bloque de código ejecutar hasta el momento de ejecución del programa (runtime), en lugar de tomar esta decisión durante la compilación (compile time). Al invocarse un método a través de una referencia, el entorno de ejecución evalúa cuál es el tipo real del objeto en memoria para llamar a la implementación correcta.

Este concepto tiene una relación intrínseca e inseparable con el polimorfismo. El polimorfismo es el concepto teórico o el efecto visible (la capacidad de tratar objetos distintos uniformemente), mientras que la ligadura dinámica es el mecanismo técnico subyacente que lo hace posible. Sin ligadura dinámica, la llamada al método siempre se resolvería basándose en el tipo de la variable de referencia, anulando el comportamiento polimórfico.

La necesidad de indicar esta ligadura explícitamente depende fuertemente del diseño del lenguaje de programación. En C++, la ligadura es estática por defecto para mejorar el rendimiento; si se desea ligadura dinámica, es obligatorio emplear explícitamente la palabra reservada virtual en la declaración del método en la clase base. Por el contrario, en Java, la filosofía es totalmente orientada a objetos: todos los métodos de instancia no privados operan con ligadura dinámica por defecto, sin necesidad de usar ninguna palabra clave especial. En el caso de lenguajes de tipado dinámico como Python, el concepto se lleva al extremo mediante el duck typing; no existe la comprobación de tipos en compilación y la ligadura siempre ocurre en tiempo de ejecución de manera natural.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### class Soldado {
    public void saludar() {
        System.out.println("Soldado raso a sus ordenes.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("¡Zapador listo para detonar obstáculos!");
    }
}

class Artillero extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Artillero en posición, cañones preparados.");
    }
}

public class MainPolimorfismo {
    public static void main(String[] args) {
        // Se crea un array de tipo base y se llena con objetos de las subclases
        Soldado[] peloton = new Soldado[3];
        peloton[0] = new Zapador();
        peloton[1] = new Artillero();
        peloton[2] = new Soldado();

        // Se recorre el array empleando una referencia genérica
        for (int i = 0; i < peloton.length; i++) {
            Soldado s = peloton[i];
            s.saludar(); // Ligadura dinámica en acción
        }
    }
}


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Sí, es completamente posible y una práctica muy común en diseño de software orientado a objetos. Frecuentemente, al sobreescribir un método, no se busca descartar el trabajo que ya realiza la clase padre, sino ampliarlo o especializarlo. De este modo, se puede ejecutar la lógica original y, a continuación o previamente, ejecutar el código adicional pertinente a la subclase. 

class Zapador extends Soldado {
    @Override
    public void saludar() {
        // Se invoca el método original de la clase base
        super.saludar();
        // Se añade el comportamiento específico
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Existen reglas estrictas al sobreescribir un método. El nombre del método y la lista exacta de tipos de parámetros deben ser idénticos. Respecto al tipo de retorno, a partir de versiones modernas de Java, se permite que sea covariante, es decir, el método sobreescrito puede retornar el mismo tipo o una subclase del tipo de retorno original. Además, el nivel de visibilidad no puede ser más restrictivo que el del método padre (por ejemplo, no se puede sobreescribir un método public y hacerlo private), y no se pueden declarar excepciones comprobadas (checked exceptions) más amplias que las originales.

Es vital no confundir sobreescritura con sobrecarga. La sobreescritura (overriding) requiere herencia y consiste en reemplazar el comportamiento de un método base manteniendo su firma, resuelto esto mediante polimorfismo en tiempo de ejecución. Por el contrario, la sobrecarga (overloading) ocurre dentro de una misma clase (o heredando) y consiste en tener múltiples métodos con el mismo nombre pero con diferentes listas de parámetros. La decisión de qué método sobrecargado ejecutar se resuelve estáticamente en tiempo de compilación según los argumentos pasados.

La anotación @Override es un metadato que se coloca justo encima de la declaración del método en la subclase. Su propósito es informar explícitamente al compilador de la intención de sobreescribir un método del padre. Si debido a un error tipográfico o a una discrepancia en los parámetros no se produce una sobreescritura real (cayendo accidentalmente en una sobrecarga o en un método nuevo), el compilador arrojará un error, previniendo así errores lógicos muy difíciles de rastrear. Por ello, su uso se considera una buena práctica fundamental.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Efectivamente, en Java el polimorfismo se encuentra presente desde las etapas más tempranas del aprendizaje. A diferencia de C++, donde se puede programar sin usar orientación a objetos, el diseño de Java fuerza a que cualquier pieza de código pertenezca a una clase y participe en una gran jerarquía. Todo objeto creado hereda, directa o indirectamente, de una superclase universal llamada Object.

Por lo tanto, al proporcionar una implementación propia para métodos heredados como toString() o equals(), se está ejerciendo el mecanismo del polimorfismo de manera directa. Cuando métodos de la biblioteca estándar, como System.out.println(), reciben una instancia genérica y solicitan su representación en texto llamando a toString(), la ligadura dinámica asegura que se ejecute la versión específica escrita por el programador para esa clase particular, y no la genérica definida en la clase base Object.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Una clase abstracta es un tipo de clase que se diseña exclusivamente para ser heredada, funcionando como un contrato o plantilla estructural para sus subclases. Un método abstracto, por su parte, es un método que se declara con su firma pero carece de implementación (no tiene llaves con código, termina en punto y coma); su única finalidad es obligar a todas las subclases concretas a proveer el comportamiento necesario.

Por definición, no es posible crear instancias de una clase abstracta mediante el operador new. Al carecer de la implementación completa de todos sus comportamientos (los métodos abstractos), la creación directa de un objeto generaría una entidad incompleta e inconsistente. La palabra clave abstract debe colocarse en dos lugares: en la firma de la clase general y en la firma de cada método que carezca de cuerpo.

abstract class Soldado {
    public void saludar() {
        System.out.println("Soldado raso a sus ordenes.");
    }
    
    // Método abstracto: fuerza a los hijos a implementarlo
    public abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Zapador plantando explosivos C4.");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Artillero disparando obús de 155mm.");
    }
}


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### La palabra clave final actúa como un restrictor definitivo dentro del diseño orientado a objetos. Cuando se aplica a la declaración de una clase, indica categóricamente que dicha clase no puede tener subclases (es imposible heredar de ella). Si se aplica a un método específico dentro de una clase, permite la herencia de la clase en sí, pero prohíbe explícitamente que las subclases puedan sobreescribir ese método en particular.

Su relación con el polimorfismo es de contención o límite. El uso de final detiene la cadena de polimorfismo y ligadura dinámica para esa jerarquía o comportamiento particular. Se utiliza cuando el diseñador del sistema considera que un comportamiento es crítico, seguro o completo y no desea que extensiones futuras alteren esa lógica fundamental, garantizando así un comportamiento predecible.

En la API estándar de Java existen numerosos ejemplos, siendo el más representativo la clase String. Al estar declarada como final, se garantiza su inmutabilidad y seguridad en todo el entorno de ejecución, evitando que un programador cree un "StringHackeable" que se comporte de manera anómala en sistemas que dependen de la integridad de los textos (como accesos a bases de datos o rutas de red). Otro ejemplo clásico es la clase Math, que contiene operaciones matemáticas esenciales.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Una interfaz en Java es una estructura fundamental que define un contrato estricto de comportamiento. Tradicionalmente, se componía únicamente de constantes y firmas de métodos totalmente abstractos, actuando como un molde puro de lo que una clase debe saber hacer. Las clases que deciden acatar ese contrato utilizan la palabra reservada implements y se comprometen a proporcionar el código real para todas las operaciones definidas.

Aunque guardan similitudes con las clases abstractas (tampoco pueden instanciarse de forma directa y sirven para agrupar tipos polimórficos), su propósito es distinto. Una clase abstracta captura relaciones de identidad fundamental (qué es el objeto, como "un Zapador es un Soldado") y puede mantener estado (atributos) o código base compartido. Una interfaz define roles o capacidades adicionales (qué puede hacer el objeto, como "SerImprimible" o "SerAlmacenable") sin aportar estado interno.

La mayor diferencia estructural respecto a las clases radica en la herencia. Mientras que Java prohíbe tajantemente la herencia múltiple de clases para evitar conflictos de diseño (el llamado problema del diamante en C++), sí permite que una misma clase implemente una cantidad ilimitada de interfaces separadas por comas. Esto otorga al lenguaje una flexibilidad extrema para construir tipos complejos sin sufrir las ambigüedades técnicas de la herencia múltiple tradicional.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### abstract class Punto {
    public abstract double calcularDistanciaA(Punto p);
}

class Punto2D extends Punto {
    double x, y;
    public Punto2D(double x, double y) { this.x = x; this.y = y; }

    @Override
    public double calcularDistanciaA(Punto p) {
        if (p instanceof Punto2D) {
            // Downcasting necesario para acceder a x e y del parámetro
            Punto2D p2 = (Punto2D) p;
            return Math.sqrt(Math.pow(this.x - p2.x, 2) + Math.pow(this.y - p2.y, 2));
        }
        throw new IllegalArgumentException("Incompatible, se requiere Punto2D");
    }
}

class Punto3D extends Punto {
    double x, y, z;
    public Punto3D(double x, double y, double z) { this.x = x; this.y = y; this.z = z; }

    @Override
    public double calcularDistanciaA(Punto p) {
        if (p instanceof Punto3D) {
            Punto3D p3 = (Punto3D) p;
            return Math.sqrt(Math.pow(this.x - p3.x, 2) + 
                             Math.pow(this.y - p3.y, 2) + 
                             Math.pow(this.z - p3.z, 2));
        }
        throw new IllegalArgumentException("Incompatible, se requiere Punto3D");
    }
}

class Linea {
    private Punto origen;
    private Punto destino;

    public Linea(Punto origen, Punto destino) {
        this.origen = origen;
        this.destino = destino;
    }

    public double longitud() {
        // El polimorfismo se encarga de llamar a la versión 2D o 3D
        return origen.calcularDistanciaA(destino);
    }
}


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### La herencia de interfaces en Java es un mecanismo que permite que una interfaz tome como base otra interfaz previamente existente para extender su contrato. En lugar de modificar la interfaz original (lo que rompería la compatibilidad de código heredado), se crea una nueva que hereda las responsabilidades de su predecesora empleando la misma palabra clave extends. Así, cualquier clase que implemente la interfaz hija estará obligada a cumplir tanto los requisitos nuevos como los heredados.

Respecto a la herencia múltiple de interfaces, sí existe y está completamente soportada en Java. Puesto que históricamente las interfaces no dictaban cómo debían hacerse las cosas, sino solamente qué se debía hacer, no existe conflicto de implementaciones superpuestas. Por tanto, una interfaz puede extender de varias interfaces al mismo tiempo separándolas por comas (ej. interface C extends A, B).

interface Fichero {
    String leerContenido();
}

// FicheroEscribible hereda de Fichero.
// Quien implemente FicheroEscribible deberá proveer los 3 métodos.
interface FicheroEscribible extends Fichero {
    void escribirContenido(String texto);
    void eliminarFichero();
}

class DocumentoTexto implements FicheroEscribible {
    private String contenido = "";

    @Override
    public String leerContenido() {
        return this.contenido;
    }

    @Override
    public void escribirContenido(String texto) {
        this.contenido += texto;
    }

    @Override
    public void eliminarFichero() {
        this.contenido = "";
        System.out.println("Fichero virtual borrado.");
    }
}

