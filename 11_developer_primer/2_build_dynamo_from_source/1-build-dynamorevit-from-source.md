# Compilar DynamoRevit a partir del código fuente 

Los archivos de código fuente de DynamoRevit también están alojados en el GitHub de DynamoDS para que los desarrolladores puedan realizar contribuciones y compilar versiones beta. Por lo general, la compilación de DynamoRevit a partir del código fuente sigue el mismo proceso que Dynamo, con la excepción de algunos detalles importantes, como los siguientes:

* DynamoRevit hace referencia a los montajes de Dynamo, por lo que estos deben crearse con los paquetes NuGet correspondientes. Por ejemplo, DynamoRevit 2.x no se cargará en Dynamo 1.3.
* DynamoRevit es específico de las versiones de Revit; por ejemplo, la ramificación de DynamoRevit 2018 debe ejecutarse en Revit 2018.

Para esta guía, utilizaremos lo siguiente:

* Revit 2023
* La última versión de DynamoRevit en la ramificación `Revit2023`
* La compilación más reciente de Dynamo

Para garantizar una compilación correcta, clonaremos y compilaremos los repositorios de Dynamo y DynamoRevit para utilizarlos en esta guía.

_Nota: Solo es necesario compilar Dynamo manualmente antes de DynamoRevit si compila Dynamo 1.x y DynamoRevit 1.x; las versiones más recientes del repositorio de DynamoRevit dependen del administrador de paquetes NuGet para las dependencias de Dynamo necesarias para la compilación. Aunque, para la compilación de DynamoRevit 2.x, no es necesario extraer Dynamo manualmente, seguirá necesitando los archivos `dlls` de Core en alguna otra ubicación para ejecutar el `addin` DynamoRevit, por lo que vale la pena extraer y compilar Dynamo de todos modos. Consulte más información a continuación en_ [_Compilación del repositorio mediante Visual Studio_](#building-the-repository-using-Visual-Studio).

#### Ubicación del repositorio de DynamoRevit en GitHub <a href="#locating-the-dynamorevit-repository-on-github" id="locating-the-dynamorevit-repository-on-github"></a>

El código del proyecto DynamoRevit se encuentra en un repositorio de GitHub distinto al del código fuente principal de Dynamo. Este repositorio contiene los archivos de código fuente de los nodos específicos de Revit y el complemento de Revit que carga Dynamo. Las compilaciones de DynamoRevit para diferentes versiones de Revit (por ejemplo, 2016, 2017 o 2018) se organizan como ramificaciones en el repositorio.

El código fuente de DynamoRevit se encuentra aquí: [https://github.com/DynamoDS/DynamoRevit](https://github.com/DynamoDS/DynamoRevit).

![DynamoRevit en GitHub](images/github-dynamorevit.jpg)

> 1. Clone o descargue el repositorio.
> 2. Las ramificaciones de DynamoRevit hacen referencia a las versiones de Revit.

#### Clonación del repositorio mediante git <a href="#cloning-the-repository-using-git" id="cloning-the-repository-using-git"></a>

En un proceso similar al de extracción del repositorio de Dynamo, utilizaremos el comando "git clone" para clonar DynamoRevit y especificar la ramificación que coincida con nuestra versión de Revit. Para empezar, abriremos una interfaz de línea de comando y estableceremos el directorio actual en la ubicación en la que deseamos clonar los archivos.

`cd C:\Users\username\Documents\GitHub` cambia el directorio actual.

> Sustituya `username` por su nombre de usuario.

![Interfaz de línea de comando](images/cli-cd-revit.jpg)

Ahora podemos clonar el repositorio en este directorio. Aunque necesitaremos especificar una ramificación del repositorio, podemos cambiar a ella después de la clonación.

`git clone https://github.com/DynamoDS/DynamoRevit.git` clona el repositorio desde una dirección URL remota y cambia por defecto a la ramificación principal.

![Interfaz de línea de comando después de clonar el repositorio](images/cli-clone-revit.jpg)

Una vez que el repositorio haya terminado de clonarse, cambie el directorio actual a la carpeta del repositorio y pase a la ramificación que coincida con la versión instalada de Revit. En este ejemplo, se utiliza Revit RC2.13.1_Revit2023. Todas las ramificaciones remotas se pueden ver en el menú desplegable de ramificaciones de la página de GitHub.

`cd C:\Users\username\Documents\GitHub\DynamoRevit` cambia el directorio a DynamoRevit.\
 `git checkout RC2.13.1_Revit2023` establece la ramificación actual en `RC2.13.1_Revit2023`.\
 `git branch` comprueba en que ramificación se encuentra y muestra las demás que existen localmente.

![Directorio cambiado a una ramificación](images/cli-branch-revit.jpg)

> La ramificación marcada con un asterisco es la que se ha restaurado en ese momento. Se muestra la ramificación `Revit2018` porque se ha restaurado anteriormente, por lo que existe localmente.

Es importante elegir la ramificación correcta del repositorio para asegurarse de que cuando el proyecto se compile en Visual Studio, este hará referencia a los montajes de la versión correcta del directorio de instalación de Revit, en concreto, a `RevitAPI.dll` y `RevitAPIUI.dll`.

#### Compilación del repositorio mediante Visual Studio <a href="#building-dynamo-revit" id="building-dynamo-revit"></a>

Antes de crear el repositorio, necesitaremos restablecer los paquetes NuGet con el archivo `restorepackages.bat` ubicado en la carpeta `src`. Este archivo bat utiliza el administrador de paquetes [NuGet](https://www.nuget.org) para extraer los archivos binarios de Dynamo Core compilados necesarios para DynamoRevit. También puede optar por compilarlos manualmente, pero solo si realiza cambios en DynamoRevit y no en Dynamo Core. Esto agiliza el proceso de inicio. Asegúrese de ejecutar este archivo como administrador.

![Ejecutar como administrador](images/fe-restorepackages.jpg)

> 1. Haga clic con el botón derecho en `restorepackages.bat` y seleccione `Run as administrator`.

Si los paquetes se restablecen correctamente, se añadirá una carpeta `packages` a la carpeta `src`con los últimos paquetes NuGet beta.

![Los paquetes NuGet beta más recientes de Dynamo](images/fe-packages.jpg)

> 1. Los paquetes NuGet beta más recientes de Dynamo

Con los paquetes restablecidos, abra el archivo de la solución de Visual Studio `DynamoRevit.All.sln` en `src` y compile la solución. Es posible que la compilación no pueda encontrar inicialmente el archivo `AssemblySharedInfo.cs`. Si es así, al volver a ejecutar la compilación, se resuelve este problema.

![Compilación de una solución](images/vs-build-dynamorevit.jpg)

> 1. Seleccione `Build > Build Solution`.
> 2. Compruebe que la compilación se haya realizado correctamente en la ventana de salida. Debería aparecer el siguiente mensaje. `===== Build: 13 succeeded, 0 failed, 0 up-to-date, 0 skipped =====`.

#### Ejecución de una compilación local de DynamoRevit en Revit <a href="#running-a-local-build-of-dynamorevit-in-revit" id="running-a-local-build-of-dynamorevit-in-revit"></a>

Revit requiere un archivo de complemento para reconocer DynamoRevit, algo que el [instalador](http://dynamobim.org/download/) creará automáticamente. Durante el desarrollo, debemos crear manualmente un archivo de complemento que señale a la compilación de DynamoRevit que deseamos utilizar, en concreto, el montaje `DynamoRevitDS.dll`. También es necesario señalar DynamoRevit a una compilación de Dynamo.

Cree un archivo `Dynamo.addin` en la carpeta de complementos de Revit, que se encuentra en `C:\ProgramData\Autodesk\Revit\Addins\2023`. Ya hay instalada una versión de DynamoRevit, por lo que modificaremos el archivo existente para que señale a la nueva compilación.

```
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<RevitAddIns>
<AddIn Type="Application">
<Name>Dynamo For Revit</Name>
<Assembly>"C:\Users\username\Documents\GitHub\DynamoRevit\bin\AnyCPU\Debug\Revit\DynamoRevitDS.dll"</Assembly>
<AddInId>8D83C886-B739-4ACD-A9DB-1BC78F315B2B</AddInId>
<FullClassName>Dynamo.Applications.DynamoRevitApp</FullClassName>
<VendorId>ADSK</VendorId>
<VendorDescription>Dynamo</VendorDescription>
</AddIn>
</RevitAddIns>
```

* Especifique la ruta de archivo de `DynamoRevitDS.dll` dentro de `<Assembly>...</Assembly>`.

También podemos conseguir que el complemento cargue el selector de versión en lugar de un montaje específico.

```
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<RevitAddIns>
<AddIn Type="Application">
<Name>Dynamo For Revit</Name>
<Assembly>"C:\Users\username\Documents\GitHub\DynamoRevit\bin\AnyCPU\Debug\Revit\DynamoRevitVersionSelector.dll"</Assembly>
<AddInId>8D83C886-B739-4ACD-A9DB-1BC78F315B2B</AddInId>
<FullClassName>Dynamo.Applications.VersionLoader</FullClassName>
<VendorId>ADSK</VendorId>
<VendorDescription>Dynamo</VendorDescription>
</AddIn>
</RevitAddIns>
```

* Establezca la ruta de archivo `<Assembly>...</Assembly>` en `DynamoRevitVersionSelector.dll`.
* `<FullClassName>...</FullClassName>` especifica la clase para la que se crea una instancia desde el montaje al que se ha señalado con la ruta del elemento de montaje anterior. Esta clase será el punto de entrada del complemento.

Además, es necesario eliminar la instancia de Dynamo existente que se suministra con Revit. Para ello, vaya a `C:\\Program Files\Autodesk\Revit 2023\AddIns ` y elimine las dos carpetas que contienen **Dynamo**, `DynamoForRevit` y `DynamoPlayerForRevit`. Puede suprimirlas o realizar una copia de seguridad de ellas en una carpeta independiente si necesita recuperar el archivo original de Dynamo para Revit.

![Carpetas DynamoForRevit y DynamoPlayerforRevit](images/fe-dynamo-folders-remove.jpg)

El segundo paso consiste en añadir una ruta de archivo para los montajes de Dynamo Core al archivo `Dynamo.config` de la carpeta `bin` de DynamoRevit. DynamoRevit los cargará cuando se abra el complemento en Revit. Este archivo de configuración permite señalar el complemento DynamoRevit a diferentes versiones de Dynamo Core para desarrollar y probar cambios tanto en Core como en DynamoRevit.

El código debe presentar un aspecto similar al siguiente.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <appSettings>
     <add key="DynamoRuntime" value="C:\Users\username\Documents\GitHub\Dynamo\bin\AnyCPU\Debug"/>
  </appSettings>
</configuration>
```

* Añada la ruta de directorio de la carpeta `bin` a `<add key/>`.

> Hemos clonado y compilado Dynamo justo antes de esta guía para asegurarnos de que funcionará correctamente con DynamoRevit. La ruta de directorio señala a esta compilación.

Ahora, al abrir Revit, debería haber un complemento de Dynamo ubicado en la ficha Gestionar.

![El complemento de Dynamo se encuentra en la ficha Gestionar](images/revit-dynamo.jpg).

> 1. Seleccione `Manage`.
> 2. Haga clic en el icono del complemento de Dynamo.
> 3. Un ejemplar de DynamoRevit.

Si aparece un cuadro de diálogo de error que muestra los montajes que faltan, es probable que haya una discrepancia entre las versiones de DynamoCore que ha creado y las que está cargando en el tiempo de ejecución. Por ejemplo, DynamoRevit con los paquetes beta 2.0 más recientes de DynamoCore no funcionará si intenta iniciarlo con los archivos dll de Dynamo 1.3. Asegúrese de que ambos repositorios presenten la misma versión y de que DynamoRevit utilice una versión coincidente de las dependencias de NuGet. Estos se definen en el archivo `package.json` del repositorio de DynamoRevit.

#### Depuración de DynamoRevit mediante Visual Studio <a href="#debugging-dynamorevit-using-visual-studio" id="debugging-dynamorevit-using-visual-studio"></a>

En la sección anterior, **Compilar Dynamo a partir del código fuente**, se presentó brevemente la depuración en Visual Studio y cómo enlazar Visual Studio a un proceso. Mediante una excepción en el nodo Wall.ByCurveAndHeight como ejemplo, veremos cómo enlazar a un proceso, establecer puntos de interrupción, recorrer el código y utilizar la pila de llamadas para determinar el origen de la excepción. Estas herramientas de depuración se aplican generalmente a los flujos de trabajo de desarrollo de .net y vale la pena explorarlas fuera de esta guía.

* La opción **Asociar al proceso** vincula una aplicación en ejecución a Visual Studio para la depuración. Si deseamos depurar el comportamiento que se produce en una compilación de DynamoRevit, podemos abrir los archivos de código fuente de DynamoRevit en Visual Studio y asociar el proceso `Revit.exe`, que es el proceso principal del complemento DynamoRevit. Visual Studio utiliza un [archivo de símbolos](https://msdn.microsoft.com/en-us/library/ms241613.aspx) (`.pbd`) para establecer la conexión entre los montajes que ejecute DynamoRevit y el código fuente.
* Los **puntos de interrupción** establecen líneas en el código fuente donde la aplicación realizará una pausa antes de ejecutarse. Si un nodo provoca que DynamoRevit se bloquee o devuelva un resultado inesperado, podemos añadir un punto de interrupción al código fuente del nodo para pausar el proceso, entrar en el código e inspeccionar los valores activos de las variables hasta que encontremos la raíz del problema
* Al **recorrer el código**, se revisa este línea a línea. Se pueden ejecutar funciones de una en una, entrar en una llamada a función o salir de la función que estamos ejecutando.
*   La **pila de llamadas** muestra la función que un proceso está ejecutando en relación con las llamadas a funciones anteriores que invocaron esta llamada a función. Visual Studio cuenta con una ventana de pila de llamadas para mostrar esto. Por ejemplo, si recibimos una excepción fuera del código fuente, podemos ver la ruta al código de llamada en la pila de llamadas.

    > La guía [2,000 Things You Should Know About C#](https://csharp.2000things.com/2013/05/20/847-how-the-call-stack-works/) ofrece una explicación más detallada de las pilas de llamadas.

El nodo **Wall.ByCurveAndHeight** genera una excepción cuando se le asigna un valor de PolyCurve como su entrada de curva con el mensaje: _"A BSPlineCurve no implementado"_. Con la depuración, podemos averiguar por qué el nodo no acepta este tipo de geometría como entrada para el parámetro de curva. En este ejemplo, se supone que DynamoRevit se ha generado correctamente y se puede ejecutar como complemento de Revit.

![Excepción generada en el nodo Wall.ByCurveAndHeight](images/dyn-wallbycurveandheight.jpg)

> 1. Excepción generada en el nodo Wall.ByCurveAndHeight

Abra primero el archivo de la solución `DynamoRevit.All.sln`, inicie Revit e inicie el complemento DynamoRevit. A continuación, enlace Visual Studio al proceso de Revit con la ventana `Attach to Process`.

![Ventana Asociar al proceso](images/vs-debug-attachprocess.jpg)

> Revit y DynamoRevit deben estar en ejecución para mostrarse como un proceso disponible.
>
> 1. Abra la ventana `Attach to Process`. Para ello, seleccione `Debug > Attach to Process...`.
> 2. Establezca `Transport` en `Default`.
> 3. Seleccione `Revit.exe`.
> 4. Seleccione `Attach`.

Con Visual Studio enlazado a Revit, abra el código fuente de Wall.ByCurveAndHeight en `Wall.cs`. Podemos encontrarlo en el Explorador de soluciones, en `Libraries > RevitNodes > Elements`, en la región `Public static constructors` del archivo. Establezca un punto de interrupción en el constructor del tipo de muro para que, cuando se ejecute el nodo en Dynamo, el proceso se interrumpa y se pueda recorrer cada línea de código de forma individual. Por lo general, los constructores de tipo Zero-Touch de Dynamo comienzan por `By<parameters>`.

![Establecimiento de un punto de interrupción](images/vs-debugging-breakpoint.jpg)

> 1. El archivo de clase con el constructor de Wall.ByCurveAndHeight.
> 2. Establezca un punto de interrupción. Para ello, haga clic a la izquierda del número de línea o haga clic con el botón derecho en la línea de código y seleccione `Breakpoint > Insert Breakpoint`.

Con el punto de interrupción establecido, es necesario que el proceso se ejecute a través de la función Wall.ByCurveAndHeight. La función se puede volver a ejecutar en Dynamo. Para ello, es necesario volver a conectar un cable a uno de sus puertos, lo que obligará al nodo a ejecutarse de nuevo. El punto de interrupción se activará en Visual Studio.

![Activación de puntos de interrupción en Visual Studio](images/vs-breakpoint.jpg)

> 1. El icono de punto de interrupción cambia cuando se activa
> 2. La ventana de pila de llamadas que muestra el método que se activará a continuación

Ahora recorra cada línea del constructor hasta que se alcance la excepción. El código resaltado en amarillo es la siguiente instrucción que se ejecutará.

![Recorrido en Visual Studio](images/vs-stepover.jpg)

> 1. Las herramientas de depuración para desplazarse por el código.
> 2. Pulse `Step Over` para ejecutar el código resaltado y, a continuación, suspender la ejecución tras la devolución de la función.
> 3. La siguiente instrucción que se ejecutará se indica mediante el resaltado amarillo y la flecha.

Si seguimos avanzando por la función, se producirá la excepción que se muestra en la ventana de DynamoRevit. En la ventana de pila de llamadas, se puede ver que la excepción se produjo originalmente desde un método denominado `Autodesk.Revit.CurveAPIUtils.CreateNurbsCurve`. Afortunadamente, la excepción se ha solucionado aquí, por lo que no se ha bloqueado Dynamo. El proceso de depuración ha proporcionado contexto para el problema al llevarnos a otro método en el código fuente.

Dado que no se trata de una biblioteca de código abierto, no podemos realizar cambios en ella; ahora que hay más información, se puede notificar el problema con más contexto mediante la presentación de una [incidencia](https://guides.github.com/features/issues/) en GitHub o se puede proponer una solución para este problema mediante una solicitud de incorporación de cambios.

![Excepción en Visual Studio](images/vs-exception.jpg)

> 1. Cuando se llega a la instrucción que provoca la excepción en `Walls.cs`, el proceso de depuración nos acerca lo máximo posible a la raíz del problema en el código de usuario dentro de `ProtoToRevitCurve.cs`.
> 2. La instrucción que provoca la excepción en `ProtoToRevitCurve.cs`.
> 3. En la pila de llamadas, se puede ver que la excepción procede de código que no es de usuario.
> 4. Una ventana emergente que proporciona información sobre la excepción.

Este proceso se puede aplicar a cualquier archivo de código fuente con el que se trabaje. Al desarrollar una biblioteca de nodos Zero-Touch para Dynamo Studio, podemos abrir el código fuente de la biblioteca y asociar un proceso de Dynamo para depurar la biblioteca de nodos. Aunque todo funcione perfectamente, la depuración es una excelente forma de explorar el código y descubrir el funcionamiento de los componentes.

#### Extraer la última compilación <a href="#pull-latest-build" id="pull-latest-build"></a>

Este proceso es casi idéntico al de extracción de cambios para Dynamo, excepto en que tendremos que asegurarnos de que estamos en la ramificación correcta. Utilice el comando `git branch` en el repositorio de DynamoRevit para ver las ramificaciones que están disponibles localmente y las que se han restaurado.

`cd C:\Users\username\Documents\GitHub\DynamoRevit` establece el directorio actual en el repositorio de DynamoRevit.\
 `git branch` comprueba que nos encontramos en la ramificación correcta, `RC2.13.1_Revit2023`.\
 `git pull origin RC2.13.1_Revit2023` extrae los cambios de la ramificación `RC2.13.1_Revit2023` de origen remoto.

El origen simplemente señala a la dirección URL original que hemos clonado.

![Configuración de un directorio en la interfaz de línea de comando](images/cli-pull-revit.jpg)

> Deseamos ser conscientes aquí de en qué ramificación nos encontramos y de cuál vamos a obtener datos, por ejemplo, para evitar extraer cambios de `RC2.13.1_Revit2023` e incorporarlos en `Revit2018`.

Como se ha mencionado en **Compilar Dynamo a partir del código fuente**, cuando estemos listos para enviar un cambio al repositorio de DynamoRevit, podemos crear una solicitud de incorporación de cambios siguiendo las directrices del equipo de Dynamo establecidas en la sección de este tipo de solicitudes.
