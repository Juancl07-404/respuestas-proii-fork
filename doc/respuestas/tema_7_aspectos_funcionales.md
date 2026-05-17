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


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
