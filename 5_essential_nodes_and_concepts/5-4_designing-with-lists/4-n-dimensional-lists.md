# n 维列表

再往下钻，让我们为层次结构添加更多层。数据结构可以扩展到远超二维列表的列表。由于列表是 Dynamo 中的项目，而且它们本身也是项目，因此我们可以创建尽可能多维的数据。

我们将在此处使用的类比是俄罗斯套娃。每个列表可视为一个包含多个项的容器。每个列表都有自己的属性，也被视为自己的对象。

![娃娃](../images/5-4/4/145493363\_fc9ff5164f\_o.jpg)

> 一组俄罗斯套娃（拍摄者[Zeta](https://www.flickr.com/photos/beppezizzi/145493363)）是 n 维列表的类比。每个层表示一个列表，每个列表在其中包含项目。在 Dynamo 的情况下，每个容器内可以有多个容器（表示每个列表的项目）。

n 维列表很难用直观的方式进行解释，但是我们在本章中设置了一些练习，这些练习着重于处理超出二维范围的列表。

### 映射和组合

映射无疑是 Dynamo 中数据管理最复杂的部分，并且在处理列表的复杂层次结构时尤其重要。在下面的一系列练习中，我们将演示在数据变为多维时何时使用映射和组合。

在上一节中，可以找到 **“List.Map”** 和 **“List.Combine”** 的初步介绍。在下面的最后一个练习中，我们将对复杂数据结构使用这些节点。

## 练习 - 二维列表 - 基本

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/5-4/4/n-Dimensional-Lists.zip" %}

本练习是三个练习中的第一个，侧重于阐述输入的几何图形。本系列练习中的每个部分都将增加数据结构的复杂性。

![Exercise](<../images/5-4/4/n-dimensional lists - 2d lists basic 01.jpg>)

> 1. 让我们从练习文件文件夹中的 .sat 文件开始。我们可以使用**“文件路径”(File Path)** 节点抓取此文件。
> 2. 使用 **Geometry.ImportFromSAT**，该几何图形将作为两个曲面输入到 Dynamo 预览中。

在本练习中，我们希望保持简单并处理其中一个曲面。

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 02.jpg>)

> 1. 让我们选择索引 1 以抓取上方曲面。 我们使用 **List.GetItemAtIndex** 节点来执行此操作。
> 2. 关闭 **“Geometry.ImportFromSAT”** 预览中的几何图形预览。

下一步是将曲面分割为点栅格。

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 03.jpg>)

> 1\.使用 **“代码块”**，插入以下两行代码：`0..1..#10;` `0..1..#5;`
>
> 2\.使用 **“Surface.PointAtParameter”**，将两个代码块值连接到 u 和 _v_。将此节点的 _连缀_ 更改为 _“叉积”_。
>
> 3\.输出显示数据结构，这在 Dynamo 预览中也可见。

接下来，使用上一步中的点以沿曲面生成十条曲线。

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 04.jpg>)

> 1. 要了解数据结构的组织方式，我们将 **NurbsCurve.ByPoints** 连接到 **Surface.PointAtParameter** 的输出。
> 2. 现在，可以关闭 **“List.GetItemAtIndex”** 节点的预览，以获得更清晰的结果。

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 05.jpg>)

> 1. 基本 **List.Transpose** 将翻转一列列表的列和行。
> 2. 通过将 **List.Transpose** 的输出连接到 **NurbsCurve.ByPoints**，我们现在得到五条曲线在整个曲面上水平延伸。
> 3. 可以在上一步中关闭 **“NurbsCurve.ByPoints”** 节点的预览，以在图像中获得相同的结果。

## 练习 - 二维列表 - 高级

让我们增加复杂性。假定我们要对上一个练习中创建的曲线执行操作。也许，我们希望将这些曲线与其他曲面相关联，并在它们之间进行放样。这需要更加注意数据结构，但基本逻辑是相同的。

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 01.jpg>)

> 1. 从上一练习的步骤开始，使用 **List.GetItemAtIndex** 节点隔离已输入几何图形的上曲面。

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 02.jpg>)

> 1. 使用 **Surface.Offset**，将曲面偏移值 _10_。

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 03.jpg>)

> 1. 按照与上一练习相同的方式，使用以下两行代码定义_代码块_：`0..1..#10;` `0..1..#5;`
> 2. 将这些输出连接到两个 **Surface.PointAtParameter** 节点，每个节点的 _连缀_ 设置为 _“叉积”_。 其中一个节点连接到原始曲面，而另一个节点连接到偏移曲面。

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 04.jpg>)

> 1. 关闭这些曲面的预览。
> 2. 与上一练习中一样，将输出连接到两个 **NurbsCurve.ByPoints** 节点。 结果显示与两个曲面对应的曲线。

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 05.jpg>)

> 1. 通过使用 **List.Create**，我们可以将两组曲线合并为一列列表。
> 2. 在输出中注意到，我们有两个列表，其中每个列表包含十个项目，分别表示每个 NURBS 曲线的连接集。
> 3. 通过执行 **Surface.ByLoft**，我们可以直观地了解此数据结构。该节点将放样每个子列表中的所有曲线。

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 06.jpg>)

> 1. 在上一步中，关闭 **“Surface.ByLoft”** 节点的预览。
> 2. 通过使用 **List.Transpose**，请记住，我们将翻转所有列和行。此节点会将两列（每个列表十条曲线）转换为十列（每个列表两条曲线）。现在，我们得到与另一个曲面上的相邻曲线相关的每条 NURBS 曲线。
> 3. 使用 **Surface.ByLoft**，我们得到一个带肋的结构。

接下来，我们将演示实现此结果的替代流程

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 07.jpg>)

> 1. 在开始之前，请在上一步中关闭 **“Surface.ByLoft”** 预览以避免混淆。
> 2. 除 **List.Transpose** 之外，还可以使用 **List.Combine**。这将对每个子列表运算 _“连结符”_。
> 3. 在本例中，我们使用 **List.Create** 作为 _“连结符”_，这将在子列表中创建每个项目的列表。
> 4. 使用 **Surface.ByLoft** 节点，我们得到与上一步中相同的曲面。在这种情况下，转置更容易使用，但当数据结构变得更加复杂时，**List.Combine** 更加可靠。

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 08.jpg>)

> 1. 如果要切换带肋结构中曲线的方向，请后退几步，我们需要先使用 **“List.Transpose”**，然后再连接到 **“NurbsCurve.ByPoints”**。这将翻转列和行，从而得到 5 个水平加强筋。

## 练习 - 三维列表

现在，我们将更进一步。在本练习中，我们将使用两个输入的曲面，从而创建复杂的数据层次结构。尽管如此，我们的目标是使用相同的基础逻辑来完成相同的操作。

从上一练习中输入的文件开始。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 01.jpg>)

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 02.jpg>)

> 1. 与上一练习中一样，使用 **Surface.Offset** 节点按值 _10_ 进行偏移。
> 2. 在输出中注意到，我们创建了两个具有偏移节点的曲面。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 03.jpg>)

> 1. 按照与上一练习相同的方式，使用以下两行代码定义 **“代码块”**：`0..1..#20;` `0..1..#20;`
> 2. 将这些输出连接到两个 **Surface.PointAtParameter** 节点，每个节点的连缀设置为 _“叉积”_。其中一个节点连接到原始曲面，而另一个节点连接到偏移曲面。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 04.jpg>)

> 1. 与上一练习中一样，将输出连接到两个 **NurbsCurve.ByPoints** 节点。
> 2. 查看 **“NurbsCurve.ByPoints”** 的输出，注意到这是一列两个列表，比上一练习更复杂。数据按基础曲面分类，因此我们为结构化数据添加了另一个层级。
> 3. 请注意，**Surface.PointAtParameter** 节点中的对象变得更加复杂。在本例中，我们会看到一列列表。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 05.jpg>)

> 1. 在继续操作之前，请关闭现有曲面的预览。
> 2. 使用 **“List.Create”** 节点，我们将 NURBS 曲线合并为一个数据结构，从而创建一列列表（其中每个元素也是一个列表）。
> 3. 通过连接 **Surface.ByLoft** 节点，我们得到原始曲面的版本，因为它们各自保留在由原始数据结构创建的自己列表中。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 06.jpg>)

> 1. 在上一练习中，我们能够使用 **List.Transpose** 创建带肋结构。这在此处不起作用。对二维列表应使用转置，但由于我们有三维列表，因此“翻转列和行”操作不会像之一样轻松。请记住，列表是对象，因此 **List.Transpose** 将翻转包含子列表的列表，但不会将 NURBS 曲线在层次结构中进一步向下翻转一个列表。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 07.jpg>)

> 1. **List.Combine** 在此处将更加适用。当访问更复杂的数据结构时，我们要使用 **List.Map** 和 **List.Combine** 节点。
> 2. 使用 **List.Create** 作为 _“连结符”_，我们可以创建一个数据结构，使其更加适用。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 08.jpg>)

> 1. 数据结构仍需要在层次结构上向下转置一步。为此，我们将使用 **List.Map**。除了使用一个输入列表（而不是两个或更多）之外，这类似于使用 **List.Combine**。
> 2. 我们将应用于 **List.Map** 的函数是 **List.Transpose**，这将翻转主列表中子列表的列和行。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 09.jpg>)

> 1. 最后，我们可以结合使用正确的数据层次结构放样 NURBS 曲线，从而提供带肋结构。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 10.jpg>)

> 1. 我们使用 **“Surface.Thicken”** 节点和输入设置为几何图形添加一些深度，如图所示。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 11.jpg>)

> 1. 最好添加一个曲面来支撑这两个结构，因此请添加另一个 **“Surface.ByLoft”** 节点，并使用上一步中 **“NurbsCurve.ByPoints”** 的第一个输出作为输入。
> 2. 随着预览变得混乱，请通过在每个节点上单击鼠标右键并取消选中“预览”以关闭这些节点的预览，以便更好地查看结果。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 12.jpg>)

> 1. 加厚这些选定曲面，完成连接。

这不是有史以来最舒适的摇椅，但还会进行大量数据处理。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 13.jpg>)

最后一步，我们反转带条纹成员的方向。如在上一练习中使用转置一样，我们将在此处执行类似操作。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 14.jpg>)

> 1. 由于我们的层次结构中还有一层级，因此我们需要将 **“List.Map”** 与 **“List.Tranpose”** 函数一起使用来更改 NURBS 曲线的方向。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 15.jpg>)

> 1. 我们可能希望增加踏板数，因此可以将 **“代码块”** 更改为 `0..1..#20;` `0..1..#30;`

摇椅的第一个版本很流畅，因此我们的第二个模型提供了越野、运动多功能版本的靠背。

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 16.jpg>)
