# Introducción a los paquetes

Dynamo ofrece un gran número de funciones listas para usar y también mantiene una extensa biblioteca de paquetes que puede ampliar considerablemente la capacidad de Dynamo. Un paquete es una colección de nodos personalizados o funciones adicionales. Dynamo Package Manager es un portal para que la comunidad descargue cualquier paquete que se haya publicado en línea. Estos conjuntos de herramientas los desarrollan terceros para ampliar la funcionalidad principal de Dynamo. Están disponibles para todos los usuarios y listos para descargar con solo hacer clic.

![Sitio de Package Manager](../images/6-2/1/dpm.jpg)

Un proyecto de código abierto como Dynamo crece con este tipo de participación de la comunidad. Con desarrolladores independientes dedicados, Dynamo puede ampliar su alcance a los flujos de trabajo de una amplia gama de sectores. Por este motivo, el equipo de Dynamo ha realizado esfuerzos coordinados para optimizar el desarrollo y la publicación de paquetes (lo cual se trata en mayor detalle en las siguientes secciones).

### Instalación de un paquete

La forma más sencilla de instalar un paquete es mediante la opción de menú Paquetes de la interfaz de Dynamo. Pongámonos manos a la obra e instalemos ahora un paquete. En este ejemplo rápido, instalaremos un paquete popular para la creación de paneles de cuadrados en una rejilla.

En Dynamo, vaya a _Paquetes > Package Manager_.

<figure><img src="../../.gitbook/assets/package-manager-menu.png" alt=""><figcaption></figcaption></figure>

En la barra de búsqueda, buscamos "Quads from Rectangular Grid". Tras unos segundos, deberían aparecer todos los paquetes que coincidan con esta consulta de búsqueda. Vamos a seleccionar el primer paquete cuyo nombre coincida.

Haga clic en Instalar para añadir este paquete a la biblioteca y acepte la confirmación. Listo.

<figure><img src="../../.gitbook/assets/quads-from-rectangular-grid.png" alt=""><figcaption></figcaption></figure>

Observe que ahora tenemos otro grupo en la biblioteca de Dynamo denominado "buildz". Este nombre hace referencia al desarrollador del paquete y el nodo personalizado se encuentra en este grupo. Podemos empezar a utilizarlo al instante.

![](../images/6-2/1/packageintroduction-installingapackage03.jpg)

Utilice **Code Block** para definir rápidamente una rejilla rectangular, generar el resultado en un nodo **Polygon.ByPoints** y, posteriormente, en un nodo **Surface.ByPatch** a fin de ver la lista de paneles rectangulares que acaba de crear.

![](../images/6-2/1/packageintroduction-installingapackage04.jpg)

### Instalación de la carpeta de paquetes: DynamoUnfold

El ejemplo anterior se centra en un paquete con un nodo personalizado, pero se utiliza el mismo proceso para descargar paquetes con varios nodos personalizados y archivos de datos complementarios. Vamos a demostrar esto a continuación con un paquete más completo: Dynamo Unfold.

Como en el ejemplo anterior, seleccione primero _Paquetes > Package Manager_.

Esta vez, buscaremos _"DynamoUnfold"_, todo en una sola palabra. Cuando aparezcan los paquetes, descárguelos. Para ello, haga clic en Instalar a fin de añadir Dynamo Unfold a la biblioteca de Dynamo.

<figure><img src="../../.gitbook/assets/unfold.png" alt=""><figcaption></figcaption></figure>

En la biblioteca de Dynamo, tenemos un grupo de _DynamoUnfold_ con varias categorías y nodos personalizados.

![](../images/6-2/1/packageintroduction-installingpackagefolder02.jpg)

Ahora, veamos la estructura de archivos del paquete.

1. Vaya primero a Paquetes > Package Manager > Paquetes instalados.
2. Junto a DynamoUnfold, seleccione el menú de opciones <img src="../images/6-2/1/packageintroduction-verticaldotsmenu.jpg" alt="" data-size="line">.
3. A continuación, haga clic en Mostrar directorio raíz para abrir la carpeta raíz de este paquete.

<figure><img src="../../.gitbook/assets/view-root-directory.png" alt=""><figcaption></figcaption></figure>

Esto nos llevará al directorio raíz del paquete. Observe que hay tres carpetas y un archivo.

![](../images/6-2/1/packageintroduction-installingpackagefolder05.jpg)

> 1. La carpeta _bin_ contiene archivos .dll. Este paquete de Dynamo se desarrolló mediante el uso de Zero-Touch, por lo que los nodos personalizados se guardan en esta carpeta.
> 2. La carpeta _dyf_ contiene los nodos personalizados. Este paquete no se desarrolló mediante nodos personalizados de Dynamo, por lo que esta carpeta está vacía para este paquete.
> 3. La carpeta extra contiene todos los archivos adicionales, incluidos los archivos de ejemplo.
> 4. El archivo pkg es un archivo de texto básico que define los parámetros del paquete. Podemos pasarlo por alto por ahora.

Al abrir la carpeta "extra", vemos un conjunto de archivos de ejemplo que se han descargado con la instalación. No todos los paquetes tienen archivos de ejemplo, pero aquí es donde se pueden encontrar si forman parte de un paquete.

Abriremos "SphereUnfold".

![](../images/6-2/1/rd2.jpg)

Después de abrir el archivo y pulsar "Ejecutar" en el solucionador, tenemos una esfera desplegada. Estos archivos de ejemplo son útiles para aprender a trabajar con un nuevo paquete de Dynamo.

\![](<../images/6-2/1/packageintroduction-installingpackagefolder07 (1) (2).jpg>)

### Examinar y ver información de paquetes

En Package Manager, puede buscar paquetes mediante las opciones de orden y filtrado de la ficha Buscar paquetes. Hay varios filtros disponibles para el programa anfitrión, el estado (nuevo, obsoleto o no obsoleto) y si el paquete tiene o no dependencias.

Al ordenar los paquetes, puede identificar los más valorados o los más descargados, o aquellos con actualizaciones recientes.

También puede acceder a más información sobre cada paquete. Para ello, haga clic en Ver detalles. Se abrirá un panel lateral en Package Manager, donde podrá encontrar información como las versiones y las dependencias, la URL del sitio web o del repositorio, información sobre la licencia, etc.

### Sitio web de Dynamo Package Manager

Otra forma de descubrir los paquetes de Dynamo es explorar el sitio web de [Dynamo Package Manager](http://dynamopackages.com). Aquí puede encontrar dependencias de paquetes e información sobre compatibilidad de anfitrión y versión proporcionada por los autores de los paquetes. También puede descargar los archivos de paquete desde Dynamo Package Manager, pero este proceso es más directo si se realiza desde Dynamo.

![](../images/6-2/1/dpm2.jpg)

### ¿Dónde se almacenan los archivos de paquetes localmente?

Si desea ver dónde se guardan los archivos de los paquetes, en el panel de navegación superior haga clic en Dynamo > Preferencias > Configuración de paquetes > Ubicaciones de archivo de nodos y paquetes, donde podrá encontrar el directorio de la carpeta raíz actual.

![](../images/6-2/1/packageintroduction-installingpackagefolder08.jpg)

Por defecto, los paquetes se instalan en una ubicación similar a esta ruta de carpeta: _C:/Usuarios/[nombre de usuario]/AppData/Roaming/Dynamo/[Versión de Dynamo]_.

### Configuración de una ubicación compartida para paquetes en una oficina

Para los usuarios que preguntan si es posible implantar Dynamo (de cualquier forma) con paquetes enlazados previamente: el enfoque que resuelve este problema y permite llevar el control desde una ubicación central para todos los usuarios con instalaciones de Dynamo consiste en añadir una ruta de paquete personalizada a cada instalación.

**Añadir una carpeta de red para que el administrador de BIM u otros usuarios puedan supervisar el almacenamiento de la carpeta con paquetes aprobados por la oficina**

En la interfaz de usuario de una aplicación individual, vaya a _Dynamo -> Preferencias -> Configuración de paquetes -> Ubicaciones de archivo de nodos y paquetes_. En el cuadro de diálogo, presione el botón "Añadir ruta" y busque la ubicación de red del recurso de paquete compartido.

Como proceso automatizado, implicaría añadir información al archivo de configuración que se instala con Dynamo:\
 `C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]\DynamoSettings.xml`.

Por defecto, la configuración de Dynamo for Revit es:

`<CustomPackageFolders>`

`<string>C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]</string>`

`</CustomPackageFolders>`

La adición de una ubicación personalizada tendría el siguiente aspecto:

`<CustomPackageFolders>`

`<string>C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]</string>`

`<string>N:\OfficeFiles\Dynamo\Packages_Limited</string>`

`</CustomPackageFolders>`

La administración central de esta carpeta también se puede controlar simplemente haciendo que la carpeta sea de solo lectura.

### Carga de paquetes con archivos binarios desde una ubicación de red

#### Escenario

Es posible que una organización desee estandarizar los paquetes instalados por diferentes estaciones de trabajo y usuarios. Una forma de hacerlo podría consistir en instalar estos paquetes desde _Dynamo -> Preferencias -> Configuración de paquetes -> Ubicaciones de archivo de nodos y paquetes_, seleccionar una carpeta de red como ubicación de instalación y hacer que las estaciones de trabajo añadan esa ruta a `Manage Node and Package Paths`.

#### Problema

Si bien el escenario planteado funciona correctamente para paquetes que contengan solo nodos personalizados, es posible que no funcione para los paquetes que contengan archivos binarios, como los nodos Zero Touch. Este problema se debe a las [medidas de seguridad](https://stackoverflow.com/questions/5328274/load-assembly-from-network-location) que .NET Framework aplica a la carga de ensamblajes cuando proceden de una ubicación de red. Desafortunadamente, el uso del elemento de configuración `loadFromRemoteSources`, como se sugiere en el hilo vinculado, no es una solución factible para Dynamo, ya que se distribuye como componente y no como aplicación.

#### Solución alternativa

Una posible solución alternativa es utilizar una unidad de red asignada que apunte a la ubicación de la red y hacer que las estaciones de trabajo hagan referencia a esa ruta. Los pasos para crear una unidad de red asignada se describen [aquí](https://support.microsoft.com/es-es/help/4026635/windows-10-map-a-network-drive).

### Más detalles sobre los paquetes

La comunidad de Dynamo está en constante crecimiento y evolución. Si explora el administrador de paquetes de Dynamo de vez en cuando, descubrirá algunos avances excelentes. En las secciones siguientes, analizaremos en profundidad los paquetes, desde la perspectiva del usuario final hasta la autoría de un paquete de Dynamo propio.
