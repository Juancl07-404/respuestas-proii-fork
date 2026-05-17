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

### Respuesta
Un puntero a una función es una variable que almacena la dirección de memoria donde residen las instrucciones ejecutables de una función, en lugar de almacenar un valor de datos convencional. Esto permite invocar la función de manera indirecta a través del puntero, pasarlo como argumento a otras funciones o incluso almacenarlo en estructuras de datos, habilitando un nivel básico de comportamiento dinámico en lenguajes estructurados.

En lenguajes como C, los punteros a funciones exigen que la firma (tipo de retorno y parámetros) coincida exactamente entre la función apuntada y la declaración del puntero. A continuación se muestra un ejemplo donde se define una función que modifica un array de caracteres para pasarlo a mayúsculas, y luego se invoca a través de un puntero diseñado específicamente para su firma.

#include <stdio.h>
#include <ctype.h>

// Definición de la función
void convertirAMayusculas(char* cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper(cadena[i]);
    }
}

int main() {
    char texto[] = "hola mundo";
    
    // Declaración e inicialización del puntero a la función
    void (*aMayusculas)(char*) = convertirAMayusculas;
    
    // Invocación indirecta mediante el puntero
    aMayusculas(texto);
    
    printf("%s\n", texto); // Imprime: HOLA MUNDO
    return 0;
}

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta
Una función lambda (o función anónima) es una subrutina definida de forma compacta y directamente en el lugar donde se necesita, sin requerir un nombre identificativo formal ni una declaración completa. Nacen del cálculo lambda en matemáticas y permiten escribir bloques de código de un solo uso de manera muy expresiva, reduciendo la necesidad de crear métodos o clases separadas únicamente para encapsular un comportamiento simple.

Al igual que ocurre con los punteros a funciones, las expresiones lambda pueden ser asignadas a variables, lo que permite ejecutarlas posteriormente. En JavaScript, al ser un lenguaje dinámico, la asignación es directa e inferida. En Java, al ser de tipado estático, la lambda debe ajustarse a un tipo conocido, como la interfaz genérica predefinida Function<T, R>.

// Ejemplo en JavaScript
const aMayusculas = (cadena) => cadena.toUpperCase();

let texto = aMayusculas("hola mundo");
console.log(texto); // Imprime: HOLA MUNDO

import java.util.function.Function;

public class EjemploLambda {
    public static void main(String[] args) {
        // Ejemplo en Java
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();
        
        String texto = aMayusculas.apply("hola mundo");
        System.out.println(texto); // Imprime: HOLA MUNDO
    }
}

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta
El paradigma funcional es un estilo de programación que trata la computación como la evaluación de funciones matemáticas. A diferencia del paradigma imperativo u orientado a objetos, se centra en describir qué se debe resolver en lugar de cómo hacerlo paso a paso. Sus principios fundamentales incluyen evitar el estado mutable (los datos no se modifican una vez creados) y evitar efectos secundarios, asegurando que para una misma entrada, una función siempre produzca la misma salida.

A lenguajes como Java (a partir de su versión 8) se les denomina multi-paradigma porque integran capacidades funcionales dentro de su base intrínsecamente orientada a objetos. Esto significa que los desarrolladores pueden modelar dominios de datos mediante clases y objetos, pero al mismo tiempo procesar esos datos utilizando técnicas funcionales, escogiendo la herramienta más adecuada y expresiva para cada fragmento del sistema.

Que las funciones sean tratadas como "ciudadanos de primera clase" implica que el lenguaje otorga a las funciones los mismos derechos y privilegios que a cualquier variable tradicional (como un entero o un objeto). Por tanto, una función puede ser guardada en una variable, pasada como argumento a otra función, o retornada como resultado desde dentro de una función, lo cual es la base para construir operaciones lógicas abstractas y reutilizables.

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
La sintaxis básica de una expresión lambda en Java se divide en tres partes: una lista de parámetros separados por comas y encerrados entre paréntesis, el operador flecha (->), y el cuerpo de la función. El cuerpo puede consistir en una única expresión o en un bloque de código entre llaves. Si es una única expresión, el valor resultante se devuelve implícitamente sin necesidad de utilizar la palabra clave return.

El compilador de Java posee mecanismos de inferencia de tipos, lo que permite simplificar aún más la sintaxis. A menudo, no es necesario especificar el tipo de dato de los parámetros de entrada, ya que se deduce del contexto. Además, si la lambda recibe exactamente un solo parámetro, se pueden omitir los paréntesis que lo rodean.

Por ejemplo, la sintaxis completa sería (String s) -> { return s.toLowerCase(); }. Aprovechando las reglas de simplificación e inferencia descritas, la misma lambda se reduce a su forma más compacta e idiomática: s -> s.toLowerCase().

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
Recibir funciones como parámetros es una de las aplicaciones directas de tratar a las funciones como ciudadanos de primera clase. A las funciones o métodos que aceptan otras funciones como argumentos se les conoce como "funciones de orden superior" (higher-order functions). Esto permite separar el algoritmo de iteración o ejecución de la lógica específica de transformación.

A continuación se muestra cómo se implementa este concepto en JavaScript, pasando la función por referencia y ejecutándola internamente con los datos proporcionados.

// Definición del método de orden superior
function transformar(texto, funcionTransformadora) {
    return funcionTransformadora(texto);
}

const aMayusculas = (cadena) => cadena.toUpperCase();
let resultado = transformar("hola mundo", aMayusculas);
console.log(resultado);

En Java, se emplea la misma idea utilizando la interfaz genérica Function. El método transformar define explícitamente el tipo de la función esperada, garantizando el chequeo estático de tipos durante la compilación antes de invocar el método apply().

import java.util.function.Function;

public class OrdenSuperior {
    // Definición del método estático de orden superior
    public static String transformar(String texto, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();
        
        String resultado = transformar("hola mundo", aMayusculas);
        System.out.println(resultado);
    }
}

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta
La verdadera expresividad de las expresiones lambda se evidencia al pasarlas directamente como argumentos ("en línea") sin necesidad de almacenarlas previamente en una variable. Esto resulta extremadamente útil para comportamientos que no van a reutilizarse en ninguna otra parte del código, evitando contaminar el espacio de nombres con variables innecesarias.

En JavaScript, la inversión de una cadena se realiza convirtiéndola a un array, invirtiéndolo y volviéndolo a unir, pasando la lógica directamente en la invocación del método transformar.

// Reutilizando la función transformar() anterior
let textoInvertido = transformar("hola", s => s.split('').reverse().join(''));
console.log(textoInvertido); // Imprime: aloh

En Java, la lógica se introduce de la misma forma directamente en el paso de argumentos. Dado que la clase StringBuilder posee un método nativo para invertir caracteres, el código resulta directo y fuertemente tipado.

public class LlamadaEnLinea {
    public static void main(String[] args) {
        // Se define la lambda en el mismo momento de la invocación
        String textoInvertido = OrdenSuperior.transformar(
            "hola", 
            s -> new StringBuilder(s).reverse().toString()
        );
        
        System.out.println(textoInvertido); // Imprime: aloh
    }
}

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta
Un cierre, también conocido como closure, es la capacidad de una función lambda para "recordar" y acceder al entorno en el que fue creada. Esto significa que la lambda puede leer variables locales y parámetros del método exterior que la envuelve, incluso si la ejecución de ese método exterior ya ha terminado. La función "captura" (cierra sobre) el estado de su contexto léxico.

En Java, existe una restricción importante respecto a los cierres: las variables locales externas capturadas por una lambda deben ser finales o "efectivamente finales" (es decir, que su valor no se modifique una vez inicializado). Esto garantiza la consistencia del estado en un entorno multihilo sin necesidad de bloqueos complejos.

En el siguiente ejemplo, la función lambda accede y concatena el valor de la variable local sufijo, a pesar de que esta variable no fue pasada como argumento a la propia función.

import java.util.function.Function;

public class EjemploClosure {
    public static void main(String[] args) {
        // Variable local en el entorno exterior
        String sufijo = "!!!"; 
        
        // La lambda hace un "cierre" sobre la variable 'sufijo'
        Function<String, String> enfatizar = s -> s + sufijo;
        
        String resultado = OrdenSuperior.transformar("hola", enfatizar);
        System.out.println(resultado); // Imprime: hola!!!
        
        // Si intentáramos modificar 'sufijo' aquí (e.g., sufijo = "?"), 
        // el compilador daría un error en la definición de la lambda.
    }
}

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta
La diferencia más profunda radica precisamente en la capacidad de generar cierres (closures). Un puntero a función en C es únicamente una dirección de memoria estática que apunta a unas instrucciones compiladas; no posee memoria inherente ni "estado". No tiene forma de recordar ni encapsular variables locales de la función externa donde se referenció el puntero, dependiendo exclusivamente de parámetros globales o de estado pasado explícitamente mediante argumentos.

Por otro lado, una función lambda no es solo código, sino "código combinado con su entorno". Cuando se instancia una lambda que captura variables locales, el entorno de ejecución (como la JVM) crea internamente un objeto que envuelve tanto la lógica a ejecutar como copias ocultas de las referencias capturadas del contexto.

Por tanto, mientras que en C se deben usar estructuras auxiliares (pasando punteros void* adicionales) para simular el paso de un contexto junto con una función, la lambda moderna abstrae toda esta complejidad y encapsula ambos conceptos (comportamiento y estado inmutable circundante) en una única entidad cohesiva y segura en tiempo de compilación.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
Otra capacidad esencial de tratar a las funciones como ciudadanos de primera clase es la posibilidad de ser retornadas por otros métodos. Este patrón de diseño funcional (que simula la aplicación parcial de funciones) es ideal para preconfigurar algoritmos (en este caso, descuentos) y almacenarlos para su posterior ejecución iterativa.

En el ejemplo inferior, el método de fábrica crearDescuento genera y retorna lambdas configuradas dinámicamente. El closure aquí es crucial: la lambda devuelta captura la variable local porcentaje proporcionada durante su creación. Aunque el método crearDescuento termine su ejecución y su marco de pila desaparezca, la función de retorno seguirá conservando el valor específico de porcentaje congelado en su memoria interna.

import java.util.function.Function;

public class GeneradorDescuentos {

    // Método que fabrica y devuelve una nueva función lambda
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // La lambda captura el valor de 'porcentaje' en su closure
        return precioBase -> precioBase - (precioBase * porcentaje / 100.0);
    }

    public static void main(String[] args) {
        // Se instancian dos lógicas distintas mediante la captura de estado
        Function<Double, Double> rebajasBlackFriday = crearDescuento(30.0);
        Function<Double, Double> rebajasSocio = crearDescuento(10.0);
        
        double precioOriginal = 100.0;
        
        // Ejecución de las funciones pre-configuradas
        System.out.println("Precio Black Friday: " + rebajasBlackFriday.apply(precioOriginal));
        System.out.println("Precio Socio: " + rebajasSocio.apply(precioOriginal));
    }
}

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
Una interfaz funcional en Java es una interfaz convencional que impone una restricción muy específica: debe contener de forma estricta un único método abstracto no implementado (conocido como Single Abstract Method o SAM). Este único método proporciona la firma (parámetros y tipo de retorno) contra la que el compilador verificará la expresión lambda.

Cuando se asigna una lambda a una variable en Java, en segundo plano el compilador está creando anónimamente una implementación transparente de esa interfaz funcional, inyectando el código de la lambda dentro del único método abstracto disponible. Para que el compilador tenga certeza inequívoca de a qué método corresponde la lambda, solo puede haber uno en la jerarquía.

Opcionalmente, estas interfaces pueden etiquetarse con la anotación @FunctionalInterface. Esto no es obligatorio para su funcionamiento, pero actúa como una directiva de seguridad; si otro desarrollador intenta añadir un segundo método abstracto por error a la interfaz, el compilador emitirá un fallo inmediato, preservando así la compatibilidad con expresiones lambda. Pueden contener, no obstante, métodos predeterminados (default) o métodos estáticos sin perder su estatus funcional.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
Para definir una interfaz funcional propia que replique el comportamiento de transformación de textos, simplemente se declara una interfaz con un único método que reciba y retorne el tipo String. Se recomienda incorporar la anotación comentada previamente para blindar su diseño.

Una vez definida, esta interfaz puede utilizarse como tipo de dato para cualquier expresión lambda que coincida con esa firma específica, en sustitución de la interfaz predefinida del sistema.

@FunctionalInterface
public interface Transformador {
    // Único método abstracto: recibe un String y retorna un String
    String transformar(String textoOriginal);
}

// Ejemplo de su uso directo
// Transformador aMayusculas = s -> s.toUpperCase();
// String resultado = aMayusculas.transformar("hola");

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
La interfaz funcional anterior era rígida y solo permitía trabajar con texto. Aprovechando el mecanismo de parámetros de tipo (generics), se puede generalizar la interfaz para que acepte dos tipos distintos definidos en el momento de su instanciación: un tipo para el dato de entrada y otro tipo para el resultado.

Al combinar generics e interfaces funcionales, se logra un grado de reutilización de código extremadamente alto, manteniendo la total seguridad en el chequeo de tipos estáticos en tiempo de compilación.

@FunctionalInterface
public interface TransformadorGenerico<T, R> {
    // T: Tipo del argumento de entrada
    // R: Tipo de retorno
    R transformar(T entrada);
}

public class UsoTransformadorGenerico {
    public static void main(String[] args) {
        // Implementación de T=Double, R=Integer
        TransformadorGenerico<Double, Integer> redondeador = 
            numero -> (int) Math.round(numero);
            
        Integer resultado = redondeador.transformar(5.7);
        System.out.println(resultado); // Imprime: 6
    }
}

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
Efectivamente, para evitar que cada programador recree constantemente la interfaz genérica de transformación, Java incluye en el paquete java.util.function un abanico de interfaces funcionales predefinidas que cubren la inmensa mayoría de casos de uso habituales.

Las interfaces fundamentales se dividen según lo que reciben y lo que devuelven:

Function<T, R>: Recibe un argumento de tipo T y devuelve un resultado de tipo R (equivalente al ejemplo anterior).

Consumer<T>: Recibe un argumento de tipo T y no devuelve nada (void). Útil para consumir datos, procesarlos o imprimirlos, realizando operaciones con efectos secundarios.

Supplier<T>: No recibe ningún argumento y devuelve un resultado de tipo T. Actúa como un proveedor o generador de objetos.

Predicate<T>: Recibe un argumento de tipo T y devuelve siempre un valor booleano (boolean). Se emplea de manera intensiva para evaluar condiciones y aplicar filtros a colecciones de datos.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta
El método forEach incluido en la API de colecciones de Java representa el paso del modelo imperativo de iteración al modelo declarativo funcional. En lugar de gestionar manualmente la estructura del bucle y los índices, se le dice directamente a la colección que aplique una operación (un Consumer) a cada uno de sus elementos internos.

En el siguiente ejemplo, se emplea una expresión lambda para construir ese Consumer sobre la marcha, verificando la condición e imprimiendo el resultado en unas pocas líneas de código concisas.

import java.util.Arrays;
import java.util.List;

public class BucleFuncional {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-2, 5, -8, 12, 0);
        
        // Iteración funcional usando forEach y un Consumer en forma de lambda
        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("El número " + n + " es positivo");
            }
        });
    }
}
## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
La firma Consumer<? super T> emplea contravarianza (el comodín ? super) para brindar máxima flexibilidad al usar la colección. La sigla PECS significa "Producer Extends, Consumer Super". Esta regla mnemotécnica indica que si la estructura va a consumir datos proporcionados, se debe usar ? super (permitiendo que el consumidor acepte la clase base o cualquiera de sus superclases). Si la interfaz requiriera estrictamente un Consumer<Integer>, no se podría pasar a forEach un consumidor más genérico como un Consumer<Number> u Object, a pesar de que este último es perfectamente capaz de procesar e imprimir un Integer sin riesgo.

Aplicando la regla PECS para diseñar una versión robusta del método genérico transformar(entrada, funcion), se deben analizar los roles. El método transformar consume la variable de entrada de tipo T y la provee a la función matemática, y a su vez extrae y devuelve un objeto de tipo R generado por esa función.

Por lo tanto, la mejora de la firma del método empleando wildcards resultaría en public static <T, R> R transformar(T entrada, Function<? super T, ? extends R> funcion). Esto garantiza que la función sea capaz de aceptar elementos del tipo T o supertipos generales (actuando de consumidor de la entrada) y devolver elementos de tipo R o sus subtipos específicos (actuando de productor para el retorno global).
## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Las referencias a métodos son una forma compacta y legible de escribir expresiones lambda preexistentes, apuntando de forma directa a un método sin escribir el bloque de código que lo invoca.

En JavaScript, se puede guardar una referencia directa al método de un objeto. Sin embargo, para no perder el contexto interno de la instancia (this), resulta necesario enlazar (hacer un bind) la referencia al propio objeto.

class Persona {
    constructor(nombre) { this.nombre = nombre; }
    saludar() { console.log(`Hola, soy ${this.nombre}`); }
}

const p = new Persona("Ana");
// Obtención de referencia ligando el contexto del objeto
const refSaludar = p.saludar.bind(p); 

refSaludar(); // Imprime: Hola, soy Ana

En Java, se emplea el operador de doble dos puntos (::). El compilador interpreta automáticamente la firma del método apuntado para encajarla dentro de una interfaz funcional predefinida, como Runnable en caso de que no reciba parámetros ni retorne valores.

class Persona {
    private String nombre;
    public Persona(String nombre) { this.nombre = nombre; }
    public void saludar() { System.out.println("Hola, soy " + this.nombre); }
}

public class Referencias {
    public static void main(String[] args) {
        Persona p = new Persona("Ana");
        
        // Referencia directa al método de la instancia 'p'
        Runnable refSaludar = p::saludar; 
        
        // Ejecución
        refSaludar.run(); // Imprime: Hola, soy Ana
    }
}

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
Existen cuatro categorías principales para obtener referencias mediante el operador :: en Java, cubriendo los distintos modos en los que se alojan los métodos en la orientación a objetos.

Método Estático: Apunta a métodos asociados a la clase, no al objeto. Ejemplo: transformar un string a entero.

Referencia al Constructor: Sirve para emular fábricas de objetos de un tipo específico llamando a su constructor.

Método de una instancia concreta: (Ya visto en la pregunta anterior), ejecuta el método atado a las variables locales de un objeto preexistente.

Método de instancia arbitraria de un tipo particular: El método se llamará sobre el primer parámetro proporcionado en tiempo de ejecución, en lugar de ligarlo a un objeto concreto de antemano.

import java.util.function.*;

public class TiposReferencias {
    public static void main(String[] args) {
        // 1. Método Estático (Math.sqrt recibe double, devuelve double)
        Function<Double, Double> raizCuadrada = Math::sqrt;

        // 2. Referencia a Constructor (El String se pasa como argumento al constructor)
        Function<String, StringBuilder> crearBuilder = StringBuilder::new;

        // 3. Método de Instancia Concreta
        String prefijo = "Dato: ";
        Function<String, String> añadirPrefijo = prefijo::concat;

        // 4. Método de Instancia Arbitraria
        // El objeto sobre el que se aplica pasa a ser el primer parámetro
        Function<String, String> pasarMayusculas = String::toUpperCase;
    }
}

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
El uso de funciones lambda brilla particularmente al escribir criterios de ordenación, reduciendo notablemente el código tedioso derivado de implementar clases separadas solo para la interfaz Comparator.

A continuación se muestra una aproximación puramente algorítmica y manual, implementando el método de comparación de la interfaz mediante una lambda tradicional y evaluando las reglas con un bloque imperativo habitual.

import java.util.*;

class Persona {
    String nombre; int edad;
    Persona(String n, int e) { this.nombre = n; this.edad = e; }
    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }
}

public class Ordenacion {
    public static void main(String[] args) {
        List<Persona> lista = Arrays.asList(new Persona("Zoe", 30), new Persona("Ana", 30), new Persona("Luis", 25));

        // VERSIÓN MANUAL: Lógica de comparación anidada
        Collections.sort(lista, (p1, p2) -> {
            int comparacionEdad = Integer.compare(p1.getEdad(), p2.getEdad());
            if (comparacionEdad == 0) {
                return p1.getNombre().compareTo(p2.getNombre());
            }
            return comparacionEdad;
        });
    }
}

La segunda versión recurre a los métodos utilitarios estáticos y predeterminados añadidos a la propia interfaz Comparator en Java 8. Esta aproximación encadena referencias a métodos funcionales preexistentes (Persona::getEdad y Persona::getNombre), configurando un motor de ordenación declarativo ("ordenar por esto, y luego por aquello") inmensamente más fácil de leer, comprender y mantener.

import java.util.*;

public class OrdenacionFuncional {
    public static void main(String[] args) {
        List<Persona> lista = Arrays.asList(new Persona("Zoe", 30), new Persona("Ana", 30), new Persona("Luis", 25));

        // VERSIÓN MODERNA: Composición de comparadores mediante referencias a método
        lista.sort(Comparator.comparingInt(Persona::getEdad)
                             .thenComparing(Persona::getNombre));
                             
        // Salida tras el sort: Luis (25), Ana (30), Zoe (30)
    }
}
```</T,></Number></Integer></T></T></T></T></T,></T,></Double,></T,></String,>