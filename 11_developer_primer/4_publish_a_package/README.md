# 发布软件包

### 发布软件包 <a href="#publish-a-package" id="publish-a-package"></a>

软件包是一种用于存储节点并与 Dynamo 社区共享节点的便捷方式。软件包可以包含从在 Dynamo 工作空间中创建的自定义节点到 NodeModel 派生节点等的所有内容。发布和安装软件包使用的是软件包管理器。除了此页面，[Primer](https://primer2.dynamobim.org/v/zh-cn/6_custom_nodes_and_packages/6-2_packages/1-introduction) 还提供了有关软件包的常规手册。

#### 什么是软件包管理器？<a href="#what-is-a-package-manager" id="what-is-a-package-manager"></a>

Dynamo Package Manager 是一个软件注册表（类似于 npm），可以从 Dynamo 或 Web 浏览器中访问。软件包管理器包括安装、发布、更新和查看软件包。与 npm 一样，它会维护不同版本的软件包。它还有助于管理项目的依赖关系。

在浏览器中，搜索软件包并查看统计信息：[https://dynamopackages.com/](https://dynamopackages.com)

* 在 Dynamo 中，软件包管理器包括安装、发布和更新软件包。

![搜索软件包](images/dynamopackagemanager.jpg)

> 1. 联机搜索软件包：`Packages > Search for a Package...`
> 2. 查看/编辑已安装的软件包：`Packages > Manage Packages...`
> 3. 发布新软件包：`Packages > Publish New Package...`

#### 发布软件包 <a href="#publishing-a-package" id="publishing-a-package"></a>

软件包是从 Dynamo 内的软件包管理器发布的。建议的过程是本地发布、测试软件包，然后联机发布以与社区共享。通过使用 NodeModel 案例研究，我们将完成本地发布 RectangularGrid 节点为软件包和联机发布的必要步骤。

启动 Dynamo，然后选择 `Packages > Publish New Package...` 以打开 `Publish a Package` 窗口。

![发布软件包](images/dyn-publish-package-add-files.jpg)

> 1. 选择 `Add file...` 以浏览要添加到软件包的文件
> 2. 从 NodeModel 案例研究中选择两个 `.dll` 文件
> 3. 选择 `Ok`

在将文件添加到软件包内容中后，为软件包指定名称、描述和版本。使用 Dynamo 发布软件包会自动创建 `pkg.json` 文件。

![软件包设置](images/dyn-publish-package.jpg)

> 准备好发布的软件包。
>
> 1. 提供所需的名称、描述和版本信息。
> 2. 通过单击“本地发布”进行发布，然后选择 Dynamo 的软件包文件夹“`AppData\Roaming\Dynamo\Dynamo Core\1.3\packages`”以使节点在核心中可用。始终本地发布，直到软件包准备好共享。

发布软件包后，节点将在 Dynamo 库中的 `CustomNodeModel` 类别下可用。

![Dynamo 库中的软件包](images/dyn-publish-package-library.jpg)

> 1. 我们刚刚在 Dynamo 库中创建的软件包

在软件包准备好联机发布后，打开软件包管理器、选择 `Publish`，然后选择 `Publish Online`。

![在软件包管理器中发布软件包](images/dyn-publish-package-directory.jpg)

> 1. 要查看 Dynamo 如何设置软件包的格式，请单击“CustomNodeModel”右侧的三个垂直点，然后选择“显示根目录”
> 2. 在“发布 Dynamo 软件包”窗口中，选择 `Publish`，然后选择 `Publish Online`。
> 3. 要删除软件包，请选择 `Delete`。

#### 如何更新软件包？<a href="#how-do-i-update-a-package" id="how-do-i-update-a-package"></a>

更新软件包的过程与发布过程类似。打开软件包管理器、在需要更新的软件包上选择 `Publish Version...`，然后输入更高版本。

![发布软件包版本](images/dyn-publish-package-version.jpg)

> 1. 选择 `Publish Version` 以使用根目录中的新文件更新现有软件包，然后选择它应本地发布还是联机发布。

#### 软件包管理器 Web 客户端 <a href="#package-manager-web-client" id="package-manager-web-client"></a>

软件包管理器 Web 客户端允许用户搜索和查看软件包数据，包括版本控制、下载统计信息和其他相关信息。此外，软件包作者可以直接通过 Web 客户端登录以更新其软件包详细信息，例如兼容性信息。

有关这些功能的详细信息，请参阅此处的博客帖子：[https://dynamobim.org/discover-the-new-dynamo-package-management-experience/](https://dynamobim.org/discover-the-new-dynamo-package-management-experience/)。

可以通过以下链接访问软件包管理器 Web 客户端：[https://dynamopackages.com/](https://dynamopackages.com)

![软件包管理器 Web 客户端](images/packagemanager-browser.jpg)

##### 更新软件包详细信息

作者可以按照以下步骤编辑其软件包说明、网站链接和存储库链接：  

> 1. 在 **“我的软件包”** 下，选择软件包，然后单击 **“编辑软件包详细信息”**。  
> 2. 使用各自的字段添加或修改**网站**和**存储库**链接。  
> 3. 根据需要更新 **“软件包说明”**。  
> 4. 单击 **“保存更改”** 以应用更新。  

 **注意**：由于服务器更新需要一些时间，因此更新可能需要长达 15 分钟才能在 Dynamo 内的软件包管理器中刷新。正在努力减少这种延误。  

 ![用于更新已发布软件包的软件包详细信息的新 UI](images/Package-Manager_Image_5.png)

##### 编辑已发布软件包版本的兼容性信息  

兼容性信息可以针对以前发布的软件包版本进行追溯更新。请遵循下列步骤：  

![编辑已发布软件包的兼容性信息 - 第 1 步](images/Package-Manager_Image_6.png)

**第 1 步：**  

1. 单击要更新的软件包版本。  
2. **依赖于**列表将自动填充您的软件包所依赖的软件包。  
3. 单击 **“兼容性”** 旁边的铅笔图标以打开 **“编辑兼容性信息”** 工作流。  

**第 2 步：**  

请按照流程图并参考下表，帮助您了解哪个选项最适合您的软件包。

![为“编辑兼容性信息”工作流选择哪个选项](images/Package-Manager_Image_7.png)

让我们使用一些示例来演练一些方案：

**示例软件包 # 1** \- Civil 连接：此软件包与 Revit 和 Civil 3D 都有 API 依存关系，并且不包括核心节点的集合（例如：几何函数、数学函数和/或列表管理）。因此，在这种情况下，理想的选择是使用选项 1。该软件包在 Revit 和 Civil 3D 中将显示为兼容，与版本范围和/或单个版本列表匹配。

**示例软件包 # 2** \- Rhythm：此软件包是 Revit 特定节点以及核心节点的集合。在这种情况下，软件包具有主机依存关系。但还包括将在 Dynamo Core 中工作的核心节点。因此，在这种情况下，理想的选择是选项 2。该软件包在与版本范围和/或单个版本列表匹配的 Revit 和 Dynamo Core（也称为 Dynamo Sandbox）环境中将显示为“兼容”。

**示例软件包 # 3** \- Mesh Toolkit：此软件包是 Dynamo Core 软件包，它是不依赖于主机的几何图形节点的集合。因此，在这种情况下，理想的选择是选项 3。该软件包在 Dynamo 和所有与版本范围和/或单个版本列表匹配的主机环境中将显示为“兼容”。

![编辑兼容性信息选项](images/Package-Manager_Image_8.png)

根据选定的选项，将弹出 Dynamo 和/或主机特定字段，如下图所示。

![编辑兼容性信息 - 第 2 步](images/Package-Manager_Image_9.png)