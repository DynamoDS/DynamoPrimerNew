# 发布到库 

我们刚创建了一个自定义节点，并将它应用到 Dynamo 图形中的特定流程。我们非常喜欢这个节点，我们想将它保留在 Dynamo 库中，以便在其他图形中引用。为此，我们将本地发布节点。这与发布软件包的过程类似，我们将在下一章中详细介绍。

通过本地发布节点，可以在打开新会话时在 Dynamo 库中访问该节点。如果不发布节点，则引用自定义节点的 Dynamo 图形也必须在其文件夹中具有该自定义节点（或必须使用 _“文件”>“输入库”_ 将自定义节点输入到 Dynamo 中）。

{% hint style="warning" %}只要 Dynamo Sandbox 2.17 及更高版本中的自定义节点和软件包没有宿主 API 依存关系，就可以发布它们。在早期版本中，只能在 Dynamo for Revit 和 Dynamo for Civil 3D 中发布自定义节点和软件包。{% endhint %}

## 练习：本地发布自定义节点

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/6-1/3/PointsToSurface.dyf" %}

让我们继续处理上一节中创建的自定义节点。打开“PointsToSurface”自定义节点后，我们会在 Dynamo 自定义节点编辑器中看到该图形。还可以在 Dynamo 图形编辑器中双击自定义节点来打开它。

![](../images/6-1/3/publishcustomnodelocally01.jpg)

要在本地发布自定义节点，只需在画布上单击鼠标右键，然后选择 _“发布此自定义节点...”_ 。

![](../images/6-1/3/publishcustomnodeexercise-02.jpg)

填写与上图类似的相关信息，然后选择 _“本地发布”_ 。请注意，“组”字段定义可从 Dynamo 菜单访问的主图元。

<figure><img src="../../.gitbook/assets/publish_a_package.png" alt=""><figcaption></figcaption></figure>

选择一个文件夹以容纳计划在本地发布的所有自定义节点。Dynamo 每次加载时都会检查该文件夹，因此请确保该文件夹处于永久位置。导航到此文件夹，然后选择 _“选择文件夹”_ 。现在，Dynamo 节点在本地发布，每次加载程序时都会保留在 Dynamo 库中！

![](../images/6-1/3/publishcustomnodeexercise-04.jpg)

要检查自定义节点文件夹位置，请转到 _“Dynamo”>“首选项”>“软件包设置”>“节点和软件包路径”_ 。

<figure><img src="../../.gitbook/assets/settings.png" alt="" width="520"><figcaption></figcaption></figure>

在此窗口中，我们会看到路径列表。

<figure><img src="../../.gitbook/assets/package-locations.png" alt=""><figcaption></figcaption></figure>

> 1. _“Documents\\DynamoCustomNodes...”_ 是指我们已本地发布的自定义节点的位置。
> 2. _“AppData\\Roaming\\Dynamo...”_ 是指联机安装的 Dynamo 软件包的默认位置。
> 3. 您可能希望按照列表顺序将本地文件夹路径下移（通过单击路径名左侧的向下箭头）。顶层文件夹是安装软件包的默认路径。因此，通过保留默认的 Dynamo 软件包安装路径作为默认文件夹，联机软件包将与本地发布的节点分离。

我们切换了路径名称的顺序，以便让 Dynamo 的默认路径作为软件包安装位置。

<figure><img src="../../.gitbook/assets/updated-package-locations.png" alt=""><figcaption></figcaption></figure>

导航到此本地文件夹，我们可以在 _“.dyf”_ 文件夹中找到原始自定义节点，该文件夹是 Dynamo 自定义节点文件的扩展名。我们可以编辑此文件夹中的文件，并且节点将在 UI 中更新。我们还可以向 _“DynamoCustomNode”_ 主文件夹添加更多节点，Dynamo 会在重新启动时将它们添加到您的库中！

![](../images/6-1/3/publishcustomnodeexercise-08.jpg)

现在，每次使用 Dynamo 库的“DynamoPrimer”组中的“PointsToSurface”时，Dynamo 都会载入。

![](../images/6-1/3/publishcustomnodeexercise-09.jpg)
