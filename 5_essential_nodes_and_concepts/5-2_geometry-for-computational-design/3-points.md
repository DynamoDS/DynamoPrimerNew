# Puntos

## Puntos en Dynamo

### ¿Qué es un punto?

Un [punto](5-3\_points.md#point-as-coordinates) se define por uno o más valores denominados coordenadas. El número de valores de coordenadas necesarios para definir el punto depende del sistema de coordenadas o del contexto en el que se encuentra.

### Punto 2D y 3D

El tipo de punto más común en Dynamo se encuentra en el sistema de coordenadas universales tridimensional y tiene tres coordenadas \[X,Y,Z] (punto 3D en Dynamo).

![](<../images/5-2/3/points - 3d point in dynamo.jpg>)

Un punto 2D de Dynamo tiene dos coordenadas \[X,Y].

![](<../images/5-2/3/points - 2d point in dynamo.jpg>)

### Punto en curvas y superficies

Los parámetros de las curvas y las superficies son continuos y se extienden más allá del borde de la geometría especificada. Como las formas que definen el espacio paramétrico se encuentran en un sistema de coordenadas universales tridimensional, siempre se puede convertir una coordenada paramétrica en una coordenada "universal". Por ejemplo, el punto \[0,2, 0,5] de la superficie es el mismo que el punto \[1,8, 2,0, 4,1] de las coordenadas universales.

![](<../images/5-2/3/points - xyz vs coord sys vs uv.jpg>)

> 1. Punto en presuntas coordenadas XYZ globales
> 2. Punto relativo a un sistema de coordenadas especificado (cilíndrico)
> 3. Punto como coordenada UV en una superficie

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/5-2/3/Geometry for Computational Design - Points.dyn" %}

## Información más detallada sobre...

Si la geometría es el idioma de un modelo, los puntos son el alfabeto. Los puntos son la base sobre la que se crea el resto de la geometría; se necesitan al menos dos puntos para crear una curva. tres puntos para crear un polígono o una cara de malla, etc. La definición de la posición, el orden y la relación entre los puntos (pruebe con una función de seno) nos permite definir geometrías de orden superior, como elementos que reconocemos como círculos o curvas.

![Del punto a la curva](../images/5-2/3/PointsAsBuildingBlocks-1.jpg)

> 1. Un círculo que utiliza las funciones `x=r*cos(t)` y `y=r*sin(t)`
> 2. Una curva seno que utiliza las funciones `x=(t)` y `y=r*sin(t)`

### Punto como coordenadas

Los puntos también pueden existir en un sistema de coordenadas bidimensional. La convención tiene diferentes notaciones de letras en función del tipo de espacio con el que se trabaje; es posible que se utilice \[X,Y] en un plano o \[U,V] en una superficie.

![Punto como coordenadas](../images/5-2/3/Coordinates.jpg)

> 1. Un punto en el sistema de coordenadas euclidiano: \[X,Y,Z]
> 2. Un punto en un sistema de coordenadas de parámetro de curva: \[t]
> 3. Un punto en un sistema de coordenadas de parámetro de superficie: \[U,V]
