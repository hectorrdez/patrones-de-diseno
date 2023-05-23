## PATRON  ITERATOR
### ¿que es el patron iterator?
-Iterator es un patrón de diseño de comportamiento que te permite recorrer elementos de una colección sin exponer su representación subyacente (lista, pila, árbol, etc.).
### ¿que problemas resuelve?
-el problema principal que nos resuelve iterator es el de recorrer colecciones de datos, aunque la mayoria de colecciones almacena sus elementos en listas simples hay algunas que se basan en arboles, grafos, pilas y otras estructuras mas complejas de almacenamiento.

siempre ha de hbaer una forma de que un codigo pueda acceder y utilizar esos datos.

en los casos en los que tenemos una lista simple, esta tarea es sencilla ya que podremos usar un bucle para recorrer todos los elementos, pero, esta tarea se complica la encontrarnos otros tipos de estructuras como arboles,grafos,etc.

la idea que nos propone el patron iterator es la de implementar un objeto llamado iterator el cual recorra toda la coleccion y almacene todos los datos como la posicion y el numero de elemntos restantes hasta el final del recorrido,
normalamente los iteradores aportan un método principal para extraer elementos de la colección. 
El cliente puede continuar ejecutando este método hasta que no devuelva nada, lo que significa que el iterador ha recorrido todos los elementos.

### ¿como se implementa?
-Declara la interfaz iteradora, como mínimo, debe tener un método para extraer el siguiente elemento de una colección,Declara la interfaz de colección y describe un método para buscar iteradores, el tipo de retorno debe ser igual al de la interfaz iteradora,Implementa clases iteradoras concretas para las colecciones que quieras que sean recorridas por iteradores,Implementa la interfaz de colección en tus clases de colección. 
La idea principal es proporcionar al cliente un atajo para crear iteradores personalizados para una clase de colección particular.
### diagrama de clases
![diagrama](diagrama.jpg)
### ejemplo funcional 



```javascript
import java.util.Iterator;

// Interfaz Iterable
interface MyIterable<T> {
    Iterator<T> iterator();
}

// Clase que implementa el Iterable
class MyCollection<T> implements MyIterable<T> {
    private T[] elements;
    private int size;

    public MyCollection(int capacity) {
        elements = (T[]) new Object[capacity];
        size = 0;
    }

    public void add(T element) {
        if (size < elements.length) {
            elements[size++] = element;
        }
    }

    // Implementación del método iterator() del Iterable
    public Iterator<T> iterator() {
        return new MyIterator();
    }

    // Clase interna que implementa el Iterator
    private class MyIterator implements Iterator<T> {
        private int currentIndex;

        public MyIterator() {
            currentIndex = 0;
        }

        // Implementación del método hasNext() del Iterator
        public boolean hasNext() {
            return currentIndex < size;
        }

        // Implementación del método next() del Iterator
        public T next() {
            return elements[currentIndex++];
        }
    }
}

// Clase principal que utiliza el Iterable
public class IteratorExample {
    public static void main(String[] args) {
        MyCollection<String> collection = new MyCollection<>(3);
        collection.add("Hola");
        collection.add("Mundo");
        collection.add("!");

        // Utilizar el Iterator para recorrer la colección
        Iterator<String> iterator = collection.iterator();
        while (iterator.hasNext()) {
            String element = iterator.next();
            System.out.println(element);
        }
    }
}
```
### explicacion:

1. Se define la interfaz MyIterable<T> que contiene el método iterator().

2. La clase MyCollection<T> implementa la interfaz MyIterable<T> y proporciona una implementación de los métodos iterator().

3. Dentro de MyCollection<T>, se define una clase interna MyIterator que implementa la interfaz Iterator<T>. Esta clase es responsable de iterar sobre los elementos de la colección.

4. La clase MyCollection<T> contiene un array de elementos y un contador de tamaño. El método add(T element) se utiliza para agregar elementos a la colección.

5. El método iterator() de MyCollection<T> devuelve una instancia de MyIterator, que se utiliza para recorrer la colección.

6. En la clase principal IteratorExample, se crea una instancia de MyCollection<String>, se agregan elementos a la colección y luego se utiliza un bucle while junto con el método hasNext() y next() del iterador para recorrer la colección y mostrar cada elemento en la consola.
### venatajas de iterator
1. Principio de responsabilidad única. Puedes limpiar el código cliente y las colecciones extrayendo algoritmos de recorrido voluminosos y colocándolos en clases independientes.

2. Principio de abierto/cerrado. Puedes implementar nuevos tipos de colecciones e iteradores y pasarlos al código existente sin descomponer nada.

3. Puedes recorrer la misma colección en paralelo porque cada objeto iterador contiene su propio estado de iteración.

4. Por la misma razón, puedes retrasar una iteración y continuar cuando sea necesario.
### desentajas de iterator
1. Aplicar el patrón puede resultar excesivo si tu aplicación funciona únicamente con colecciones sencillas.

2. Utilizar un iterador puede ser menos eficiente que recorrer directamente los elementos de algunas colecciones especializadas.
### webgrafia
https://refactoring.guru/es/design-patterns/iterator

https://www.aprenderaprogramar.com/index.php?option=com_content&view=article&id=589:interface-iterable-y-metodo-iterator-api-java-recorrer-colecciones-ejercicio-y-ejemplo-resuelto-cu00915c&catid=58&Itemid=180

http://arantxa.ii.uam.es/~eguerra/docencia/0809/04%20Iterator.pdf







