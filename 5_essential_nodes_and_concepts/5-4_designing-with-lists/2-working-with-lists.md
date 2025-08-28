# 使用列表

### 使用列表

既然我们已经建立了列表，那么让我们来介绍如何对它执行操作。将一个列表想象为一副纸牌。一副纸牌即是列表，每张纸牌表示一个项目。

![纸牌](../images/5-4/2/Playing_cards_modified.jpg)

> 照片由 [Christian Gidlöf](https://commons.wikimedia.org/wiki/File:Playing_cards_modified.jpg) 提供

### 查询

我们可以在列表中进行哪些**查询**？这将访问现有特性。

* 一副纸牌中纸牌的张数？52\.
* 玩家人数？4\.
* 材料？纸。
* 长度？3.5" 或 89mm。
* 宽度？2.5" 或 64mm。

### 操作

我们可以对列表执行哪些**操作**？这将基于给定操作更改列表。

* 我们可以洗牌。
* 我们可以按值对一副纸牌进行排序。
* 我们可以按玩家对一副纸牌进行排序。
* 我们可以拆分一副纸牌。
* 我们可以通过发牌来划分一副纸牌。
* 我们可以选择一副纸牌中某张特定纸牌。

上面列出的所有操作都有类似 Dynamo 节点来用于处理常规数据列表。下面的课程将演示可以对列表执行的一些基本操作。

## **练习**

### **列表操作**

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/5-4/2/List-Operations.dyn" %}

下图是我们在两个圆之间绘制直线以表示基本列表操作的基础图形。我们将探讨如何管理列表中的数据，并通过下面的列表操作演示可视结果。

![](../images/5-4/2/workingwithlist-listoperation.jpg)

> 1. 从 **“代码块”** 开始，其中值为 `500;`
> 2. 连接到 **“Point.ByCoordinates”** 节点的 x 输入。
> 3. 将上一步中的节点连接到 **“Plane.ByOriginNormal”** 节点的原点输入。
> 4. 使用 **“Circle.ByPlaneRadius”** 节点，将上一步中的节点连接到平面输入。
> 5. 使用 **“代码块”** ，为半径指定值 `50;`。这是我们将创建的第一个圆。
> 6. 使用 **“Geometry.Translate”** 节点，将圆沿 Z 方向向上移动 100 个单位。
> 7. 使用 **“Code Block”** 节点，通过以下一行代码定义一系列 10 个介于 0 和 1 之间的数字：`0..1..#10;`
> 8. 将上一步中的代码块连接到两个 **“Curve.PointAtParameter”** 节点的 _“param”_ 输入。将 **“Circle.ByPlaneRadius”** 插入到顶部节点的曲线输入，并将 **“Geometry.Translate”** 连接到其下节点的曲线输入。
> 9. 使用 **Line.ByStartPointEndPoint**，连接两个 **Curve.PointAtParameter** 节点。

### List.Count

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/5-4/2/List-Count.dyn" %}

_“List.Count”_ 节点简单明了：它计算列表中值的数量，并返回该数量。随着我们使用列表的列表，此节点会变得更加微妙，但我们会在接下来的各部分中进行演示。

![合计](../images/5-4/2/workingwithlist-listoperation-listcount.jpg)

> 1. **“List.Count”**节点会返回**“Line.ByStartPointEndPoint”**节点中线的数量。在本例中，该值为 10，表示与从原始 **“Code Block”** 节点创建的点数一致。

### List.GetItemAtIndex

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/5-4/2/List-GetItemAtIndex.dyn" %}

**“List.GetItemAtIndex”** 是用于查询列表中项的基本方法。

![练习](../images/5-4/2/workingwithlist-getitemindex01.jpg)

> 1. 首先，在 **“Line.ByStartPointEndPoint”** 节点上单击鼠标右键以关闭其预览。
> 2. 使用 **“List.GetItemAtIndex”** 节点，我们选择索引 _“0”_ 或线列表中的第一项。

将滑块值更改为介于 0 和 9 之间，以使用 **“List.GetItemAtIndex”** 选择其他项目。

![](../images/5-4/2/workingwithlist-getitemindex02.gif)

### List.Reverse

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/5-4/2/List-Reverse.dyn" %}

_“List.Reverse”_ 可反转列表中所有项的顺序。

![练习](../images/5-4/2/workingwithlist-listreverse.jpg)

> 1. 要正确显示反转的线列表，请通过将 **“代码块”** 更改为 `0..1..#50;` 来创建更多线
> 2. 复制 **“Line.ByStartPointEndPoint”** 节点，在 **“Curve.PointAtParameter”** 和第二个 **“Line.ByStartPointEndPoint”** 之间插入“List.Reverse”节点
> 3. 使用 **“Watch3D”** 节点预览两个不同的结果。第一个显示没有反向列表的结果。这些线垂直连接到相邻点。但是，反转列表会将所有点以相反顺序连接到其他列表。

### List.ShiftIndices <a href="#listshiftindices" id="listshiftindices"></a>

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/5-4/2/List-ShiftIndices.dyn" %}

**“List.ShiftIndices”** 是适用于创建扭曲或螺旋图案或者任何其他类似数据操作的工具。此节点会将列表中的项目移动给定数量的索引。

![练习](../images/5-4/2/workingwithlist-shiftIndices01.jpg)

> 1. 在与反转列表相同的过程中，将 **“List.ShiftIndices”** 插入到 **“Curve.PointAtParameter”** 和 **“Line.ByStartPointEndPoint”** 中。
> 2. 使用 **“代码块”**，指定值为“1”以将列表移动一个索引。
> 3. 请注意，更改很细微，但在连接到另一组点时，较低 **“Watch3D”** 节点中的所有线都已移动一个索引。

例如，通过将 **“代码块”** 更改为较大值（_“30”_），我们注意到对角线存在明显差异。在本例中，该移动类似于照相机的光圈，从而以原始圆柱形式创建扭曲。

![](../images/5-4/2/workingwithlist-shiftIndices02.jpg)

### List.FilterByBooleanMask <a href="#listfilterbybooleanmask" id="listfilterbybooleanmask"></a>

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/5-4/2/List-FilterByBooleanMask.dyn" %}

![](../images/5-4/2/ListFilterBool.png)

**“List.FilterByBooleanMask”** 将基于布尔值列表移除某些项目，或通过读取“true”或“false”值来移除某些项目。

![练习](../images/5-4/2/workingwithlist-filterbyboolmask.jpg)

为了创建读取“true”或“false”的值列表，我们需要做更多的工作...

> 1. 使用 **“代码块”**，通过以下语法定义一个表达式：`0..List.Count(list);`。将 **“Curve.PointAtParameter”** 节点连接到 _“list”_ 输入。我们将在代码块章节中详细介绍此设置，但本例中的该行代码会为我们提供一个列表，该列表表示 **”Curve.PointAtParameter”** 节点的每个索引。
> 2. 使用 _**“%”**_ **（求模）**节点，将 _代码块_ 的输出连接到 _x_ 输入，将值 _4_ 连接到 _y_ 输入。当将索引列表除以 4 时，这将为我们提供余数。求模节点对于创建图案而言确实非常有用。所有值将读取为 4 的可能余数：0、1、2、3。
> 3. 在 _**“%”**_ **（求模）** 节点中，我们知道值为 0 意味着索引是 4 的倍数（0、4、8，依此类推）。通过使用 **“==”** 节点，我们可以针对值 _“0”_ 对其进行测试，以测试其可除性。
> 4. **“Watch”** 节点仅显示以下情况：我们有一个“true/false”模式，其读取：_true,false,false,false..._。
> 5. 使用此 true/false 模式，连接到两个 **“List.FilterByBooleanMask”** 节点的遮罩输入。
> 6. 将 **“Curve.PointAtParameter”** 节点连接到 **“List.FilterByBooleanMask”** 的每个列表输入。
> 7. **“Filter.ByBooleanMask”** 的输出读取 _“in”_ 和 _“out”_。_“in”_ 表示遮罩值为 _“true”_ 的值，而 _“out”_ 表示值为 _“false”_ 的值。通过将 _“in”_ 输出连接到 **“Line.ByStartPointEndPoint”** 节点的 _“startPoint”_ 和 _“endPoint”_ 输入，我们创建了过滤后的线列表。
> 8. **“Watch3D”** 节点显示线数少于点数。通过仅过滤 true 值，我们仅选择了 25% 的节点！
