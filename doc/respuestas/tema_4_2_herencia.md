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
El acceso protegido es un nivel de visibilidad intermedio. Permite que los atributos o métodos sean accesibles para las clases que se encuentran dentro del mismo paquete y, de forma exclusiva, por cualquier subclase que herede de ella, aunque esté en un paquete diferente.

Se implementa en Java utilizando la palabra clave protected. Ofrece una manera cómoda para que los desarrolladores de subclases manipulen variables internas heredadas directamente, sin necesidad de recurrir a intermediarios públicos.
class Soldado {
    protected String nombre; // Visible para subclases
    public Soldado(String nombre) { this.nombre = nombre; }
}

class Zapador extends Soldado {
    public Zapador(String nombre) { super(nombre); }
    public void ponerBomba() {
        System.out.println(this.nombre + " pone una bomba."); // Acceso directo válido
    }
}

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
No ocurre en todos los lenguajes. En entornos como C++, no existe una raíz común obligatoria, permitiendo crear múltiples árboles de herencia totalmente independientes y aislados entre sí.

En Java, el diseño es monomórfico: todas las clases heredan implícitamente de una única clase base universal llamada java.lang.Object. Si una clase no extiende a otra, el compilador la vincula automáticamente a esta raíz.

Gracias a este diseño, se garantiza que cualquier objeto de Java comparta un conjunto mínimo de métodos estándar predefinidos, como toString(), equals() o hashCode().

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
La herencia múltiple es la capacidad de un lenguaje de programación para permitir que una subclase herede el estado y comportamiento de varias superclases simultáneamente (por ejemplo, que una clase copie propiedades de dos madres distintas).

En Java no existe la herencia múltiple de clases. Se limitó el diseño a herencia simple (extends a una sola clase) para evitar problemas de ambigüedad y conflictos de nombres, conocidos habitualmente como el "problema del diamante".

Para cubrir las necesidades donde una clase debe cumplir con diferentes comportamientos a la vez, Java sustituye la herencia múltiple mediante el uso de interfaces.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
Para crear una excepción personalizada no controlada (Unchecked), la clase debe heredar obligatoriamente de RuntimeException. De esta manera, el compilador no forzará al desarrollador a gestionarla de manera estricta mediante bloques try-catch allá donde se use.

Es una buena práctica añadir atributos a estas excepciones para dotarlas de contexto técnico. Al incluir un objeto Usuario, el código que capture el error dispondrá de la información exacta sobre qué entidad provocó el fallo.
class Usuario { private String login; public Usuario(String login) { this.login = login; } }

class UsuarioNoEncontradoException extends RuntimeException {
    private final Usuario usuarioAfectado;

    public UsuarioNoEncontradoException(String msg, Usuario usuario) {
        super(msg);
        this.usuarioAfectado = usuario;
    }

    public UsuarioNoEncontradoException(String msg, Usuario usuario, Throwable causa) {
        super(msg, causa); // Permite añadir la causa subyacente
        this.usuarioAfectado = usuario;
    }
}

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
Porque la herencia establece una unión estructural extremadamente rígida y permanente. Si solo se busca reaprovechar unas líneas de código sin que exista una relación conceptual real de "es-un", se acaba dañando la arquitectura del programa.

Al heredar sin justificación, la subclase se ve obligada a arrastrar métodos públicos de la superclase que no necesita, exponiendo una interfaz confusa y peligrosa que dificulta el mantenimiento futuro del software.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
Porque la composición ofrece un acoplamiento mucho más débil y flexible. En lugar de ligar la identidad de una clase a otra, un objeto simplemente guarda una referencia de un componente externo para delegarle el trabajo (relación "tiene-un").

Esta separación permite cambiar el comportamiento del objeto de forma dinámica en tiempo de ejecución (intercambiando el componente apuntado), facilita el mantenimiento del código y simplifica la realización de pruebas unitarias mediante réplicas (mocks).

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
Se refiere a que la subclase depende íntimamente de los detalles de implementación interna de su superclase para funcionar correctamente. La barrera de aislamiento que promete la encapsulación se vuelve difusa.

Si el desarrollador de la clase base modifica la lógica interna de sus métodos, puede romper el comportamiento de las subclases de manera imprevista, a pesar de que la firma del método público no haya cambiado.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
A continuación se muestran los dos enfoques estructurales básicos para unificar campos compartidos:

Alternativa A: Por Herencia

class Persona {
    private String dni, nombre;
    public Persona(String dni, String nombre) { this.dni = dni; this.nombre = nombre; }
}
class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) { super(dni, nombre); }
}
class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) { super(dni, nombre); }
}

Alternativa B: Por Composición

class DatosPersonales {
    private String dni, nombre;
    public DatosPersonales(String dni, String nombre) { this.dni = dni; this.nombre = nombre; }
}
class EstudianteComposicion {
    private DatosPersonales datos;
    public EstudianteComposicion(DatosPersonales datos) { this.datos = datos; }
}
class TrabajadorComposicion {
    private DatosPersonales datos;
    public TrabajadorComposicion(DatosPersonales datos) { this.datos = datos; }
}