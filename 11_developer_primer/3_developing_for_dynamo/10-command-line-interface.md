# Interfaz de línea de comandos de Dynamo

***
      -o, -O, --OpenFilePath        Instruct Dynamo to open a command file and run the commands it contains at 
                                    this path, this option is only supported when run from DynamoSandbox
***
      -c, -C, --CommandFilePath     Instruct Dynamo to open a command file and run the commands it contains at 
                                    this path, this option is only supported when run from DynamoSandbox                      
***
      -v, -V, --Verbose             Instruct Dynamo to output all evaluations it performs to an XML file at this path
***                                        
      -g, -G, --Geometry            Instruct Dynamo to output geometry from all evaluations to a JSON file at this path
***
      -h, -H, --help                Get some help
***
      -i, -I, --Import              Instruct Dynamo to import an assembly as a node library. This argument should be a 
                                    file path to a single.dll - if you wish to import multiple dlls - list the dlls 
                                    separated by a space: -i 'assembly1.dll' 'assembly2.dll'
***
      --GeometryPath                Relative or absolute path to a directory containing ASM. When supplied, instead of 
                                    searching the hard disk for ASM, it will be loaded directly from this path
***
      -k, -K, --KeepAlive           Keepalive mode, leave the Dynamo process running until a loaded extension shuts it 
                                    down
***
      --HostName                    Identify Dynamo variation associated with the host
***
      -s, -S, --SessionId           Identify Dynamo host analytics session id
***
      -p, -P, --ParentId            Identify Dynamo host analytics parent id
***
      -x, -X, --ConvertFile         When used in combination with the 'O' flag, opens a .dyn file from the specified 
                                    path and converts it to .json. The file will have the .json extension and be 
                                    located in the same directory as the original file
***
      -n, -N, --NoConsole           Don't rely on the console window to interact with CLI in Keepalive mode
***
      -u, -U  --UserData            Specify user data folder to be used by PathResolver with CLI
***
      --CommonData                  Specify common data folder to be used by PathResolver with CLI
***
      --DisableAnalytics            Disables analytics in Dynamo for the process lifetime
***
      --CERLocation                 Specify the crash error report tool located on the disk
***
      --ServiceMode                 Specify the service mode startup


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
 
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
 
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
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
```
`-v` puede utilizarse cuando Dynamo se ejecuta en el modo sin interfaz gráfica ("headless") cuando se ha utilizado `-o` para abrir un archivo; este indicador iterará todos los nodos del gráfico y volcará sus valores de salida a un simple archivo XML. Como el indicador `--ServiceMode` puede forzar a Dynamo a ejecutar varias evaluaciones de gráficos, el archivo de salida contendrá los valores de cada evaluación que se produzca.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"
```
        
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
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"
```  
El archivo de geometría JSON presentaría el siguiente formato:
```
 TBD - Work in progress
```
`-h` permite obtener una lista de las posibles opciones.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h
```
El indicador -i puede utilizarse varias veces para importar varios montajes que el gráfico que está intentando abrir requiere para ejecutarse.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"
```

El indicador -l puede utilizarse para ejecutar Dynamo con una configuración regional diferente. Sin embargo, por lo general, la configuración regional no afecta a los resultados del gráfico.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"
```

El indicador --GeometryPath puede utilizarse para dirigir DynamoSandbox o la CLI a un conjunto específico de archivos binarios ASM. Utilícelo de la siguiente forma:
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```

O
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```
El indicador -k permite mantener el proceso de Dynamo en ejecución hasta que una extensión lo cierre.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k
```
El indicador --HostName puede utilizarse para identificar la variación de Dynamo asociada al anfitrión.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"
```
O
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"
```
El indicador -s permite identificar el ID de la sesión de análisis del anfitrión de Dynamo.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]
```
El indicador -p permite identificar el ID principal de análisis del anfitrión de Dynamo.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"