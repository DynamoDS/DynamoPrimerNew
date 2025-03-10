# 软件包简介

Dynamo 提供了大量现成功能，还维护了一个丰富的软件包库，可显著扩展 Dynamo 的功能。软件包是自定义节点或附加功能的集合。Dynamo Package Manager 是一个社区门户，用于下载已在线发布的任何软件包。这些工具集由第三方开发，用于扩展 Dynamo 的核心功能、可供所有人访问，只需单击相应按钮即可下载。

![Package Manager 站点](../images/6-2/1/dpm.jpg)

诸如 Dynamo 之类的开源项目通过此类社区参与有了蓬勃发展。借助专门的第三方开发人员，Dynamo 能够将其应用范围扩展到各行各业的工作流。因此，Dynamo 团队已共同努力来简化软件包的开发和发布（将在以下各节中详细讨论）。

### 安装软件包

安装软件包的最简单方法是使用 Dynamo 界面中的“软件包”菜单选项。现在，让我们直接跳转到该菜单选项并安装软件包。在本快速示例中，我们将安装一个常用软件包，用于在栅格上创建四边形嵌板。

在 Dynamo 中，转到 _“软件包”>“软件包管理器...”_。

<figure><img src="../../.gitbook/assets/package-manager-menu.png" alt=""><figcaption></figcaption></figure>

在搜索栏中，让我们搜索“quads from rectangular grid”。片刻之后，您应该会看到与此搜索查询匹配的所有软件包。我们想要选择具有匹配名称的第一个软件包。

单击“安装”以将此软件包添加到库，然后接受确认。完成！

<figure><img src="../../.gitbook/assets/quads-from-rectangular-grid.png" alt=""><figcaption></figcaption></figure>

请注意，现在 Dynamo 库中有另一个名为“buildz”的组。该名称指代软件包的开发人员，并且自定义节点将放置在此组中。我们可以立即开始使用此组。

![](../images/6-2/1/packageintroduction-installingapackage03.jpg)

使用 **“代码块”** 以快速定义矩形栅格、将结果输出到 **“Polygon.ByPoints”** 节点，然后输出到 **“Surface.ByPatch”** 节点以查看刚创建的矩形嵌板列表。

![](../images/6-2/1/packageintroduction-installingapackage04.jpg)

### 安装软件包文件夹 - DynamoUnfold

上述示例重点介绍内含一个自定义节点的软件包，但相同过程可用于下载内含多个自定义节点的软件包以及支持数据文件。现在，我们通过一个更全面的软件包来进行演示：Dynamo Unfold。

如上例中所示，首先选择 _“软件包”>“软件包管理器...”_。

这次，我们将搜索 _“DynamoUnfold”_ 一词。当我们看到该软件包时，请通过单击“安装”下载，以将“Dynamo Unfold”添加到 Dynamo 库中。

<figure><img src="../../.gitbook/assets/unfold.png" alt=""><figcaption></figcaption></figure>

在 Dynamo 库中，我们有一个 _“DynamoUnfold”_ 组，其中包含多个类别和自定义节点。

![](../images/6-2/1/packageintroduction-installingpackagefolder02.jpg)

现在，让我们来看一下软件包的文件结构。

1. 首先，转到“软件包”>“软件包管理器”>“已安装的软件包”。
2. 在“DynamoUnfold”的旁边，选择菜单菜单 <img src="../images/6-2/1/packageintroduction-verticaldotsmenu.jpg" alt="" data-size="line">。
3. 然后，单击“显示根目录”以打开此软件包的根文件夹。

<figure><img src="../../.gitbook/assets/view-root-directory.png" alt=""><figcaption></figcaption></figure>

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

\![](<../images/6-2/1/packageintroduction-installingpackagefolder07 (1) (2).jpg>)

### 浏览和查看软件包信息

在软件包管理器中，可以在“搜索软件包”选项卡中使用排序和过滤选项浏览软件包。有若干过滤器可用于查找宿主程序、状态（新的、已弃用或未弃用），以及软件包是否具有依存关系。

通过对软件包进行排序，可以识别评分较高或下载次数最多的软件包，也可以查找包含最新更新的软件包。

还可以通过单击“查看详细信息”，来访问每个软件包的更多详细信息。这将在“软件包管理器”中打开一个侧面板，在其中可以查找版本控制和依存关系、网站或存储库 URL、许可信息等信息。

### Dynamo Package Manager 网站

了解 Dynamo 软件包的另一种方法是浏览 [Dynamo Package Manager](http://dynamopackages.com) 网站。在此处，您可以找到软件包作者提供的软件包依赖项和主机/版本兼容性信息。还可以从 Dynamo Package Manager 下载软件包文件，但直接在 Dynamo 中进行下载是一个更无缝的过程。

![](../images/6-2/1/dpm2.jpg)

### 软件包文件存储在本地何处？

如果要查看软件包文件的保存位置，请在顶部导航中，依次单击“Dynamo”>“首选项”>“软件包设置”>“节点和软件包文件位置”，可以在此处查找当前根文件夹目录。

![](../images/6-2/1/packageintroduction-installingpackagefolder08.jpg)

默认情况下，软件包安装在与以下文件夹路径类似的位置：_C:/Users/[用户名]/AppData/Roaming/Dynamo/[Dynamo 版本]_。

### 在办公室中为软件包设置共享位置

对于询问是否可以部署具有预附着软件包的 Dynamo（以任何形式）的用户：解决此问题并允许安装了 Dynamo 的所有用户在中心位置进行控制的方法是将自定义软件包路径添加到每个安装。

**添加网络文件夹，以便 BIM 经理或其他人可以使用办公室批准的软件包监督文件夹的存放情况**  

在单个应用程序的 UI 中，转到 *Dynamo -> 首选项 -> 软件包设置 -> 节点和软件包文件位置*。在对话框中，按“添加路径”按钮，然后浏览到共享软件包资源的网络位置。 
 
作为一个自动化过程，它将涉及将信息添加到随 Dynamo 一起安装的配置文件：  
 `C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]\DynamoSettings.xml`

默认情况下，Dynamo for Revit 的配置为：
 
 
`<CustomPackageFolders>`  

`<string>C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]</string>`  

`</CustomPackageFolders>`

添加自定义位置将如下所示：  

`<CustomPackageFolders>`  

`<string>C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]</string>`  

`<string>N:\OfficeFiles\Dynamo\Packages_Limited</string>`  

`</CustomPackageFolders>`


也可以通过简单地将文件夹设为只读来控制此文件夹的集中管理。

### 从网络位置加载包含二进制文件的软件包

#### 场景

组织可能希望标准化由不同工作站和用户安装的包。执行此操作的一种方法是从 *Dynamo -> 首选项 -> 软件包设置 -> 节点和软件包文件位置*安装这些软件包，选择网络文件夹作为安装位置，然后让工作站将该路径添加到 `Manage Node and Package Paths`。

#### 问题

虽然该方案适用于仅包含自定义节点的软件包，但它可能不适用于包含二进制文件的软件包，例如 Zero-Touch 节点。此问题是由[安全措施](https://stackoverflow.com/questions/5328274/load-assembly-from-network-location)引起的，当程序集来自网络位置时，.NET Framework 会过度加载程序集。遗憾的是，使用链接线程中建议的 `loadFromRemoteSources` 配置元素对 Dynamo 来说不是一个可行的解决方案，因为它是作为组件而不是应用程序分发的。

#### 解决方法

一种可能的解决方法是使用指向网络位置的映射网络驱动器，并让工作站引用该路径。创建映射网络驱动器的步骤在[此处](https://support.microsoft.com/en-us/help/4026635/windows-10-map-a-network-drive)描述。

### 进一步了解软件包

Dynamo 社区不断发展壮大。通过不时浏览 Dynamo Package Manager，您会发现一些令人兴奋的新进展。在以下各部分中，我们将更深入地探索软件包，从最终用户角度到您自己的 Dynamo 软件包制作。
