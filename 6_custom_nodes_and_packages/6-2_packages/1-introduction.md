# Introducción a los paquetes

En pocas palabras, un paquete es un conjunto de nodos personalizados. El administrador de paquetes de Dynamo es un portal para que la comunidad descargue cualquier paquete que se haya publicado en línea. Estos conjuntos de herramientas los desarrollan terceros para ampliar la funcionalidad principal de Dynamo. Están disponibles para todos los usuarios y listos para descargar con solo hacer clic.

![Sitio de Package Manager](../images/6-2/1/dpm.jpg)

Un proyecto de código abierto como Dynamo crece con este tipo de participación de la comunidad. Con desarrolladores independientes dedicados, Dynamo puede ampliar su alcance a los flujos de trabajo de una amplia gama de sectores. Por este motivo, el equipo de Dynamo ha realizado esfuerzos coordinados para optimizar el desarrollo y la publicación de paquetes (lo cual se trata en mayor detalle en las siguientes secciones).

### Instalación de un paquete

La forma más sencilla de instalar un paquete es mediante la barra de herramientas Paquetes de la interfaz de Dynamo. Vamos a ir directos a la acción e instalar uno ahora. En este ejemplo rápido, instalaremos un paquete popular para la creación de paneles de cuadrados en una rejilla.

En Dynamo, vaya a _Paquetes > Buscar un paquete..._

![](<../images/6-2/1/package introduction - installing a package 01.jpg>)

En la barra de búsqueda, buscamos "cuadrados de rejilla rectangular". Tras unos segundos, deberían aparecer todos los paquetes que coincidan con esta consulta de búsqueda. Vamos a seleccionar el primer paquete cuyo nombre coincida.

Haga clic en Instalar para añadir este paquete a la biblioteca. Listo.

![](<../images/6-2/1/package introduction - installing a package 02.jpg>)

Observe que ahora tenemos otro grupo en la biblioteca de Dynamo denominado "buildz". Este nombre hace referencia al desarrollador del paquete y el nodo personalizado se encuentra en este grupo. Podemos empezar a utilizarlo al instante.

![](<../images/6-2/1/package introduction - installing a package 03.jpg>)

Utilice **Code Block** para definir rápidamente una rejilla rectangular, generar el resultado en un nodo **Polygon.ByPoints** y, posteriormente, en un nodo **Surface.ByPatch** a fin de ver la lista de paneles rectangulares que acaba de crear.

![](<../images/6-2/1/package introduction - installing a package 04.jpg>)

### Instalación de la carpeta de paquetes: DynamoUnfold

El ejemplo anterior se centra en un paquete con un nodo personalizado, pero se utiliza el mismo proceso para descargar paquetes con varios nodos personalizados y archivos de datos complementarios. Vamos a demostrar esto a continuación con un paquete más completo: Dynamo Unfold.

Como en el ejemplo anterior, seleccione _Paquetes > Buscar un paquete..._

Esta vez, buscaremos _"DynamoUnfold"_, en una sola palabra y respetando las mayúsculas y minúsculas. Cuando aparezcan los paquetes, descárguelos. Para ello, haga clic en Instalar a fin de añadir Dynamo Unfold a la biblioteca de Dynamo.

![](<../images/6-2/1/package introduction - installing package folder 01.jpg>)

En la biblioteca de Dynamo, tenemos un grupo de _DynamoUnfold_ con varias categorías y nodos personalizados.

![](<../images/6-2/1/package introduction - installing package folder 02.jpg>)

Ahora, veamos la estructura de archivos del paquete. En primer lugar, seleccione Dynamo > Preferencias.

![](<../images/6-2/1/package introduction - installing package folder 03.jpg>)

En la ventana emergente Preferencias, abra Package Manager > junto a DynamoUnfold, seleccione el menú de puntos verticales ![](<../images/6-2/1/package introduction - vertical dots menu.jpg>) > Mostrar directorio raíz para abrir la carpeta raíz de este paquete.

![](<../images/6-2/1/package introduction - installing package folder 04.jpg>)

Esto nos llevará al directorio raíz del paquete. Observe que hay tres carpetas y un archivo.

![](<../images/6-2/1/package introduction - installing package folder 05.jpg>)

> 1. La carpeta _bin_ contiene archivos .dll. Este paquete de Dynamo se desarrolló mediante el uso de Zero-Touch, por lo que los nodos personalizados se guardan en esta carpeta.
> 2. La carpeta _dyf_ contiene los nodos personalizados. Este paquete no se desarrolló mediante nodos personalizados de Dynamo, por lo que esta carpeta está vacía para este paquete.
> 3. La carpeta extra contiene todos los archivos adicionales, incluidos los archivos de ejemplo.
> 4. El archivo pkg es un archivo de texto básico que define los parámetros del paquete. Podemos pasarlo por alto por ahora.

Al abrir la carpeta "extra", vemos un conjunto de archivos de ejemplo que se han descargado con la instalación. No todos los paquetes tienen archivos de ejemplo, pero aquí es donde se pueden encontrar si forman parte de un paquete.

Abriremos "SphereUnfold".

![](../images/6-2/1/rd2.jpg)

Después de abrir el archivo y pulsar "Ejecutar" en el solucionador, tenemos una esfera desplegada. Estos archivos de ejemplo son útiles para aprender a trabajar con un nuevo paquete de Dynamo.

![](<../images/6-2/1/package introduction - installing package folder 07.jpg>)

### Administrador de paquetes de Dynamo

Otra forma de descubrir los paquetes de Dynamo es explorar [Dynamo Package Manager](http://dynamopackages.com) en línea. Es una buena forma de buscar paquetes, ya que el repositorio ordena los paquetes por número de descargas y popularidad. Asimismo, es una forma sencilla de recopilar información sobre las actualizaciones recientes para los paquetes, ya que algunos paquetes de Dynamo están sujetos a las versiones y dependencias de Dynamo.

Al hacer clic en _"Quads from Rectangular Grid"_ en el administrador de paquetes de Dynamo, aparecen las descripciones, las versiones, el desarrollador y las posibles dependencias.

![](../images/6-2/1/dpm2.jpg)

También puede descargar los archivos de paquete desde el administrador de paquetes de Dynamo, pero este proceso es más directo si se realiza desde Dynamo.

### ¿Dónde se almacenan los archivos de paquetes localmente?

Si descarga archivos desde Dynamo Package Manager o si desea ver dónde se conservan todos los archivos de paquetes, haga clic en Dynamo > Package Manager > 
Rutas de nodos y paquetes; en esta ubicación, puede encontrar el directorio de carpeta raíz actual.

![](<../images/6-2/1/package introduction - installing package folder 08.jpg>)

Por defecto, los paquetes se instalan en una ubicación similar a esta ruta de carpeta: _C:/Usuarios/[nombre de usuario]/AppData/Roaming/Dynamo/[Versión de Dynamo]_.

### Más detalles sobre los paquetes

La comunidad de Dynamo está en constante crecimiento y evolución. Si explora el administrador de paquetes de Dynamo de vez en cuando, descubrirá algunos avances excelentes. En las secciones siguientes, analizaremos en profundidad los paquetes, desde la perspectiva del usuario final hasta la autoría de un paquete de Dynamo propio.
