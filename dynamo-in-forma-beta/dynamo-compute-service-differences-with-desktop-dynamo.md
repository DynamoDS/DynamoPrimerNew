# Diferencias en el servicio de cálculo de Dynamo con la versión de escritorio de Dynamo

En esta página, se destacan las diferencias que debe tener en cuenta al escribir programas de Dynamo para que se ejecuten en el contexto de la nube del servicio de cálculo de Dynamo.

## ¿Qué es DaaS?

DaaS, Dynamo como servicio, servicio de cálculo de Dynamo y otros términos relacionados hacen referencia a lo mismo, el tiempo de ejecución central de Dynamo al ejecutarse en un contexto de nube. Esto significa que el gráfico no se está ejecutando en el equipo. Actualmente, solo se puede acceder a DaaS a través de la extensión Dynamo Player para Forma, donde los usuarios pueden cargar y gestionar archivos `.dyn` creados en el entorno de escritorio, ejecutar archivos `.dyn` compartidos por sus compañeros a través de la extensión o utilizar rutinas de `.dyn` precargadas proporcionadas por Autodesk como ejemplos.

Debido a que los gráficos se ejecutan en este contexto de nube y no en el equipo, por el momento, DaaS no puede utilizar directamente los contextos de anfitrión tradicionales de Dynamo (Revit, Civil 3D, etc.). Si desea utilizar tipos de esos programas en el gráfico, deberá serializarlos (guardarlos) en el gráfico mediante el nodo `Data.Remember` u otras técnicas de serialización dentro del gráfico. Estos son similares a los flujos de trabajo que debe utilizar al escribir gráficos para la función Diseño generativo de Revit.

## ¿Qué versión de Dynamo ejecuta el código?

La versión se basa en 3.x y se actualiza con frecuencia en función de la rama principal de código abierto de Dynamo.

## ¿Qué paquetes o nodos están disponibles en esta versión de Dynamo?

* La mayoría de los nodos principales; consulte la siguiente sección para conocer algunas limitaciones específicas.
* Paquete `DynamoFormaBeta` para interactuar con la API de Forma.
* `VASA` para voxelización/análisis eficiente.
* `MeshToolKit` para la manipulación de mallas. El Kit de herramientas de malla también está disponible de forma predefinida a partir de Dynamo 3.4.
* `RefineryToolkit` para algoritmos útiles que permiten pruebas de conflictos, distancia de visualización, la ruta más corta, isovistas, etc.

## ¿Qué debo tener en cuenta al escribir gráficos para DaaS?

* Los nodos de Python no funcionarán. Estos no se ejecutarán _actualmente_.
* No se pueden utilizar paquetes personalizados.
* La capa de interfaz de usuario/vista de los nodos de interfaz de usuario no se ejecutará. No prevemos que esto sea un problema con la funcionalidad principal, pero es recomendable tenerlo en cuenta si observa un error asociado a un nodo con la interfaz de usuario personalizada.
* Las funciones solo para Windows no estarán operativas. Por ejemplo, si intenta utilizar el Registro de Windows o WPF, se producirá un error.
* No se cargarán las extensiones de vista.
* Los nodos filesystem no resultarán muy útiles. Todos los archivos a los que haga referencia en el equipo local no existirán cuando se ejecuten en DaaS.
* Los nodos de interoperabilidad de Excel/DSOffice no funcionarán. Deberían funcionar los nodos de Open XML.
* En general, las solicitudes de red no funcionarán, aunque podrá realizar llamadas a la API de Forma.

## ¿Cómo se supone que voy a recordar todo eso? ¿Y si hay otros cambios?

* En el futuro, tenemos la intención de proporcionar herramientas dentro de Dynamo para la versión de escritorio que facilitarán que el gráfico se ejecute de la misma forma en ambos contextos.

## ¿Cuánto cuesta esto?

* Durante esta versión beta, no se cobrará el tiempo de cálculo.

## ¿Cómo empiezo?

Consulte la [entrada del blog](https://dynamobim.org/dynamo-as-a-service-powers-up-dynamo-player-in-forma/), la [serie de YouTube](https://www.youtube.com/playlist?list=PLY-ggSrSwbZqlbQG1i45bpT8clCJp08wD) o los ejemplos de la extensión de Forma para comenzar. Estos le guiarán a través de los siguientes procedimientos:

* Obtención de acceso a Autodesk Forma
* Instalación de DynamoFormaBeta para Dynamo en el escritorio y la extensión de Dynamo en Forma
* Escritura del primer gráfico

## Seguridad

* Tenga en cuenta que los gráficos compartidos se almacenan en Forma.
* El tiempo máximo de ejecución del gráfico actual es inferior a 30 minutos. Este valor puede cambiar.
* Las solicitudes de ejecución presentan una velocidad limitada, por lo que es posible que se produzcan errores si realiza muchas solicitudes de cálculo en un periodo demasiado corto.
