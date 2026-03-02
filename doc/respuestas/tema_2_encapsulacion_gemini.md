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

### Encapsulación: consiste en agrupar los datos (atributos) y los comportamientos (métodos) que operan sobre ellos en una única unidad lógica o entidad, que es la clase.
### Ocultación de información: mecanismo que restringe el acceso directo a los detalles internos de dicha entidad desde el exterior. Ventajas: mayor facilidad de mantenimiento (los cambios en la estructura de los datos internos no obligan a modificar el código externo), aumento de la seguridad y fiabilidad (se evitan modificaciones arbitrarias o accidentales del estado interno), y la reducción de la complejidad global del sistema al exponer únicamente los servicios estrictamente necesarios.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Se entiende por interfaz pública al conjunto de métodos, constructores y atributos (aunque lo ideal es que estos últimos nunca se expongan) que una clase hace accesibles al resto del programa.
### La relación con la ocultación de información es directa y de naturaleza complementaria. Mientras que la ocultación se encarga de esconder el "cómo" se hacen las cosas (la implementación del código y la estructura de las variables), la interfaz pública define exclusivamente el "qué" se puede hacer.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Porque establece un contrato vinculante con el resto del sistema. Una vez que otras clases o programas comienzan a utilizar los métodos públicos de un objeto, se genera una dependencia directa.
### No es fácil cambiar una interfaz pública una vez que el sistema crece. Si se modifica el nombre de un método público, se elimina o se alteran sus parámetros, se "romperá" automáticamente todo el código externo que dependía de él, obligando a localizar, reescribir y recompilar múltiples partes del programa para subsanar el error.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Condiciones lógicas o reglas de dominio que deben cumplirse siempre para que el estado de un objeto se considere válido a lo largo de su ciclo de vida. Por ejemplo, en una clase que represente una fecha, una invariante sería que el mes esté comprendido entre el 1 y el 12; en un sistema bancario, podría ser que el número de cuenta tenga un formato específico.
### La ocultación de información es la herramienta técnica fundamental que permite garantizar que estas invariantes se mantengan intactas. Si los atributos fuesen de acceso libre, cualquier parte del código podría asignar valores inválidos, violando la regla lógica de la clase en cualquier instante sin posibilidad de intervención.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### public class Punto {
    // Atributos privados: Ocultación de información
    private double x;
    private double y;

    // Constructor
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método para calcular la distancia al origen (0,0)
    // Usamos el Teorema de Pitágoras: d = sqrt(x² + y²)
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(Math.pow(this.x, 2) + Math.pow(this.y, 2));
    }

    // Métodos Getter y Setter (Interfaz pública para acceder a los datos)
    public double getX() { return x; }
    public void setX(double x) { this.x = x; }

    public double getY() { return y; }
    public void setY(double y) { this.y = y; }
}

###La interfaz pública es lo que otros programadores pueden usar sin necesidad de saber cómo funciona el código por dentro.
###private (Privado): Indica que el miembro (atributo o método) solo es accesible dentro de la propia clase.
###public (Público): Indica que el miembro es accesible desde cualquier otra clase. Es la "puerta abierta"

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### En Java, los modificadores public y private se pueden aplicar a clases, métodos, atributos y constructores.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### En POO existen más tipos de visibilidad, además de public y private. En Java y en otros lenguajes de programación orientados a objetos, se manejan diferentes niveles de acceso para controlar la visibilidad y alcance de clases, métodos y atributos.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Nuevo método: calcula la distancia entre "este" punto y "otro" punto
    public double calcularDistanciaAPunto(Punto otro) {
        // ACCESO DIRECTO: Aunque x e y son privados, 
        // este código es legal porque estamos dentro de la clase Punto.
        double catetoX = this.x - otro.x; 
        double catetoY = this.y - otro.y;
        
        return Math.sqrt(Math.pow(catetoX, 2) + Math.pow(catetoY, 2));
    }
}


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### El getter devuelve el valor de un atributo privado. Su propósito es proporcionar acceso de solo lectura a ese atributo
### El setter permite modificar el valor de un atributo privado. Se usa para controlar o validar el nuevo valor antes de asignarlo.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Miembro de Instancia: Pertenece a un objeto específico. Cada vez que creas un new Punto(), el objeto tiene su propia copia de x e y. Si cambias la x de un punto, el otro no se entera.

###Miembro de Clase (static): Pertenece a la clase en sí, no a los objetos. Es una única copia compartida por todas las instancias. Se usa para valores comunes, como un contador de cuántos puntos se han creado.

###Sí, absolutamente. Puedes declarar un miembro static como private.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Se declara un constructor como privado principalmente para tomar el control total sobre la creación de objetos. Al hacerlo, impides que cualquier otra clase use el operador new


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Para indicar que un miembro es un miembro de clase, se utiliza la palabra reservada static.

Esto le indica a Java que el atributo o método pertenece a la propia clase y no a los objetos individuales. Solo existe una copia de ese miembro en memoria, compartida por todas las instancias.

Ejemplo en la clase Punto
Para rastrear los valores máximos de x e y entre todos los puntos creados, definimos dos variables static. Cada vez que el constructor o un "setter" modifique las coordenadas, actualizaremos estos máximos de clase.

Java
public class Punto {
    // Miembros de instancia
    private double x;
    private double y;

    // Miembros de clase (compartidos por todos los objetos)
    private static double xMaximo = Double.NEGATIVE_INFINITY;
    private static double yMaximo = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximos(x, y);
    }

    // Método privado para centralizar la lógica de actualización
    private static void actualizarMaximos(double nuevoX, double nuevoY) {
        if (nuevoX > xMaximo) xMaximo = nuevoX;
        if (nuevoY > yMaximo) yMaximo = nuevoY;
    }

    // Métodos de clase para consultar los valores
    public static double getXMaximo() { return xMaximo; }
    public static double getYMaximo() { return yMaximo; }

    // Si usamos un setter, también debemos actualizar los máximos de clase
    public void setX(double x) {
        this.x = x;
        actualizarMaximos(x, this.y);
    }
}


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### public static Punto crearRedondeado(double x, double y) {
    // Redondeamos las coordenadas al entero más cercano
    double xRedondeado = Math.round(x);
    double yRedondeado = Math.round(y);
    
    // Retornamos una nueva instancia de Punto con los valores procesados
    return new Punto(xRedondeado, yRedondeado);
}


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### public class Punto {
    // Atributo interno cambiado: ahora es un array de 2 posiciones
    // [0] representa la 'x', [1] representa la 'y'
    private double[] coordenadas;

    // Miembros de clase para los máximos (se mantienen igual)
    private static double xMaximo = Double.NEGATIVE_INFINITY;
    private static double yMaximo = Double.NEGATIVE_INFINITY;

    // Constructor: La interfaz sigue recibiendo (double x, double y)
    public Punto(double x, double y) {
        this.coordenadas = new double[]{x, y};
        actualizarMaximos(x, y);
    }

    // Método Factoría: Sigue funcionando igual para el exterior
    public static Punto crearRedondeado(double x, double y) {
        return new Punto(Math.round(x), Math.round(y));
    }

    // Método de cálculo: Ahora accede al array internamente
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(Math.pow(this.coordenadas[0], 2) + Math.pow(this.coordenadas[1], 2));
    }

    // Getters y Setters: Mantienen su firma original (interfaz pública)
    public double getX() { 
        return this.coordenadas[0]; 
    }

    public void setX(double x) {
        this.coordenadas[0] = x;
        actualizarMaximos(x, this.coordenadas[1]);
    }

    public double getY() { 
        return this.coordenadas[1]; 
    }

    public void setY(double y) {
        this.coordenadas[1] = y;
        actualizarMaximos(this.coordenadas[0], y);
    }

    // Lógica interna para los miembros de clase
    private static void actualizarMaximos(double nuevoX, double nuevoY) {
        if (nuevoX > xMaximo) xMaximo = nuevoX;
        if (nuevoY > yMaximo) yMaximo = nuevoY;
    }

    public static double getXMaximo() { return xMaximo; }
    public static double getYMaximo() { return yMaximo; }
}


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

###¿Es mejor declararlo público si tiene getter/setter?
No. Aunque parezca lo mismo, el método actúa como un "filtro". Si el atributo es público, pierdes el control sobre quién lo cambia y cómo. Con métodos, mantienes la flexibilidad de cambiar la lógica interna en el futuro sin romper el código de los demás.

###¿Cuál es la convención más habitual?
La convención estándar en Java es que los atributos sean siempre private. Solo se exponen al exterior mediante métodos públicos (get y set) si es estrictamente necesario.

###¿Tiene que ver con las "invariantes de clase"?
Sí, totalmente. Una invariante es una regla que el objeto debe cumplir para ser válido (ej. "el radio no puede ser negativo"). Si el atributo es privado, el setter puede validar el dato antes de asignarlo, garantizando que la invariante nunca se rompa. Si fuera público, cualquiera podría darle un valor inválido al objeto.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### No siempre es recomendable. Incluir setters por defecto rompe el principio de encapsulación y puede permitir modificaciones no deseadas. Casos como clases inmutables o si el atributo solo se debe asignar una vez no es recomendable; en otros casos como en una clase inmutable o cuando se necesita modificar atributos con validaciones específicas, sí que podemos usar el setter.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### String es inmutable en Java, lo que significa que su valor no puede cambiar después de su creación.
Al concatenar dos cadenas seguidas en java usando “ + ” , se crea un nuevo objeto string, lo que implica un mayor consumo de memoria y un menor rendimiento, ya que cada concatenación genera un nuevo objeto y copia los valores previos.
Si necesitamos concatenar repetidamente usar StringBuilder en lugar de String es mucho más eficiente. StringBuilder es una clase inmutable, por lo que puede modificar el mismo objeto sin crear nuevos y es mucho más rápido y eficiente en uso de memoria.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Para comparar objetos de una misma clase lo podemos hacer de dos maneras usando “ == “ si lo queremos hacer por identidad o usando el método equals() en caso de hacerlo por contenido.
El método equals() se usa para comparar el contenido de un objeto. Es un método de la clase Object que debe sobrescribirse (@Override) para definir qué significa "igualdad" en una clase. Por defecto compara por identidad por eso debemos sobrescribirlo, para que pueda comparar cantidad.
En java debemos usar el método equals() para comparar dos cadenas y en casa de no querer hacer distinción entre mayúsculas o minúsculas debemos usar equalsIgnoreCase().


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Las clases wrapper son clases que envuelven un tipo primitivo y lo convierten en un objeto.
En muchos lenguajes modernos como Java, este proceso de conversión entre tipos primitivos y sus respectivos wrappers es automático gracias a la técnica llamada autoboxing (Es el proceso en el que el compilador convierte un tipo primitivo en su clase wrapper correspondiente de manera automática.) y unboxing(Es el proceso inverso, donde un objeto de una clase wrapper se convierte automáticamente en un tipo primitivo).
Usar el método wrapper tiene unas ventajas que incluyen la integración con colecciones y la capacidad de representar valores nulos.
No todos los lenguajes orientados a objetos necesitan tipos primitivos ni clases wrapper, ya que algunos lenguajes tratan todos los valores como objetos.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### public enum Mes {
    // Definición de las 12 instancias con sus valores internos
    ENERO(31, 1), 
    FEBRERO(28, 2), 
    MARZO(31, 3), 
    ABRIL(30, 4), 
    MAYO(31, 5), 
    JUNIO(30, 6),
    JULIO(31, 7), 
    AGOSTO(31, 8), 
    SEPTIEMBRE(30, 9), 
    OCTUBRE(31, 10), 
    NOVIEMBRE(30, 11), 
    DICIEMBRE(31, 12);

    // Atributos privados: Ocultación de información
    private final int numDias;
    private final int posicion;

    // Constructor del enumerado (siempre es privado implícitamente)
    private Mes(int numDias, int posicion) {
        this.numDias = numDias;
        this.posicion = posicion;
    }

    // Método para obtener el número de días
    public int getNumDias() {
        return this.numDias;
    }

    // Método para obtener el ordinal (1-12)
    public int getPosicion() {
        return this.posicion;
    }
}


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4), 
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8), 
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private final int numDias;
    private final int posicion;

    private Mes(int numDias, int posicion) {
        this.numDias = numDias;
        this.posicion = posicion;
    }

    // --- Métodos de Estaciones ---

    public boolean esDePrimavera(boolean enHemisferioNorte) {
        // Norte: Marzo, Abril, Mayo, Junio | Sur: Septiembre, Octubre, Noviembre, Diciembre
        return enHemisferioNorte ? (posicion >= 3 && posicion <= 6) 
                                 : (posicion >= 9 && posicion <= 12);
    }

    public boolean esDeVerano(boolean enHemisferioNorte) {
        // Norte: Junio, Julio, Agosto, Septiembre | Sur: Diciembre, Enero, Febrero, Marzo
        return enHemisferioNorte ? (posicion >= 6 && posicion <= 9) 
                                 : (posicion >= 12 || posicion <= 3);
    }

    public boolean esDeOtoño(boolean enHemisferioNorte) {
        // Norte: Septiembre, Octubre, Noviembre, Diciembre | Sur: Marzo, Abril, Mayo, Junio
        return enHemisferioNorte ? (posicion >= 9 && posicion <= 12) 
                                 : (posicion >= 3 && posicion <= 6);
    }

    public boolean esDeInvierno(boolean enHemisferioNorte) {
        // Norte: Diciembre, Enero, Febrero, Marzo | Sur: Junio, Julio, Agosto, Septiembre
        return enHemisferioNorte ? (posicion >= 12 || posicion <= 3) 
                                 : (posicion >= 6 && posicion <= 9);
    }

    // Getters básicos
    public int getNumDias() { return numDias; }
    public int getPosicion() { return posicion; }
}
