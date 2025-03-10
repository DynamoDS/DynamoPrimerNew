# 快速入门

现在，您对整体情况有了更多了解，让我们开始在 Civil 3D 中构建您的第一个 Dynamo 图形！

{% hint style="info" %}这是一个简单示例，旨在演示 Dynamo 的基本功能。建议您在新的空 Civil 3D 文档中完成这些步骤。{% endhint %}

## 打开 Dynamo

首先，在 Civil 3D 中打开一个空文档。进入后，导航到 Civil 3D 功能区中的 **“管理”** 选项卡，然后查找 **“可视化编程”** 面板。

\![](<../.gitbook/assets/image (7).png>)

单击 **“Dynamo”** 按钮，这即会在单独窗口中启动 Dynamo。

{% hint style="info" %}**Dynamo 和 Dynamo 播放器之间有何区别？**

Dynamo 是用于构建和运行图形的工具。Dynamo 播放器是一种运行图形的简单方法，无需在 Dynamo 中打开这些图形。

准备好尝试以后，转到 [Dynamo 播放器.md](dynamo-player.md "mention")部分。{% endhint %}

## 启动一个新图形

Dynamo 打开后，您会看到开始屏幕。单击 **“新建”** 以打开一个空白工作空间。

<figure><img src="../.gitbook/assets/c3d-start.png" alt=""><figcaption><p>Dynamo 开始屏幕</p></figcaption></figure>

{% hint style="info" %}**有哪些样例？**

Dynamo for Civil 3D 附带了一些预构建的图形，有助于激发更多有关如何使用 Dynamo 的想法。建议您在某个时候查看这些内容，以及在本 Primer 中此处参见[样例工作流](sample-workflows/ "mention")部分。{% endhint %}

## 添加节点

您现在应该看到一个空的工作空间。让我们看一看 Dynamo 的实际应用！我们的目标是：

>  :dart: **构建一个会将文字插入到模型空间中的 Dynamo 图形。**

很简单，对不对？但在开始之前，我们需要先介绍一些基础知识。

Dynamo 图形的核心构建块称为 **“节点”**。一个节点就像一台小型机器 - 您将数据输入到该机器中、它对该数据进行一些处理，然后输出结果。Dynamo for Civil 3D 有一个节点 **库**，可以使用 **导线** 将这些节点连接起来以形成一个比单个节点本身的作用更大更好的 **图形**。

{% hint style="info" %}**等一等，如果我以前从未使用过 Dynamo，该怎么办？**

您可能对其中的一些操作并不完全熟悉，没关系！以下部分会有所帮助。

[3_用户界面](../3\_user\_interface/ "mention")\
 [4_节点和导线](../4\_nodes\_and\_wires/ "mention")\
 [5_基本节点和概念](../5\_essential\_nodes\_and\_concepts/ "mention"){% endhint %}

好，让我们来构建图形。下面列出了我们需要的所有节点。

<figure><img src="../.gitbook/assets/c3d-create-text-node-list.png" alt=""><figcaption></figcaption></figure>

可以通过以下方式来找到这些节点：在本库的搜索栏中键入节点名称，或者在画布中的任意位置单击鼠标右键并在其中搜索。

<figure><img src="../.gitbook/assets/c3d-create-text-node-placement.gif" alt=""><figcaption><p>节点可以从库放置，也可以通过在画布中单击鼠标右键来放置</p></figcaption></figure>

{% hint style="info" %}**如何知道要使用哪些节点以及在何处找到它们？**

库中的节点根据它们的作用分组为逻辑类别。如需更深入浏览，请参见[节点库.md](node-library.md "mention")部分。{% endhint %}

您的最终图形应该如下所示。

<figure><img src="../.gitbook/assets/c3d-text-create-final (2).png" alt=""><figcaption><p>完成的图形</p></figcaption></figure>

让我们总结一下执行了哪些操作：

> 1. 我们选择了要使用的文档。在这种情况（以及许多情况）下，我们希望在 Civil 3D 中的活动文档中工作。
> 2. 我们定义了应创建文字对象的目标块（在本例中为模型空间）。
> 3. 我们使用了 _String_ 节点来指定应放置文字的图层。
> 4. 我们使用 _Point.ByCoordinates_ 节点创建了一个点来定义应放置文字的位置。
> 5. 我们使用两个 _Number Slider_ 节点定义了文字插入点的 X 和 Y 坐标。
> 6. 我们使用了另一个 _String_ 节点来定义文字对象的内容。
> 7. 最后，我们创建了文字对象。

让我们看一看闪亮新图形的结果！

## 查看结果

返回到 Civil 3D 中，确保 **“模型”** 选项卡处于选中状态。您应该会看到 Dynamo 已创建的新文字对象。

{% hint style="info" %}如果您看不到文字，则可能需要运行“ZOOM”->“EXTENTS”命令来缩放到正确的位置。{% endhint %}

<figure><img src="../.gitbook/assets/c3d-create-text-result.png" alt="" width="413"><figcaption></figcaption></figure>

酷！现在，让我们对文字进行一些更新。

返回到您的 Dynamo 图形中，继续更改一些输入值（如文字字符串、插入点坐标等）。您应该会在 Civil 3D 中看到文字自动更新。另请注意，如果断开连接其中一个输入端口，文字会被删除。如果重新连接所有内容，则会再次创建该文字。

<div data-full-width="false">

<figure><img src="../.gitbook/assets/c3d-create-text.gif" alt=""><figcaption><p>已完成的图形正在运行</p></figcaption></figure>

</div>

{% hint style="info" %}**为什么 Dynamo 不在每次运行图形时插入一个新的文字对象？**

默认情况下，Dynamo 会“记住”它创建的对象。如果更改节点输入值，则 Civil 3D 中的对象会更新，而不是创建全新的对象。有关此行为的详细信息，请参见[对象绑定.md](advanced-topics/object-binding.md "mention")部分。{% endhint %}

> :tada: 任务完成！

## 后续步骤

本例仅对 Dynamo for Civil 3D 的作用进行了简要介绍。继续阅读以了解更多信息！
