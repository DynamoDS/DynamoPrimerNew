# Compilar un paquete desde Visual Studio 

Si está desarrollando montajes para publicarlos como un paquete para Dynamo, el proyecto se puede configurar para agrupar todos los componentes necesarios y colocarlos en una estructura de directorios compatible con paquetes. Esto permitirá probar rápidamente el proyecto como un paquete y simular la experiencia de un usuario.

#### Cómo compilar directamente en la carpeta de paquetes <a href="#how-to-build-directly-to-the-package-folder" id="how-to-build-directly-to-the-package-folder"></a>

Existen dos métodos para compilar un paquete en Visual Studio:

* Añada eventos posteriores a la compilación a través del cuadro de diálogo Configuración del proyecto que utilizan secuencias de comando de xcopy o Python para copiar los archivos necesarios.
* Utilice el destino de compilación AfterBuild del archivo `.csproj` para crear tareas de copia de archivos y directorios.

AfterBuild es el método preferido para estos tipos de operaciones (y el que se describe en esta guía) ya que no se basa en la copia de archivos, que puede no estar disponible en el equipo de compilación.

#### Copiar archivos de paquetes con el método AfterBuild <a href="#copy-package-files-with-the-afterbuild-method" id="copy-package-files-with-the-afterbuild-method"></a>

Configure la estructura de directorios en el repositorio de forma que los archivos de código fuente sean independientes de los archivos de paquetes. Al trabajar con el caso real CustomNodeModel, incluya el proyecto de Visual Studio y todos los archivos asociados en una nueva carpeta `src`. Almacenará todos los paquetes generados por el proyecto en esta carpeta. La estructura de carpetas debería presentar el siguiente aspecto:

```
CustomNodeModel
> src
  > CustomNodeModel
  > CustomNodeModelFunction
  > packages
  > CustomNodeModel.sln
```

![Desplazamiento de archivos de proyecto](images/fe-proj-directory.jpg)

> 1. Desplace los archivos de proyecto a la nueva carpeta `src`.

Ahora que los archivos de código fuente se encuentran en una carpeta independiente, añada un destino `AfterBuild` al archivo `CustomNodeModel.csproj` en Visual Studio. Esta acción debería copiar los archivos necesarios en una nueva carpeta de paquetes. Abra el archivo `CustomNodeModel.csproj` en un editor de texto (hemos utilizado [Atom](https://atom.io)) y coloque el destino de compilación antes de la etiqueta `</Project>` de cierre. Este destino AfterBuild copiará todos los archivos .dll, .pbd, .xml y .config en una nueva carpeta bin y creará una carpeta dyf y otras adicionales.

```
  <Target Name="AfterBuild">
    <ItemGroup>
      <Dlls Include="$(OutDir)*.dll" />
      <Pdbs Include="$(OutDir)*.pdb" />
      <Xmls Include="$(OutDir)*.xml" />
      <Configs Include="$(OutDir)*.config" />
    </ItemGroup>
    <Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <Copy SourceFiles="@(Pdbs)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <Copy SourceFiles="@(Xmls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <Copy SourceFiles="@(Configs)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\dyf" />
    <MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
  </Target>
```

![Colocación del destino AfterBuild](images/atom-afterbuild.jpg)

> Deberá asegurarse de que el destino se haya añadido al archivo `CustomNodeModel.csproj` (no a otro archivo de proyecto) y de que el proyecto no presente ninguna configuración posterior a la compilación.
>
> 1. Coloque el destino AfterBuild antes de la etiqueta `</Project>` final.

En la sección `<ItemGroup>`, se definen varias variables para representar tipos de archivo específicos. Por ejemplo, la variable `Dll` representa todos los archivos del directorio de salida cuya extensión es `.dll`.

```
<ItemGroup>
  <Dlls Include="$(OutDir)*.dll" />
</ItemGroup>
```

La tarea `Copy` consiste en copiar todos los archivos `.dll` en un directorio, en concreto, la carpeta de paquetes en la que se está realizando la compilación.

```
<Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
```

Los paquetes de Dynamo suelen incluir las carpetas `dyf` y `extra` para los nodos personalizados de Dynamo y otros componentes como imágenes. Para crear estas carpetas, debemos utilizar una tarea `MakeDir`. Esta creará una carpeta si no existe. Puede añadir archivos manualmente a esta carpeta.

```
<MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
```

Si compila el proyecto, su carpeta debería incluir ahora una carpeta `packages` junto a la carpeta `src` creada anteriormente. En el directorio `packages`, hay una carpeta que contiene todo lo necesario para el paquete. También es necesario copiar el archivo `pkg.json` en la carpeta de paquetes para que Dynamo sepa cómo cargar el paquete.

![Copiar archivos](images/fe-proj-directory-package.jpg)

> 1. La nueva carpeta de paquetes creada por el destino AfterBuild.
> 2. La carpeta src existente con el proyecto.
> 3. Las carpetas `dyf` y `extra` creadas a partir del destino AfterBuild.
> 4. Copie manualmente el archivo `pkg.json`.

Ahora puede publicar el paquete con la herramienta Package Manager de Dynamo o copiarlo directamente en el directorio de paquetes de Dynamo, `<user>\AppData\Roaming\Dynamo\1.3\packages`.
