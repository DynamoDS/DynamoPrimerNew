# 软件包形式的扩展

### 软件包形式的扩展 <a href="#extensions-as-packages" id="extensions-as-packages"></a>

### 概述 <a href="#overview" id="overview"></a>

Dynamo 扩展可以像常规 Dynamo 节点库一样展开到软件包管理器。如果安装的软件包包含视图扩展，则会在 Dynamo 载入时的运行时载入扩展。可以查看 Dynamo 控制台，以确认扩展是否已正确载入。

### 软件包结构 <a href="#package-structure" id="package-structure"></a>

扩展包的结构与普通软件包相同，其中包含...

```
C:\Users\User\AppData\Roaming\Dynamo\Dynamo Core\2.1\packages\Sample View Extension
│   pkg.json
├───bin
│       SampleViewExtension.dll
├───dyf
└───extra
        SampleViewExtension_ViewExtensionDefinition.xml
```

假定您已构建扩展，您将（至少）有一个 .NET 程序集和一个清单文件。该程序集应包含实现 `IViewExtension` 或 `IExtension` 的类。清单 .XML 文件会告知 Dynamo 要实例化哪个类才能启动扩展。为了使软件包管理器能够正确定位扩展，清单文件应准确对应于程序集位置和命名。

将任何程序集文件放置在 `bin` 文件夹中，并将清单文件放置在 `extra` 文件夹中。任何其他资源也可以放置在此文件夹中。

清单 .XML 文件示例：

```
<ViewExtensionDefinition>
  <AssemblyPath>..\bin\MyViewExtension.dll</AssemblyPath>
  <TypeName>MyViewExtension.MyViewExtension</TypeName>
</ViewExtensionDefinition>
```

### 上传 <a href="#uploading" id="uploading"></a>

在您有包含上述子目录的文件夹后，即可将其推送（上传）到软件包管理器。需要注意的一点是，您当前无法从 Dynamo 沙箱发布软件包。这意味着您需要使用 Dynamo Revit。在进入 Dynamo Revit 后，导航到“软件包”=>“发布新软件包”。这将提示用户登录到其要与软件包关联的 Autodesk 帐户。

此时，您应该位于常规发布软件包窗口，在该窗口中将输入有关软件包/扩展的所有必填字段。还有一个**非常重要**的附加步骤，要求您确保没有任何程序集文件标记为节点库。为此，请在已输入的文件（在上面创建的软件包文件夹）上单击鼠标右键。一个上下文菜单即会显示，让您可以选中（或取消选中）此选项。应取消选中所有扩展程序集。

![发布软件包](images/ViewExtension_Search.png)

在公开发布之前，应始终在本地发布，以确保一切正常。在确认一切正常后，即可通过选择“发布”来联机发布。

### 拉取 <a href="#pulling" id="pulling"></a>

要确认软件包是否已成功上传，您应该能够根据在发布步骤中指定的命名和关键字搜索到相应软件包。最后，请务必注意，相同的扩展需要重新启动 Dynamo，然后才能正常运行。通常，这些扩展需要在 Dynamo 启动时指定的参数。

![搜索软件包](images/ViewExtension_Search.jpg)
