# 软件包简介

简而言之，软件包是自定义节点集。Dynamo Package Manager 是一个社区门户，可以下载已在线发布的任何软件包。这些工具集由第三方开发，以扩展 Dynamo 的核心功能，可供所有人访问，并可以通过单击相应按钮进行下载。

![Package Manager 站点](../images/6-2/1/dpm.jpg)

诸如 Dynamo 之类的开源项目通过此类社区参与有了蓬勃发展。借助专门的第三方开发人员，Dynamo 能够将其应用范围扩展到各行各业的工作流。因此，Dynamo 团队已共同努力来简化软件包的开发和发布（将在以下各节中详细讨论）。

### 安装软件包

安装软件包的最简单方法是使用 Dynamo 界面中的“软件包”工具栏。让我们直接跳转到该工具栏，然后立即安装一个软件包。在本快速示例中，我们将安装一个常用软件包，用于在栅格上创建四边形嵌板。

在 Dynamo 中，转到 _“软件包”>“搜索软件包...”_

![](../images/6-2/1/packageintroduction-installingapackage01.jpg)

在搜索栏中，我们搜索“quads from rectangular grid”。片刻之后，您应该会看到与此搜索查询匹配的所有软件包。我们想要选择具有匹配名称的第一个软件包。

单击“安装”以将此软件包添加到库中。完成！

![](../images/6-2/1/packageintroduction-installingapackage02.jpg)

请注意，现在 Dynamo 库中有另一个名为“buildz”的组。该名称指代软件包的开发人员，并且自定义节点将放置在此组中。我们可以立即开始使用此组。

![](../images/6-2/1/packageintroduction-installingapackage03.jpg)

使用 **“代码块”** 以快速定义矩形栅格、将结果输出到 **“Polygon.ByPoints”** 节点，然后输出到 **“Surface.ByPatch”** 节点以查看刚创建的矩形嵌板列表。

![](../images/6-2/1/packageintroduction-installingapackage04.jpg)

### 安装软件包文件夹 - DynamoUnfold

上述示例重点介绍内含一个自定义节点的软件包，但相同过程可用于下载内含多个自定义节点的软件包以及支持数据文件。现在，我们通过一个更全面的软件包来进行演示：Dynamo Unfold。

如上例中所示，首先选择 _“软件包”>“搜索软件包...”_。

这次，我们将搜索 _“DynamoUnfold”_ （一个字词，注意大写）。当我们看到该软件包时，请通过单击“安装”下载，以将“Dynamo Unfold”添加到 Dynamo 库中。

![](../images/6-2/1/packageintroduction-installingpackagefolder01.jpg)

在 Dynamo 库中，我们有一个 _“DynamoUnfold”_ 组，其中包含多个类别和自定义节点。

![](../images/6-2/1/packageintroduction-installingpackagefolder02.jpg)

现在，我们来看一下软件包的文件结构。首先，选择“Dynamo”>“首选项”

![](../images/6-2/1/packageintroduction-installingpackagefolder03.jpg)

在“首选项”弹出窗口中，打开“Package Manager”> 在“DynamoUnfold”旁边，选择垂直点菜单 ![](../images/6-2/1/packageintroduction-verticaldotsmenu.jpg) >“显示根目录”以打开此软件包的根文件夹。

![](../images/6-2/1/packageintroduction-installingpackagefolder04.jpg)

这会转到该软件包的根目录。请注意，我们有 3 个文件夹和一个文件。

![](../images/6-2/1/packageintroduction-installingpackagefolder05.jpg)

> 1. _“bin”_ 文件夹中存储了 .dll 文件。此 Dynamo 软件包使用 Zero-Touch 开发，因此自定义节点保存在此文件夹中。
> 2. _“dyf”_ 文件夹中存储了自定义节点。此软件包不是使用 Dynamo 自定义节点开发的，因此此软件包的该文件夹为空。
> 3. “extra”文件夹中存储了所有附加文件，包括我们的示例文件。
> 4. pkg 文件是一个基本文本文件，用于定义软件包设置。我们现在可以忽略该文件。

打开“extra”文件夹，我们会在其中看到随安装下载的一系列示例文件。并非所有软件包中都有示例文件，如果示例文件是软件包的一部分，那么就可以在该位置找到它们。

让我们打开“SphereUnfold”。

![](../images/6-2/1/rd2.jpg)

在打开该文件并点击求解器上的“运行”后，我们会得到一个展开的球体！此类示例文件有助于了解如何使用新的 Dynamo 软件包。

![](<../images/6-2/5/packageintroduction-installingpackagefolder07 (1).jpg>)

### Dynamo Package Manager

了解 Dynamo 软件包的另一种方法是在线浏览[“Dynamo Package Manager”](http://dynamopackages.com)。这是一种浏览软件包的好方法，因为存储库会按照下载数量和受欢迎程度对软件包进行排序。此外，它也是一种用于收集有关最新软件包更新信息的简便方法，因为某些 Dynamo 软件包受 Dynamo 各内部版本的版本控制和依存关系的制约。

通过在 Dynamo Package Manager 中单击 _“Quads from Rectangular Grid”_，可查看其描述、版本、开发人员以及可能的依存关系。

![](../images/6-2/1/dpm2.jpg)

还可以从 Dynamo Package Manager 下载软件包文件，但直接在 Dynamo 中进行下载是一个更无缝的过程。

### 软件包文件存储在本地何处？

如果确实要从 Dynamo 软件包管理器下载文件，或者要查看所有软件包文件的保存位置，请依次单击“Dynamo”>“Package Manager”>“节点和软件包路径”，可以从此处找到当前根文件夹目录。

![](../images/6-2/1/packageintroduction-installingpackagefolder08.jpg)

默认情况下，软件包安装在与以下文件夹路径类似的位置：_C:/Users/[用户名]/AppData/Roaming/Dynamo/[Dynamo 版本]_。

### 进一步了解软件包

Dynamo 社区不断发展壮大。通过不时浏览 Dynamo Package Manager，您会发现一些令人兴奋的新进展。在以下各部分中，我们将更深入地探索软件包，从最终用户角度到您自己的 Dynamo 软件包制作。
