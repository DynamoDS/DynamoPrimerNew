# 灯杆放置

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption></figcaption></figure>

Dynamo 的许多绝佳用例之一是沿道路模型动态放置离散对象。通常情况下，需要将对象放置在与沿道路插入的装配无关的位置，手动完成这项任务非常繁琐。当道路的水平或垂直几何图形发生更改时，会引入大量返工。

## 目标

> :dart: 沿道路在 Excel 文件中指定的桩号值处放置灯杆块参照。

## 关键概念

> * 从外部文件读取数据（在本例中为 Excel）
> * 组织词典中的数据
> * 使用坐标系控制位置/比例/旋转
> * 放置块参照
> * 在 Dynamo 中可视化几何图形

## 版本兼容性

{% hint style="success" %}此图形将在 **Civil 3D 2020** 及更高版本上运行。{% endhint %}

## 数据集

首先下载下面的样例文件，然后打开 DWG 文件和 Dynamo 图形。

{% hint style="info" %}最好将 Excel 文件和 Dynamo 图形保存在同一目录中。{% endhint %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs (1).dyn" %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs.dwg" %}

{% file src="../../../.gitbook/assets/LightPoles.xlsx" %}

## 解决方案

下面概述了此图形中的逻辑。

> 1. 读取 Excel 文件并将数据输入到 Dynamo 中
> 2. 从指定的道路基准线获取要素线
> 3. 沿道路要素线以所需的桩号生成坐标系
> 4. 使用坐标系将块参照放置在模型空间中

开始吧！

### 获取 Excel 数据

在本示例图形中，我们将使用 Excel 文件来存储 Dynamo 将用于放置灯杆块参照的数据。该表如下所示。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_ExcelFile.png" alt=""><figcaption><p>Excel 文件表结构</p></figcaption></figure>

{% hint style="info" %}使用 Dynamo 从外部文件（如 Excel 文件）读取数据是一个很好的策略，尤其是当数据需要与其他团队成员共享时。{% endhint %}

Excel 会如下所示输入到 Dynamo 中。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetExcelData (1).png" alt="" width="548"><figcaption><p>将 Excel 数据输入到 Dynamo 中</p></figcaption></figure>

现在，我们已有数据，需要按列（_“道路”_、_“基准线”_、_“点代码”_ 等）对数据进行拆分，以便可以在图形的其余部分中使用该数据。执行此操作的常见方法是使用 **List.GetItemAtIndex** 节点并指定我们所需每列的索引号。例如，_“道路”_ 列位于索引 0 处，_“基准线”_ 列位于索引 1 处，依此类推。

看起来不错，对吧？但这种方法有潜在问题。如果 Excel 文件中列的顺序将来发生更改，该怎么办？在两列之间添加新列，又该怎么办？然后，图形将无法正常运行，需要更新。我们可以通过将数据放入到**词典**中、将 Excel 列标题作为 _键_、将数据的其余部分作为 _值_，来使图形适应未来变化。

{% hint style="info" %}如果您对使用词典不熟悉，请参见 [5-5_Dynamo 中的词典](../../../5\_essential\_nodes\_and\_concepts/5-5\_dictionaries-in-dynamo/ "mention")部分。{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dictionary.png" alt=""><figcaption><p>将 Excel 数据放入到词典中</p></figcaption></figure>

由于这允许灵活更改 Excel 中列的顺序，从而使图形更具弹性。只要列标题保持不变，即可使用 _键_（即列标题）从词典中检索数据，这是我们接下来要执行的操作。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_DictionaryRetrieval.png" alt=""><figcaption><p>从词典中检索数据</p></figcaption></figure>

### 获取道路要素线

现在，我们已输入 Excel 数据并准备就绪，让我们开始使用该数据以从 Civil 3D 中获取有关道路模型的一些信息。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCorridorFeatureLines.png" alt=""><figcaption></figcaption></figure>

> 1. 按名称选择道路模型。
> 2. 获取道路内的特定基准线。
> 3. 通过点代码获取基准线内的要素线。

### 生成坐标系

现在，我们将沿道路要素线以我们在 Excel 文件中指定的桩号值生成**坐标系**。这些坐标系将用于定义灯杆块参照的位置、旋转和比例。

{% hint style="info" %}如果您对使用坐标系不熟悉，请参见 [2-向量.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention")部分。{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCoordinateSystems (1).png" alt=""><figcaption><p>沿道路要素线获取坐标系</p></figcaption></figure>

请注意，在此处使用代码块可旋转坐标系，具体取决于坐标系位于基准线的哪一侧。这可以使用多个节点序列来实现，但这是一个很好的例子，说明只需编写代码块即可。

{% hint style="info" %}如果您对使用代码块不熟悉，请参见 [8-1_代码块和 DesignScript](../../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/ "mention")部分。{% endhint %}

### 创建块参照

我们即将完成！我们已有能够实际放置块参照所需的所有信息。首先，使用 Excel 文件中的 _块名称_ 列获取我们所需的块定义。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetBlockDefinitions.png" alt=""><figcaption><p>从文档中获取我们所需的块定义</p></figcaption></figure>

在此处，最后一步是创建块参照。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_CreateBlockReferences.png" alt=""><figcaption><p>在模型空间中创建块参照</p></figcaption></figure>

### 结果

当运行图形时，您应该会在模型空间中看到新的块参照沿道路显示。以下是一个很酷的部分 - 如果图形的执行模式设置为“自动”，然后您编辑 Excel 文件，则块参照会自动更新！

{% hint style="info" %}可以在 [3_用户界面](../../../3\_user\_interface/ "mention")部分中，了解有关图形执行模式的详细信息。{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Excel.gif" alt=""><figcaption><p>更新 Excel 文件并在 Civil 3D 中快速查看结果</p></figcaption></figure>

以下是一个使用 **Dynamo 播放器**运行图形的示例。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption><p>使用 Dynamo 播放器运行图形并在 Civil 3D 中查看结果</p></figcaption></figure>

{% hint style="info" %}如果您对使用 Dynamo 播放器不熟悉，请参见 [Dynamo 播放器.md](../../dynamo-player.md "mention")部分。{% endhint %}

> :tada: 任务完成！

### 小贴士：在 Dynamo 中可视化

在 Dynamo 中可视化道路几何图形以提供上下文可能会很有帮助。此特定模型已在模型空间中提取了道路实体，因此让我们将这些道路实体输入到 Dynamo 中。

但我们还需要考虑其他一些事项。实体是一种较“重”的几何图形类型，这意味着此操作会降低图形的运行速度。如果有一种简单方法来_选择_我们是否需要查看实体，那就太好了。显而易见的答案是，只需断开连接 **Corridor.GetSolids** 节点，但这会对所有下游节点产生警告，这有点乱。在这种情况下，**ScopeIf** 节点的作用确实显著。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_VisualizeCorridor (1).png" alt=""><figcaption></figcaption></figure>

> 1. 请注意，**Object.Geometry** 节点的底部有一个灰色条。这意味着节点预览处于关闭状态（可通过在节点上单击鼠标右键进行访问），从而使 **GeometryColor.ByGeometryColor** 节点能够避免与其他几何图形“争抢”后台预览中的显示优先级。
> 2. **ScopeIf** 节点基本上使您能够有选择地运行整个节点分支。如果 _test_ 输入为 false，则连接到 **ScopeIf** 节点的每个节点都不会运行。

以下是 Dynamo 后台预览中的结果。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dynamo.png" alt=""><figcaption><p>在 Dynamo 中可视化道路几何图形</p></figcaption></figure>

## 想法

以下是一些有关如何扩展此图形功能的想法。

{% hint style="info" %}将 **旋转** 列添加到 Excel 文件中，然后使用该列来驱动旋转坐标系。{% endhint %}

{% hint style="info" %}将 **水平或垂直偏移** 添加到 Excel 文件中，以便灯杆可以根据需要偏离道路要素线。{% endhint %}

{% hint style="info" %} **直接在 Dynamo** 中使用起点桩号和典型间距生成桩号值，而不是使用具有桩号值的 Excel 文件。{% endhint %}
