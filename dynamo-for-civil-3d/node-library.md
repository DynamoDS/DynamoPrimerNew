# 节点库

我们之前提到，**节点**是 Dynamo 图形的核心构建块，它们在**库**中组织为逻辑组。在 Dynamo for Civil 3D 中，库中有两个类别（或**工具架**），其中包含用于处理 AutoCAD 和 Civil 3D 对象（如路线、轮廓、道路、块参照等）的专用节点。库的其余部分包含实际上更通用的节点，这些节点在 Dynamo 的所有“规格”（例如，适用于 Revit 的 Dynamo、Dynamo Sandbox 等）之间保持一致。

{% hint style="info" %}有关如何组织 Dynamo 核心库中节点的详细信息，请参见 [2-库.md](../3\_user\_interface/2-library.md "mention")部分。{% endhint %}

<figure><img src="../.gitbook/assets/c3d-node-library.png" alt="" width="563"><figcaption><p>Dynamo for Civil 3D 中的节点库</p></figcaption></figure>

> 1. 用于处理 AutoCAD 和 Civil 3D 对象的特定节点
> 2. 通用节点
> 3. 可以单独安装的第三方**软件包**中的节点

{% hint style="warning" %}使用在 AutoCAD 和 Civil 3D 工具架下找到的节点，即表示您的 Dynamo 图形将仅在 Dynamo for Civil 3D 中运行。如果在别处（例如，在适用于 Revit 的 Dynamo 中）打开 Dynamo for Civil 3D 图形，则这些节点会被标记并显示警告，并且不会运行。{% endhint %}

{% hint style="info" %}**为什么 AutoCAD 和 Civil 3D 有两个单独的工具架？**

此组织方式将用于原生 AutoCAD 对象（直线、多段线、块参照等）的节点与用于 Civil 3D 对象（路线、道路、曲面等）的节点区分开来。从技术角度而言，AutoCAD 和 Civil 3D 是两个独立的应用程序 - AutoCAD 是基础应用程序，而 Civil 3D 是在其基础上构建的。{% endhint %}

## 节点层次结构

要使用 AutoCAD 和 Civil 3D 节点，请务必全面了解每个工具架中的对象层次结构。还记得生物学的分类法吗？界、门类、类别、等级、族、属、种？AutoCAD 和 Civil 3D 对象以类似方式进行分类。让我们通过一些示例来进行讲解。

### Civil 对象

让我们以路线为例。

<figure><img src="../.gitbook/assets/c3d-node-library-alignment.png" alt=""><figcaption></figcaption></figure>

假定您要更改路线的名称。在此处，您要添加的下一个节点是 **CivilObject.SetName** 节点。

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-name (1).png" alt=""><figcaption></figcaption></figure>

起初，这看起来可能不太直观。**CivilObject** 有什么作用，为什么本库没有 **Alignment.SetName** 节点？答案与 _可重用性_ 和 _简便性_ 有关。如果您想一想，无论 Civil 3D 对象是路线、道路、轮廓还是其他对象，更改该对象名称的过程都是相同的。因此，与其使用本质上都执行相同操作的重复节点（例如，**Alignment.SetName、Corridor.SetName、Profile.SetName** 等），不如将该功能封装到一个节点中。这正是 **CivilObject.SetName** 的作用！

此情况的另一种思考方式是就 _关系_ 而言。路线和道路都是 **Civil 对象**类型，就像苹果和梨都是水果一样。Civil 对象节点适用于任何类型的 Civil 对象，就像您可能希望使用单个削皮器来削苹果和梨一样。如果您为每种水果都配备单独的削皮器，那么您的厨房会变得非常杂乱！从这个意义上讲，Dynamo 节点库就像您的厨房一样。

### 对象

现在，让我们再深入一步。假定您要更改路线的图层。您将使用的节点是 **Object.SetLayer** 节点。

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-layer.png" alt=""><figcaption></figcaption></figure>

为什么没有名为 **CivilObject.SetLayer** 的节点？我们前面讨论过的相同可重用性和简便性原则也适用于此。_图层_ 特性是 AutoCAD 中任何可绘制或插入的对象（如直线、多段线、文字、块参照等）所共有的特性。Civil 3D 对象（如路线和道路）属于同一类别，因此适用于**对象**的任何节点也可用于任何 **Civil 对象**。

