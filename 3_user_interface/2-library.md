# Biblioteca

La biblioteca contiene todos los nodos cargados, incluidos los 10 nodos de categorías por defecto que se suministran con la instalación, así como los paquetes o los nodos personalizados cargados de forma adicional. Los nodos de la biblioteca se organizan jerárquicamente en bibliotecas, categorías y, si procede, subcategorías.

\![](<../.gitbook/assets/library - library UI.png>)

* Nodos básicos: se incluye con la instalación por defecto.
* Nodos personalizados: almacene las rutinas o los gráficos especiales utilizados con frecuencia como nodos personalizados. También puede compartir los nodos personalizados con la comunidad.
* Nodos de Package Manager: colección de nodos personalizados publicados.

Repasaremos las categorías de [jerarquía de nodos](2-library.md#library-hierarchy-for-categories), mostraremos cómo realizar rápidamente [búsquedas en la biblioteca](2-library.md#search-by-hierarchy) y conoceremos algunos de los [nodos utilizados con frecuencia](2-library.md#frequently-used-nodes).

### Jerarquía de categorías de la biblioteca

La exploración a través de estas categorías es la manera más rápida de conocer la jerarquía de lo que podemos añadir a nuestro espacio de trabajo y la mejor forma de descubrir nuevos nodos que no ha utilizado antes.

Examine la biblioteca. Para ello, haga clic en los menús para expandir cada categoría y su subcategoría.

{% hint style="info" %} Núcleo y Geometría son excelentes menús para empezar a explorar, ya que contienen la mayor cantidad de nodos. {% endhint %}

\![](<../.gitbook/assets/library - modified and resize library categories.jpg>)

> 1. Biblioteca
> 2. Categoría
> 3. Subcategoría
> 4. Nodo

Estos clasifican aún más los nodos en la misma subcategoría en función de si los nodos **crean** datos, ejecutan una **acción** o **consultan** datos.

* \![](<../.gitbook/assets/user interface - create.jpg>) **Crear**: cree o construya una geometría desde cero. Por ejemplo, un círculo.
* \![](<../.gitbook/assets/user interface - action.jpg>) **Acción**: realice una acción en un objeto. Por ejemplo, ajuste la escala de un círculo.
* \![](<../.gitbook/assets/user interface - query.jpg>) **Query**: obtenga una propiedad de un objeto existente. Por ejemplo, obtenga el radio de un círculo.

Coloque el puntero en un nodo para visualizar información más detallada, además de su nombre e icono. Esto nos permite conocer rápidamente lo que realiza el nodo, lo que necesitará para las entradas y lo que proporcionará como salida.

\![](<../.gitbook/assets/user interface - node description.jpg>)

> 1. Descripción: descripción del nodo en lenguaje normal.
> 2. Icono: versión más grande del icono en el menú Biblioteca.
> 3. Entrada(s): el nombre, y el tipo y la estructura de datos.
> 4. Salida(s): el tipo y la estructura de datos.

### Búsqueda rápida en la biblioteca

Si sabe con precisión relativa qué nodo desea añadir al espacio de trabajo, especifíquelo en el campo **Buscar** para buscar todos los nodos coincidentes.

Haga clic en el nodo que desea añadir o pulse Intro para añadir los nodos resaltados al centro del espacio de trabajo.

\![](<../.gitbook/assets/user interface - search.jpg>)

#### Buscar por jerarquía

Además de utilizar palabras clave para intentar encontrar nodos, podemos escribir la jerarquía separada por un punto en el campo de búsqueda o con bloques de código (que utilizan el _lenguaje textual de Dynamo_).

La jerarquía de cada biblioteca se refleja en el nombre de los nodos añadidos al espacio de trabajo.

Al escribir distintas partes de la ubicación del nodo en la jerarquía de biblioteca con el formato `library.category.nodeName`, se obtienen resultados diferentes.

* `library.category.nodeName`

\![](<../.gitbook/assets/library - search by hierarchy 1 geometry point by coordinates.jpg>)

* `category.nodeName`

\![](<../.gitbook/assets/library - search by hierarchy 2 point by coordinates.jpg>)

* `nodeName` o `keyword`

\![](<../.gitbook/assets/library - search by hierarchy 3 by coordinates.jpg>)

Por lo general, el nombre del nodo en el espacio de trabajo se renderizará con el formato `category.nodeName`, con algunas excepciones destacadas, sobre todo, en las categorías de entrada y vista.

Tenga en cuenta la diferencia de categoría en nodos con nombres similares:

* Los nodos de la mayoría de las bibliotecas incluirán el formato de categoría.

\![](<../.gitbook/assets/library - node category differences 1.jpg>)

* `Point.ByCoordinates` y `UV.ByCoordinates` tienen el mismo nombre, pero proceden de diferentes categorías.

\![](<../.gitbook/assets/library - node category differences 2.jpg>)

* Entre las excepciones destacables, se incluyen las funciones integradas, Core.Input, Core.View y los operadores.

\![](<../.gitbook/assets/library - node category differences 3.jpg>)

### Nodos utilizados frecuentemente

Con cientos de nodos incluidos en la instalación básica de Dynamo, ¿cuáles son fundamentales para desarrollar nuestros programas visuales? Veamos los parámetros de nuestro programa (**Input**), consultemos los resultados de la acción de un nodo (**Watch**) y definamos entradas o funciones mediante un acceso directo (**Code Block**).

#### Nodos de entrada

Los nodos de entrada son el principal medio para que el usuario de nuestro programa visual (ya sea usted mismo u otro usuario) interactúe con los parámetros clave. A continuación, se muestran algunos de la biblioteca principal:

| Nodo           |                                                        | Nodo           |                                                        |
| -------------- | ------------------------------------------------------ | -------------- | ------------------------------------------------------ |
| Booleano        | \![](<../.gitbook/assets/library - boolean.jpg>)        | Número         | \![](<../.gitbook/assets/library - number.jpg>)         |
| Cadena         | \![](<../.gitbook/assets/library - string.jpg>)         | Number Slider  | \![](<../.gitbook/assets/library - number slider.jpg>)  |
| Directory Path | \![](<../.gitbook/assets/library - directory path.jpg>) | Integer Slider | \![](<../.gitbook/assets/library - integer slider.jpg>) |
| File Path      | \![](<../.gitbook/assets/library - file path.jpg>)      |                |                                                        |

#### Watch y Watch3D

Los nodos de visualización, Watch, son esenciales para administrar los datos que se ejecutan a través del programa visual. Puede ver el resultado de un nodo a través de la **vista preliminar de datos del nodo**. Para ello, coloque el cursor sobre el nodo.

\![](<../.gitbook/assets/library - node preview.jpg>)

Será útil mantenerlo visible en un nodo **Watch**.

\![](<../.gitbook/assets/library - watch node.jpg>)

O bien, consulte los resultados de la geometría a través de un nodo **Watch3D**.

\![](<../.gitbook/assets/library - watch3d node.gif>)

Ambos se encuentran en la categoría de vista de la biblioteca principal.

{% hint style="info" %} Consejo: En ocasiones, la vista preliminar 3D puede ser molesta cuando el programa visual contiene muchos nodos. Considere la posibilidad de desactivar la opción Mostrando vista preliminar 3D en segundo plano en el menú Configuración y utilizar un nodo Watch3D para obtener una vista preliminar de la geometría. {% endhint %}

#### Bloque de código

Los nodos de bloque de código, Code Block, se pueden utilizar para definir un bloque de código con líneas separadas por signos de punto y coma. Esto puede ser tan sencillo como `X/Y`.

También podemos utilizar bloques de código como un acceso directo para definir una entrada de número o una llamada a la función de otro nodo. La sintaxis para esta acción sigue la convención de nomenclatura del lenguaje textual de Dynamo, [DesignScript](../8_coding_in_dynamo/8-1_code-blocks-and-design-script/2-design-script-syntax.md).

A continuación, se muestra una sencilla demostración (con instrucciones) de uso del bloque de código en la secuencia de comandos.

![](../.gitbook/assets/library-codeblockdemo.gif)

1. Haga doble clic para crear un nodo de bloque de código.
2. Escriba `Circle.ByCenterPointRadius(x,y);`
3. Haga clic en el espacio de trabajo para borrar la selección; se añadirán automáticamente las entradas `x` y `y`.
4. Cree un nodo Point.ByCoordinates y un control deslizante de número; a continuación, conecte las entradas al bloque de código.
5. El resultado de la ejecución del programa visual se muestra como un círculo en la vista preliminar 3D.
