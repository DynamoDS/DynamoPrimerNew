# Creación de un nodo personalizado

Dynamo ofrece varios métodos diferentes para crear nodos personalizados. Puede crear nodos personalizados desde cero, a partir de un gráfico existente o de forma explícita en C#. En esta sección, vamos a cubrir la generación de un nodo personalizado en la interfaz de usuario de Dynamo a partir de un gráfico existente. Este método es ideal para limpiar el espacio de trabajo, así como para empaquetar una secuencia de nodos con el fin de reutilizarlos en otros entornos.

## Ejercicio: nodos personalizados para la asignación de UV

### Parte I: comenzar con un gráfico

En la imagen siguiente, se asigna un punto de una superficie a otra mediante coordenadas UV. Utilizaremos este concepto para crear una superficie panelizada que haga referencia a curvas en el plano XY. Crearemos paneles de cuadrados para esta panelización pero, con esta misma lógica, podemos crear una amplia variedad de paneles con asignación de UV. Esta es una gran oportunidad para el desarrollo de nodos personalizados, ya que vamos a poder repetir un proceso similar más fácilmente en este gráfico o en otros flujos de trabajo de Dynamo.

![](<../images/6-1/2/custom node for uv mapping pt I - 01.jpg>)

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/6-1/2/UV-CustomNode.zip" %}

Comenzaremos con la creación de un gráfico que deseamos anidar en un nodo personalizado. En este ejemplo, crearemos un gráfico que asigna polígonos de una superficie base a una superficie de destino mediante coordenadas UV. Este proceso de asignación de UV es algo que se utiliza con frecuencia, lo que lo convierte en un buen candidato para un nodo personalizado. Para obtener más información sobre superficies y el espacio UV, consulte la página [Superficie](../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/5-surfaces.md). El gráfico completo es _UVmapping\_Custom-Node.dyn_ del archivo .zip descargado anteriormente.

![](<../images/6-1/2/custom node for uv mapping pt I - 02.jpg>)

> 1. **Code Block:** utilice esta línea para crear un intervalo de 10 números entre -45 y 45 `45..45..#10;`.
> 2. **Point.ByCoordinates:** conecte la salida del **bloque de código** a las entradas "x" e "y", y establezca el encaje como producto vectorial. Ahora debería ver una rejilla de puntos.
> 3. **Plane.ByOriginNormal:** conecte la salida _"Point"_ a la entrada _"origin"_ para crear un plano en cada uno de los puntos. Se utilizará el vector normal por defecto (0,0,1).
> 4. **Rectangle.ByWidthLength:** conecte los planos del paso anterior a la entrada _"plane"_ y utilice un **bloque de código** con un valor de _10_ para especificar la anchura y la longitud.

Ahora debería ver una rejilla de rectángulos. Asignemos ahora estos rectángulos a una superficie de destino mediante coordenadas UV.

![](<../images/6-1/2/custom node for uv mapping pt I - 03.jpg>)

> 1. **Polygon.Points:** conecte la salida **Rectangle.ByWidthLength** del paso anterior a la entrada _"polygon"_ para extraer los puntos de esquina de cada rectángulo. Estos son los puntos que asignaremos a la superficie de destino.
> 2. **Rectangle.ByWidthLength:** utilice un **bloque de código** con un valor de _100_ para especificar la anchura y la longitud del rectángulo. Este será el contorno de la superficie base.
> 3. **Surface.ByPatch:** conecte la salida **Rectangle.ByWidthLength** del paso anterior a la entrada _"closedCurve"_ para crear una superficie base.
> 4. **Surface.UVParameterAtPoint:** conecte la salida _"Point"_ del nodo **Polygon.Points** y la salida _"Surface"_ del nodo **Surface.ByPatch** para devolver el parámetro UV de cada punto.

Ahora que tenemos una superficie base y un conjunto de coordenadas UV, podemos importar una superficie de destino y asignar los puntos entre las superficies.

![](<../images/6-1/2/custom node for uv mapping pt I - 04.jpg>)

> 1. **File Path:** seleccione la ruta de archivo de la superficie que desea importar. El tipo de archivo debe ser .SAT. Haga clic en el botón _"Examinar..."_ y desplácese hasta el archivo _UVmapping\_srf.sat_ del archivo .zip que hemos descargado anteriormente.
> 2. **Geometry.ImportFromSAT:** conecte la ruta de archivo para importar la superficie. La superficie importada se muestra en la vista preliminar de la geometría.
> 3. **UV:** conecte la salida del parámetro UV a un nodo _UV.U_ y un nodo _UV.V_.
> 4. **Surface.PointAtParameter:** conecte la superficie importada, así como las coordenadas u y v. Ahora debería ver una rejilla de puntos 3D en la superficie de destino.

El último paso consiste en utilizar los puntos 3D para crear parches de superficie rectangulares.

![](<../images/6-1/2/custom node for uv mapping pt I - 05.jpg>)

> 1. **PolyCurve.ByPoints:** conecte los puntos de la superficie para construir una PolyCurve a través de los puntos.
> 2. **Boolean:** añada un nodo **Boolean** al espacio de trabajo y conéctelo a la entrada _"connectLastToFirst"_ y, a continuación, cambie el valor a "True" (verdadero) para cerrar las PolyCurves. Ahora debería ver los rectángulos asignados a la superficie.
> 3. **Surface.ByPatch:** conecte las PolyCurves a la entrada _"closedCurve"_ para construir parches de superficie.

### Parte II: del gráfico al nodo personalizado

Ahora vamos a seleccionar los nodos que queremos anidar en un nodo personalizado teniendo en cuenta las entradas y salidas que este debe tener. Queremos que nuestro nodo personalizado sea lo más flexible posible, de modo que debería poder asignar cualquier polígono, no solo rectángulos.

Seleccione los siguientes nodos (comenzando por Polygon.Points), haga clic con el botón derecho en el espacio de trabajo y seleccione "Crear nodo personalizado".

![](<../images/6-1/2/custom node for uv mapping pt II - 01.jpg>)

En el cuadro de diálogo Propiedades de nodo personalizado, asigne un nombre, una descripción y una categoría al nodo personalizado.

![](<../images/6-1/2/custom node for uv mapping pt II - 02.jpg>)

> 1. Nombre: MapPolygonsToSurface.
> 2. Descripción: asigne polígonos de una base a una superficie de destino.
> 3. Categoría de complementos: Geometry.Curve.

El nodo personalizado ha limpiado considerablemente el espacio de trabajo. Observe que las entradas y salidas se han nombrado en función de los nodos originales. Editaremos el nodo personalizado para que los nombres sean más descriptivos.

![](<../images/6-1/2/custom node for uv mapping pt II - 03.jpg>)

Haga doble clic en el nodo personalizado para editarlo. Se abrirá un espacio de trabajo con un fondo amarillo que representa el interior del nodo.

![](<../images/6-1/2/custom node for uv mapping pt II - 04.jpg>)

> 1. **Entradas:** cambie los nombres de entrada a _baseSurface_ y _targetSurface_.
> 2. **Salidas:** añada una salida adicional para los polígonos asignados.

Guarde el nodo personalizado y vuelva al espacio de trabajo de inicio. Observe que el nodo **MapPolygonsToSurface** refleja los cambios que acabamos de realizar.

![](<../images/6-1/2/custom node for uv mapping pt II - 05.jpg>)

También podemos reforzar la solidez del nodo personalizado añadiendo **comentarios personalizados**. Los comentarios pueden servir como ayuda en las entradas y salidas o explicar el funcionamiento del nodo. Los comentarios aparecerán cuando el usuario coloque el cursor sobre una entrada o una salida de un nodo personalizado.

Haga doble clic en el nodo personalizado para editarlo. De este modo, se volverá a abrir el espacio de trabajo con fondo amarillo.

![](<../images/6-1/2/custom node for uv mapping pt II - 06.jpg>)

> 1. Edite primero el **bloque de código** de entrada. Para iniciar un comentario, escriba "//" seguido del texto del comentario. Escriba cualquier información que pueda ayudar a entender mejor el nodo; aquí describiremos _targetSurface_.
> 2. Vamos a definir también el valor por defecto de _inputSurface_ estableciendo el valor a que equivale un tipo de entrada. Aquí, vamos a definir el valor por defecto en el conjunto **Surface.ByPatch** original.

También se pueden aplicar comentarios a las salidas.

![](<../images/6-1/2/custom node for uv mapping pt II - 07.jpg>)

> Edite el texto en el bloque de código Output. Escriba "//" seguido del texto del comentario. Aquí vamos a aclarar las salidas _Polygons_ y _surfacePatches_ añadiendo una descripción más detallada.

![](<../images/6-1/2/custom node for uv mapping pt II - 08.jpg>)

> 1. Coloque el cursor sobre las entradas del nodo personalizado para ver los comentarios.
> 2. Con el valor por defecto establecido en _inputSurface_, también podemos ejecutar la definición sin una entrada de superficie.
