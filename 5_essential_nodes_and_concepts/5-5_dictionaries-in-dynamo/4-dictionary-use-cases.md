# Casos de uso de Revit

¿Alguna vez ha deseado buscar algo en Revit mediante un segmento de datos incluido en la solución?

Si la respuesta es afirmativa, es probable que haya hecho algo similar a lo que aparece en el ejemplo siguiente.

En la imagen mostrada a continuación, recopilamos todas las habitaciones del modelo de Revit, obtenemos el índice de la habitación que deseamos (por número de habitación) y, por último, obtenemos la habitación en el índice.

![](../images/5-5/4/dictionary-collectroominrevitmodel.jpg)

> 1. Recopile todas las habitaciones del modelo.
> 2. Especifique el número de habitación que se va a buscar.
> 3. Obtenga el número de habitación y busque el índice en el que se encuentra.
> 4. Obtenga la habitación en el índice.

## Ejercicio: diccionario de habitaciones

### Parte I: creación de un diccionario de habitaciones

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/5-5/4/roomDictionary.dyn" %}

Ahora vamos a recrear esta idea mediante diccionarios. Debemos recopilar primero todas las habitaciones de nuestro modelo de Revit.

![](../images/5-5/4/dictionary-exerciseI-01.jpg)

> 1. Seleccionamos la categoría de Revit con la que deseamos trabajar (en este caso, trabajaremos con habitaciones).
> 2. Le indicamos a Dynamo que recopile todos esos elementos.

A continuación, debemos decidir las claves que vamos a utilizar para buscar estos datos. (La información sobre las claves se encuentra en la sección [¿Qué es un diccionario?](9-1\_what-is-a-dictionary.md)).

![](../images/5-5/4/dictionary-exerciseI-02.jpg)

> 1. Los datos que usaremos son el número de habitación.

Ahora crearemos el diccionario con las claves y los elementos especificados.

![](../images/5-5/4/dictionary-exerciseI-03.jpg)

> 1. El nodo **Dictionary.ByKeysValues** creará un diccionario con las entradas correspondientes especificadas.
> 2. `Keys` debe ser una cadena, mientras que `values` puede ser diversos tipos de objeto.

Por último, ahora podemos recuperar una habitación del diccionario con su número de habitación.

![](../images/5-5/4/dictionary-exerciseI-04.jpg)

> 1. `String` será la clave que utilizaremos para buscar un objeto en el diccionario.
> 2. **Dictionary.ValueAtKey** obtendrá ahora el objeto del diccionario.

### Parte II: búsqueda de valores

Con esta misma lógica de diccionario, también podemos crear diccionarios con objetos agrupados. Si deseamos buscar todas las habitaciones en un nivel determinado, podemos modificar el gráfico anterior de la siguiente manera.

![](../images/5-5/4/dictionary-exerciseII-01.jpg)

> 1. En lugar de utilizar el número de habitación como clave, ahora podemos utilizar un valor de parámetro (en este caso, utilizaremos nivel).

![](../images/5-5/4/dictionary-exerciseII-02.jpg)

> 1. Ahora podemos agrupar las habitaciones por el nivel en el que residen.

![](../images/5-5/4/dictionary-exerciseII-03.jpg)

> 1. Con los elementos agrupados por el nivel, ahora podemos utilizar las claves compartidas (claves exclusivas) como nuestra clave para el diccionario y las listas de habitaciones como elementos.

![](../images/5-5/4/dictionary-exerciseII-04.jpg)

> 1. Por último, mediante los niveles del modelo de Revit, podemos buscar en el diccionario las habitaciones que se encuentran en ese nivel. `Dictionary.ValueAtKey` utilizará el nombre de nivel y devolverá los objetos de habitación presentes en ese nivel.

Las oportunidades de uso del diccionario son realmente infinitas. La capacidad de relacionar los datos de BIM de Revit con el propio elemento plantea diversos casos de uso.
