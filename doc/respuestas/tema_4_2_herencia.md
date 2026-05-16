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

### Respuesta
La herencia es un mecanismo que permite crear una clase nueva (subclase) a partir de una ya existente (superclase). Establece una relación jerárquica del tipo "A es-un B". Por ejemplo, si un Artillero hereda de Soldado, significa que conceptualmente un artillero es un soldado y posee todas sus características básicas.

La primera implicación es la compatibilidad de tipos. Un objeto de una subclase puede ser tratado como si fuera del tipo de la superclase. Esto permite agrupar diferentes objetos derivados en estructuras unificadas (como un array de la clase base) y gestionarlos de manera uniforme.

La segunda implicación es la herencia de estado y comportamiento. La subclase copia automáticamente los atributos (estado) y métodos (comportamiento) de la clase madre. No es necesario reescribir el código común, bastando con añadir las particularidades de cada subtipo.
class Soldado {
    private String nombre;
    public Soldado(String nombre) { this.nombre = nombre; }
    public void saludar() { System.out.println("Hola, soy " + nombre); }
}

class Artillero extends Soldado {
    private int cohetes;
    public Artillero(String nombre, int cohetes) { super(nombre); this.cohetes = cohetes; }
    public int getCohetes() { return cohetes; }
}

class Zapador extends Soldado {
    private int minas;
    public Zapador(String nombre, int minas) { super(nombre); this.minas = minas; }
    public int getMinas() { return minas; }
}

public class Principal {
    public static void main(String[] args) {
        Soldado[] ejercito = { new Soldado("Ramírez"), new Artillero("Pérez", 10), new Zapador("Gómez", 5) };
        for (Soldado s : ejercito) { s.saludar(); }
    }
}

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta
Se ejecutan siempre al menos dos constructores: el de la superclase y el de la subclase. El orden de ejecución es estrictamente descendente, comenzando por la clase base (más alta en la jerarquía) y terminando en la subclase concreta que se está instanciando.

La palabra clave super actúa como una llamada explícita al constructor de la superclase. Se utiliza para pasarle los parámetros necesarios y asegurar que la clase base inicialice sus atributos antes de que la subclase empiece a configurar los suyos. Debe ser siempre la primera línea de código del constructor.

Si la clase base no tiene un constructor sin parámetros (porque se definió uno con argumentos), el uso de super(...) es estrictamente obligatorio. De lo contrario, Java intentará buscar un constructor vacío en la superclase y el código dará un error de compilación.
## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta
Sí, los atributos privados de la superclase forman parte física de la memoria del objeto de la subclase. Al crear un Artillero, el bloque de memoria reservado contiene espacio tanto para el atributo nombre (de Soldado) como para cohetes (de Artillero).

No obstante, la reserva de espacio no otorga permisos de acceso. Al estar declarados como private en la superclase, el código de la subclase no puede modificarlos ni leerlos de forma directa. Se está obligado a interactuar con ellos mediante los constructores o métodos públicos que ofrezca la clase base.
## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta
Implica que el código se vuelve altamente extensible y tolerante a futuros cambios. Permite programar algoritmos genéricos orientados a la superclase. De este modo, se pueden añadir nuevas funciones o clases derivadas más adelante sin necesidad de modificar ni una sola línea del código principal ya existente.

Si el día de mañana se añade una nueva clase al sistema, las estructuras de control que manejan a los soldados no sufrirán ningún impacto, evitando los extensos bloques condicionales (if/else o switch) habituales en la programación estructurada.
class Medico extends Soldado {
    public Medico(String nombre) { super(nombre); }
}

// El array acepta el nuevo tipo y el bucle for de la Pregunta 1 funciona EXACTAMENTE IGUAL.

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta
Es totalmente posible asignar un objeto de un subtipo a una referencia del supertipo. Sin embargo, no se pueden invocar los métodos específicos del subtipo a través de esa referencia; el compilador solo permite usar los métodos que existan formalmente en la clase del supertipo.

El upcasting es la conversión de una referencia de subclase a superclase, un proceso automático y seguro. El downcasting es el camino inverso (pasar de referencia base a subclase); no es seguro y requiere un moldeado explícito, ya que Java necesita comprobar que el objeto real coincide con el tipo solicitado.

Para evitar errores críticos al hacer downcasting se utiliza el operador instanceof. Este comprueba en tiempo de ejecución si un objeto pertenece a una clase determinada, devolviendo un valor booleano.
public class EjemploCast {
    public static void comprobar(Soldado[] ejercito) {
        for (Soldado s : ejercito) {
            s.saludar();
            if (s instanceof Artillero) {
                Artillero a = (Artillero) s; // Downcasting seguro
                System.out.println("Cohetes: " + a.getCohetes());
            }
        }
    }
}

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
