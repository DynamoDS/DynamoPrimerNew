# Trabajo con listas

### Trabajo con listas

Ahora que hemos establecido lo que es una lista, hablemos de las operaciones que podemos realizar en ella. Imagine una lista como un mazo de cartas. El mazo es la lista y cada carta representa un elemento.

![cartas](../images/5-4/2/Playing\_cards\_modified.jpg)

> Fotografía de [Christian Gidlöf](https://commons.wikimedia.org/wiki/File:Playing\_cards\_modified.jpg)

### Consulta

¿Qué **consultas** se pueden realizar a partir de la lista? Esto permite acceder a las propiedades existentes.

* ¿Número de cartas del mazo? 52.
* ¿Número de palos? 4.
* ¿Material? Papel.
* ¿Longitud? 3,5" u 89 mm.
* ¿Anchura? 2,5" o 64 mm.

### Acción

¿Qué **acciones** se pueden realizar en la lista? Estas acciones cambian la lista en función de una operación especificada.

* Podemos barajar el mazo de cartas.
* Podemos ordenar el mazo por valor.
* Podemos ordenar el mazo por palo.
* Podemos dividir el mazo.
* Podemos dividir el mazo repartiendo manos individuales.
* Podemos seleccionar una carta específica del mazo.

Todas las operaciones mencionadas anteriormente tienen nodos análogos de Dynamo para trabajar con listas de datos genéricos. Las lecciones siguientes muestran algunas de las operaciones fundamentales que podemos realizar en las listas.

## **Ejercicio**

### **Operaciones de lista**

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/5-4/2/List-Operations.dyn" %}

La imagen siguiente es el gráfico base en el que se dibujan líneas entre dos círculos para representar operaciones de lista básicas. Exploraremos cómo administrar los datos de una lista y mostraremos los resultados visuales a través de las acciones de lista que aparecen a continuación.

![](<../images/5-4/2/working with list - list operation.jpg>)

> 1. Comience con un **bloque de código** con un valor de `500;`.
> 2. Conéctelo a la entrada x de un nodo **Point.ByCoordinates**.
> 3. Conecte el nodo del paso anterior a la entrada origin de un nodo **Plane.ByOriginNormal**.
> 4. Con un nodo **Circle.ByPlaneRadius**, conecte el nodo del paso anterior en la entrada plane.
> 5. Mediante el **bloque de código**, designe un valor de `50;` para el radio. Este es el primer círculo que crearemos.
> 6. Con un nodo **Geometry.Translate**, desplace el círculo hacia arriba 100 unidades en la dirección Z.
> 7. Con un nodo **Code Block**, defina un rango de diez números entre 0 y 1 con esta línea de código: `0..1..#10;`.
> 8. Conecte el bloque de código del paso anterior a la entrada _param_ de dos nodos **Curve.PointAtParameter**. Conecte **Circle.ByPlaneRadius** a la entrada curve del nodo superior y **Geometry.Translate** a la entrada curve del nodo situado debajo del mismo.
> 9. Con un nodo **Line.ByStartPointEndPoint**, conecte los dos nodos **Curve.PointAtParamete**_r_.

### List.Count

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/5-4/2/List-Count.dyn" %}

El nodo _List.Count_ es sencillo: cuenta el número de valores de una lista y devuelve ese número. Este nodo presenta más detalles cuando se trabaja con listas de listas, pero eso lo veremos en las secciones siguientes.

![Count](<../images/5-4/2/working with list - list operation - list count.jpg>)

> 1. El nodo **List.Count **_****_ devuelve el número de líneas del nodo **Line.ByStartPointEndPoint**. En este caso, el valor es 10, lo que coincide con el número de puntos creados a partir del nodo **Code Block** original.

### List.GetItemAtIndex

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/5-4/2/List-GetItemAtIndex.dyn" %}

**List.GetItemAtIndex** es una forma fundamental de consultar un elemento de la lista.

![Exercise](<../images/5-4/2/working with list - get item index 01.jpg>)

> 1. En primer lugar, haga clic con el botón derecho en el nodo **Line.ByStartPointEndPoint** para desactivar su vista preliminar.
> 2. Con el nodo **List.GetItemAtIndex**, seleccionamos el índice _"0"_ o el primer elemento de la lista de líneas.

Cambie el valor del control deslizante entre 0 y 9 para seleccionar un elemento diferente mediante **List.GetItemAtIndex**.

![](<../images/5-4/2/working with list - get item index 02.gif>)

### List.Reverse

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/5-4/2/List-Reverse.dyn" %}

_List.Reverse_ invierte el orden de todos los elementos de una lista.

![Exercise](<../images/5-4/2/working with list - list reverse.jpg>)

> 1. Para visualizar correctamente la lista de líneas invertida, cree más líneas. Para ello, cambie el nodo **Code Block** a `0..1..#50;`.
> 2. Duplique el nodo **Line.ByStartPointEndPoint**, inserte un nodo List.Reverse entre **Curve.PointAtParameter** y el segundo nodo **Line.ByStartPointEndPoint**.
> 3. Utilice los nodos **Watch3D** para obtener una vista preliminar de dos resultados diferentes. El primero muestra el resultado sin una lista invertida. Las líneas se conectan verticalmente con los puntos adyacentes. Sin embargo, la lista invertida conectará todos los puntos con la otra lista en el orden opuesto.

### List.ShiftIndices <a href="#listshiftindices" id="listshiftindices"></a>

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/5-4/2/List-ShiftIndices.dyn" %}

**List.ShiftIndices** es una buena herramienta para crear torsiones o patrones helicoidales, así como cualquier otra manipulación de datos similar. Este nodo desplaza los elementos de una lista un determinado número de índices.

![Exercise](<../images/5-4/2/working with list - shiftIndices 01.jpg>)

> 1. En el mismo proceso que la lista inversa, inserte un nodo **List.ShiftIndices** en **Curve.PointAtParameter** y **Line.ByStartPointEndPointPoint**.
> 2. Mediante un **bloque de código**, designe un valor de "1" para desplazar la lista un índice.
> 3. Observe que el cambio es sutil, pero todas las líneas del nodo **Watch3D** inferior se han desplazado un índice al conectarlas al otro conjunto de puntos.

Al cambiar el **bloque de código** a un valor superior, por ejemplo _"30"_, observamos una diferencia significativa en las líneas diagonales. El desplazamiento funciona como el iris de una cámara en este caso, creando una torsión en la forma cilíndrica original.

![](<../images/5-4/2/working with list - shiftIndices 02.jpg>)

### List.FilterByBooleanMask <a href="#listfilterbybooleanmask" id="listfilterbybooleanmask"></a>

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/5-4/2/List-FilterByBooleanMask.dyn" %}

![](../images/5-4/2/ListFilterBool.png)

**List.FilterByBooleanMask** eliminará determinados elementos en función de una lista de operaciones booleanos o valores de "verdadero" o "falso".

![Exercise](<../images/5-4/2/working with list - filter by bool mask.jpg>)

Para crear una lista de valores de "verdadero" o "falso", necesitamos realizar un poco más de trabajo...

> 1. Mediante un **bloque de código**, defina una expresión con la sintaxis: `0..List.Count(list);`. Conecte el nodo **Curve.PointAtParameter** a la entrada _list_. Veremos más detalles sobre esta configuración en el capítulo del bloque de código, pero la línea de código en este caso nos proporciona una lista que representa cada índice del nodo **Curve.PointAtParameter**.
> 2. Mediante un nodo _**%**_** (módulo)**, conecte la salida del _bloque de código_ en la entrada _x_ y un valor de _4_ en la salida _y_. Esto nos dará el resto al dividir la lista de índices entre 4. El módulo es un nodo muy útil para la creación de patrones. Todos los valores serán los restos posibles de 4: 0, 1, 2, 3.
> 3. En el nodo _**%**_** (módulo)**, sabemos que el valor 0 significa que el índice es divisible por 4 (0, 4, 8, etc.). Mediante un nodo **==**, podemos probar la divisibilidad frente a un valor de _"0"_.
> 4. El nodo **Watch** muestra solo esto: disponemos de un patrón verdadero/falso que dice: _verdadero,falso,falso,falso,falso..._
> 5. Mediante este patrón verdadero/falso, conéctelo a la entrada mask de dos nodos **List.FilterByBooleanMask**.
> 6. Conecte el nodo **Curve.PointAtParameter** en cada una de las entradas list de los nodos **List.FilterByBooleanMask**.
> 7. Las salidas de **Filter.ByBooleanMask** son _"in"_ y _"out"_. _"In"_ representa los valores que tenían un valor de máscara de _"Verdadero (True)"_ mientras que _"out"_ representa los valores que tenían un valor de _"Falso (False)"_. Al conectar las salidas _"in"_ a las entradas _startPoint_ y _endPoint_ de un nodo **Line.ByStartPointEndPoint**, hemos creado una lista filtrada de líneas.
> 8. El nodo **Watch3D** indica que hay menos líneas que puntos. Hemos seleccionado solo el 25% de los nodos filtrando solo los valores verdaderos.
