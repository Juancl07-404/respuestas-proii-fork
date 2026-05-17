<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta
Para lograr una estructura capaz de almacenar cualquier tipo de dato, se puede recurrir al tipo más genérico disponible en el lenguaje. En Java, dado que todas las clases heredan de Object, un array de este tipo permite guardar cualquier instancia. En C, al carecer de orientación a objetos, este mismo concepto se simula utilizando punteros genéricos void*, los cuales pueden apuntar a cualquier dirección de memoria sin importar el tipo de dato subyacente.

A continuación, se muestra un ejemplo en Java mediante una clase que encapsula un array de Object. Esta estructura permite añadir tanto cadenas de texto, como números (mediante sus clases envolventes como Integer o Double) o cualquier otro objeto de diseño propio.

public class AlmacenGenerico {
    private Object[] elementos;
    private int contador;

    public AlmacenGenerico(int capacidad) {
        this.elementos = new Object[capacidad];
        this.contador = 0;
    }

    public void add(Object elemento) {
        if (contador < elementos.length) {
            elementos[contador++] = elemento;
        }
    }

    public Object get(int indice) {
        return elementos[indice];
    }
}
## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta
La programación genérica es un paradigma en el que los algoritmos y las estructuras de datos se escriben en términos de tipos que se especifican más adelante, es decir, cuando se instancian o se invocan. El objetivo es escribir código reutilizable e independiente del tipo de dato, manteniendo al mismo tiempo un estricto control y seguridad de tipos durante la compilación.

El ejemplo anterior (usando Object o void*) representa un enfoque primitivo para lograr la reutilización de código frente a múltiples tipos, a menudo conocido como polimorfismo de inclusión. Sin embargo, no se considera verdadera programación genérica en los lenguajes modernos. Esto se debe a que el compilador no tiene información sobre los tipos reales que se están manejando, perdiéndose la seguridad en tiempo de compilación que caracteriza a los mecanismos genéricos actuales.
## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta
El principal problema radica en la pérdida de información sobre el tipo de dato original, lo que anula la capacidad del compilador para realizar comprobaciones de seguridad (chequeo de tipos). Cuando se extrae un elemento de una estructura basada en Object (o void*), el entorno no sabe si ese elemento es una cadena, un entero o cualquier otra cosa.

Debido a esto, surge un segundo problema: la necesidad constante de realizar conversiones explícitas de tipo (conocidas como cast o downcasting). Si se introduce accidentalmente un tipo de dato incorrecto en la estructura (por ejemplo, un número donde se esperaban textos), el compilador no mostrará ningún error. El fallo se manifestará de forma abrupta durante la ejecución del programa (provocando un ClassCastException en Java o un comportamiento indefinido en C), lo cual dificulta la creación de software robusto.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta
Los parámetros de tipo son identificadores (comúnmente letras mayúsculas como T, E, K o V) que actúan como marcadores de posición o "comodines" para los tipos de datos reales que se utilizarán más adelante. En lugar de codificar una clase o método para que trabaje exclusivamente con un String o un Object, se define para que trabaje con un tipo T.

Cuando se hace uso de esa clase o método, se debe especificar qué tipo concreto sustituirá al parámetro T (por ejemplo, instanciando una lista de enteros proporcionando el tipo Integer). Gracias a esto, el compilador puede verificar que todas las operaciones que involucran a T son válidas y seguras para el tipo concreto proporcionado, eliminando la necesidad de hacer castings manuales al recuperar los datos.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta
A continuación, se muestra cómo se utilizan las plantillas (templates) en C++ instanciando un vector de la biblioteca estándar, seguido de un recorrido donde cada elemento se trata directamente como una cadena sin conversiones adicionales.

#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> listaCadenas;
    listaCadenas.push_back("Hola");
    listaCadenas.push_back("Mundo");

    // Recorrido seguro sin necesidad de casting
    for (size_t i = 0; i < listaCadenas.size(); i++) {
        std::string texto = listaCadenas[i];
        std::cout << texto << std::endl;
    }
    return 0;
}

En Java, se emplea la característica generics mediante el uso del operador diamante (<>) junto con la colección ArrayList. Al igual que en C++, no se requiere downcasting para recuperar el objeto original.

import java.util.ArrayList;

public class EjemploGenerics {
    public static void main(String[] args) {
        ArrayList<String> listaCadenas = new ArrayList<>();
        listaCadenas.add("Hola");
        listaCadenas.add("Mundo");

        // Recorrido seguro sin casting
        for (int i = 0; i < listaCadenas.size(); i++) {
            String texto = listaCadenas.get(i);
            System.out.println(texto);
        }
    }
}

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
El comportamiento interno de los compiladores difiere drásticamente entre ambos lenguajes. En C++, la "instanciación de plantillas" funciona casi como un sofisticado sistema de macros. Cuando se declara un vector<int> y un vector<string>, el compilador de C++ genera y compila dos copias de código binario completamente independientes, una para cada tipo. Esto optimiza el rendimiento y mantiene la información del tipo en tiempo de ejecución, pero puede inflar el tamaño del archivo ejecutable.

Por el contrario, Java no genera múltiples clases compiladas. En su lugar, emplea un proceso llamado "type erasure" (borrado de tipos). Durante la compilación, se verifican que los tipos sean correctos, pero luego se eliminan los parámetros de tipo del código binario final (el bytecode). El compilador sustituye automáticamente las apariciones de <T> por el límite del tipo (generalmente Object) y añade transparentemente las conversiones de tipo (castings) necesarias donde se recuperan los datos.

Por lo tanto, en Java, una ArrayList<String> y una ArrayList<Integer> comparten exactamente la misma clase y código en tiempo de ejecución. Esto asegura la compatibilidad hacia atrás con versiones antiguas de Java que no poseían generics, pero implica que en tiempo de ejecución no es posible determinar con qué parámetro de tipo exacto se instanció un objeto.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
