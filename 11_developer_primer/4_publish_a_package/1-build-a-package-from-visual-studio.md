# 从 Visual Studio 构建软件包 

如果要开发要发布为 Dynamo 软件包的程序集，可以将项目配置为对所有必需资源进行分组，并将其放置在与软件包兼容的目录结构中。这将使项目能够作为软件包快速进行测试，并模拟用户体验。

#### 如何直接构建到软件包文件夹 <a href="#how-to-build-directly-to-the-package-folder" id="how-to-build-directly-to-the-package-folder"></a>

在 Visual Studio 中构建软件包有两种方法：

* 通过“项目设置”对话框，添加使用 xcopy 或 Python 脚本复制必需文件的构建后事件
* 在 `.csproj` 文件中，使用“AfterBuild”构建目标来创建文件和目录复制任务

由于“AfterBuild”不依赖于可能在构建计算机上不可用的文件复制操作，因此“AfterBuild”是这些类型操作（以及本手册中介绍的操作）的首选方法。

#### 使用 AfterBuild 方法复制软件包文件 <a href="#copy-package-files-with-the-afterbuild-method" id="copy-package-files-with-the-afterbuild-method"></a>

在储存库中设置目录结构，以使源文件与软件包文件分开。使用 CustomNodeModel 案例研究时，将 Visual Studio 项目和所有关联文件放置到新的 `src` 文件夹中。您会将项目生成的所有软件包都存储在此文件夹中。文件夹结构现在应如下所示：

```
CustomNodeModel
> src
  > CustomNodeModel
  > CustomNodeModelFunction
  > packages
  > CustomNodeModel.sln
```

![移动项目文件](images/fe-proj-directory.jpg)

> 1. 将项目文件移动到新的 `src` 文件夹

由于源文件位于单独的文件夹中，因此在 Visual Studio 中将 `AfterBuild` 目标添加到 `CustomNodeModel.csproj` 文件。这应该会将必需文件复制到新的软件包文件夹中。在文本编辑器（我们使用的是 [Atom](https://atom.io)）中，打开 `CustomNodeModel.csproj` 文件，然后在结束标记 `</Project>` 之前放置构建目标。此 AfterBuild 目标会将所有 .dll、.pbd、.xml 和 .config 文件复制到新的 bin 文件夹中，并创建 dyf 文件夹和 extra 文件夹。

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

![放置 AfterBuild 目标](images/atom-afterbuild.jpg)

> 我们需要确保目标已添加到 `CustomNodeModel.csproj` 文件（而非其他项目文件）中，并确保项目没有任何现有的“构建后”设置。
>
> 1. 将 AfterBuild 目标放置在结束标记 `</Project>` 之前。

在 `<ItemGroup>` 部分中，定义了许多变量来表示特定的文件类型。例如，`Dll` 变量表示输出目录中扩展名为 `.dll` 的所有文件。

```
<ItemGroup>
  <Dlls Include="$(OutDir)*.dll" />
</ItemGroup>
```

`Copy` 任务是将所有 `.dll` 文件复制到某个目录，尤其是我们要构建到的软件包文件夹。

```
<Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
```

Dynamo 软件包通常有 `dyf` 和 `extra` 文件夹，用于 Dynamo 自定义节点和其他资源（如图像）。要创建这些文件夹，我们需要使用 `MakeDir` 任务。如果某个文件夹不存在，则此任务会创建该文件夹。可以手动将文件添加到此文件夹。

```
<MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
```

如果您构建项目，则项目文件夹现在应该有 `packages` 文件夹以及之前创建的 `src` 文件夹。`packages` 目录内是一个文件夹，其中包含软件包所需的所有内容。我们还需要将 `pkg.json` 文件复制到软件包文件夹中，以使 Dynamo 知道要载入软件包。

![复制文件](images/fe-proj-directory-package.jpg)

> 1. AfterBuild 目标创建的新软件包文件夹
> 2. 项目的现有 src 文件夹
> 3. 从 AfterBuild 目标创建的 `dyf` 和 `extra` 文件夹
> 4. 手动复制 `pkg.json` 文件。

现在，可以使用 Dynamo 的软件包管理器发布软件包，也可以直接将软件包复制到 Dynamo 的软件包目录：`<user>\AppData\Roaming\Dynamo\1.3\packages`。
