# 重命名结构

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption></figcaption></figure>

当将管道和结构添加到管网时，Civil 3D 会使用模板自动指定名称。通常，这在初始放置期间已够用；但随着设计的展开，这些名称在将来不可避免地需要更改。此外，可能还需要许多不同的命名模式；例如，从最远的下游结构开始按顺序命名管道管路中的结构，或遵循与本地机构的数据模式保持一致的命名模式。本例将演示如何使用 Dynamo 来定义任何类型的一贯应用的命名策略。

## 目标

> :dart: 根据路线的桩号标注按顺序重命名管网结构。

## 关键概念

> * 使用边界框
> * 使用 **List.FilterByBoolMask** 节点过滤数据
> * 使用 **List.SortByKey** 节点对数据排序
> * 生成和修改文字字符串

## 版本兼容性

{% hint style="success" %} 此图形将在 **Civil 3D 2020** 及更高版本上运行。 
{% endhint %} 

## 数据集

首先下载下面的样例文件，然后打开 DWG 文件和 Dynamo 图形。

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dyn" %}

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dwg" %}

## 解决方案

下面概述了此图形中的逻辑。

> 1. 按图层选择结构
> 2. 获取结构位置
> 3. 按偏移过滤结构，然后按桩号对结构排序
> 4. 生成新名称
> 5. 重命名结构

开始吧！

### 选择结构

首先，我们需要选择要处理的所有结构。为此，我们只需选择特定图层上的所有对象，这意味着我们可以选择不同管网中的结构（假定它们共享同一图层）。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectStructures (1).png" alt=""><figcaption><p>选择给定图层上的结构</p></figcaption></figure>

> 1. 此节点确保我们不会意外检索到任何可能与结构共享同一图层的并不需要的对象类型。

### 获取结构位置

现在，我们已有这些结构，我们需要确定它们在空间中的位置，以便可以根据它们的位置对它们进行排序。为此，我们将利用每个对象的边界框。对象的**边界框**是完全包含该对象几何范围的最小大小的框。通过计算边界框的中心，我们会获取结构插入点的较佳近似值。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GetPosition.png" alt=""><figcaption><p>使用边界框获取每个结构的近似插入点</p></figcaption></figure>

我们将使用这些点，来获取结构相对于选定路线的桩号和偏移。

<div>

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectAlignment.png" alt="" width="268"><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StationOffset.png" alt="" width="306"><figcaption></figcaption></figure>

</div>

### 过滤和排序

以下是事情开始变得有点棘手的地方。在此阶段，我们已有一个包含指定图层上所有结构的大列表，然后选择我们要对沿其的结构排序的路线。问题是列表中可能有我们不想要重命名的结构。例如，它们可能不是我们感兴趣的特定管路的一部分。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StructureIllustration.png" alt="" width="555"><figcaption></figcaption></figure>

> 1. 选定的路线
> 2. 要重命名的结构
> 3. 应忽略的结构

因此，我们需要过滤结构列表，以便我们不必考虑大于与路线的特定偏移量的结构。最好使用 **List.FilterByBoolMask** 节点完成此操作。过滤结构列表后，我们使用 **List.SortByKey** 节点以按其桩号值对结构进行排序。

{% hint style="info" %}
 如果您对操作列表不熟悉，请参见 [2-使用列表.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention") 部分。 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_FilterAndSort.png" alt=""><figcaption><p>过滤和排序结构</p></figcaption></figure>

> 1. 检查以查看结构的偏移量是否小于阈值
> 2. 将任何空值替换为 _false_
> 3. 过滤结构和桩号列表
> 4. 按桩号对结构排序

### 生成新名称

最后，我们需要为结构创建新名称。我们将使用的格式是 `<alignment name>-STRC-<number>`。此处有一些额外节点，可用于根据需要为数字填充额外的零（例如，“01”而不是“1”）。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GenerateNames.png" alt=""><figcaption><p>生成新的结构名称</p></figcaption></figure>

### 重命名结构

最后但同样重要的是，我们重命名结构。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SetName.png" alt="" width="289"><figcaption><p>设置结构的名称</p></figcaption></figure>

### 结果

以下是一个使用 **Dynamo 播放器**运行图形的示例。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption><p>使用 Dynamo 播放器运行图形并在 Civil 3D 中查看结果</p></figcaption></figure>

{% hint style="info" %}
 如果您对使用 Dynamo 播放器不熟悉，请参见 [Dynamo 播放器.md](../../dynamo-player.md "mention") 部分。 
{% endhint %} 

> :tada: 任务完成！

### 小贴士：在 Dynamo 中可视化

它有助于利用 Dynamo 的三维背景预览来可视化图形的中间输出，而不仅仅是可视化最终结果。我们可以轻松地显示结构的边界框。此外，此特定数据集在文档中包含“道路”，因此我们可以将“道路要素线”几何图形导入 Dynamo，以便为结构在空间中的位置提供一些上下文。如果图形用于没有任何道路的数据集，这些节点将不会执行任何操作。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Visualize (2).png" alt=""><figcaption><p>可视化结构和道路要素线的几何图形</p></figcaption></figure>

现在，我们可以更好地了解按偏移过滤结构的过程是如何工作的。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Dynamo (1).gif" alt=""><figcaption><p>调整“路线偏移”阈值，并在 Dynamo 中可视化受影响的结构</p></figcaption></figure>

## 想法

以下是一些有关如何扩展此图形功能的想法。

{% hint style="info" %}
 根据结构的 **最近路线** 来重命名结构，而不是选择特定路线。  
{% endhint %} 

{% hint style="info" %}
 除了结构外，还 **重命名管道**。 
{% endhint %} 

{% hint style="info" %}
 根据结构的管路，来 **设置其图层**。 
{% endhint %} 
