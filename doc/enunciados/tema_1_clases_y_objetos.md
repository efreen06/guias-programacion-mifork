<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### 
· Encapsulación: Unir en una misma unidad datos y funciones que los manejan. Ocultación de parte de los elementos de la cápsula al exterior (OCULTACIÓN DE INFORMACIÓN). Esto se utiliza para evitar errores de programación desde el exterior del módulo (es decir, por personas que no tienen el control del módulo), no por ciberseguridad.
· Abstracción: Permite representar entidades del mundo real en el código a través de clases y objetos, ocultando los detalles internos y mostrando solo lo esencial para el usuario.  “Olvidarse de detalles”
·Herencia: Es el mecanismo que permite a una clase derivar de otra, heredando sus atributos y métodos. Facilita la reutilización del código y la creación de jerarquías.  Ayuda a la abstracción.
·Polimorfismo: Funciones iguales (en nombre y parámetros) se pueden implementar de más de una forma. 


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Java, c++,Python, c# 


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### · Programación estructurada: Paradigma que define las tres estructuras básicas: secuencia, selección/bifurcación (if-else) e iteración (bucles).  Elimina el uso del go to  (uso del go to controlado) y mejora la legibilidad y mantenimiento del código.
· Programación modular: Extensión de la programación estructurada que divide el código en módulos independientes (con partes accesibles y no accesibles) y reutilizables. 


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### · Estado (atributos o propiedades): valor de sus variables en un momento dado (variables = atributos).
· Comportamiento (métodos o funciones): Son las funciones que el objeto puede ejecutar. Hay dos tipos, privados y públicos.
· Identidad: Es una referencia única que los distingue unívocamente (normalmente la dirección de memoria),


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### · Clase: Es un modelo o plantilla que define las propiedades (atributos) y comportamientos (métodos) que tendrán los objetos creados a partir de ella.
· ¿Es lo mismo que un objeto?: No, una clase es solo una definición, mientras que un objeto es una entidad concreta creada a partir de una clase.
· Instancia: Es un objeto específico generado a partir de una clase. Instanciar una clase significa crear un objeto basado en su definición.
· ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?: No. Algunos lenguajes, como Java y C++, son basados en clases, pero otros, como JavaScript y Python, permiten crear objetos sin necesidad de definir una clase explícita (programación basada en prototipos o clases dinámicas).



## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### · ¿Dónde se almacenan los objetos en memoria?: En la mayoría de los lenguajes orientados a objetos, los objetos se almacenan en el heap (montículo), mientras que las referencias a esos objetos suelen estar en la stack (pila de ejecución) – también se guardan en la pila variables y parámetros –.
· ¿Es igual en todos los lenguajes?: No, depende del lenguaje y su modelo de administración de memoria. (En Java si es así)
· ¿Qué es la recolección de basura?: Es un proceso automático que libera la memoria ocupada por objetos que ya no son accesibles (no tienen llamadas) desde el stack. Lenguajes como Java, Python y C# incluyen este mecanismo, mientras que en C++ debe gestionarse manualmente.



## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### · Método: Es una función definida dentro de una clase que representa el comportamiento de los objetos. Los métodos permiten que los objetos realicen acciones o manipulen sus atributos.
· Sobrecarga de métodos: Es una característica que permite definir múltiples métodos con el mismo nombre dentro de una clase, pero con diferentes parámetros (cantidad o tipo). Se usa para proporcionar distintas formas de ejecutar una operación. No todos los lenguajes soportan sobrecarga; por ejemplo, Java y C++ sí la permiten, mientras que Python la maneja de manera diferente.



## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Copiar el texto de la imagen


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### · Punto de entrada en un programa en Java: El punto de entrada de un programa en Java es el método main. Este método es el primero en ejecutarse cuando se inicia la aplicación. Es el encargado de iniciar la ejecución de un programa  y, generalmente, se encuentra en la clase principal del programa.
· ¿Qué es static y para qué vale?: static es una palabra clave en Java que indica que el miembro (ya sea un método, variable o bloque) pertenece a la clase y no a instancias específicas de la clase. Esto significa que puede acceder al miembro sin necesidad de crear un objeto de la clase.
En el caso del método main, se declara como static porque no se puede crear un objeto antes de que el programa inicie, y el método main necesita ser accesible sin instanciar la clase.
· ¿Sólo se emplea para ese método main?: No. Static se puede usar en cualquier miembro de la clase, no solo en el main. Por ejemplo, variables o métodos static pueden ser accedidos directamente a través de la clase sin necesidad de crear instancias.
· ¿Para qué se combina con final?: Cuando static se combina con final, se suele indicar que el miembro es una constante. Es decir, su valor no puede ser cambiado una vez asignado. La palabra final asegura que la variable no se puede modificar después de su inicialización.


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### · Java es compilado?:  Java no es un lenguaje puramente compilado, sino que tiene un enfoque intermedio. El código fuente de Java se compila en un archivo de bytecode (archivo .class), que luego es interpretado y ejecutado por la Java Virtual Machine (JVM). Es decir, el código Java es "compilado" a bytecode, pero no directamente a código máquina como en otros lenguajes compilados como C++. La JVM se encarga de ejecutar el bytecode en diferentes plataformas.
· ¿Qué es la máquina virtual?: La Java Virtual Machine (JVM) es un componente del entorno de ejecución de Java que interpreta el bytecode generado por el compilador javac y lo ejecuta en la máquina física. La JVM actúa como una capa de abstracción entre el programa Java y el sistema operativo, lo que permite que los programas Java sean portables y puedan ejecutarse en diferentes plataformas (Windows, Linux, macOS, etc.) sin necesidad de re-compilar el código.



## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### · New: La palabra clave new en Java se utiliza para crear una nueva instancia (objeto) de una clase. Al usar new, Java reserva espacio en memoria para un nuevo objeto y luego ejecuta el constructor de la clase para inicializar ese objeto.
En el ejemplo:
Punto p = new Punto(3, 4);
Aquí, new Punto(3, 4) crea un nuevo objeto de la clase Punto y pasa los valores 3 y 4 al constructor para inicializar los atributos x e y.
· Constructor: Un constructor es un método especial de una clase que se llama automáticamente cuando se crea un objeto de esa clase. Su propósito es inicializar los atributos del objeto y realizar cualquier otra tarea de configuración necesaria al momento de crear el objeto.
En Java, el constructor tiene el mismo nombre que la clase y no tiene tipo de retorno (ni void). 


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### this en Java se refiere al objeto actual sobre el que se está ejecutando el método o accediendo a un atributo. Se usa para diferenciar entre atributos de la clase y parámetros con el mismo nombre.
En otros lenguajes, el concepto es el mismo, pero el nombre puede cambiar:
· Java, C++, C#, JavaScript: Usan this.
· Python, Ruby: Usan self.
En resumen, this es común en muchos lenguajes, pero algunos usan self en su lugar.



## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Copiar texto de la imagen


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### La diferencia entre pasar un objeto como parámetro y pasar un tipo primitivo (como un entero) en Java radica en cómo se manejan los valores en la memoria.
· Paso de objetos (Referencia): Cuando pasas un objeto como parámetro (como un Punto), lo que se pasa es la referencia al objeto, no el objeto mismo. Esto significa que el método tiene acceso al mismo objeto en memoria, por lo que cualquier cambio realizado en los atributos del objeto dentro del método afectará al objeto original fuera de la función.
· Paso de tipos primitivos (Valor): Cuando pasas un tipo primitivo (como un int), lo que se pasa es el valor del tipo primitivo, no una referencia a la variable original. Esto significa que cualquier cambio realizado sobre la copia dentro del método no afectará a la variable original fuera de la función.



## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Método toString() en Java:  El método toString() en Java es un método heredado de la clase base Object que devuelve una representación en cadena (texto) del objeto. Si no lo sobrescribes, devuelve una cadena con el formato por defecto (generalmente el nombre de la clase seguido de un código hash). Sin embargo, se recomienda sobrescribir este método en tus clases para proporcionar una representación más legible o útil del objeto.
Existe en otros lenguajes: Sí, muchos lenguajes orientados a objetos tienen una función o método similar a toString() que permite obtener una representación de cadena de un objeto. Por ejemplo:
· En Python se usa el método __str__().
· En C# se usa ToString().
· En JavaScript, aunque no tiene toString() específicamente como en Java, todos los objetos pueden ser convertidos a cadenas mediante el método toString() heredado de Object.



## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Similitudes:
•	Tanto clases como structs almacenan datos relacionados.
•	Ambos permiten crear instancias de objetos.
Diferencias clave:
1.	Encapsulamiento:
o	Clase: Permite control de acceso (atributos privados o públicos).
o	Struct: En C, todos los miembros son públicos, aunque en C++ puedes usar visibilidad, pero sigue siendo más limitado.
2.	Métodos y comportamientos:
o	Clase: Puede tener métodos, constructores, destructores y comportamientos.
o	Struct: Solo almacena datos (en C). En C++ puede tener métodos, pero no es su propósito principal.
3.	Herencia:
o	Clase: Soporta herencia, polimorfismo y abstracción.
o	Struct: En C no tiene herencia; en C++ puede tenerla, pero no se usa igual que en clases.
¿Qué le falta al struct para ser como una clase?
•	Encapsulamiento (atributos privados, getters/setters).
•	Métodos (constructores, destructores).
•	Herencia y polimorfismo en algunos lenguajes.



## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Copiar texto de la imagen
