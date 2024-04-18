# 发布软件包 

在前面的各部分中，我们详细介绍了如何使用自定义节点和示例文件构建 _“MapToSurface”_ 软件包。但是，我们如何发布本地开发的软件包？本案例研究演示了如何发布由本地文件夹中的一组文件构成的软件包。

![](<../images/6-2/3/develop package - custom nodes 01 (1) (1).jpg>)

有多个方法可以发布软件包。下面是我们建议的流程：**本地发布、本地开发，然后联机发布**。我们首先从包含软件包中所有文件的文件夹开始。

### 卸载软件包

在开始发布 MapToSurface 软件包之前，如果您安装了上一课中的软件包，请将其卸载，以免使用相同的软件包。

首先，转到“软件包”>“软件包管理器”>“已安装的软件包”选项卡 >“MapToSurface”的旁边，依次单击垂直点菜单 >“删除”。

<figure><img src="../../.gitbook/assets/delete-map-to-surface.png" alt=""><figcaption></figcaption></figure>

然后，重新启动 Dynamo。重新打开后，选中 _“管理软件包”_ 窗口时，_“MapToSurface”_ 应不再存在。现在，我们已准备好从头开始！

### 本地发布软件包

{% hint style="warning" %}只要 Dynamo Sandbox 2.17 及更高版本中的自定义节点和软件包没有宿主 API 依存关系，就可以发布它们。在早期版本中，只能在 Dynamo for Revit 和 Dynamo for Civil 3D 中发布自定义节点和软件包。{% endhint %}

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/6-2/4/MapToSurface.zip" %}

这是软件包的首次提交，我们已将所有示例文件和自定义节点放入一个文件夹中。准备好此文件夹后，我们便可以上传到 Dynamo 软件包管理器。

![](../images/6-2/4/publishapackage-publishlocally01.jpg)

> 1. 此文件夹中包含五个自定义节点 (.dyf)。
> 2. 此文件夹中还包含五个示例文件 (.dyn) 和一个输入的矢量文件 (.svg)。这些文件将用作介绍性练习，以向用户介绍如何使用自定义节点。

在 Dynamo 中，首先依次单击 _“软件包”>“软件包管理器”>“发布新软件包”_ 选项卡。

在 _“发布软件包”_ 选项卡中，填写窗口左侧的相关字段。

<figure><img src="../../.gitbook/assets/package-details.png" alt=""><figcaption></figcaption></figure>

接下来，我们将添加软件包文件。可以逐个添加文件，也可以通过选择“添加目录” (1) 来添加整个文件夹。要添加并非 .dyf 文件的文件，请确保在浏览器窗口中将文件类型更改为 **“所有文件(**_._**)”**。请注意，我们将随意添加每个文件、自定义节点 (.dyf) 或示例文件 (.dyn)。当我们发布软件包时，Dynamo 会对这些项目进行分类。

<figure><img src="../../.gitbook/assets/map-to-surface-contents.png" alt=""><figcaption></figcaption></figure>

选择“MapToSurface”文件夹后，“软件包管理器”会显示文件夹内容。如果您正在上传具有复杂文件夹结构的软件包，并且不希望 Dynamo 对文件夹结构进行更改，可以启用“保留文件夹结构”开关。此选项适用于高级用户，如果您的软件包并非以特定方式有意设置，则最好使此开关保持处于关闭状态，允许 Dynamo 根据需要组织文件。单击“下一步”以继续操作。

<figure><img src="../../.gitbook/assets/map-to-surface-contents-preview.png" alt=""><figcaption></figcaption></figure>

在此处，您有机会在发布之前预览 Dynamo 将如何组织软件包文件。单击“完成”以继续。

<figure><img src="../../.gitbook/assets/publish-locally.png" alt=""><figcaption></figcaption></figure>

单击“本地发布” (1) 即可发布。如果您遵照执行，请务必单击 _“本地发布”_ （而 **不是** _“联机发布”_ ）以避免在“软件包管理器”中出现一系列重复的软件包。

发布后，应该会在“DynamoPrimer”组或 Dynamo 库下提供自定义节点。

![](<../images/6-2/3/develop package - install package 02 (1) (1).jpg>)

现在，我们来查看根目录，以了解 Dynamo 如何设置刚刚创建的软件包的格式。要执行此操作，请转到“已安装的软件包”选项卡 >“MapToSurface”的旁边，单击垂直点菜单 > 选择“显示根目录”。

<figure><img src="../../.gitbook/assets/show-root-directory.png" alt=""><figcaption></figcaption></figure>

请注意，根目录位于软件包的本地位置（请记住，我们已“本地”发布了软件包）。Dynamo 当前正在引用此文件夹来读取自定义节点。因此，请务必将目录本地发布到永久文件夹位置（即：不是桌面）。以下内容详细介绍了 Dynamo 软件包文件夹。

![](../images/6-2/4/publishapackage-publishlocally06.jpg)

> 1. _“bin”_ 文件夹中存储了使用 C# 或 Zero-Touch 库创建的 .dll 文件。我们没有任何此软件包的内容，因此在本例中，此文件夹为空。
> 2. _“dyf”_ 文件夹中存储了自定义节点。打开此文件夹将显示该软件包的所有自定义节点（.dyf 文件）。
> 3. 附加文件夹中存储了所有附加文件。这些文件可能是 Dynamo 文件 (.dyn)，也可能是所需的任何其他文件（.svg、.xls、.jpeg、.sat 等）。
> 4. pkg 文件是一个基本文本文件，用于定义软件包设置。这是在 Dynamo 中自动生成的，但如果您想要了解详细信息，可以对其进行编辑。

### 联机发布软件包

{% hint style="warning" %}注意：除非您实际上发布自己的软件包，否则请勿遵循此步骤！{% endhint %}

<figure><img src="../../.gitbook/assets/publish-version.png" alt=""><figcaption></figcaption></figure>

1. 准备好发布后，在“软件包”>“软件包管理器”>“已安装的软件包”窗口中，选择要发布的软件包右侧的按钮，然后选择“发布”。
2. 如果要更新已发布的软件包，请选择“发布版本”，然后 Dynamo 会根据该软件包的根目录中的新文件联机更新软件包。就这么简单！

### 发布版本...

当更新已发布软件包的根文件夹中的文件时，还可以通过在 _“我的软件包”_ 选项卡中选择 _“发布版本...”_ 来发布该软件包的新版本。这是对内容进行必要更新并与社区共享的无缝方式。仅当您是该软件包的维护人员时，_“发布版本”_ 才起作用。
