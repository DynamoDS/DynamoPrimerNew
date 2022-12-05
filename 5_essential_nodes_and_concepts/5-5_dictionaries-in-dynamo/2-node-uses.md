# Nodos de diccionario

Dynamo 2.0 presenta una gran variedad de nodos de diccionario para nuestro uso. Esto incluye los nodos de _creación, acción y consulta_.

![](../images/5-5/2/dictionarynodes-nodes.jpg)

#### Crear

1.`Dictionary.ByKeysValues` creará un diccionario con las claves y los valores proporcionados. _(El número de entradas será el de la entrada de lista más corta)._

#### Acción

2\. `Dictionary.Components` generará los componentes del diccionario de entrada. _(Es el proceso inverso al nodo creado)._

3\. `Dictionary.RemoveKeys` generará un nuevo objeto de diccionario con las claves de entrada eliminadas.

4\. `Dictionary.SetValueAtKeys` creará un nuevo diccionario basado en las claves de entrada y los valores para reemplazar el valor actual en las claves correspondientes.

5\. `Dictionary.ValueAtKey` devolverá el valor en la clave de entrada.

#### Conteo

6\. `Dictionary.Count` le indicará cuántos pares de clave-valor hay en el diccionario.

7\. `Dictionary.Keys` devolverá las claves almacenadas actualmente en el diccionario.

8\. `Dictionary.Values` devolverá los valores almacenados actualmente en el diccionario.

Relacionar de forma general datos con diccionarios es una magnífica alternativa al antiguo método de trabajo con índices y listas.
