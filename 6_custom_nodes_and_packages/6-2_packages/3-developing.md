# Desarrollo de un paquete

Dynamo ofrece una gran variedad de métodos para crear un paquete para uso personal o para compartir con la comunidad de Dynamo. En el caso real incluido a continuación, veremos cómo se configura un paquete mediante la desconstrucción de uno existente. Este caso real se basa en las lecciones del capítulo anterior y proporciona un conjunto de nodos personalizados para la asignación de geometría, mediante coordenadas UV, de una superficie de Dynamo a otra.

## Paquete MapToSurface

Vamos a trabajar con un paquete de ejemplo que demuestra la asignación de UV de puntos de una superficie a otra. Ya hemos aprendido los conceptos básicos de la herramienta en la sección [Creación de un nodo personalizado](../10\_custom-nodes/10-2\_creating.md) de este manual de introducción. Los archivos siguientes muestran cómo podemos utilizar el concepto de asignación de UV y desarrollar un conjunto de herramientas para una biblioteca publicable.

En esta imagen, se asigna un punto de una superficie a otra mediante coordenadas UV. El paquete se basa en este concepto, pero con una geometría más compleja.

![](../images/6-2/3/uvMap.jpg)

### Instalación del paquete

En el capítulo anterior, exploramos métodos para panelizar una superficie en Dynamo según las curvas definidas en el plano XY. Este caso real amplía estos conceptos a más dimensiones de la geometría. Vamos a instalar este paquete como se creó para mostrar cómo se ha desarrollado. En la siguiente sección, mostraremos cómo se ha publicado este paquete.

En Dynamo, haga clic en \_Paquetes > Buscar un paquete y b\_usque el paquete "MapToSurface". Haga clic en Instalar para iniciar la descarga y añada el paquete a la biblioteca.

![](<../images/6-2/3/develop package - install package 01.jpg>)

Después de la instalación, los nodos personalizados deben estar disponibles en la sección Complementos > Dynamo Primer.

![](<../images/6-2/3/develop package - install package 02 (1) (1).jpg>)

Con el paquete instalado, veamos cómo se configura.

### Nodos personalizados

El paquete que estamos creando utiliza cinco nodos personalizados que hemos creado como referencia. Veamos qué hace cada nodo a continuación. Algunos nodos personalizados se basan en otros nodos personalizados y los gráficos tienen un diseño que los demás usuarios pueden comprender con total claridad.

Este es un paquete sencillo con cinco nodos personalizados. En los pasos siguientes, hablaremos brevemente de la configuración de cada nodo personalizado.

![](<../images/6-2/3/develop package - custom nodes 01 (1) (1).jpg>)

#### **PointsToSurface**

Se trata de un nodo personalizado básico en el que se basan los demás nodos de asignación. En pocas palabras, el nodo asigna un punto desde una coordenada UV de la superficie de origen a la ubicación de la coordenada UV de la superficie de destino. Como los puntos son la geometría más primitiva, a partir de la cual se genera geometría más compleja, podemos utilizar esta lógica para asignar geometría 2D e incluso geometría 3D de una superficie a otra.

![](<../images/6-2/3/develop package -pointToSurface.jpg>)

#### **PolygonsToSurface**

La lógica de ampliar puntos asignados de geometría 1D a geometría 2D se muestra aquí de forma sencilla con polígonos. Observe que se ha anidado el nodo _"PointsToSurface"_ en este nodo personalizado. De esta forma, podemos asignar los puntos de cada polígono a la superficie y, a continuación, volver a generar el polígono a partir de los puntos asignados. Si se mantiene la estructura de datos correcta (una lista de listas de puntos), se pueden mantener los polígonos separados después de reducirlos a un conjunto de puntos.

![](<../images/6-2/3/develop package -polygonsToSurface.jpg>)

#### **NurbsCrvtoSurface**

Aquí se aplica la misma lógica que en el nodo _"PolygonsToSurface"_. En lugar de asignar puntos poligonales, asignamos puntos de control de una curva NURBS.

![](<../images/6-2/3/develop package -nurbsCrvtoSurface.jpg>)

**OffsetPointsToSurface**

Este nodo es un poco más complejo, pero el concepto es simple. Como el nodo _"PointsToSurface"_, este nodo asigna puntos de una superficie a otra. Sin embargo, también considera los puntos que no se encuentran en la superficie de origen inicial, obtiene su distancia al parámetro UV más cercano y asigna esta distancia a la normal de la superficie de destino en la coordenada UV correspondiente. Esto se ve con mayor claridad al examinar los archivos de ejemplo.

![](<../images/6-2/3/develop package -OffsetPointsToSurface.jpg>)

#### **SampleSrf**

Se trata de un nodo simple que crea una superficie paramétrica para asignar de la rejilla de origen a una superficie ondulada en los archivos de ejemplo.

![](<../images/6-2/3/develop package -sampleSrf.jpg>)

### Archivos de ejemplo

Los archivos de ejemplo se pueden encontrar en la carpeta raíz del paquete. Haga clic en Dynamo > Preferencias > Package Manager.

Junto a MapToSurface, haga clic en el menú de puntos verticales > Mostrar directorio raíz.

![](<../images/6-2/3/develop package - example files 01.jpg>)

A continuación, abra la carpeta _"extra"_, que aloja todos los archivos del paquete que no son nodos personalizados. Aquí es donde se almacenan los archivos de ejemplo (si existen) de los paquetes de Dynamo. Las capturas de pantalla que se muestran a continuación explican los conceptos demostrados en cada archivo de ejemplo.

#### **01-PanelingWithPolygons**

Este archivo de ejemplo muestra cómo se puede utilizar _"PointsToSurface"_ para panelizar una superficie en función de una rejilla de rectángulos. Esto debería resultarle familiar, ya que hicimos una demostración de un flujo de trabajo similar en el [capítulo anterior](../10\_custom-nodes/10-2\_creating.md).

![](<../images/6-2/3/develop package -sample file 01.jpg>)

#### **02-PanelingWithPolygons-II**

Mediante un flujo de trabajo similar, este archivo de ejercicio muestra una configuración para la asignación de círculos (o polígonos que representan círculos) de una superficie a otra. Utiliza el nodo _"PolygonsToSurface"_.

![](<../images/6-2/3/develop package -sample file 02.jpg>)

#### **03-NurbsCrvsAndSurface**

Este archivo de ejemplo añade cierta complejidad al trabajar con el nodo "NurbsCrvToSurface". La superficie de destino se desfasa una distancia dada y la curva NURBS se asigna a la superficie de destino original y a la superficie de desfase. A partir de ahí, las dos curvas asignadas se solevan para crear una superficie que, a continuación, se engrosa. Este sólido resultante tiene una ondulación que es representativa de las normales de la superficie de destino.

![](<../images/6-2/3/develop package -sample file 03.jpg>)

#### **04-PleatedPolysurface-OffsetPoints**

Este archivo de ejemplo muestra cómo asignar una PolySurface plegada de una superficie de origen a una superficie de destino. Las superficies de origen y de destino son superficies rectangulares que abarcan la rejilla y una superficie de revolución, respectivamente.

![](<../images/6-2/3/develop package -sample file 04a.jpg>)

La PolySurface de origen asignada desde la superficie de origen a la superficie de destino.

![](<../images/6-2/3/develop package -sample file 04b.jpg>)

#### **05-SVG-Import**

Dado que los nodos personalizados pueden asignar diferentes tipos de curvas, este último archivo hace referencia a un archivo SVG exportado de Illustrator y asigna las curvas importadas a una superficie de destino.

![](<../images/6-2/3/develop package -sample file 05a.jpg>)

Mediante el análisis de la sintaxis de un archivo .svg, las curvas se trasladan del formato .xml a PolyCurves de Dynamo.

![](<../images/6-2/3/develop package -sample file 05b.jpg>)

Las curvas importadas se asignan a una superficie de destino. Esto nos permite diseñar de forma explícita (señalar y hacer clic) una panelización en Illustrator, importarla en Dynamo y aplicarla a una superficie de destino.

![](<../images/6-2/3/develop package -sample file 05c.jpg>)
