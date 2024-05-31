# ironhack-code-optimization

### Objective:
-   Enhance the performance of provided code snippets by applying code-level optimization techniques manually. 
-   This lab focuses on deepening your understanding and application of optimization principles in coding.


### Instructions:
### Introduction:
-   At the beginning of the lab, you will receive a set of code snippets that contain common performance issues. 
-   These snippets will come from different programming languages, including JavaScript, Java, and C#, to cover a variety of scenarios and common inefficiencies found in real-world programming.


### JavaScript Snippet:

``` javascript
// Inefficient loop handling and excessive DOM manipulation
function updateList(items) {
  let list = document.getElementById("itemList");
  list.innerHTML = "";
  for (let i = 0; i < items.length; i++) {
    let listItem = document.createElement("li");
    listItem.innerHTML = items[i];
    list.appendChild(listItem);
  }
}

```

Solution

``` javascript

// Manejo de bucles optimizado y reducción de manipulaciones del DOM
function updateList(items) {
  let list = document.getElementById("itemList");
  list.innerHTML = ""; // Limpiar la lista existente

  // Crear un fragmento de documento para reducir las manipulaciones del DOM
  let fragment = document.createDocumentFragment();

  items.forEach(item => {
    let listItem = document.createElement("li");
    listItem.textContent = item; // Usar textContent para mejor rendimiento y seguridad
    fragment.appendChild(listItem);
  });

  // Agregar el fragmento a la lista en una sola operación
  list.appendChild(fragment);
}


```

### Explicacion:
-   Se crea un fragmento de documento para mantener temporalmente los nuevos elementos de la lista. Esto nos permite realizar múltiples manipulaciones del DOM en memoria, lo que es más rápido que hacerlo directamente en el DOM.

``` javascript
let fragment = document.createDocumentFragment();
```

-   Usar forEach para la iteración y textContent para establecer el contenido del elemento de la lista mejora el rendimiento y la seguridad.

``` javascript
items.forEach(item => {
  let listItem = document.createElement("li");
  listItem.textContent = item;
  fragment.appendChild(listItem);
});
```

-   El fragmento con todos los elementos de la lista se agrega al DOM en una sola operación, lo que reduce significativamente el número de manipulaciones del DOM.

``` javascript
list.appendChild(fragment);
```


### Ejercicio 2

``` java
// Redundant database queries
public class ProductLoader {
    public List<Product> loadProducts() {
        List<Product> products = new ArrayList<>();
        for (int id = 1; id <= 100; id++) {
            products.add(database.getProductById(id));
        }
        return products;
    }
}

```

El código proporcionado contiene un problema de rendimiento común relacionado con consultas redundantes a la base de datos. 

Esto puede ralentizar significativamente el programa, especialmente cuando la cantidad de productos es grande. El codigo actual realiza 100 consultas a la base de datos para cargar 100 productos, lo cual es ineficiente.

### Código Optimizado
La principal optimización aquí es reemplazar las múltiples consultas individuales por una única consulta en lote para obtener todos los productos de una vez. Suponiendo que el objeto database tiene un método para recuperar múltiples productos en una sola llamada, podemos usarlo para optimizar el código.

``` java
import java.util.ArrayList;
import java.util.List;

public class Products {
    private Database database;

    public Products(Database database) {
        this.database = database;
    }

    public List<Product> loadProducts() {
        return database.getProductsByIds(List<Integer> ids);
    }

}
```

Consulta: El método loadProducts ahora llama a un solo método getProductsByIds que recupera todos los productos de una vez

### Ejercicio 3

``` c#
// Unnecessary computations in data processing
public List<int> ProcessData(List<int> data) {
    List<int> result = new List<int>();
    foreach (var d in data) {
        if (d % 2 == 0) {
            result.Add(d * 2);
        } else {
            result.Add(d * 3);
        }
    }
    return result;
}

```

Código Optimizado
-   Preasignar la Lista de Resultados: 
    -   Si sabemos el tamaño de la lista resultante, podemos preasignarla para evitar redimensionamientos durante la adición.
-   Evitar Múltiples Operaciones de Módulo: 
    -   Aunque la implementación actual ya es eficiente, podemos considerar que cada iteración tiene una operación de módulo. Sin embargo, esto es necesario para determinar si el número es par o impar.


``` c#
public List<int> ProcessData(List<int> data) {
    List<int> result = new List<int>(data.Count);
    
    foreach (var d in data) {
        result.Add((d % 2 == 0) ? d * 2 : d * 3);
    }
    
    return result;
}
```