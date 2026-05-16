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

### Respuesta
El polimorfismo es un pilar fundamental de la programación orientada a objetos que define la capacidad de variables con el mismo tipo formal (referencia) para apuntar a objetos de diferentes tipos reales (subclases) y comportarse de forma distinta ante la misma llamada a un método. En términos prácticos, permite tratar múltiples objetos especializados de manera uniforme a través de una interfaz o clase base común, ocultando los detalles específicos de cada implementación.

Su utilidad principal radica en la reducción del acoplamiento y el aumento de la extensibilidad del software. En lenguajes estructurados como C, para lograr un comportamiento similar, se depende de estructuras condicionales complejas (switch o if/else) o punteros a funciones difíciles de mantener. El polimorfismo elimina esta necesidad, permitiendo añadir nuevas funcionalidades o subclases al sistema sin tener que modificar el código encargado de gestionarlas.

La sobreescritura de métodos (method overriding) es el mecanismo que hace posible el polimorfismo. Consiste en volver a definir en una subclase un método que ya existía en la superclase, manteniendo exactamente la misma firma (nombre, tipo de retorno y parámetros). Al hacer esto, la subclase proporciona su propia implementación especializada, sustituyendo o complementando el comportamiento de la clase base cuando el método es invocado sobre un objeto del subtipo.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
La ligadura dinámica, también conocida como enlace tardío (dynamic binding o late binding), es el mecanismo en tiempo de ejecución por el cual el entorno decide qué versión concreta de un método debe ejecutarse. A diferencia de la ligadura estática, donde el compilador asocia la llamada al método según el tipo de la variable (referencia), la ligadura dinámica ignora el tipo de la variable y busca el método basándose estrictamente en el tipo real del objeto que reside en la memoria.

La relación con el polimorfismo es de total dependencia: la ligadura dinámica es la tecnología subyacente que lo implementa. Sin ella, el polimorfismo no funcionaría en tiempo de ejecución, ya que el sistema operativo o la máquina virtual siempre ejecutarían el código asociado al tipo de la referencia declarada en el código, anulando la especialización de las subclases.

La necesidad de indicar explícitamente este comportamiento varía según el lenguaje:

C++: La ligadura estática es la opción por defecto por motivos de eficiencia. Si se desea activar la ligadura dinámica y el polimorfismo, el programador debe declarar explícitamente el método como virtual en la clase base.

Java: Todos los métodos no estáticos, no privados y no finales utilizan ligadura dinámica de forma nativa y automática. No se requiere ninguna palabra clave para activarla.

Python: Al ser un lenguaje dinámico, no existe la ligadura estática para los métodos. El polimorfismo y la resolución tardía se aplican siempre de forma implícita mediante el concepto de Duck Typing ("tipado de pato").

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
El polimorfismo permite redefinir el comportamiento de una acción común en la jerarquía. En el siguiente ejemplo, la clase base Soldado define una acción estándar, el Artillero la hereda directamente y el Zapador la reemplaza por completo para adaptarla a su naturaleza.

Al almacenar las diferentes instancias dentro de un array de tipo uniforme Soldado[], se demuestra la compatibilidad de tipos. El bucle posterior interactúa con los objetos usando una referencia genérica, pero la salida en consola demuestra que cada objeto reacciona ejecutando el código de su tipo real gracias a la ligadura dinámica.

class Soldado {
    public void saludar() {
        System.out.println("Soldado reportándose.");
    }
}

class Artillero extends Soldado {
    // Hereda el comportamiento de saludar() tal cual
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        // Sustituye por completo el comportamiento de la base
        System.out.println("Zapador despejando el terreno. ¡Cuidado!");
    }
}

public class Principal {
    public static void main(String[] args) {
        // Creación del array polimórfico
        Soldado[] peloton = new Soldado[2];
        peloton[0] = new Artillero();
        peloton[1] = new Zapador();

        // Recorrido empleando referencias del supertipo
        for (Soldado s : peloton) {
            s.saludar(); 
        }
    }
}

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, es completamente lícito y habitual invocar la implementación del método de la clase base desde el método sobreescrito de la subclase. Esto evita la duplicación de código innecesaria y permite aplicar el concepto de extensión en lugar de sustitución, construyendo un comportamiento más específico sobre la base existente.

Para romper la ambigüedad e indicar al compilador que se desea ejecutar el método original de la superclase, y no el de la subclase actual, se utiliza la palabra clave super. Sin esta palabra clave, si se llamara al método directamente por su nombre, el programa entraría en una recursión infinita invocándose a sí mismo.

class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // Se invoca el método de la clase base (Soldado)
        System.out.println("ZAPADOR A SUS ORDENES."); // Se añade comportamiento específico
    }
}
## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
Al sobreescribir un método, los parámetros deben ser idénticos en número, tipo y orden a los del método original; si se altera algún parámetro, se estará creando un método diferente en lugar de sobreescribir el existente. Respecto al tipo de retorno, este debe ser igual o una subclase del tipo devuelto por el método base (lo que se conoce como retorno covariante). Además, el método sobreescrito no puede reducir la visibilidad del original (por ejemplo, pasar de public a private).

La diferencia entre ambos conceptos radica en el momento de su resolución y su firma:

Sobreescritura (Overriding): Ocurre entre clases con relación de herencia. El método mantiene la misma firma y se resuelve en tiempo de ejecución mediante ligadura dinámica.

Sobrecarga (Overloading): Ocurre en la misma clase (o heredada). Son métodos con el mismo nombre pero diferente firma (distintos parámetros). Se resuelve en tiempo de compilación según los argumentos pasados.

La anotación @Override es una directiva para el compilador que valida explícitamente que el método decorado realmente está sobreescrita de una superclase o interfaz. Su uso es altamente recomendado porque previene errores sutiles de escritura, como equivocarse en una mayúscula o en el tipo de un parámetro, lo que transformaría silenciosamente una sobreescritura intencionada en una sobrecarga accidental.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Sí, en Java el polimorfismo se utiliza de manera implícita desde las primeras etapas del aprendizaje. Debido a que toda clase en Java extiende automáticamente de la raíz universal java.lang.Object, cualquier clase creada forma parte de un árbol de herencia y tiene acceso a sus métodos públicos.

Cuando se redefine el método public String toString() o public boolean equals(Object obj) en una clase propia para personalizar la representación en texto o la lógica de comparación, se está realizando una sobreescritura de manual.

El polimorfismo entra en acción, por ejemplo, cuando se pasa cualquier objeto al método System.out.println(miObjeto). Internamente, este método recibe una referencia genérica de tipo Object y ejecuta .toString(). Gracias a la ligadura dinámica, la máquina virtual ejecuta la versión específica programada en la clase del usuario, demostrando que el polimorfismo opera constantemente entre bastidores.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una clase abstracta es una clase incompleta diseñada con el único propósito de servir como un molde conceptual o base común en una jerarquía de herencia. Se utiliza para definir la estructura general y las responsabilidades compartidas de un grupo de subtipos, pero impidiendo que se puedan modelar objetos genéricos directos de dicha clase.

Un método abstracto es una firma de método que se declara sin proporcionar ninguna implementación (carece de cuerpo con llaves {}). Representa una obligación contractual: cualquier subclase concreta que herede de esta clase estará estrictamente obligada a sobreescribir el método y aportarle un comportamiento real, bajo penalización de error de compilación.

No es posible crear instancias de una clase abstracta utilizando el operador new. Intentar hacer un new Soldado() provocará un error de diseño. La palabra clave abstract debe colocarse tanto en la declaración de la estructura de la clase como en la firma de cada método que carezca de código ejecutable.

// Clase abstracta: no se puede instanciar
abstract class Soldado {
    public void saludar() {
        System.out.println("Reportándose.");
    }

    // Método abstracto: no tiene cuerpo, define la obligación de implementar
    public abstract void atacar();
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Disparando cohetes de largo alcance.");
    }
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Colocando minas antipersona en el perímetro.");
    }
}

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
La palabra clave final actúa como un modificador de restricción absoluta en Java. Cuando se aplica a una clase, impide por completo que esta pueda ser extendida por otra (prohíbe la herencia). Si se aplica a un método, permite que la clase sea heredada, pero prohíbe terminantemente que las subclases puedan sobreescribir ese método en particular.

Su relación con el polimorfismo es de limitación intencionada. Al declarar una clase o método como final, se desactiva la posibilidad de aplicar ligadura dinámica sobre ellos. El compilador sabe con certeza absoluta qué código se va a ejecutar, lo que permite realizar optimizaciones de rendimiento en tiempo de compilación (como el inlining de funciones) y garantiza la seguridad de que el comportamiento no será alterado por terceros.

Un ejemplo omnipresente en la API estándar de Java es la clase java.lang.String. Está declarada explícitamente como public final class String. El motivo de este diseño es garantizar la inmutabilidad y la seguridad del sistema, impidiendo que un programador pueda crear una subclase que altere el comportamiento básico de las cadenas de texto del lenguaje.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
Las interfaces son estructuras de programación que definen un contrato puro de comportamiento. En su concepción clásica, no contienen estado (atributos de instancia) ni código, sino únicamente un conjunto de firmas de métodos públicos que determinan qué capacidades debe tener un objeto, sin preocuparse en absoluto de cómo se van a programar internamente.

Aunque comparten similitudes con las clases abstractas en el sentido de que ninguna puede ser instanciada directamente y ambas exigen la implementación de métodos, conceptualmente son distintas. Una clase abstracta define la identidad de un objeto ("qué es"), mientras que una interfaz define una aptitud o rol ("qué puede hacer"). Además, las interfaces carecen de constructores y no participan en la estructura de memoria de herencia simple de atributos.

Una de sus mayores ventajas arquitectónicas es que una clase sí puede implementar múltiples interfaces de forma simultánea. Esto mitiga la ausencia de herencia múltiple en Java, permitiendo que una clase mantenga su herencia única estructural con una superclase y, al mismo tiempo, adquiera múltiples roles independientes en la aplicación.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta
El diseño polimórfico permite delegar cálculos especializados a las clases que conocen sus propios atributos internos. En la estructura propuesta, la clase Linea gestiona dos extremos espaciales de forma genérica a través del tipo base abstracto Punto, logrando calcular su longitud mediante ligadura dinámica sin requerir conocer si opera en dos o tres dimensiones.

abstract class Punto {
    // Método polimórfico abstracto
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    private double x, y;

    public Punto2D(double x, double y) { this.x = x; this.y = y; }
    public double getX() { return x; }
    public double getY() { return y; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p = (Punto2D) otro; // Downcasting seguro
            return Math.sqrt(Math.pow(p.getX() - this.x, 2) + Math.pow(p.getY() - this.y, 2));
        }
        throw new IllegalArgumentException("Incompatibilidad de dimensiones en Punto2D");
    }
}

class Punto3D extends Punto {
    private double x, y, z;

    public Punto3D(double x, double y, double z) { this.x = x; this.y = y; this.z = z; }
    public double getX() { return x; }
    public double getY() { return y; }
    public double getZ() { return z; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p = (Punto3D) otro; // Downcasting seguro
            return Math.sqrt(Math.pow(p.getX() - this.x, 2) + Math.pow(p.getY() - this.y, 2) + Math.pow(p.getZ() - this.z, 2));
        }
        throw new IllegalArgumentException("Incompatibilidad de dimensiones en Punto3D");
    }
}

class Linea {
    private Punto origen, fin; // Composición polimórfica (desconoce la dimensión real)

    public Linea(Punto origen, Punto fin) {
        this.origen = origen;
        this.fin = fin;
    }

    public double obtenerLongitud() {
        // La ligadura dinámica resuelve el cálculo correcto según el tipo real en memoria
        return origen.calcularDistanciaA(fin);
    }
}

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
La herencia de interfaces es el mecanismo mediante el cual una interfaz puede extender a otra interfaz existente utilizando la palabra clave extends. Esto permite aplicar los principios de reutilización y especialización de contratos, de modo que la interfaz derivada hereda todas las firmas de métodos de la interfaz base y añade nuevas obligaciones específicas.

A diferencia de las clases, sí existe la herencia múltiple de interfaces en Java. Una interfaz puede extender varias interfaces de manera simultánea separándolas por comas (por ejemplo, interface C extiende A, B). Esto es completamente seguro y lícito en el lenguaje debido a que las interfaces solo heredan declaraciones de comportamiento, eliminando de raíz cualquier conflicto de colisión de estado o de ambigüedad de implementación que sufre la herencia de clases.

// Interfaz base
interface Fichero {
    String leer();
}

// Herencia de interfaces: extiende el contrato original
interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}

// Una clase concreta que decida implementar el contrato especializado
class DocumentoTexto implements FicheroEscribible {
    @Override
    public String leer() { 
        return "Contenido de lectura"; 
    }

    @Override
    public void escribir(String contenido) { 
        System.out.println("Escribiendo: " + contenido); 
    }

    @Override
    public void eliminar() { 
        System.out.println("Fichero eliminado."); 
    }
}