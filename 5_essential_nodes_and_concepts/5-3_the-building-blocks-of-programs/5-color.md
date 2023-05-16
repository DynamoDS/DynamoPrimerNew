# Color

El color es un excelente tipo de datos para crear imágenes convincentes, así como para renderizar la diferencia en la salida del programa visual. Cuando se trabaja con datos abstractos y números variables, a veces es difícil ver qué se está cambiando y en qué grado. Esta es una aplicación excelente para los colores.

### Creación de colores

Los colores de Dynamo se crean con entradas de ARGB. Estas corresponden a los canales alfa, rojo, verde y azul. El canal alfa representa la _transparencia_ del color, mientras que los otros tres se utilizan como colores principales para generar todo el espectro de color sincronizado.

| Icono                                     | Nombre (sintaxis)                 | Entradas  | Salidas |
| ---------------------------------------- | ----------------------------- | ------- | ------- |
| \![](<../images/5-1/ColorbyARGB (1).jpg>) | Color ARGB (**Color.ByARGB**) | A,R,G,B | color   |

### Consulta de los valores de color

Los colores de la tabla siguiente consultan las propiedades utilizadas para definir el color: alfa, rojo, verde y azul. Observe que el nodo Color.Components nos proporciona las cuatro salidas diferentes, lo que hace que este nodo sea preferible para consultar las propiedades de un color.

| Icono                                          | Nombre (sintaxis)                     | Entradas | Salidas    |
| --------------------------------------------- | --------------------------------- | ------ | ---------- |
| \![](<../images/5-1/ColorAlpha(1)(1) (1).jpg>) | Alfa (**Color.Alpha**)           | color  | A          |
| \![](<../images/5-1/ColorRed (1).jpg>)         | Rojo (**Color.Red**)               | color  | R          |
| \![](<../images/5-1/ColorGreen(1)(1) (1).jpg>) | Verde (**Color.Green**)           | color  | G          |
| \![](<../images/5-1/ColorBlue (1).jpg>)        | Azul (**Color.Blue**)             | color  | B          |
| \![](<../images/5-1/ColorComponent (1).jpg>)   | Componentes (**Color.Components**) | color  | A, R, G, B |

Los colores de la tabla siguiente corresponden al **espacio de color HSB**. La división del color en el matiz, la saturación y el brillo es, probablemente, más intuitiva en la forma en que se interpreta el color: ¿Qué color debería ser? ¿Debería ser más o menos colorido? ¿Y debería ser más o menos claro u oscuro? Este es el desglose del matiz, la saturación y el brillo respectivamente.

| Icono                                         | Nombre (sintaxis)                     | Entradas | Salidas    |
| -------------------------------------------- | --------------------------------- | ------ | ---------- |
| \![](<../images/5-1/ColorHue (1).jpg>)        | Matiz (**Color.Hue**)               | color  | Matiz        |
| \![](<../images/5-1/ColorSaturation (1).jpg>) | Saturación (**Color.Saturation**) | color  | Saturación |
| \![](<../images/5-1/ColorBrightness (1).jpg>) | Brillo (**Color.Brightness**) | color  | Brillo |

### Rango de colores

El rango de color es similar al nodo **Remap Range** del ejercicio [\#part-ii-from-logic-to-geometry](3-logic.md#part-ii-from-logic-to-geometry "mention"); asigna de nuevo una lista de números a otro dominio. Sin embargo, en lugar de asignarla a un dominio de _número_, la asigna a un _degradado de color_ basado en números de entrada que van del 0 al 1.

El nodo actual funciona bien, pero puede resultar un poco incómodo para que todo funcione la primera vez. La mejor forma de familiarizarse con el degradado de color es probarlo de forma interactiva. Hagamos un ejercicio rápido para revisar cómo configurar un degradado con colores de salida que corresponden a números.

![](../images/5-3/5/color-colorrange.jpg)

> 1. Definir tres colores: con un nodo **Code Block**, defina _rojo, verde_ y _azul_ mediante la conexión de las combinaciones adecuadas de _0_ y _255_.
> 2. **Crear lista**:fusione los tres colores en una lista.
> 3. Definir índices: cree una lista para definir las posiciones de pinzamiento de cada color (de 0 a 1). Observe el valor de 0,75 para el verde. De este modo, se coloca el color verde a tres cuartos de camino en el degradado horizontal del control deslizante de rango de colores.
> 4. **Code Block**: valores de entrada (entre 0 y 1) para convertir en colores.

### Vista previa de color

El nodo **Display.ByGeometry** nos permite colorear la geometría en la ventana gráfica de Dynamo. Esto resulta útil para separar diferentes tipos de geometría, demostrar un concepto paramétrico o definir una leyenda de análisis para la simulación. Las entradas son sencillas, geometría y color. Para crear un degradado como el de la imagen anterior, la entrada de color se conecta al nodo **Color** **Range**.

![](../images/5-3/5/color-colorpreview.jpg)

### Color en superficies

El nodo **Display.BySurfaceColors** nos permite asignar datos en una superficie mediante los colores. Esta función ofrece posibilidades interesantes para visualizar los datos obtenidos a través de análisis independientes como análisis solares, de energía o de proximidad. La aplicación de color a una superficie en Dynamo es similar a la aplicación de una textura a un material en otros entornos de CAD. Vamos a demostrar cómo utilizar esta herramienta en el siguiente ejercicio breve.

![](../images/5-3/5/12\(1\).jpg)

## Ejercicio

### Hélice básica con colores

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/5-3/5/Building Blocks of Programs - Color.dyn" %}

Este ejercicio se centra en el control paramétrico del color en paralelo a la geometría. La geometría es una hélice básica que se define a continuación mediante el **bloque de código**. Esta es una forma rápida y sencilla de crear una función paramétrica y, como nuestro enfoque está en el color (en lugar de en la geometría), utilizamos el bloque de código para crear la hélice de forma eficaz sin sobrecargar el lienzo. Utilizaremos el bloque de código con más frecuencia a medida que el manual de introducción se adentre en materiales más avanzados.

![](../images/5-3/5/color-basichelixwithcolors01.jpg)

> 1. **Code Block**: defina los dos bloques de código con las fórmulas mostradas anteriormente. Este es un método paramétrico rápido para crear una espiral.
> 2. **Point.ByCoordinates**: conecte las tres salidas del bloque de código a las coordenadas del nodo.

Ahora se muestra una matriz de puntos que crean una hélice. El siguiente paso consiste en crear una curva que atraviese los puntos para que podamos visualizar la hélice.

![](../images/5-3/5/color-basichelixwithcolors02.jpg)

> 1. **PolyCurve.ByPoints**: conecte la salida **Point.ByCoordinates** en la entrada _points_ del nodo. Se obtiene una curva helicoidal.
> 2. **Curve.PointAtParameter**: conecte la salida de **PolyCurve.ByPoints** a la entrada _curve_. La finalidad de este paso es crear un punto atractor paramétrico que se deslice a lo largo de la curva. Dado que la curva está evaluando un punto en el parámetro, necesitaremos introducir un valor en _param_ entre 0 y 1.
> 3. **Number Slider**: después de añadirlo al lienzo, cambie el valor _mínimo_ a _0,0_, el valor _máximo_ a _1,0_ y el valor de _paso_ a _0,01_. Conecte la salida del control deslizante a la entrada _param_ de **Curve.PointAtParameter**. Ahora se muestra un punto a lo largo de la longitud de la hélice representado por un porcentaje del control deslizante (0 en el punto inicial, 1 en el punto final).

Con el punto de referencia creado, ahora comparamos la distancia desde el punto de referencia a los puntos originales que definen la hélice. Este valor de distancia controlará la geometría y el color.

![](../images/5-3/5/color-basichelixwithcolors03.jpg)

> 1. **Geometry.DistanceTo**: conecte la salida de **Curve.PointAtParameter** a la _entrada_. Conecte **Point.ByCoordinates** en la entrada geometry.
> 2. **Watch**: la salida resultante muestra una lista de las distancias desde cada punto helicoidal hasta el punto de referencia a lo largo de la curva.

El siguiente paso consiste en controlar los parámetros con la lista de distancias entre los puntos helicoidales y el punto de referencia. Utilizamos estos valores de distancia para definir los radios de una serie de esferas a lo largo de la curva. Para mantener las esferas en un tamaño adecuado, es necesario _reasignar_ los valores de la distancia.

![](../images/5-3/5/color-basichelixwithcolors04.jpg)

> 1. **Math.RemapRange**: conecte la salida de **Geometry.DistanceTo** a la entrada "numbers".
> 2. **Code Block**: conecte un bloque de código con un valor de _0,01_ a la entrada _newMin_ y un bloque de código con un valor de _1_ a la entrada _newMax_.
> 3. **Watch**: conecte la salida de **Math.RemapRange** a un nodo y la salida de **Geometry.DistanceTo** a otro. Compare los resultados.

Este paso ha reasignado la lista de distancias para que tenga un rango más pequeño. Podemos editar los valores de _newMin_ y _newMax_ según consideremos adecuado. Los valores se reasignarán y tendrán la misma _relación de distribución_ en todo el dominio.

![](../images/5-3/5/color-basichelixwithcolors05.jpg)

> 1. **Sphere.ByCenterPointRadius**: conecte la salida de **Math.RemapRange** a la entrada _radius_ y la salida de **Point.ByCoordinates** original a la entrada _centerPoint_.

Cambie el valor del control deslizante de número y observe cómo se actualiza el tamaño de las esferas. Ahora tenemos una guía paramétrica.

![](../images/5-3/5/color-basichelixwithcolors06.gif)

El tamaño de las esferas muestra la matriz paramétrica definida por un punto de referencia a lo largo de la curva. Usaremos el mismo concepto para que el radio de la esfera controle su color.

![](../images/5-3/5/color-basichelixwithcolors07.jpg)

> 1. **Color Range**: añádalo al lienzo. Al pasar el cursor sobre la entrada _value_, observamos que los números solicitados se encuentran entre 0 y 1. Es necesario volver a asignar los números de la salida de **Geometry.DistanceTo** para que sean compatibles con este dominio.
> 2. **Sphere.ByCenterPointRadius**: por el momento, desactivemos la vista preliminar en este nodo (_Haga clic con el botón derecho > Vista preliminar_).

![](../images/5-3/5/color-basichelixwithcolors08.jpg)

> 1. **Math.RemapRange**: este proceso debería resultarle familiar. Conecte la salida de **Geometry.DistanceTo** a la entrada "numbers".
> 2. **Code Block**: como hicimos en un paso anterior, cree el valor _0_ para la entrada _newMin_ y el valor _1_ para la entrada _newMax_. Observe que, en este caso, podemos definir dos salidas de un bloque de código.
> 3. **Color Range**: conecte la salida de **Math.RemapRange** a la entrada _value_.

![](../images/5-3/5/color-basichelixwithcolors09.jpg)

> 1. **Color.ByARGB**: esto es lo que haremos para crear dos colores. Aunque este proceso puede parecer incómodo, es el mismo que el utilizado con los colores RGB en otro software, solo que usamos programación visual para ello.
> 2. **Code Block**: cree dos valores de _0_ y _255_. Conecte las dos salidas a las dos entradas de **Color.ByARGB** de acuerdo con la imagen anterior (o cree sus dos colores favoritos).
> 3. **Color Range**: la entrada _colors_ requiere una lista de colores. Se debe crear esta lista a partir de los dos colores creados en el paso anterior.
> 4. **List.Create**: combine los dos colores en una lista. Conecte la salida a la entrada _colors_ de **Color Range**.

![](../images/5-3/5/color-basichelixwithcolors10.jpg)

> 1. **Display.ByGeometryColor**: conecte **Sphere.ByCenterPointRadius** a la entrada _geometry_ y _Color Range_ a la entrada _color_. Ahora tenemos un degradado suave en el dominio de la curva.

Si se cambia el valor del **control deslizante de número** mostrado anteriormente, se actualizan los colores y los tamaños. Los colores y el tamaño del radio están directamente relacionados en este caso; ahora tenemos un vínculo visual entre dos parámetros.

![](../images/5-3/5/color-basichelixwithcolors11.gif)

### Ejercicio de color en superficies

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/5-3/5/BuildingBlocks of Programs - ColorOnSurface.zip" %}

Es necesario crear primero una superficie o hacer referencia a ella para utilizarla como entrada en el nodo **Display.BySurfaceColors**. En este ejemplo, se realiza la solevación entre una curva seno y coseno.

![](../images/5-3/5/color-coloronsurface01.jpg)

> 1. Este grupo de nodos crea puntos a lo largo del eje Z y los desplaza según las funciones de seno y coseno. Las dos listas de puntos se utilizan a continuación para generar curvas NURBS.
> 2. **Surface.ByLoft**: genere una superficie interpolada entre las curvas NURBS de la lista.

![](../images/5-3/5/color-coloronsurface02.jpg)

> 1. **File Path**: seleccione un archivo de imagen para muestrear datos de píxeles.
> 2. Utilice **File.FromPath** para convertir la ruta de archivo en un archivo y, a continuación, páselo a **Image.ReadFromFile** para obtener una imagen para el muestreo.
> 3. **Image.Pixels**: introduzca una imagen y proporcione un valor de muestra para utilizarlo en las cotas X e Y de la imagen.
> 4. **Control deslizante**: proporcione valores de muestra para **Image.Pixels**.
> 5. **Display.BySurfaceColors**: asigne los valores de matriz de color de la superficie a lo largo de X e Y respectivamente.

Vista preliminar ampliada de la superficie de salida con resolución de 400 x 300 muestras

![](../images/5-3/5/color-coloronsurface03.jpg)
