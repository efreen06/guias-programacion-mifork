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

### Opción 1: Usar un valor de retorno especial. 
Una forma común es devolver un valor especial que indique un error. Por ejemplo, si la función de raíz cuadrada recibe un número 
negativo, podrías devolver un valor como -1 (o cualquier otro que no sea válido en el contexto de la función). Luego, el código que llama 
a la función verifica este valor especial para determinar si ocurrió un error. 
Opción 2: Usar un parámetro de salida para el estado de error. 
Otra opción es usar un parámetro adicional (por ejemplo, un puntero a un entero) para indicar si hubo un error. La función devuelve el 
resultado normal, pero el parámetro de salida se usa para comunicar el estado de la operación (éxito o fallo). 

#include <stdio.h>
#include <math.h>

float raiz_opcion1(float n) {
    if (n < 0) {
        return -1.0; // Valor especial para indicar error
    }
    return sqrt(n);
}

int main() {
    float numero = -5.0;
    float resultado = raiz_opcion1(numero);

    if (resultado == -1.0) {
        printf("Error: No se puede calcular la raiz de un numero negativo (%.2f).\n", numero);
    } else {
        printf("La raiz de %.2f es %.2f\n", numero, resultado);
    }

    return 0;
}

#include <stdio.h>
#include <math.h>

// La función devuelve el resultado, y usa un puntero para el error
float raiz_opcion2(float n, int *error_code) {
    if (n < 0) {
        *error_code = 1; // 1 significa "Error de dominio"
        return 0.0;
    }
    
    *error_code = 0; // 0 significa "Éxito"
    return sqrt(n);
}

int main() {
    float numero = -9.0;
    int error;
    float resultado = raiz_opcion2(numero, &error);

    if (error != 0) {
        printf("Error detectado: Codigo %d. El numero %.2f es invalido.\n", error, numero);
    } else {
        printf("La raiz de %.2f es %.2f\n", numero, resultado);
    }

    return 0;
}

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Una excepción es un evento que ocurre durante la ejecución de un programa que interrumpe el flujo normal de las instrucciones. 
Representa un error o una condición inesperada que necesita ser manejada, como división por cero, acceso a memoria no válida o 
entrada incorrecta. 
El objetivo de usar excepciones es separar la lógica principal del programa del manejo de errores. Cuando un programador implementa 
funciones, las excepciones permiten indicar que algo ha ido mal sin tener que devolver valores especiales o usar parámetros adicionales. 
Al llamar a una función, el programador puede capturar y manejar estas excepciones para evitar que el programa falle abruptamente, 
proporcionando una forma estructurada y clara de gestionar errores. Esto mejora la legibilidad, mantenibilidad y robustez del código. 


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### public class Calculadora {

    /**
     * Calcula la raíz cuadrada de un número.
     * @param n El número del que queremos la raíz.
     * @return El resultado de la raíz.
     * @throws IllegalArgumentException Si el número es negativo.
     */
    public double raiz(double n) {
        if (n < 0) {
            // "Lanzamos" el error hacia afuera
            throw new IllegalArgumentException("No se puede calcular la raíz de un número negativo: " + n);
        }
        return Math.sqrt(n);
    }

    public static void main(String[] args) {
        Calculadora miCalc = new Calculadora();
        double numero = -16.0;

        try {
            // Intentamos ejecutar el código que podría fallar
            double resultado = miCalc.raiz(numero);
            System.out.println("El resultado es: " + resultado);
            
        } catch (IllegalArgumentException e) {
            // Aquí es donde "controlamos" el error desde fuera
            System.err.println("¡Error detectado! Mensaje: " + e.getMessage());
            System.out.println("Por favor, introduce un número positivo la próxima vez.");
        }

        System.out.println("El programa continúa su ejecución normal...");
    }
}


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### “Lanzar” una excepción: Lanzar una excepción significa indicar que ha ocurrido un error o una condición excepcional en un punto 
específico del código. En el ejemplo, el método calcularRaiz lanza una excepción (IllegalArgumentException) cuando el número es 
negativo: 
throw new IllegalArgumentException(“No se puede calcular la raíz de un número negativo”) 
Esto detiene la ejecución normal del método y transfiere el control al mecanismo de manejo de excepciones. 
“Controlar” o “capturar” una excepción: Controlar o capturar una excepción significa detectar y manejar la excepción lanzada. Esto se 
hace usando un bloque try-catch. En el ejemplo, el método main captura la excepción lanzada por calcularRaiz: 
try { 
double resultado = Calculadora.calcularRaiz(-4); 
System.out.println("La raíz es: " + resultado); 
} catch (IllegalArgumentException e) { 
System.out.println("Error: " + e.getMessage()); 
} 
Si la excepción es capturada, el programa no se detiene abruptamente, sino que ejecuta el código dentro del bloque catch. 
“Propagar” una excepción: Propagar una excepción significa que, si no se captura en el método donde se lanzó, la excepción se pasa 
al método que lo llamó, y así sucesivamente, subiendo por la pila de llamadas. En el ejemplo, si no hubiera un bloque try-catch en 
el main, la excepción se propagaría hasta el sistema de ejecución de Java, que mostraría un mensaje de error y detendría el programa. 
Comportamiento de las funciones en la pila de llamadas: Cuando una excepción se lanza y no se captura en un método, este método 
se interrumpe y la excepción se propaga al método que lo llamó. Si ese método tampoco la captura, la excepción sigue propagándose 
hacia arriba en la pila de llamadas. En el ejemplo: - - 
Si calcularRaiz lanza una excepción y no se captura en el main, el programa termina abruptamente. 
Las funciones que no controlan la excepción no se reanudan. Una vez que se lanza una excepción, el flujo normal de ejecución 
se interrumpe. 
¿Las funciones que no controlan la excepción se reanudan?: No, las funciones que no controlan la excepción no se reanudan. Una 
excepción no capturada provoca que el método actual se interrumpa y la excepción se propague hacia arriba en la pila de llamadas. Si 
nadie la captura, el programa termina. En el ejemplo, si no hubiera un bloque try-catch en el main, el programa se detendría después de 
lanzar la excepción. 


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### La propagación natural de excepciones simplifica el manejo de errores, mejora la claridad del código, proporciona información detallada 
sobre los errores y evita problemas comunes como la pérdida de errores o la liberación incorrecta de recursos. En comparación, el 
manejo de errores en C requiere más esfuerzo manual y es más propenso a errores humanos.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### - - - 
Las excepciones como objetos aprovechan la encapsulación para almacenar información y proporcionar funcionalidad. 
Esto permite crear excepciones personalizadas para manejar errores específicos de tu aplicación. 
Las excepciones personalizadas mejoran la claridad, organización y capacidad de diagnóstico del código. 
Este enfoque hace que el manejo de errores sea más robusto, flexible y alineado con los principios de la orientación a objetos.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### La encapsulación en las excepciones de Java proporciona información esencial como: 
1. Mensajes descriptivos. 
2. Traza de la pila. 
3. Tipo de excepción. 
4. Causa raíz. 
5. Información adicional personalizada. 
Esto hace que el manejo de errores sea más informado, estructurado y fácil de depurar, en comparación con el enfoque de C, donde la 
información es más limitada y menos organizada.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Sí, se pueden tener múltiples bloques catch en un try-catch, pero solo se ejecuta el primero que coincida con la excepción lanzada. Esto 
permite manejar distintos tipos de excepciones de forma específica. 


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Para garantizar que un código se ejecute siempre, incluso si ocurre una excepción, se usa el bloque finally. Este bloque se ejecuta antes 
de que la excepción se propague o después de la ejecución normal del try. 
Ejemplo con catch 
try { 
int resultado = 10 / 0; // Provoca una excepción 
} catch (ArithmeticException e) { 
System.out.println("Error: División por cero"); 
} finally { 
System.out.println("Este bloque siempre se ejecuta"); 
} 
Ejemplo sin catch 
try { 
int[] arr = new int[2]; 
arr[5] = 10; // Provoca una excepción 
} finally { 
System.out.println("Liberando recursos..."); 
}


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

###  - - 
Sí, finally puede ir sin catch, siempre que haya un try. 
f
inally siempre se ejecuta, haya o no una excepción. 
Si hay un return en el try, el finally se ejecuta antes de que el método retorne. 
public static int metodo() { 
try { 
return 10; 
} finally { 
System.out.println("Se ejecuta finalmente"); 
} 
} 


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Excepciones controladas (Checked): Son verificadas en compilación. Deben capturarse (try-catch) o declararse con throws. 
Ejemplo: IOException. 
Excepciones no controladas (Unchecked): Son subclases de RuntimeException y no requieren ser manejadas obligatoriamente. 
Ejemplo: NullPointerException. 
throws y su uso: - - 
Permite propagar excepciones sin capturarlas. 
Alternativa a try-catch para excepciones controladas. 
Ejemplo de excepciones 
// Excepción controlada (Checked) 
void leerArchivo() throws IOException { 
FileReader fr = new FileReader("archivo.txt");  
// Debe manejarse o declararse 
} 
Ejemplo de throws 
void metodo() throws IOException { 
throw new IOException("Error en archivo"); 
} 
// Excepción no controlada (Unchecked) 
void divisionPorCero() { 
int resultado = 10 / 0;  
// Lanza ArithmeticException sin necesidad de throws 
}


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Es una palabra clave que se pone en la firma de un método para avisar que dicho método puede lanzar una o varias excepciones.
En lugar de usar un bloque try-catch para gestionar el error en ese mismo instante, usas throws para propagarlo hacia arriba en la jerarquía de llamadas.

Capturar (try-catch): Te haces responsable del error ahora mismo.

Declarar (throws): Le pasas "la patata caliente" al método que te invocó para que él decida qué hacer.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### import java.io.*;

public class GestorArchivos {

    // El 'throws' avisa que este método es "peligroso" y no captura el error
    public void leerArchivo(String ruta) throws IOException {
        FileReader fr = null;
        
        try {
            System.out.println("Intentando abrir el archivo...");
            fr = new FileReader(ruta);
            // ... lógica para leer el archivo ...
            
        } finally {
            // Este bloque SE EJECUTA SIEMPRE, haya error o no.
            // Es sagrado para cerrar recursos y evitar fugas de memoria.
            if (fr != null) {
                System.out.println("Cerrando el archivo obligatoriamente.");
                fr.close();
            } else {
                System.out.println("No había nada que cerrar, pero el finally se ejecutó igual.");
            }
        }
    }

    public static void main(String[] args) {
        GestorArchivos gestor = new GestorArchivos();
        
        try {
            // El main es el que finalmente decide "dar la cara" ante el usuario
            gestor.leerArchivo("archivo_que_no_existe.txt");
        } catch (IOException e) {
            System.err.println("CONTROLADO DESDE MAIN: El archivo no está, informa al usuario.");
        }
    }
}


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Sí, podemos declarar excepciones no controladas con throws, aunque no es obligatorio porque el compilador no las fuerza a manejar. 
¿Debe 
el 
método 
llamador 
No es obligatorio, pero puede hacerlo si quiere evitar que la excepción termine el programa. 
public static void lanzarError() throws RuntimeException { 
throw new IllegalArgumentException("Valor inválido"); 
} 
public static void main(String[] args) { 
lanzarError(); // No requiere try-catch, pero el programa fallará si se lanza la excepción. 
} 
Sentido de usar throws con unchecked exceptions: - 
Documenta que un método puede lanzar una excepción. - - -  
Puede ser útil en librerías o APIs para advertir a los desarrolladores. 
Permite delegar el manejo de errores a niveles superiores del programa.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Excepciones controladas (Checked Exceptions, como IOException) se usan cuando el error es previsible y recuperable, y el 
programador debe manejarlo. Ejemplo: fallos en la lectura de un archivo. 
Excepciones no controladas (Unchecked Exceptions, como IllegalArgumentException) se usan cuando el error es un fallo de 
programación o un problema lógico, y no es obligatorio capturarlas. Ejemplo: pasar un argumento inválido a un método. 
No todos los lenguajes tienen esta distinción. En lenguajes como Python o JavaScript, solo hay excepciones no controladas, que es la 
opción más habitual en la mayoría de los lenguajes. 



## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Sí, tiene sentido si queremos encapsular la excepción original en otra más específica o si queremos simplemente propagarla después 
de realizar alguna acción (como registrar un log). 
try { 
int resultado = 10 / 0; // Provoca ArithmeticException 
} catch (ArithmeticException e) { 
throw new IllegalArgumentException("Error: No se puede dividir por cero", e); 
}


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Una excepción puede ser la causa de otra cuando un error de bajo nivel se encapsula dentro de una excepción de más alto nivel para 
agregar contexto. 
class MiExcepcionPersonalizada extends Exception { 
public MiExcepcionPersonalizada(String mensaje, Throwable causa) { 
super(mensaje, causa); 
} 
} 
try { 
Files.readAllLines(Paths.get("archivo_inexistente.txt")); // Lanza IOException 
} catch (IOException e) { 
throw new MiExcepcionPersonalizada("No se pudo leer el archivo", e); 
}

