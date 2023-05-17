# Создание пакета из Visual Studio

При разработке сборок для публикации в виде пакета для Dynamo можно настроить проект таким образом, чтобы сгруппировать все необходимые компоненты и поместить их в структуру папок, совместимую с пакетом. Это позволит быстро протестировать проект в виде пакета и смоделировать работу пользователя.

#### Сборка в папку пакета <a href="#how-to-build-directly-to-the-package-folder" id="how-to-build-directly-to-the-package-folder"></a>

Существует два способа сборки пакета в Visual Studio.

* Добавьте события после сборки в диалоговом окне «Параметры проекта», в котором для копирования необходимых файлов используются сценарии xcopy или Python.
* Используйте целевой объект сборки AfterBuild в файле `.csproj` для создания задач копирования файлов и каталогов.

Метод AfterBuild предпочтителен для этих типов операций (в том числе в данном руководстве), так как он не зависит от копирования файлов, которые могут быть недоступны на компьютере сборки.

#### Копирование файлов пакетов с помощью метода AfterBuild <a href="#copy-package-files-with-the-afterbuild-method" id="copy-package-files-with-the-afterbuild-method"></a>

Настройте структуру папок в репозитории так, чтобы исходные файлы были отделены от файлов пакетов. При работе с примером CustomNodeModel поместите проект Visual Studio и все связанные файлы в новую папку `src`. Все пакеты, созданные в проекте, будут сохранены в этой папке. Структура папок должна выглядеть следующим образом:

```
CustomNodeModel
> src
  > CustomNodeModel
  > CustomNodeModelFunction
  > packages
  > CustomNodeModel.sln
```

![Перемещение файлов проекта](images/fe-proj-directory.jpg)

> 1. Переместите файлы проекта в новую папку `src`.

Теперь, когда исходные файлы находятся в отдельной папке, добавьте целевой объект `AfterBuild` в файл `CustomNodeModel.csproj` в Visual Studio. При этом необходимые файлы будут скопированы в новую папку пакета. Откройте файл `CustomNodeModel.csproj` в текстовом редакторе (мы использовали [Atom](https://atom.io)) и поместите цель сборки перед закрывающим тегом `</Project>`. Целевой объект AfterBuild скопирует все файлы DLL, PBD, XML и CONFIG в новую папку bin и создаст папку dyf и дополнительные папки.

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

![Размещение целевого объекта AfterBuild](images/atom-afterbuild.jpg)

> Необходимо убедиться, что целевой объект добавлен в файл `CustomNodeModel.csproj` (а не в другой файл проекта) и что в проекте нет параметров, заданных после сборки.
>
> 1. Поместите целевой объект AfterBuild перед закрывающим тегом `</Project>`.

В разделе `<ItemGroup>` задается ряд переменных, представляющих определенные типы файлов. Например, переменная `Dll` представляет все файлы в выходной папке с расширением `.dll`.

```
<ItemGroup>
  <Dlls Include="$(OutDir)*.dll" />
</ItemGroup>
```

Задача `Copy` состоит в том, чтобы скопировать все файлы `.dll` в каталог, а именно в папку пакета, в которую мы выполняем сборку.

```
<Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
```

Пакеты Dynamo обычно содержат папку `dyf` и `extra` для пользовательских узлов Dynamo и других компонентов, например изображений. Чтобы создать эти папки, необходимо использовать задачу `MakeDir`. Если папка отсутствует, эта задача создаст ее. В эту папку можно добавить файлы вручную.

```
<MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
```

Теперь при построении проекта в папке проекта будет находится папка `packages` рядом с ранее созданной папкой `src`. В каталоге `packages` находится папка, содержащая все необходимые для пакета компоненты. Также необходимо скопировать файл `pkg.json` в папку пакета, чтобы сообщить Dynamo о необходимости загрузки пакета.

![Копирование файлов](images/fe-proj-directory-package.jpg)

> 1. Новая папка пакетов, созданная целевым объектом AfterBuild.
> 2. Существующая папка src с проектом.
> 3. Папки `dyf` и `extra`, созданные из целевого объекта AfterBuild.
> 4. Скопируйте файл `pkg.json` вручную.

Теперь можно опубликовать пакет с помощью диспетчера пакетов Dynamo или скопировать его непосредственно в папку пакетов Dynamo: `<user>\AppData\Roaming\Dynamo\1.3\packages`.
