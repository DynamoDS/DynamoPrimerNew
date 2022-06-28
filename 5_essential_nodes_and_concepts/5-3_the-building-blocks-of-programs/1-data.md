# Datos

Los datos son el contenido de nuestros programas. Viajan a través de cables proporcionando entradas a los nodos, donde se procesan y se transforman en una nueva forma de datos de salida. Revisemos la definición de los datos, cómo se estructuran y cómo utilizarlos en Dynamo.

## ¿Qué son los datos?

Los datos son un conjunto de valores de variables cualitativas o cuantitativas. La forma de datos más sencilla son números como, por ejemplo, `0`, `3.14` o `17`. Sin embargo, los datos también puede ser de diferentes tipos: una variable que representa números cambiantes (`height`); caracteres (`myName`); geometría (`Circle`), o una lista de elementos de datos (`1,2,3,5,8,13,...`).

En Dynamo, añadimos o introducimos datos en los puertos de entrada de los nodos; podemos tener datos sin acciones, pero nos hacen falta datos para procesar las acciones que representan nuestros nodos. Cuando se añade un nodo al espacio de trabajo, si no se le proporciona ninguna entrada, el resultado será una función y no el resultado de la acción en sí.

![Data and Actions](<../images/5-3/1/data - what is data.jpg>)

> 1. Datos simples.
> 2. Datos y una acción (un nodo); se ejecuta correctamente.
> 3. Una acción (un nodo) sin entradas de datos; devuelve una función genérica.

### Nulo: ausencia de datos

Cuidado con los valores nulos. El tipo `'null'` representa la ausencia de datos. Aunque se trata de un concepto abstracto, es probable que esto ocurra mientras trabaja con programación visual. Si una acción no crea un resultado válido, el nodo devolverá un valor "null".

Comprobar si existen valores "null" y eliminarlos de la estructura de datos es una parte crucial para crear programas sólidos.

| Icono | Nombre/sintaxis | Entradas | Salidas |
| ----------------------------------------------------- | ------------- | ------ | ------- |
| ![](<../images/5-3/1/data - object IsNull.jpg>) | Object.IsNull | obj | bool |

### Estructuras de datos

Cuando somos programadores visuales, podemos generar una gran cantidad de datos rápidamente y requerir un medio de administración de su jerarquía. Esta es la función de las estructuras de datos, los esquemas organizativos en los que almacenamos los datos. Las características específicas de las estructuras de datos y su uso varían de un lenguaje de programación a otro.

En Dynamo, se añade jerarquía a los datos a través de las listas. Exploraremos esto en profundidad en capítulos posteriores, pero empecemos con algo sencillo.

Una lista representa una colección de elementos colocados en una estructura de datos:

* Tengo cinco dedos (_elementos_) en la mano (_lista_).
* Hay diez casas (_elementos_) en mi calle (_lista_).

![List Breakdown](<../images/5-3/1/data - data structures.jpg>)

> 1. Un nodo **Number Sequence** define una lista de números utilizando entradas _start_, _amount_ y _step_. Con estos nodos, hemos creado dos listas independientes de diez números, una que abarca de _100 a 109_ y otra de _0 a 9_.
> 2. El nodo **List.GetItemAtIndex** selecciona un elemento de una lista en un índice determinado. Al seleccionar _0_, se obtiene el primer elemento de la lista (_100_ en este caso).
> 3. Al aplicar el mismo proceso a la segunda lista, obtenemos un valor de _0_, el primer elemento de la lista.
> 4. Ahora combinamos las dos listas en una mediante el nodo **List.Create**. Observe que el nodo crea una _lista de listas._ Esto cambia la estructura de los datos.
> 5. Al volver a utilizar **List.GetItemAtIndex**, con el índice establecido en _0_, se obtiene la primera lista de la lista de listas. Esto es lo que significa tratar una lista como un elemento, lo cual es ligeramente diferente de otros lenguajes de secuencias de comandos. En capítulos posteriores, nos adentraremos más en la manipulación de listas y la estructura de datos.

El concepto clave para comprender la jerarquía de datos en Dynamo es que **en relación con la estructura de datos, las listas se consideran elementos.** En otras palabras, Dynamo funciona con un proceso descendente para comprender las estructuras de datos. ¿Qué quiere decir esto? Veámoslo con un ejemplo.

## Ejercicio: uso de datos para crear una cadena de cilindros

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/5-3/1/Building Blocks of Programs - Data.dyn" %}

En este primer ejemplo, vamos a ensamblar un cilindro vaciado que recorre la jerarquía de geometría tratada en esta sección.

### Parte I: configurar el gráfico de un cilindro con algunos parámetros modificables

1. Añada **Point.ByCoordinates:** después de añadir el nodo al lienzo, se muestra un punto en el origen de la rejilla de vista preliminar de Dynamo. Los valores por defecto de las entradas _x, y_ y _z_ son _0,0_, lo que nos da un punto en esta ubicación.

![](<../images/5-3/1/data - exercise step 1.jpg>)

2\. **Plane.ByOriginNormal:** el siguiente paso en la jerarquía de geometría es un plano. Existen varias formas de construir un plano; utilizaremos un origen y una normal para la entrada. El origen es el nodo de punto creado en el paso anterior.

**Vector.ZAxis**: es un vector unificado en la dirección Z. Observe que no hay entradas, solo un vector con un valor \[0,0,1]. Se utiliza como la entrada _normal_ para el nodo **Plane.ByOriginNormal**. Esto nos proporciona un plano rectangular en la vista preliminar de Dynamo.

![](<../images/5-3/1/data - exercise step 2.jpg>)

3\. **Circle.ByPlaneRadius**: ascendiendo en la jerarquía, ahora creamos una curva a partir del plano del paso anterior. Después de conectar el nodo, se obtiene un círculo en el origen. El radio predeterminado del nodo es el valor de _1_.

![](<../images/5-3/1/data - exercise step 3.jpg>)

4\. **Curve.Extrude**: ahora vamos a hacer que este elemento crezca dándole profundidad y añadiendo la tercera dimensión. Este nodo crea una superficie a partir de una curva extrudiéndola. La distancia por defecto en el nodo es _1_, y debería aparecer un cilindro en la ventana gráfica.

![](<../images/5-3/1/data - exercise step 4.jpg>)

5\. **Surface.Thicken**: este nodo nos proporciona un sólido cerrado desfasando la superficie una distancia dada y cerrando la forma. El valor de grosor por defecto es _1_, y vemos un cilindro vaciado en la ventana gráfica en línea con estos valores.

![](<../images/5-3/1/data - exercise step 5.jpg>)

6\. **Number Slider**: en lugar de utilizar los valores por defecto para todas estas entradas, vamos a añadir algún control paramétrico al modelo.

**Edición del dominio:** después de añadir el control deslizante de número al lienzo, haga clic en el signo de intercalación situado en la parte superior izquierda para ver las opciones de dominio.

**Min/Max/Step**: cambie los valores _min_, _max_ y _step_ a _0_, _2_ y _0,01_ respectivamente. Esto lo hacemos para controlar el tamaño de la geometría global.

![](<../images/5-3/1/data - exercise step 6.gif>)

7\. Nodos **Number Slider:** en todas las entradas por defecto, copiemos y peguemos este control deslizante de número (seleccione el control deslizante, pulse Ctrl+C y, a continuación, Ctrl+V) varias veces hasta que todas las entradas con valores por defecto tengan un control deslizante. Algunos de los valores del control deslizante tendrán que ser mayores que cero para que la definición funcione (es decir, para que una superficie se engrose, se necesita una profundidad de extrusión).

![](<../images/5-3/1/data - exercise step 7a.gif>)

![](<../images/5-3/1/data - exercise step 7b.gif>)

8\. Hemos creado un cilindro paramétrico vaciado con estos controles deslizantes. Pruebe a modificar algunos de estos parámetros y ver cómo la geometría se actualiza de forma dinámica en la ventana gráfica de Dynamo.

![](<../images/5-3/1/data - exercise step 8a.gif>)

Nodos **Number Slider**: al avanzar en este ejemplo, hemos añadido muchos controles deslizantes al lienzo y debemos limpiar la interfaz de la herramienta que acabamos de crear. Haga clic con el botón derecho en un control deslizante, seleccione "Cambiar nombre..." y cambie cada control deslizante al nombre apropiado para su parámetro (Grosor, Radio, Altura, etc.).

![](<../images/5-3/1/data - exercise step 8b step.jpg>)

### Parte II: llenar la matriz de cilindros de la parte I

9\. En este punto, hemos creado un fantástico elemento de cilindro engrosado. Se trata de un objeto actualmente; veamos cómo crear una matriz de cilindros que permanezca enlazada de forma dinámica. Para ello, vamos a crear una lista de cilindros, en lugar de trabajar con un único elemento.

**Adición (+):** nuestro objetivo es añadir una fila de cilindros junto al cilindro que hemos creado. Si se desea añadir un cilindro adyacente al actual, se deben tener en cuenta el radio del cilindro y el grosor del vaciado. Para obtener este número, se añaden los dos valores de los controles deslizantes.

![](<../images/5-3/1/data - exercise step 9.jpg>)

10\. Este paso es más complejo, por lo que pasaremos por él más lentamente. El objetivo es crear una lista de números que defina las ubicaciones de cada cilindro en una fila.

![](<../images/5-3/1/data - exercise step 10.jpg>)

> a. **Multiplicación**: en primer lugar, deseamos multiplicar el valor del paso anterior por 2. El valor del paso anterior representa un radio y queremos mover el cilindro por el diámetro completo.
>
> b. **Secuencia numérica**: creamos una matriz de números con este nodo. La primera entrada es el nodo de _multiplicación_ del paso anterior en el valor _step_. El valor _start_ se puede establecer en _0,0_ mediante un nodo de _Number_.
>
> c. **Control deslizante de enteros**: para el valor _amount_, conectamos un control deslizante de enteros. Esto definirá cuántos cilindros se crean.
>
> d. **Salida**: esta lista presenta la distancia desplazada para cada cilindro de la matriz y la manejan de forma paramétrica los controles deslizantes originales.

11\. Este paso es bastante sencillo: conecte la secuencia definida en el paso anterior a la entrada _x_ de **Point.ByCoordinates** original. De este modo, se sustituye el control deslizante _pointX_, que se puede eliminar. Ahora vemos una matriz de cilindros en la ventana gráfica (asegúrese de que el control deslizante de enteros es mayor que 0).

![](<../images/5-3/1/data - exercise step 11.gif>)

12\. La cadena de cilindros sigue enlazada dinámicamente a todos los controles deslizantes. Mueva cada uno de los controles deslizantes para ver cómo se actualiza la definición.

![](<../images/5-3/1/data - exercise step 12.gif>)
