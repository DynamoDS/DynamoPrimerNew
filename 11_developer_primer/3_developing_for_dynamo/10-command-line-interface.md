# Interfaz de línea de comandos de Dynamo

`-o, -O, --OpenFilePath` Indique a Dynamo que abra un archivo de comandos y ejecute los comandos que contiene en esta ruta. Solo se admite esta opción cuando se ejecuta desde DynamoSandbox.  

`-c, -C, --CommandFilePath` Indique a Dynamo que abra un archivo de comandos y ejecute los comandos que contiene en esta ruta. Solo se admite esta opción cuando se ejecuta desde DynamoSandbox.  

`-v, -V, --Verbose` Indique a Dynamo que genere todas las evaluaciones que realice en un archivo XML ubicado en la ruta especificada.  

`-g, -G, --Geometry` Indique a Dynamo que genere la geometría de todas las evaluaciones en un archivo JSON ubicado en esta ruta.  

`-h, -H, --help` Obtenga ayuda.  

`-i, -I, --Import` Indique a Dynamo que importe un ensamblaje como una biblioteca de nodos. Este argumento debe ser una ruta de archivo a un único `.dll`. Si desea importar varios `.dlls`, enumérelos separados por un espacio: `-i 'assembly1.dll' 'assembly2.dll'`.  

`--GeometryPath` Ruta relativa o absoluta a un directorio que contiene ASM. Si se especifica, en lugar de buscar ASM en la unidad de disco duro, se cargará directamente desde esta ruta.  

`-k, -K, --KeepAlive` Modo Keepalive; deje el proceso de Dynamo en funcionamiento hasta que lo cierre una extensión cargada.  

`--HostName` Identifique la variación de Dynamo asociada con el anfitrión.  

`-s, -S, --SessionId` Identifique el ID de sesión de análisis del anfitrión de Dynamo.  

`-p, -P, --ParentId` Identifique el ID principal de análisis del anfitrión de Dynamo.  

`-x, -X, --ConvertFile` Si se utiliza junto con el indicador `-O`, abre un archivo `.dyn` en la ruta especificada y lo convierte en `.json`. El archivo presentará la extensión `.json` y se ubicará en el mismo directorio que el archivo original.  

`-n, -N, --NoConsole` No utilice la ventana de la consola para interactuar con la CLI en el modo Keepalive.  

`-u, -U, --UserData` Especifique la carpeta de datos de usuario que utilizará PathResolver con la CLI.  

`--CommonData` Especifique la carpeta de datos común que utilizará PathResolver con la CLI.  

`--DisableAnalytics` Desactiva el análisis en Dynamo durante la vida útil del proceso.  

`--CERLocation` Especifique la herramienta de informes de errores de bloqueo que se encuentra en el disco.  

`--ServiceMode` Especifique el inicio del modo de servicio.  



#### ¿Por qué? 
 Es posible que desee controlar Dynamo desde la línea de comandos por diversos motivos, entre los que se incluyen: 
 
 * Automatizar muchas ejecuciones de Dynamo
 * Probar los gráficos de Dynamo (consulte también -c al utilizar Dynamo Sandbox)
 * Ejecutar una secuencia de gráficos de Dynamo en un orden específico
 * Escribir archivos por lotes que ejecuten varias líneas de comandos
 * Escribir otro programa para controlar y automatizar la ejecución de los gráficos de Dynamo y realizar operaciones interesantes con los resultados de esos cálculos

#### ¿Qué?
La interfaz de línea de comandos (DynamoCLI) es un complemento de DynamoSandbox. Se trata de una utilidad de línea de comandos para DOS/terminal diseñada para facilitar la ejecución de Dynamo mediante argumentos de línea de comandos. En su primera implementación, no se ejecuta de forma independiente, debe ejecutarse desde la carpeta donde residen los archivos binarios de Dynamo, ya que depende de los mismos DLL principales que Sandbox. No puede interoperar con otras versiones de Dynamo.

Hay cuatro formas de ejecutar CLI: desde un indicador DOS, desde archivos por lotes DOS y como un acceso directo del escritorio de Windows cuya ruta se modifica para incluir los indicadores de línea de comandos especificados. La especificación del archivo DOS puede ser completa o relativa y también se admiten las unidades asignadas y la sintaxis de URL. También puede construirse con Mono y ejecutarse en Linux o Mac desde el terminal.

Los paquetes de Dynamo son compatibles con la utilidad, pero no se pueden cargar nodos personalizados (dyf), solo gráficos independientes (dyn).

En las pruebas preliminares, la utilidad CLI admite versiones localizadas de Windows y puede especificar argumentos filespec con caracteres ASCII superiores.

Se puede acceder a CLI a través de la aplicación DynamoCLI.exe. Esta aplicación permite a un usuario o a otra aplicación interactuar con el modelo de evaluación de Dynamo mediante una llamada a DynamoCLI.exe con una cadena de comandos. Los resultados podrían presentar el siguiente aspecto:
 
 `C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`
 
Este comando le indicará a Dynamo que abra el archivo especificado en *C:\\someReallyCoolDynamoFile.Dyn* sin dibujar ninguna IU y que lo ejecute. Dynamo se cerrará cuando el gráfico haya terminado de ejecutarse. 

**Novedad en la versión 2.1**: la aplicación DynamoWPFCLI.exe. Esta aplicación admite todo lo que admite la aplicación DynamoCLI.exe, con la opción Geometría (-g) adicional. La aplicación DynamoWPFCLI.exe solo está disponible para Windows.

#### Notas importantes

* El método preferido para interactuar con DynamoCLI es a través de una interfaz de solicitud de comandos.
* En este momento, deberá ejecutar DynamoCLI desde su ubicación de instalación en la carpeta [Versión de Dynamo]. La CLI necesita acceso a los mismos archivos .dll que Dynamo, por lo que no se debe mover.
* Debería poder ejecutar gráficos que estén abiertos en ese momento en Dynamo, pero esto puede provocar efectos secundarios no deseados.
* Todas las rutas de archivos son totalmente compatibles con DOS, por lo que las rutas relativas y completas deberían funcionar, pero asegúrese de escribirlas entre comillas, *"C:ruta\\a\\archivo.dyn"*. 
* DynamoCLI es una nueva funcionalidad que actualmente está en proceso de desarrollo. Por el momento, la **CLI solo carga un subconjunto** de las bibliotecas estándar de Dynamo, por lo que tenga esto en cuenta si un gráfico no se ejecuta correctamente. Estas bibliotecas se especifican [aquí](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoApplications/PathResolvers.cs#L28). 
* Actualmente, si no se encuentran errores, la CLI simplemente se cerrará una vez completada la ejecución.
 
#### ¿Cómo?

`-o` puede abrir Dynamo señalando a un archivo .dyn en el modo sin interfaz gráfica ("headless") que ejecutará el gráfico.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`

`-v` puede utilizarse cuando Dynamo se ejecuta en el modo sin interfaz gráfica ("headless") cuando se ha utilizado `-o` para abrir un archivo; este indicador iterará todos los nodos del gráfico y volcará sus valores de salida a un simple archivo XML. Como el indicador `--ServiceMode` puede forzar a Dynamo a ejecutar varias evaluaciones de gráficos, el archivo de salida contendrá los valores de cada evaluación que se produzca.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"`

        
El archivo de salida XML presentaría el formato:
``` XML
    <evaluations>
        <evaluation0>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state1.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation0>
        <evaluation1>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state2.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation1>
    </evaluations>
```
`-g` puede utilizarse cuando Dynamo se ejecuta en el modo sin interfaz gráfica ("headless") cuando se ha utilizado `-o` para abrir un archivo; este indicador generará el gráfico y volcará la geometría resultante en un archivo JSON. 

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"`
  
El archivo de geometría JSON presentaría el siguiente formato:

 Por determinar: trabajo en curso

`-h` permite obtener una lista de las posibles opciones.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h`

El indicador -i puede utilizarse varias veces para importar varios montajes que el gráfico que está intentando abrir requiere para ejecutarse.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"`

El indicador -l puede utilizarse para ejecutar Dynamo con una configuración regional diferente. Sin embargo, por lo general, la configuración regional no afecta a los resultados del gráfico.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"`

El indicador --GeometryPath puede utilizarse para dirigir DynamoSandbox o la CLI a un conjunto específico de archivos binarios ASM. Utilícelo de la siguiente forma:

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

o

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

El indicador -k permite mantener el proceso de Dynamo en ejecución hasta que una extensión lo cierre.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k`

El indicador --HostName puede utilizarse para identificar la variación de Dynamo asociada al anfitrión.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"`

o

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"`

El indicador -s permite identificar el ID de la sesión de análisis del anfitrión de Dynamo.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]`

El indicador -p permite identificar el ID principal de análisis del anfitrión de Dynamo.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"`
