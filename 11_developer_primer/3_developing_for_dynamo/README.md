# Desarrollo para Dynamo

Independientemente del nivel de experiencia, la plataforma Dynamo se ha diseñado para que todos los usuarios puedan colaborar. Existen varias opciones de desarrollo dirigidas a diferentes capacidades y niveles de habilidad, cada una con sus puntos fuertes y débiles en función del objetivo. A continuación, describiremos las distintas opciones y cómo elegir una sobre otra.

![Tres entornos de desarrollo](images/developing-for-dynamo.png)

> Tres entornos de desarrollo: Visual Studio, el editor de Python y DesignScript para bloques de código

### ¿Qué opciones tengo? <a href="#what-are-my-options" id="what-are-my-options"></a>

Las opciones de desarrollo de Dynamo se dividen principalmente en dos categorías: _para_ Dynamo frente a _en_ Dynamo. Las dos categorías se pueden definir de la siguiente forma: "en" Dynamo implica contenido que se crea mediante el IDE de Dynamo para su uso en Dynamo y "para" Dynamo implica el uso de herramientas externas a fin de crear contenido que se importará en Dynamo para su uso. Aunque esta guía se centra en el desarrollo _para_ Dynamo, a continuación, se describen los recursos para todos los procesos.

### Para Dynamo <a href="#for-dynamo" id="for-dynamo"></a>

Estos nodos permiten el mayor grado de personalización. Muchos paquetes se compilan con este método y es necesario para colaborar en el código fuente de Dynamo. El proceso de compilación de estos elementos se abordará en esta guía.

* Nodos sin contacto
* Nodos derivados de NodeModel
* Extensiones

> Dynamo Primer incluye una guía sobre la [importación de bibliotecas Zero-Touch](https://primer2.dynamobim.org/v/es/6_custom_nodes_and_packages/6-2_packages/5-zero-touch).

En el caso siguiente, se utiliza Visual Studio como entorno de desarrollo para los nodos Zero-Touch y NodeModel.

![Interfaz de Visual Studio](images/vs-devenv.jpg)

> La interfaz de Visual Studio con un proyecto que vamos a desarrollar

### En Dynamo <a href="#in-dynamo" id="in-dynamo"></a>

Aunque estos procesos existen en el espacio de trabajo de programación visual y son relativamente sencillos, son opciones viables para personalizar Dynamo. En Dynamo Primer, se describen estos temas de forma exhaustiva y se proporcionan consejos sobre la creación de secuencias de comandos y procedimientos recomendados en el capítulo [Estrategias de creación de secuencias de comandos](../../9\_best\_practices/2-scripting-strategies.md).

*   Los bloques de código muestran DesignScript en el entorno de programación visual, lo que permite flujos de trabajo flexibles de nodos y secuencias de comandos de texto. Cualquier elemento del espacio de trabajo puede llamar a una función de un bloque de código.

    > Descargue un ejemplo de bloque de código (haga clic con el botón derecho y seleccione Guardar como) o vea un recorrido detallado en [Dynamo Primer](https://primer2.dynamobim.org/v/es/8_coding_in_dynamo/8-1_code-blocks-and-design-script/1-what-is-a-code-block).
*   Los nodos personalizados son contenedores para recopilaciones de nodos o incluso gráficos completos. Son una manera eficaz de recopilar rutinas de uso frecuente y compartirlas con la comunidad.

    > Descargue un ejemplo de nodo personalizado (haga clic con el botón derecho y seleccione Guardar como) o vea un recorrido detallado en [Dynamo Primer](https://primer2.dynamobim.org/v/es/6_custom_nodes_and_packages/6-1_custom-nodes/1-introduction).
*   Los nodos de Python son una interfaz de creación de secuencias de comandos en el espacio de trabajo de programación visual, similares a los bloques de código. Las bibliotecas de Autodesk.DesignScript utilizan una notación de punto similar a DesignScript.

    > Descargue un ejemplo de nodo de Python (haga clic con el botón derecho y seleccione Guardar como) o vea un recorrido detallado en [Dynamo Primer](https://primer2.dynamobim.org/v/es/8_coding_in_dynamo/8-3_python).

El desarrollo en el espacio de trabajo de Dynamo permite obtener de forma eficaz una respuesta inmediata.

![Desarrollo en el espacio de trabajo de Dynamo con el nodo de Python](images/python-example.jpg)

> Desarrollo en el espacio de trabajo de Dynamo con el nodo de Python

### ¿Cuáles son las ventajas o las desventajas de cada método? <a href="#what-are-the-advantagesdisadvantages-of-each" id="what-are-the-advantagesdisadvantages-of-each"></a>

Las opciones de desarrollo para Dynamo se han diseñado para abordar la complejidad de una necesidad de personalización. Tanto si el objetivo es escribir una secuencia de comandos recursiva en Python como compilar una interfaz de usuario de nodos totalmente personalizada, existen opciones para implementar código que solo implican lo necesario para ponerse en marcha.

**Bloques de código, el nodo de Python y los nodos personalizados de Dynamo**

Estas son opciones sencillas para escribir código en el entorno de programación visual de Dynamo. El espacio de trabajo de programación visual de Dynamo proporciona acceso a Python y DesignScript, y la capacidad de incluir varios nodos en un nodo personalizado.

![Bloque de código, secuencia de comandos de Python y nodo personalizado](images/Development-Icons.png)

Con estos métodos, se puede realizar lo siguiente:

* Empiece a escribir en Python o DesignScript con poca o ninguna configuración.
* Importe bibliotecas de Python en Dynamo.
* Comparta bloques de código, nodos de Python y nodos personalizados con la comunidad de Dynamo como parte de un paquete.

**Nodos Zero-Touch**

Zero-Touch hace referencia a un sencillo método de señalar y hacer clic que permite importar bibliotecas de C#. Dynamo leerá los métodos públicos de un archivo `.dll` y los convertirá en nodos de Dynamo. Puede utilizar Zero-Touch para desarrollar sus propios paquetes y nodos personalizados.

![Nodos Zero-Touch](images/ZTImport.png)

Con este método, se puede realizar lo siguiente:

* Importe una biblioteca que no se haya desarrollado necesariamente para Dynamo y cree automáticamente un conjunto de nodos nuevos, como el [ejemplo de A-Forge](../../6\_custom\_nodes\_and\_packages/6-2\_packages/5-zero-touch.md#case-study-importing-aforge) en Dynamo Primer.
* Escriba métodos de C# y utilícelos fácilmente como nodos en Dynamo.
* Comparta una biblioteca de C# como nodos con la comunidad de Dynamo en un paquete.

**Nodos derivados de NodeModel**

Estos nodos son un paso más en la estructura de Dynamo. Se basan en la clase `NodeModel` y se escriben en C#. Aunque este método proporciona la máxima flexibilidad y eficacia, la mayoría de los aspectos del nodo deben definirse explícitamente y las funciones deben residir en un montaje independiente.

![Nodos derivados de NodeModel](images/Development-Icons-NodeModel.png)

Con este método, se puede realizar lo siguiente:

* Cree una interfaz de usuario de nodo personalizable con controles deslizantes, imágenes, color, etc. (por ejemplo, un nodo ColorRange).
* Acceda a lo que ocurre en el lienzo de Dynamo e influya en ello.
* Personalice el encaje.
* Cargue contenido en Dynamo como un paquete.

### Descripción de las versiones de Dynamo y los cambios en las API (1.x → 2.x) <a href="#understanding-dynamo-versioning-and-api-changes-1x-2x" id="understanding-dynamo-versioning-and-api-changes-1x-2x"></a>

Dado que Dynamo se actualiza periódicamente, es posible que se realicen cambios en parte de la API que utiliza un paquete. Es importante realizar un seguimiento de estos cambios para garantizar que los paquetes existentes sigan funcionando correctamente.

Se realiza un seguimiento de los cambios en las API en la página [wiki de Dynamo de GitHub](https://github.com/DynamoDS/Dynamo/wiki/API-Changes). Aquí se abordan los cambios realizados en DynamoCore, las bibliotecas y los espacios de trabajo.

![Documento de cambios en las API de Dynamo](images/api-changes.jpg)

Un ejemplo de un cambio significativo que se producirá próximamente es la transición del formato de archivo XML al formato de archivo JSON en la versión 2.0. Los nodos derivados de NodeModel necesitarán un [constructor JSON](https://github.com/DynamoDS/Dynamo/wiki/Write-a-Json-Constructor-for-a-NodeModel-Node); de lo contrario, no se abrirán en Dynamo 2.0.

La documentación de las API de Dynamo aborda actualmente las funciones principales, [http://dynamods.github.io/DynamoAPI](http://dynamods.github.io/DynamoAPI).

![Documentación de las API](images/api-docs.jpg)

### Permiso para distribuir archivos binarios en un paquete <a href="#permission-to-distribute-binaries-in-a-package" id="permission-to-distribute-binaries-in-a-package"></a>

Tenga cuidado con los archivos .dll incluidos en un paquete que se cargue en Package Manager. Si el autor del paquete no ha creado el archivo .dll, debe tener los derechos necesarios para compartirlo.

Si un paquete incluye archivos binarios, al descargarlo, se debe indicar esto a los usuarios.

### Consideraciones sobre el rendimiento de la interfaz de usuario de Dynamo
En el momento de escribir este artículo, Dynamo utiliza principalmente WPF (Windows Presentation Base) para renderizar su interfaz de usuario. WPF es un sistema complejo y eficaz basado en xaml/binding. Como Dynamo tiene una interfaz de usuario compleja, es fácil provocar bloqueos, fugas de memoria o agrupar la ejecución del gráfico y las actualizaciones de la interfaz de usuario de forma que se reduzca el rendimiento.

Consulte la página wiki sobre las [consideraciones de rendimiento de Dynamo](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-UI-Performance) que le ayudará a evitar algunos errores comunes al realizar cambios en el código de Dynamo.