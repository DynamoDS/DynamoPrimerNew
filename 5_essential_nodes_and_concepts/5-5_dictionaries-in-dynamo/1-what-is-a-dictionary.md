# ¿Qué es un diccionario?

Dynamo 2.0 introduce el concepto de separación del tipo de datos del diccionario del tipo de datos de lista. Esta novedad puede conllevar algunos cambios importantes en la forma de crear y trabajar con datos en los flujos de trabajo. Antes de la versión 2.0, los diccionarios y las listas se combinaban como un tipo de datos. En resumen, las listas eran en realidad diccionarios con claves de enteros.

### **¿Qué es un diccionario?**

Un diccionario es un tipo de datos compuesto por una colección de pares de clave-valor en los que cada clave es exclusiva para cada colección. Un diccionario no tiene orden y, básicamente, se pueden "buscar elementos" mediante una clave en lugar de un valor de índice como en una lista. _En Dynamo 2.0, las claves solo pueden ser cadenas._

### **¿Qué es una lista?**

Una lista es un tipo de datos compuesto por un conjunto de valores ordenados. En Dynamo, las listas utilizan enteros como valores de índice.

### **¿Por qué se ha realizado este cambio y en qué me afecta?**

La separación de los diccionarios y las listas convierte a los diccionarios en componentes de primera clase que se pueden utilizar para almacenar y buscar valores de forma rápida y sencilla sin necesidad de recordar un valor de índice o mantener una estructura de listas estricta a lo largo del flujo de trabajo. Durante las pruebas realizadas por los usuarios, hemos detectado una reducción considerable del tamaño del gráfico cuando se utilizaron los diccionarios en lugar de varios nodos `GetItemAtIndex`.

### **¿Cuáles son los cambios?**

* Se han producido cambios en la _sintaxis_ que modifican el modo en que se inicializarán y se utilizarán los diccionarios y las listas en los bloques de código.
  * Los diccionarios utilizan la siguiente sintaxis: `{key:value}`.
  * Las listas utilizan la siguiente sintaxis: `[value,value,value]`.
* Se han introducido _nuevos nodos_ en la biblioteca para ayudarle a crear, modificar y consultar diccionarios.
*   Las listas creadas con bloques de código v1.x se migrarán automáticamente al cargar la secuencia de comandos a la nueva sintaxis de lista que utiliza corchetes `[ ]` en lugar de llaves `{ }`.



\![](<../images/5-5/1/what is a dictionary - what are the changes (1) (1) (1).jpg>)



### **¿En qué me afecta? ¿Para qué se utilizan?**

En las ciencias informáticas, los diccionarios, al igual que las listas, son colecciones de objetos. Mientras que las listas se encuentran en un orden específico, los diccionarios son colecciones _sin ordenar_. No dependen de números secuenciales (índices), sino de _claves_.

En la imagen siguiente, se muestra un posible caso de uso de un diccionario. A menudo, los diccionarios se utilizan para relacionar dos segmentos de datos que podrían no tener una correlación directa. En este caso, conectamos la versión en español de una palabra a la versión en inglés para su posterior búsqueda.

![](../images/5-5/1/whatisadictionary-whatwouldyouusethesefor.jpg)

> 1. Cree un diccionario para relacionar los dos datos.
> 2. Obtenga el valor con la clave especificada.
