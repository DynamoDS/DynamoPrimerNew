# Caso real de paquete: Kit de herramientas de malla

El Kit de herramientas de malla de Dynamo proporciona herramientas para importar mallas desde formatos de archivo externos, crear una malla a partir de objetos de geometría de Dynamo y generar manualmente mallas mediante sus vértices e índices. La biblioteca también proporciona herramientas para modificar y reparar mallas, o extraer cortes horizontales para su uso en la fabricación.

\![](<../images/6-2/2/meshToolkitcasestudy01 (2).jpg>)

El Kit de herramientas de malla de Dynamo forma parte de la investigación de mallas en curso de Autodesk y, como tal, seguirá creciendo a lo largo de los próximos años. Esperamos que aparezcan con frecuencia nuevos métodos en este kit y no dude en ponerse en contacto con el equipo de Dynamo para ofrecer comentarios, indicar errores y enviar sugerencias sobre las nuevas funciones.

### Mallas frente a sólidos

En el siguiente ejercicio, se muestran algunas operaciones básicas de malla que utiliza el Kit de herramientas de malla. En el ejercicio, intersecamos una malla con una serie de planos, lo que puede ser muy costoso desde una perspectiva computacional si se utilizan sólidos. A diferencia de los sólidos, las mallas tienen una "resolución" establecida y no se definen matemáticamente, sino topológicamente, y podemos definir esta resolución en función de la tarea que se está llevando a cabo. Para obtener más información sobre las relaciones entre mallas y sólidos, puede consultar el capítulo [Geometría para el diseño computacional](../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/) de este manual de introducción. Para examinar de forma más exhaustiva el Kit de herramientas de malla, consulte la [página wiki de Dynamo](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Mesh-Toolkit). Pasemos al paquete en el ejercicio siguiente.

### Instalar el Kit de herramientas de malla

En Dynamo, vaya a Paquetes > Package Manager en la barra de menús superior. En el campo de búsqueda, escriba "MeshToolkit", todo en una sola palabra. Haga clic en Instalar y acepte las confirmaciones para iniciar la descarga. Así de fácil.

<figure><img src="../../.gitbook/assets/install-mesh-toolkit.png" alt=""><figcaption></figcaption></figure>

## Ejercicio: intersección de malla

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/6-2/2/MeshToolkit.zip" %}

En este ejemplo, examinaremos el nodo Intersect del Kit de herramientas de malla. Importaremos una malla y la intersecaremos con una serie de planos de entrada para crear cortes. Este es el punto inicial de la preparación del modelo para la fabricación en una herramienta de corte láser o por chorro de agua, o un dispositivo de fresado CNC.

Abra primero _Mesh-Toolkit_Intersect-Mesh.dyn in Dynamo._

![](../images/6-2/2/meshToolkitcasestudy-exercise01.jpg)

> 1. **File Path**: busque el archivo de malla que desea importar (_stanford_bunny_tri.obj_). Los tipos de archivo admitidos son .mix y .obj.
> 2. **Mesh.ImportFile**: conecte la ruta de archivo para importar la malla.

![](../images/6-2/2/meshToolkitcasestudy-exercise02.jpg)

> 1. **Point.ByCoordinates**: cree un punto; este será el centro de un arco.
> 2. **Arc.ByCenterPointRadiusAngle**: cree un arco alrededor del punto. Esta curva se utilizará para colocar una serie de planos. __ La configuración es la siguiente: __ `radius: 40, startAngle: -90, endAngle:0`.

Cree una serie de planos orientados a lo largo del arco.

![](../images/6-2/2/meshToolkitcasestudy-exercise03.jpg)

> 1. **Code Block**: cree 25 números entre 0 y 1.
> 2. **Curve.PointAtParameter**: conecte el arco a la entrada _"curve"_ y la salida de bloque de código a la entrada _"param"_ para extraer una serie de puntos a lo largo de la curva.
> 3. **Curve.TangentAtParameter**: conecte las mismas entradas que en el nodo anterior.
> 4. **Plane.ByOriginNormal**: conecte los puntos a la entrada _"origin"_ y los vectores a la entrada _"normal"_ para crear una serie de planos en cada punto.

A continuación, utilizaremos estos planos para intersecar la malla.

![](../images/6-2/2/meshToolkitcasestudy-exercise04.jpg)

> 1. **Mesh.Intersect**: interseque los planos con la malla importada, creando una serie de contornos de PolyCurve. Haga clic con el botón derecho en el nodo y defina el encaje como el más largo.
> 2. **PolyCurve.Curves**: divida las PolyCurves en fragmentos de curva.
> 3. **Curve.EndPoint**: extraiga los puntos finales de cada curva.
> 4. **NurbsCurve.ByPoints**: utilice los puntos para crear una NurbsCurve. Utilice un nodo booleano establecido en _True_ (verdadero) para cerrar las curvas.

Antes de continuar, desactive la vista preliminar de algunos de los nodos, como Mesh.ImportFile, Curve.EndPoint, Plane.ByOriginNormal y Arc.ByCenterPointRadiusAngle para ver mejor el resultado.

![](../images/6-2/2/meshToolkitcasestudy-exercise05.jpg)

> 1. **Surface.ByPatch**: cree parches de superficie para cada contorno con el fin de crear "cortes" de la malla.

Añada un segundo conjunto de cortes para obtener un efecto de gofre/cartón de huevos.

![](../images/6-2/2/meshToolkitcasestudy-exercise06.jpg)

Es posible que se haya dado cuenta de que las operaciones de intersección se calculan de forma más rápida con una malla que con un sólido comparable. Los flujos de trabajo como el que se muestra en este ejercicio son excelentes para trabajar con mallas.
