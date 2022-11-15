# Revit 使用案例

您是否曾想过使用 Revit 中包含的数据查找内容？

如果您已经完成了类似以下示例的操作，则有可能出现这种情况。

在下图中，我们会收集 Revit 模型中的所有房间、获取所需房间的索引（按房间编号），最后在索引处抓取房间。

![](../images/5-5/4/dictionary-collectroominrevitmodel.jpg)

> 1. 收集模型中的所有房间。
> 2. 要查找的房间编号。
> 3. 获取房间编号并查找它所在的索引。
> 4. 获取索引处的房间。

## 练习：房间词典

### 第 I 部分：创建房间词典

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/5-5/4/roomDictionary.dyn" %}

现在，让我们使用词典重建这个想法。首先，我们需要收集 Revit 模型中的所有房间。

![](../images/5-5/4/dictionary-exerciseI-01.jpg)

> 1. 我们选择要处理的 Revit 类别（在本例中，我们处理的是房间）。
> 2. 我们告诉 Dynamo 收集所有这些元素

接下来，我们需要决定要使用哪些键来查找此数据。（键的相关信息位于[“什么是词典？”](9-1\_what-is-a-dictionary.md)部分中。）

![](../images/5-5/4/dictionary-exerciseI-02.jpg)

> 1. 我们将使用的数据是房间编号。

现在，我们将使用给定的键和元素创建词典。

![](../images/5-5/4/dictionary-exerciseI-03.jpg)

> 1. 节点**“Dictionary.ByKeysValues”**将根据相应的输入创建词典。
> 2. `Keys` 需要是字符串，而 `values` 可以是多种对象类型。

最后，我们现在可以使用房间编号从词典中检索房间。

![](../images/5-5/4/dictionary-exerciseI-04.jpg)

> 1. `String` 将是用于从词典中查找对象的键。
> 2. 现在，**“Dictionary.ValueAtKey”**将从词典中获取对象。

### 第 II 部分：值查找

使用相同的词典逻辑，我们还可以使用分组对象创建词典。如果我们要查找给定级别的所有房间，可以按如下所示修改上图。

![](../images/5-5/4/dictionary-exerciseII-01.jpg)

> 1. 我们现在可以使用参数值（在本例中，我们将使用标高），而不是将房间编号用作键。

![](../images/5-5/4/dictionary-exerciseII-02.jpg)

> 1. 现在，我们可以按房间所在的标高对房间进行分组。

![](../images/5-5/4/dictionary-exerciseII-03.jpg)

> 1. 按标高对图元进行分组后，我们现在可以使用共享键（唯一键）作为词典的键，以及将房间列表作为元素。

![](../images/5-5/4/dictionary-exerciseII-04.jpg)

> 1. 最后，使用 Revit 模型中的标高，我们可以在词典中查找位于该标高的房间。`Dictionary.ValueAtKey` 将获取标高名称并返回该标高处的房间对象。

使用词典的机会确实是无限的。在 Revit 中将 BIM 数据与元素本身关联的功能可用于各种使用案例。
