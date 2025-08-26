# Integración de Dynamo

Esta es la documentación de integración del lenguaje de programación visual de Dynamo.

En esta guía, se tratarán diversos aspectos sobre cómo alojar Dynamo en la aplicación para permitir a los usuarios interactuar con ella mediante la programación visual.

Contenido:

* [Esta introducción](13-dynamo-integration.md#dynamo-integration) ofrece una descripción general de lo que incluye esta guía y de en qué consiste Dynamo.
* [Punto de entrada personalizado de Dynamo](13-dynamo-integration.md#dynamo-custom-entry-point) describe cómo crear un modelo de Dynamo y por dónde empezar.
* [Enlace y seguimiento de elementos](13-dynamo-integration.md#-element-binding-and-trace) describe el uso del mecanismo de seguimiento de Dynamo para unir nodos del gráfico a sus resultados en el anfitrión.
* [Nodos de selección de Dynamo/Revit](13-dynamo-integration.md#-dynamo-revit-selection-nodes) describe cómo implementar nodos que permitan a los usuarios seleccionar objetos o datos del anfitrión y transferirlos como entradas al gráfico de Dynamo.
* [Descripción general de los paquetes integrados de Dynamo](13-dynamo-integration.md#dynamo-built-in-packages-overview) ofrece información sobre la biblioteca estándar de Dynamo y cómo utilizar el mecanismo subyacente para enviar paquetes con su integración.

**Acerca de la terminología:**

En estos documentos, utilizaremos indistintamente los términos secuencia de comandos, gráfico y programa para referirnos al código que los usuarios crean en Dynamo.

## Punto de entrada personalizado de Dynamo

#### Dynamo Revit como ejemplo

[https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534](https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534)

`DynamoModel` es el punto de entrada para una aplicación que aloja Dynamo. Representa una aplicación de Dynamo. El modelo es el objeto raíz de nivel superior que contiene referencias a las demás estructuras de datos y objetos importantes que componen la aplicación de Dynamo y la máquina virtual de DesignScript.

Se utiliza un objeto de configuración para establecer parámetros comunes en el `DynamoModel` cuando se construye.

Los ejemplos de este documento se han obtenido de la implementación de DynamoRevit, que es una integración en la que Revit aloja el `DynamoModel` como un complemento. (Módulo de extensión de arquitectura para Revit). Cuando este complemento se carga, inicia un `DynamoModel` y, a continuación, lo muestra al usuario con una `DynamoView` y un `DynamoViewModel`.

Dynamo es un proyecto .net basado en C# y, para utilizarlo en un proceso dentro de la aplicación, debe poder alojar y ejecutar código .net.

DynamoCore es un motor de cálculo multiplataforma y una colección de modelos de núcleo, que puede construirse con .net o mono (en el futuro .net core). Sin embargo, DynamoCoreWPF incluye los componentes de interfaz de usuario solo para Windows de Dynamo y no realizará la compilación en otras plataformas.

### Pasos para personalizar el punto de entrada de Dynamo

Para inicializar el `DynamoModel`, los integradores deberán realizar estos pasos desde alguna parte del código del anfitrión.

### Cargar previamente archivos DLL de Dynamo compartidos desde el anfitrión

Actualmente, la lista en D4R solo incluye `Revit\SDA\bin\ICSharpCode.AvalonEdit.dll.` Esto se hace para evitar conflictos de versiones de bibliotecas entre Dynamo y Revit. Por ejemplo, Cuando se producen conflictos en `AvalonEdit`, la función de bloque de código puede quedar totalmente interrumpida. La incidencia se indica en Dynamo 1.1.x, en [https://github.com/DynamoDS/Dynamo/issues/7130](https://github.com/DynamoDS/Dynamo/issues/7130), y también se puede reproducir manualmente. Si los integradores encuentran conflictos de bibliotecas entre la función del anfitrión y Dynamo, se sugiere que realicen esta operación como primer paso. En ocasiones, esto es necesario para evitar que otro complemento o la propia aplicación anfitriona carguen una versión incompatible como dependencia compartida. Una solución mejor es resolver el conflicto de versiones adaptando la versión o utilizar una redirección de enlace .net en app.config del anfitrión si es posible.

### Carga de ASM

#### ¿Qué son ASM y LibG?

ASM es la biblioteca de geometría de ADSK en la que se basa Dynamo.

LibG es un empaquetador .net fácil de usar que envuelve el núcleo geométrico de ASM. LibG comparte su esquema de versiones con ASM; utiliza el mismo número de versión principal y secundaria de ASM para indicar que es el contenedor correspondiente de una versión concreta de ASM. Cuando se proporciona una versión de ASM, la versión correspondiente de LibG debe ser la misma. En la mayoría de los casos, LibG debería funcionar con todas las versiones de ASM de una determinada versión principal. Por ejemplo, LibG 223 debería poder cargar cualquier versión de ASM 223.

#### Carga de ASM con Dynamo Sandbox

Dynamo Sandbox se ha diseñado para poder trabajar con varias versiones de ASM, por lo que se incluyen varias versiones de LibG y se envían con el núcleo. Existe una funcionalidad integrada en Shape Manager de Dynamo para buscar los productos de Autodesk que se envían con ASM, por lo que Dynamo puede cargar ASM desde estos productos y hacer que los nodos de geometría funcionen sin necesidad de cargarlos explícitamente en una aplicación anfitriona. La lista de productos actual es:

```
private static readonly List<string> ProductsWithASM = new List<string>() 

 { "Revit", "Civil", "Robot Structural Analysis", "FormIt" }; 
```

Dynamo buscará en el Registro de Windows si los productos de Autodesk de esta lista están instalados en el equipo del usuario. Si alguno de estos está instalado, buscará los archivos binarios de ASM, obtendrá la versión y buscará una versión de LibG correspondiente en Dynamo.

Teniendo en cuenta la versión de ASM, la siguiente API de ShapeManager elegirá la ubicación del precargador de LibG correspondiente para cargarlo. Si se encuentra una coincidencia de versión exacta, se utilizará esta; de lo contrario, se cargará la versión inferior más cercana de LibG con la misma versión principal.

Por ejemplo, si Dynamo se integra con una compilación en desarrollo de Revit que incluye una compilación de ASM 225.3.0 más reciente, Dynamo intentará utilizar LibG 225.3.0, si existe; de lo contrario, intentará utilizar la versión principal más cercana inferior a su primera opción, es decir, 225.0.0.

`public static string GetLibGPreloaderLocation(Version asmVersion, string dynRootFolder)`

#### Integración en proceso de Dynamo al cargar ASM desde el anfitrión

Revit es la primera entrada en la lista de búsqueda de productos de ASM, por lo que, por defecto, `DynamoSandbox.exe` intentará cargar ASM desde Revit en primer lugar. No obstante, deseamos asegurarnos de que la sesión de trabajo integrada de D4R carga ASM desde el anfitrión de Revit actual. Por ejemplo, si el usuario tiene instaladas las versiones R2018 y R2020 en el equipo, al iniciar D4R desde R2020, D4R debería utilizar ASM 225 desde R2020 en lugar de ASM 223 desde R2018. Los integradores tendrán que implementar llamadas similares a la siguiente para forzar la carga de la versión que hayan especificado.

```
internal static Version PreloadAsmFromRevit() 

{ 

     var asmLocation = AppDomain.CurrentDomain.BaseDirectory; 
     Version libGVersion = findRevitASMVersion(asmLocation); 
     var dynCorePath = DynamoRevitApp.DynamoCorePath; 
     var preloaderLocation = DynamoShapeManager.Utilities.GetLibGPreloaderLocation(libGVersion, dynCorePath); 
     Version preLoadLibGVersion = PreloadLibGVersion(preloaderLocation); 
     DynamoShapeManager.Utilities.PreloadAsmFromPath(preloaderLocation, asmLocation); 
     return preLoadLibGVersion; 

} 
```

#### Dynamo: carga de ASM desde una ruta personalizada

Recientemente, hemos añadido la capacidad de que `DynamoSandbox.exe` y `DynamoCLI.exe` carguen una versión de ASM concreta. Para omitir el comportamiento normal de búsqueda en el Registro, puede utilizar el indicador `--GeometryPath` para forzar a Dynamo a cargar ASM desde una ruta determinada.

`DynamoSandbox.exe --GeometryPath "somePath/To/ASMDirectory"`

### Crear una StartConfiguration

La StartupConfiguration se emplea para transferirla como parámetro a fin de inicializar DynamoModel, lo que indica que contiene casi todas las definiciones de cómo desea personalizar la configuración de su sesión de Dynamo. En función de cómo se configuren las siguientes propiedades, la integración de Dynamo podría variar entre distintos integradores. Por ejemplo, diferentes integradores podrían mostrar distintos formatos numéricos o rutas de plantillas de Python.

Consta de lo siguiente:

* DynamoCorePath: donde se encuentran los archivos binarios de carga de DynamoCore.
* DynamoHostPath: donde se encuentran los archivos binarios de integración de Dynamo.
* GeometryFactoryPath: donde se encuentran los archivos binarios de LibG cargados.
* PathResolver: objeto que ayuda a resolver diversos archivos.
* PreloadLibraryPaths: donde se encuentran los archivos binarios de los nodos precargados como, por ejemplo, DSOffice.dll.
* AdditionalNodeDirectories: donde se encuentran los archivos binarios de nodos adicionales.
* AdditionalResolutionPaths: rutas de resolución de montajes adicionales para otras dependencias que puedan ser necesarias al cargar bibliotecas.
* UserDataRootFolder: carpeta de datos del usuario como, por ejemplo, `"AppData\Roaming\Dynamo\Dynamo Revit"`.
* CommonDataRootFolder: carpeta por defecto para guardar definiciones personalizadas, muestras, etc.
* Contexto: nombre del anfitrión del integrador y la versión `(Revit<BuildNum>)`.
* SchedulerThread: subproceso del programador del integrador que implementa `ISchedulerThread`; para la mayoría de los integradores, se trata del subproceso de la interfaz de usuario principal o de cualquier subproceso desde el que puedan acceder a su API.
* StartInTestMode: si la sesión actual es una de automatización de pruebas, modifica un gran número de comportamientos de Dynamo; no lo utilice a menos que esté escribiendo pruebas.
* AuthProvider: implementación del integrador de IAuthProvider, por ejemplo, la implementación de RevitOxygenProvider se encuentra en Greg.dll. Se utiliza para la integración de carga de packageManager.

### Preferencias

`PathManager.PreferenceFilePath` administra la ruta de configuración de preferencias por defecto, por ejemplo, `"AppData\\Roaming\\Dynamo\\Dynamo Revit\\2.5\\DynamoSettings.xml"`. Los integradores pueden decidir si desean enviar también un archivo de configuración de preferencias personalizado a una ubicación que deba alinearse con el administrador de rutas. A continuación, se indican las propiedades de configuración de preferencias que se serializan:

* IsFirstRun: indica si es la primera vez que se ejecuta esta versión de Dynamo, por ejemplo, se utiliza para determinar si es necesario mostrar el mensaje de inclusión/exclusión de GA. También se utiliza para determinar si es necesario migrar la configuración de preferencias de Dynamo heredada al iniciar una nueva versión de Dynamo para que los usuarios tengan una experiencia coherente.
* IsUsageReportingApproved: indica si se aprueban o no los informes de uso.
* IsAnalyticsReportingApproved: indica si se aprueban o no los informes analíticos
* LibraryWidth: indica la anchura del panel izquierdo de la biblioteca de Dynamo.
* ConsoleHeight: indica la altura de la pantalla de la consola.
* ShowPreviewBubbles: indica si deben mostrarse burbujas de vista preliminar.
* ShowConnector: indica si se muestran los conectores.
* ConnectorType: indica el tipo de conector: Bezier o Polilínea.
* BackgroundPreviews: indica el estado activo de la vista preliminar del fondo especificada.
* RenderPrecision: el nivel de precisión de la renderización: cuanto más bajo, se generarán mallas con menos triángulos. Un valor más alto generará una geometría más uniforme en la vista preliminar del fondo. 128 es un valor fácil recomendado para la previsualización de la geometría.
* ShowEdges: indica si se renderizarán los bordes de la superficie y los sólidos
* ShowDetailedLayout: no se utiliza.
* WindowX, WindowY: última coordenada X, Y de la ventana de Dynamo.
* WindowW, WindowH: última anchura y altura de la ventana de Dynamo.
* UseHardwareAcceleration: indica si Dynamo debe utilizar la aceleración de hardware si es compatible
* NumberFormat: la precisión decimal utilizada para mostrar los números en la burbuja de vista preliminar toString().
* MaxNumRecentFiles: el número máximo de rutas de archivos recientes que se guardarán.
* RecentFiles: una lista de rutas de archivos abiertos recientemente. Si se modifica, afectará directamente a la lista de archivos recientes en la página de inicio de Dynamo.
* BackupFiles: una lista de las rutas de archivos de copia de seguridad.
* CustomPackageFolders: una lista de carpetas que contienen los binarios Zero-Touch y las rutas de directorio que se explorarán en busca de paquetes y nodos personalizados.
* PackageDirectoriesToUninstall: una lista de paquetes utilizada por Package Manager para determinar los paquetes marcados para su supresión. Si es posible, estas rutas se suprimirán durante el inicio de Dynamo.
* PythonTemplateFilePath: ruta al archivo de Python (.py) que se utilizará como plantilla de inicio al crear un nuevo nodo PythonScript; puede utilizarse para configurar una plantilla de Python personalizada para la integración.
* BackupInterval: indica cada cuánto tiempo (en milisegundos) se guardará automáticamente el gráfico.
* BackupFilesCount: indica cuántas copias de seguridad se realizarán.
* PackageDownloadTouAccepted: indica si el usuario ha aceptado los términos de uso para la descarga de paquetes desde Package Manager.
* OpenFileInManualExecutionMode: indica el estado por defecto de la casilla "Abrir en modo manual" en OpenFileDialog.
* NamespacesToExcludeFromLibrary: indica los espacios de nombres (si los hay) que no deben mostrarse en la biblioteca de nodos de Dynamo. Formato de cadena: "[nombre de biblioteca]:[espacio de nombres completo]".

Ejemplo de configuración de preferencias serializadas:

```xml
<PreferenceSettings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"> 

<IsFirstRun>false</IsFirstRun> 

<IsUsageReportingApproved>false</IsUsageReportingApproved> 

<IsAnalyticsReportingApproved>false</IsAnalyticsReportingApproved> 

<LibraryWidth>204</LibraryWidth> 

<ConsoleHeight>0</ConsoleHeight> 

<ShowPreviewBubbles>true</ShowPreviewBubbles> 

<ShowConnector>true</ShowConnector> 

<ConnectorType>BEZIER</ConnectorType> 

<BackgroundPreviews> 

<BackgroundPreviewActiveState> 

<Name>IsBackgroundPreviewActive</Name> 

<IsActive>true</IsActive> 

</BackgroundPreviewActiveState> 

<BackgroundPreviewActiveState> 

<Name>IsRevitBackgroundPreviewActive</Name> 

<IsActive>true</IsActive> 

</BackgroundPreviewActiveState> 

</BackgroundPreviews> 

<IsBackgroundGridVisible>true</IsBackgroundGridVisible> 

<RenderPrecision>128</RenderPrecision> 

<ShowEdges>false</ShowEdges> 

<ShowDetailedLayout>true</ShowDetailedLayout> 

<WindowX>553</WindowX> 

<WindowY>199</WindowY> 

<WindowW>800</WindowW> 

<WindowH>676</WindowH> 

<UseHardwareAcceleration>true</UseHardwareAcceleration> 

<NumberFormat>f3</NumberFormat> 

<MaxNumRecentFiles>10</MaxNumRecentFiles> 

<RecentFiles> 

<string></string> 

</RecentFiles> 

<BackupFiles> 

<string>..AppData\Roaming\Dynamo\Dynamo Revit\backup\backup.DYN</string> 

</BackupFiles> 

<CustomPackageFolders> 

<string>..AppData\Roaming\Dynamo\Dynamo Revit\2.5</string> 

</CustomPackageFolders> 

<PackageDirectoriesToUninstall /> 

<PythonTemplateFilePath /> 

<BackupInterval>60000</BackupInterval> 

<BackupFilesCount>1</BackupFilesCount> 

<PackageDownloadTouAccepted>true</PackageDownloadTouAccepted> 

<OpenFileInManualExecutionMode>false</OpenFileInManualExecutionMode> 

<NamespacesToExcludeFromLibrary> 

<string>ProtoGeometry.dll:Autodesk.DesignScript.Geometry.TSpline</string> 

</NamespacesToExcludeFromLibrary> 

</PreferenceSettings> 
```

* Extensions: una lista de extensiones que implementan IExtension; si el valor es nulo, Dynamo cargará las extensiones desde la ruta por defecto (carpeta `extensions` debajo de la carpeta Dynamo).
* IsHeadless: indica si Dynamo se inicia sin interfaz de usuario, lo que afecta a los análisis.
* UpdateManager: implementación del integrador de UpdateManager; consulte la descripción anterior.
* ProcessMode: equivalente a TaskProcessMode. Síncrono si está en modo de prueba; de lo contrario, asíncrono. Controla el comportamiento del programador. Los entornos de un solo subproceso también pueden establecer este valor en síncrono.

Usar el elemento StartConfiguration de destino para iniciar `DynamoModel`

Una vez que se haya transferido la StartConfig para iniciar `DynamoModel`, DynamoCore supervisará los detalles reales para asegurarse de que la sesión de Dynamo se inicializa correctamente con la información especificada. Debe haber algunos pasos posteriores a la configuración que los integradores individuales tendrán que realizar después de que se inicialice `DynamoModel`, por ejemplo, en D4R, se suscriben eventos para vigilar las transacciones del anfitrión de Revit o las actualizaciones de documentos, la personalización de nodos de Python, etc.

### Veamos ya la parte de "programación visual"

Para inicializar `DynamoViewModel` y `DynamoView`, deberá crear primero un `DynamoViewModel`, lo que se puede hacer mediante el método estático `DynamoViewModel.Start`. Consulte la información mostrada a continuación:

```c#

    viewModel = DynamoViewModel.Start(
                    new DynamoViewModel.StartConfiguration()
                    {
                        CommandFilePath = commandFilePath,
                        DynamoModel = model,
                        Watch3DViewModel = 
                            HelixWatch3DViewModel.TryCreateHelixWatch3DViewModel(
                                null,
                                new Watch3DViewModelStartupParams(model), 
                                model.Logger),
                        ShowLogin = true
                    });
     
     var view = new DynamoView(viewModel);

```

`DynamoViewModel.StartConfiguration` incluye muchas menos opciones que la configuración del modelo. En su mayoría, se explican por sí mismas. Los `CommandFilePath` se pueden omitir a menos que se esté escribiendo un caso de prueba.

El parámetro `Watch3DViewModel` controla cómo los nodos de vista preliminar en segundo plano y watch3d muestran la geometría 3D. Puede utilizar su propia implementación si implanta las interfaces necesarias.

Para crear la `DynamoView`, todo lo que se requiere es el `DynamoViewModel`. La vista es un control de ventana y se puede mostrar mediante WPF.

### Ejemplo de DynamoSandbox.exe:

DynamoSandbox.exe es un entorno de desarrollo para probar y utilizar DynamoCore y experimentar con él. Es un excelente ejemplo para ver cómo se cargan y se configuran los componentes `DynamoCore` y `DynamoCoreWPF`. Puede ver algunos de los puntos de entrada [aquí](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoSandbox/DynamoCoreSetup.cs#L37).

## Enlace y seguimiento de elementos

#### Descripción general

El _seguimiento_ es un mecanismo de Dynamo Core que permite serializar datos en el archivo .dyn (archivo de Dynamo). Lo más importante es que estos datos están relacionados con los sitios de llamada de los nodos dentro del gráfico de Dynamo.

Cuando se abre un gráfico de Dynamo desde el disco, los datos de seguimiento guardados en él se vuelven a asociar a los nodos del gráfico.

#### Glosario

* Mecanismo de seguimiento:
  * Implementa el enlace de elementos en Dynamo.
  * El mecanismo de seguimiento puede utilizarse para garantizar que los objetos se vuelven a enlazar a la geometría que se ha creado.
  * El mecanismo de sitio de llamada y seguimiento se encarga de proporcionar un GUID persistente que el implementador del nodo puede utilizar para la revinculación.
* Sitio de llamada
  * El archivo ejecutable contiene varios sitios de llamada. Estos se utilizan para enviar la ejecución a los distintos lugares desde donde deben enviarse:
    * Biblioteca de C#
    * Método integrado
    * Función de DesignScript
    * Nodo personalizado (función de DS)
* TraceSerializer
  * Serializa las clases `ISerializable` y `[Serializable]` marcadas en un seguimiento.
  * Controla la serialización y la deserialización de datos en seguimiento.
  * TraceBinder controla el enlace de los datos deserializados a un tipo en tiempo de ejecución. (Crea una instancia de clase real).

#### ¿Qué aspecto tiene?

***

Los datos de seguimiento se serializan en el archivo .dyn dentro de una propiedad denominada Bindings. Se trata de una matriz de ID de sitios de llamada -> datos. Un sitio de llamada es la ubicación/instancia concreta en la que se llama a un nodo en la máquina virtual de DesignScript. Cabe mencionar que se puede llamar a los nodos de un gráfico de Dynamo varias veces y, por lo tanto, se pueden crear varios sitios de llamada para una única instancia de nodo.

```json
"Bindings": [
    {
      "NodeId": "1e83cc25-7de6-4a7c-a702-600b79aa194d",
      "Binding": {
        "WrapperObject_InClassDecl-1_InFunctionScope-1_Instance0_1e83cc25-7de6-4a7c-a702-600b79aa194d":  "Base64 Encoded Data"
      }
    },
    {
      "NodeId": "c69c7bec-d54b-4ead-aea8-a3f45bea9ab2",
      "Binding": {
        "WrapperObject_InClassDecl-1_InFunctionScope-1_Instance0_c69c7bec-d54b-4ead-aea8-a3f45bea9ab2": "Base64 Encoded Data"
      }
    }
  ],

 
```

_NO_ es aconsejable depender del formato de los datos codificados en base64 serializados.

#### ¿Qué problema estamos tratando de resolver?

***

Hay muchas razones por las que se desearía guardar datos arbitrarios como resultado de la ejecución de una función, pero, en este caso, el seguimiento se ha desarrollado para resolver un problema específico con el que los usuarios se encuentran con frecuencia cuando crean y iteran programas de software que generan elementos en aplicaciones anfitrionas.

Se trata de un problema que hemos denominado `Element Binding` y el planteamiento es el siguiente:

A medida que un usuario desarrolla y ejecuta un gráfico de Dynamo, es probable que genere nuevos elementos en el modelo de la aplicación anfitriona. En nuestro ejemplo, supongamos que el usuario tiene un pequeño programa que genera 100 puertas en un modelo arquitectónico. El número y la ubicación de estas puertas se controlan mediante el programa.

La primera vez que el usuario ejecuta el programa, se generan estas 100 puertas.

Más tarde, cuando el usuario modifique una entrada en el programa y la vuelva a ejecutar, este _(sin enlace de elementos)_ creará 100 puertas nuevas; las puertas antiguas seguirán existiendo en el modelo junto con las nuevas.

***

Como Dynamo es un entorno de programación en directo y presenta un modo de ejecución `"Automatic"` en el que los cambios realizados en el gráfico desencadenan una nueva ejecución, esto puede sobrecargar rápidamente un modelo con los resultados de muchas ejecuciones del programa.

Hemos descubierto que esto no suele ser lo que los usuarios esperan, sino que, al activar el enlace de elementos, los resultados anteriores de la ejecución de un gráfico se limpian, se eliminan o se modifican. ¿Cuál (_suprimido o modificado_) depende de la flexibilidad de la API del anfitrión? Con el enlace de elementos activado, después de la segunda, tercera o quincuagésima ejecución del programa de Dynamo del usuario, solo hay 100 puertas en el modelo.

Esto requiere algo más que ser capaz de serializar datos en el archivo .dyn y como verá más adelante hay mecanismos en DynamoRevit creados a partir del seguimiento para admitir estos flujos de trabajo de reenlace.

***

Este es un momento adecuado para mencionar otro caso de uso importante del enlace de elementos para anfitriones como Revit. Como los elementos que se crearon cuando se activó el enlace de elementos intentarán mantener los ID de los elementos existentes (modificar los elementos existentes), la lógica que se generó a partir de estos elementos en la aplicación anfitriona seguirá existiendo después de que se ejecute un programa de Dynamo. Por ejemplo:

Volvamos al ejemplo del modelo arquitectónico.

Veamos primero un ejemplo con el enlace de elementos desactivado. Esta vez el usuario tiene un programa que genera unos muros arquitectónicos.

Ejecuta el programa y este genera algunos muros en la aplicación anfitriona. A continuación, sale del gráfico de Dynamo y utiliza las herramientas normales de Revit para colocar algunas ventanas en esos muros. Las ventanas están vinculadas a esos muros específicos como parte del modelo de Revit.

El usuario vuelve a poner en funcionamiento Dynamo y ejecuta el gráfico de nuevo; ahora, como en nuestro último ejemplo, tiene dos conjuntos de muros. En el primer conjunto, se añaden ventanas, pero no a los nuevos muros.

Si se ha activado el enlace de elementos, podemos conservar el trabajo existente que se realizó manualmente en la aplicación anfitriona sin Dynamo. Por ejemplo, si el usuario ejecuta el programa por segunda vez y se ha activado el enlace, los muros se modificarán, no se suprimirán, y los cambios posteriores realizados en la aplicación anfitriona se conservarán. El modelo contendría muros con ventanas en lugar de dos conjuntos de muros con distintos estados.

***

![Crear muros](images/creates_walls.png)

#### Enlace de elementos en comparación con el seguimiento

***

El seguimiento es un mecanismo de Dynamo Core que utiliza una variable estática de sitios de llamada a datos para asignar los datos al sitio de llamada de una función en el gráfico, como se ha descrito anteriormente.

También permite serializar datos arbitrarios en el archivo .dyn al escribir nodos ZeroTouch de Dynamo. Esto no suele ser recomendable, ya que significa que el código Zero-Touch, potencialmente transferible ahora depende del núcleo de Dynamo.

No confíe en el formato serializado de los datos del archivo .dyn; utilice en su lugar el atributo y la interfaz [Serializable].

ElementBinding, por su parte, se basa en las API de seguimiento y se implementa en la integración de Dynamo _(DynamoRevit, Dynamo4Civil, etc.)_.

#### API de seguimiento

Algunas de las API de seguimiento de bajo nivel que vale la pena conocer son las siguientes:

```c#
public static ISerializable GetTraceData(string key)
///Returns the data that is bound to a particular key

public static void SetTraceData(string key, ISerializable value)
///Set the data bound to a particular key
```

Puede ver cómo se utilizan en el siguiente ejemplo.

Para interactuar con los datos de seguimiento que Dynamo ha cargado desde un archivo existente o que está generando, puede consultar lo siguiente:

```c#
 public IDictionary<Guid, List<CallSite.RawTraceData>> 
 GetTraceDataForNodes(IEnumerable<Guid> nodeGuids, Executable executable)
```

[GetTraceDataForNodes](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs#L218)

[RuntimeTrace.cs](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs)

#### Ejemplo de seguimiento simple desde un nodo

***

En el repositorio [DynamoSamples](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/TraceExample.cs), se proporciona un ejemplo de nodo de Dynamo que utiliza directamente el seguimiento.

El resumen de la clase describe a continuación la esencia de lo que es el seguimiento:

```
  /*
     * After a graph update, Dynamo typically disposes of all
     * objects created during the graph update. But what if there are 
     * objects which are expensive to re-create, or which have other
     * associations in a host application? You wouldn't want those those objects
     * re-created on every graph update. For example, you might 
     * have an external database whose records contain data which needs
     * to be re-applied to an object when it is created in Dynamo.
     * In this example, we use a wrapper class, TraceExampleWrapper, to create 
     * TraceExampleItem objects which are stored in a static dictionary 
     * (they could be stored in a database as well). On subsequent graph updates, 
     * the objects will be retrieved from the data store using a trace id stored 
     * in the trace cache.
     */
```

En este ejemplo, se utilizan las API de seguimiento de DynamoCore directamente para almacenar algunos datos cada vez que se ejecuta un nodo determinado. En este caso, un diccionario desempeña el papel del modelo de aplicación anfitriona, como la base de datos del modelo de Revit.

Esta es la configuración aproximada:

Se importa un `TraceExampleWrapper` de clase de utilidad estática como un nodo en Dynamo. Contiene un único método `ByString` que crea `TraceExampleItem`. Son objetos de .net normales que contienen una propiedad `description`.

Cada `TraceExampleItem` se serializa en un seguimiento representado con un `TraceableId`; se trata simplemente de una clase que contiene un `IntId` que está marcado como `[Serializeable]` para que se pueda serializar con el formateador `SOAP`. Consulte la información mostrada [aquí](https://docs.microsoft.com/es-es/dotnet/api/system.serializableattribute?view=netframework-4.8) para obtener más información sobre el atributo serializable.

También debe implementar la interfaz de `ISerializable` definida [aquí](https://docs.microsoft.com/es-es/dotnet/api/system.runtime.serialization.iserializable?view=netframework-4.8).

```c#
    [IsVisibleInDynamoLibrary(false)]
    [Serializable]
    public class TraceableId : ISerializable
    {
    }
```

Esta clase se crea para cada `TraceExampleItem` que deseemos guardar en el seguimiento, se serializa, se codifica en base64 y se guarda en el disco cuando se guarda el gráfico para que los enlaces se puedan volver a asociar, incluso más tarde, cuando el gráfico se vuelva a abrir sobre un diccionario de elementos existente. Esto no funcionará correctamente en el caso de este ejemplo, ya que el diccionario no es realmente persistente como lo es un documento de Revit.

Por último, la última parte de la ecuación es el `TraceableObjectManager`, que es similar al `ElementBinder` en `DynamoRevit`, que administra la relación entre los objetos presentes en el modelo del documento del anfitrión y los datos que hemos almacenado en el seguimiento de Dynamo.

Cuando un usuario ejecuta un gráfico que contiene el nodo `TraceExampleWrapper.ByString` por primera vez, se crea un nuevo `TraceableId` con un nuevo ID, el `TraceExampleItem` se almacena en el diccionario asignado a ese nuevo ID y se almacena el `TraceableID` en el seguimiento.

En la siguiente ejecución del gráfico, buscamos en el seguimiento y encontramos el ID que almacenamos allí. A continuación, encontramos el objeto asignado a ese ID y lo devolvemos. En lugar de crear un objeto nuevo, modificamos el existente.

El flujo de dos ejecuciones consecutivas del gráfico que crea un único `TraceExampleItem` presenta el siguiente aspecto:

![Primera llamada](images/Trace-first-call.png)

![Segunda llamada](images/Trace-second-call.png)

La misma idea se muestra en el siguiente ejemplo con un caso de uso más realista de un nodo de DynamoRevit.

#### Diagrama de seguimiento

![Pasos de seguimiento](images/trace_diagram.png) ![Flujo de seguimiento](images/trace_alt_diagram.png)

#### NOTA

En las versiones recientes de Dynamo, el uso de TLS (almacenamiento local de subprocesos) se ha reemplazado por el uso de miembros estáticos.

#### Ejemplo de implementación de enlaces de elementos

***

Echemos un vistazo rápido al aspecto de un nodo que utiliza el enlace de elementos cuando se implementa para DynamoRevit, esto es análogo al tipo de nodo utilizado anteriormente en los ejemplos indicados de creación de muros.

***

```c#
    private void InitWall(Curve curve, Autodesk.Revit.DB.WallType wallType, Autodesk.Revit.DB.Level baseLevel, double height, double offset, bool flip, bool isStructural)
        {
            // This creates a new wall and deletes the old one
            TransactionManager.Instance.EnsureInTransaction(Document);

            //Phase 1 - Check to see if the object exists and should be rebound
            var wallElem =
                ElementBinder.GetElementFromTrace<Autodesk.Revit.DB.Wall>(Document);

            bool successfullyUsedExistingWall = false;
            //There was a modelcurve, try and set sketch plane
            // if you can't, rebuild 
            if (wallElem != null && wallElem.Location is Autodesk.Revit.DB.LocationCurve)
            {
                var wallLocation = wallElem.Location as Autodesk.Revit.DB.LocationCurve;
                <SNIP>

                    if(!CurveUtils.CurvesAreSimilar(wallLocation.Curve, curve))
                        wallLocation.Curve = curve;

                  <SNIP>
                
            }

            var wall = successfullyUsedExistingWall ? wallElem :
                     Autodesk.Revit.DB.Wall.Create(Document, curve, wallType.Id, baseLevel.Id, height, offset, flip, isStructural);
            InternalSetWall(wall);

            TransactionManager.Instance.TransactionTaskDone();

            // delete the element stored in trace and add this new one
            ElementBinder.CleanupAndSetElementForTrace(Document, InternalWall);
        }
```

En el código anterior, se muestra un constructor de ejemplo para un elemento de muro, al que se llamaría desde un nodo de Dynamo, como se indica a continuación: `Wall.byParams`

Las fases importantes de la ejecución del constructor en relación con el enlace de elementos son las siguientes:

1. Utilice el `elementBinder` para comprobar si hay objetos creados anteriormente que se hayan enlazado a este sitio de llamada en una ejecución anterior. `ElementBinder.GetElementFromTrace<Autodesk.Revit.DB.Wall>`
2. Si es así, intente modificar ese muro en lugar de crear uno nuevo.

```c#
 if(!CurveUtils.CurvesAreSimilar(wallLocation.Curve, curve))
                        wallLocation.Curve = curve;
```

3. De lo contrario, cree un nuevo muro.

```c#
  var wall = successfullyUsedExistingWall ? wallElem :
                     Autodesk.Revit.DB.Wall.Create(Document, curve, wallType.Id, baseLevel.Id, height, offset, flip, isStructural);
                     
```

4. Suprima el elemento anterior que acabamos de recuperar del seguimiento y añada el nuevo para que podamos buscarlo en el futuro, como se indica a continuación:

```c#
 ElementBinder.CleanupAndSetElementForTrace(Document, InternalWall);
```

### Debate

#### Rendimiento

* Actualmente, cada objeto de seguimiento se serializa mediante el formato XML SOAP, que es bastante detallado y duplica mucha información. A continuación, los datos se codifican en base64 dos veces, lo que no es eficaz en términos de serialización o deserialización. Esto puede mejorarse en el futuro si no se construye sobre el formato interno. Repetimos; no confíe en el formato de los datos serializados en reposo.

#### ¿ElementBinding debería estar activado por defecto?

* Hay casos de uso en los que no se desea enlazar elementos. ¿Y si uno es un usuario avanzado de Dynamo que desarrolla un programa que debe ejecutarse varias veces para generar elementos de agrupación aleatorios? La intención del programa es crear elementos adicionales cada vez que se ejecuta. Este caso de uso no es fácil de conseguir sin soluciones que impidan el funcionamiento del enlace de elementos. Es posible desactivar elementBinding en el nivel de integración, pero probablemente esta debería ser una funcionalidad central de Dynamo. No está claro cómo de detallada debe ser esta funcionalidad: ¿nivel de nodo?, ¿nivel de sitio de llamada?, ¿sesión completa de Dynamo?, ¿área de trabajo?, etc.

## ¿Qué son los nodos de selección de Dynamo Revit?

En general, estos nodos permiten al usuario describir de algún modo un subconjunto del documento de Revit activo al que se desea hacer referencia. Existen varias formas en las que el usuario puede hacer referencia a un elemento de Revit (se describen a continuación) y la salida resultante del nodo puede ser un empaquetador de elementos de Revit (contenedor de DynamoRevit) o alguna geometría de Dynamo (convertida a partir de geometría de Revit). Será útil considerar la diferencia entre estos tipos de salida en el contexto de otras integraciones de anfitriones.

De forma general, **una buena forma de conceptualizar estos nodos es como una función que acepta un ID de elemento y devuelve un indicador a ese elemento o a alguna geometría que represente ese elemento.**

Hay varios nodos `Selection` en DynamoRevit. Podemos dividirlos en al menos los siguientes dos grupos:

![Nodos de selección de Revit](images/revitSelectionNodes.png)

1.  Selección de interfaz de usuario:

    Los nodos de `DynamoRevit` de ejemplo de esta categoría son `SelectModelElement` y `SelectElementFace`.

    Estos nodos permiten al usuario cambiar al contexto de la interfaz de usuario de Revit y seleccionar un elemento o un conjunto de elementos, se capturan los ID de estos elementos y se ejecuta alguna función de conversión, se crea un empaquetador, o bien se extrae la geometría y se convierte a partir del elemento. La conversión que se ejecuta depende del tipo de nodo que elija el usuario.
2.  Consulta de documento:

    Los nodos de ejemplo de esta categoría son `AllElementsOfClass` y `AllElementsOfCategory`.

    Estos nodos permiten al usuario consultar todo el documento en busca de un subconjunto de elementos. Por lo general, estos nodos devuelven empaquetadores que señalan a los elementos de Revit subyacentes. Estos empaquetadores forman parte integral de la experiencia de DynamoRevit, ya que ofrecen una funcionalidad más avanzada, como el enlace de elementos, y permiten a los integradores de Dynamo elegir las API de anfitrión que se exponen como nodos a los usuarios.

### Flujos de trabajo de usuario de Dynamo Revit:

#### Casos de ejemplo

1.
   * El usuario selecciona un muro de Revit con `SelectModelElement`. Se devuelve un empaquetador de muros de Dynamo al gráfico (visible en la burbuja de vista preliminar del nodo).
   * El usuario coloca el nodo Element.Geometry y enlaza la salida de `SelectModelElement` a este nuevo nodo. La geometría del muro empaquetado se extrae y se convierte en geometría de Dynamo mediante la API libG.
   * El usuario cambia el gráfico al modo de ejecución automático.
   * El usuario modifica el muro original en Revit.
   * El gráfico se vuelve a ejecutar automáticamente a medida que el documento de Revit genera un evento que indica que se han actualizado algunos elementos. El nodo de selección observa este evento y ve que se ha modificado el ID de elemento seleccionado.

### Flujos de trabajo de usuario de DynamoCivil:

Los flujos de trabajo de D4C son muy similares a la descripción anterior para Revit; aquí hay dos conjuntos típicos de nodos de selección en D4C:

![Nodos de selección de Civil 3D](images/civilSelectionNodes.png)

### Incidencias:

*   Debido a la herramienta de actualización de modificaciones del documento que implementan los nodos de selección en `DynamoRevit`, se pueden crear fácilmente bucles infinitos; imagine un nodo que examina el documento en busca de todos los elementos y, a continuación, crea nuevos elementos en algún punto posterior de este nodo. Cuando se ejecute, este programa provocará un bucle. `DynamoRevit` intenta detectar estos casos de varias formas mediante identificadores de transacción y, para ello, evita modificar el documento cuando las entradas a los constructores de elementos no hayan cambiado.

    Esto debe tenerse en cuenta si la ejecución automática del gráfico se inicia cuando se modifica un elemento seleccionado en la aplicación anfitriona.
* Los nodos de selección de `DynamoRevit` se implementan en el proyecto `RevitUINodes.dll` que hace referencia a WPF. Es posible que no sea una incidencia, pero merece la pena tenerla en cuenta en función de la plataforma de destino.

### Diagramas de flujo de datos

![Flujo de selección](images/selectModelElement.png)

![Flujo de selección 2](images/selectElementFace.png)

### Implementación técnica: (consulte los diagramas anteriores):

Los nodos de selección se implementan al heredar de los tipos de `SelectionBase` genéricos: `SelectionBase<TSelection, TResult>` y un conjunto mínimo de barras:

* Implementación de un método `BuildOutputAST`: este método debe devolver un AST, que se ejecutará en algún momento en el futuro, cuando se vaya a ejecutar el nodo. En el caso de los nodos de selección, debe devolver elementos o geometría a partir de los ID de elemento. [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs#L280](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs#L280)
* La implementación de `BuildOutputAST` es una de las partes más difíciles de la implementación de nodos de `NodeModel`/interfaz de usuario. Es mejor poner toda la lógica posible en una función de C# y simplemente incrustar un nodo de llamada de función AST en AST. Tenga en cuenta que aquí `node` hace referencia a un nodo AST en el árbol de sintaxis abstracta, no a un nodo en el gráfico de Dynamo.

![Flujo de selección 2](images/selectionAST.png)

* Serialización:
  *   Dado que se trata de tipos derivados de `NodeModel` explícitos (no ZeroTouch), también requieren la implementación de un [JsonConstructor] que se utilizará durante la deserialización del nodo a partir de un archivo .dyn.

      Las referencias a elementos del anfitrión se deben guardar en el archivo .dyn para que, cuando un usuario abra un gráfico que contenga este nodo, su selección siga definida. Los nodos NodeModel en Dynamo utilizan json.net para la serialización; cualquier propiedad pública se serializará automáticamente mediante json.net; utilice el atributo [JsonIgnore] para serializar solo lo necesario.
* Los nodos de consulta de documentos son un poco más sencillos, ya que no necesitan almacenar una referencia a ningún ID de elemento. Consulte a continuación las implementaciones de la clase `ElementQueryBase` y las clases derivadas. Cuando se ejecutan, estos nodos realizan una llamada a la API de Revit y consultan los elementos en el documento subyacente; además, realizan la conversión mencionada anteriormente a la geometría o a los contenedores de elementos de Revit.

### Referencias:

#### Clases base de DynamoCore:

* [https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs](https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs)
* [Caso real de NodeModel (interfaz de usuario personalizada)](11_developer_primer/3_developing_for_dynamo/5-nodemodel-case-study-custom-ui.md)
* [Actualización de paquetes y bibliotecas de Dynamo para Dynamo 2.x](11_developer_primer/3_developing_for_dynamo/6-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
* [Actualización de paquetes y bibliotecas de Dynamo para Dynamo 3.x](11_developer_primer/3_developing_for_dynamo/updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)

#### DynamoRevit:

* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs)
* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs)

## Descripción general de los paquetes integrados de Dynamo

El mecanismo de paquetes integrados consiste en agrupar más contenido de nodos con Dynamo Core sin expandir el núcleo en sí, aprovechando la función de carga de paquetes de Dynamo implementada por las extensiones `PackageLoader` y `PackageManager`.

En este documento, utilizaremos indistintamente los términos paquetes integrados, paquetes integrados de Dynamo y paquetes incorporados.

### ¿Debo enviar un paquete como paquete integrado?

* El paquete debe tener puntos de entrada binarios firmados o no se cargará.
* Se debe hacer todo lo posible para evitar cambios importantes en estos paquetes. Esto significa que el contenido del paquete debe tener pruebas automatizadas.
* Control de versiones semántico; probablemente sea una buena idea crear versiones del paquete mediante un esquema de control de versiones semántico y comunicarlo a los usuarios en la descripción del paquete o en los documentos.
* Pruebas automatizadas. Consulte la información indicada anteriormente; si se incluye un paquete mediante el mecanismo de paquete integrado, a un usuario le parecerá que forma parte del producto y que debe probarse como un producto.
* Alto nivel de perfeccionamiento: iconos, documentación de los nodos y contenido localizado.
* No envíe paquetes que usted o su equipo no puedan mantener.
* No envíe paquetes de terceros de esta forma (consulte la información anterior).

Básicamente, debe tener control total sobre el paquete, así como capacidad para corregirlo, mantenerlo actualizado y probarlo con los cambios más recientes de Dynamo y su producto. También necesita la capacidad de firmarlo.

### Paquetes integrados frente a paquetes específicos de integración del anfitrión

Nuestra intención es que los `Built-In Packages` sean una función básica, un conjunto de paquetes al que tengan acceso todos los usuarios, incluso aunque no tengan acceso a Package Manager. Actualmente, el mecanismo subyacente para admitir esta función es una ubicación de carga por defecto adicional para los paquetes directamente en el directorio de Dynamo Core en relación con DynamoCore.dll.

Con algunas restricciones, esta ubicación podrá ser utilizada por los clientes e integradores de ADSK Dynamo para distribuir sus paquetes específicos de integración _(por ejemplo, la integración de FormIt para Dynamo requiere un paquete de FormIt para Dynamo personalizado)._

Dado que el mecanismo de carga subyacente es el mismo tanto para los paquetes del núcleo como para los específicos del anfitrión, será necesario asegurarse de que los paquetes distribuidos de esta forma no provoquen confusión en el usuario sobre los paquetes `Built-In Packages` del núcleo frente a los específicos de la integración que solo están disponibles en un único producto anfitrión. Recomendamos que, para evitar confusiones entre los usuarios, se introduzcan paquetes específicos del anfitrión en conversaciones con los equipos de Dynamo.

### Localización de paquetes

Dado que los paquetes incluidos en el `Built-In Packages` estarán disponibles para más clientes y las garantías que ofrezcamos sobre ellos serán más estrictas (véase más arriba), estos deberán localizarse.

En el caso de los paquetes de ADSK internos destinados a la inclusión de `Built-In Packages`, las limitaciones actuales de no poder publicar el contenido localizado en Package Manager no suponen un impedimento, ya que los paquetes no necesitan publicarse necesariamente en Package Manager.

Mediante una solución provisional, es posible crear manualmente (e incluso publicar) paquetes con subdirectorios de localización en la carpeta /bin de un paquete.

En primer lugar, cree manualmente los subdirectorios de localización específicos que necesita en la carpeta `/bin` de paquetes.

Si, por alguna razón, el paquete también necesita publicarse en Package Manager, primero debe publicar una versión del paquete a la que le falten estos subdirectorios de localización y, a continuación, publicar una nueva versión del paquete mediante la `publish package version` de DynamoUI. La carga de la nueva versión en Dynamo no debería suprimir las carpetas y los archivos debajo de `/bin` que haya añadido manualmente mediante el Explorador de archivos de Windows. El proceso de carga de paquetes en Dynamo se actualizará para hacer frente a los requisitos de los archivos localizados en el futuro.

Estos subdirectorios de localización se cargan sin incidencias mediante el tiempo de ejecución de .net si se encuentran en el mismo directorio que los archivos binarios de nodo/extensión.

Para obtener más información sobre los montajes de recursos y los archivos .resx, consulte [https://docs.microsoft.com/es-es/dotnet/framework/resources/creating-resource-files-for-desktop-apps](https://docs.microsoft.com/es-es/dotnet/framework/resources/creating-resource-files-for-desktop-apps).

Es probable que cree los archivos `.resx` y los compile con Visual Studio. Para un `xyz.dll` de montaje determinado, los recursos resultantes se compilarán en un nuevo montaje `xyz.resources.dll` (como se ha descrito anteriormente); la ubicación y el nombre de este montaje son importantes.

El archivo `xyz.resources.dll` generado debe encontrarse en la siguiente ubicación: `package\bin\culture\xyz.resources.dll`.

Para acceder a las cadenas localizadas del paquete, puede utilizar ResourceManager, pero lo que es aún más sencillo, debería poder hacer referencia a `Properties.Resources.YourLocalizedResourceName` desde dentro del montaje para el que ha añadido un archivo `.resx`. Por ejemplo, consulte

[https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457) para obtener un ejemplo de mensaje de error localizado.

o [https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19) para obtener un ejemplo de una cadena de atributo NodeDescription específica de Dynamo localizada.

o [https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs) para obtener otro ejemplo.

### Diseño de biblioteca de nodos

Por lo general, cuando Dynamo carga nodos de un paquete, los coloca en la sección `Addons` de la biblioteca de nodos. Para integrar mejor los nodos de paquetes incorporados con otros contenidos integrados, hemos añadido la posibilidad de que los autores de paquetes integrados proporcionen un archivo de `layout specification` parcial para ayudar a colocar los nuevos nodos en la categoría de nivel superior correcta de la sección de biblioteca `default`.

Por ejemplo, el siguiente archivo json de especificaciones de diseño; si se encuentra en la ruta `package/extra/layoutspecs.json`, colocará los nodos especificados por `path` en la categoría `Revit` en la sección `default`, que es la sección principal de nodos integrados.

Tenga en cuenta que los nodos importados de un paquete incorporado presentarán el prefijo `bltinpkg://` cuando se tengan en cuenta para compararlos con una ruta incluida en la especificación de diseño.

```json
{
  "sections": [
    {
      "text": "default",
      "iconUrl": "",
      "elementType": "section",
      "showHeader": false,
      "include": [ ],
      "childElements": [
        {
          "text": "Revit",
          "iconUrl": "",
          "elementType": "category",
          "include": [],
          "childElements": [
            {
              "text": "some sub group name",
              "iconUrl": "",
              "elementType": "group",
              "include": [
                {
                  "path": "bltinpkg://namespace.namespace",
                  "inclusive": false
                }
              ],
              "childElements": []
            }
          ]
        }
      ]
    }
  ]
}
```

Las modificaciones de diseño complejas no se prueban ni admiten correctamente; la intención de esta carga de especificación de diseño específica es mover un espacio de nombres de paquete completo a una categoría de anfitrión determinada, como `Revit` o `Formit`.
