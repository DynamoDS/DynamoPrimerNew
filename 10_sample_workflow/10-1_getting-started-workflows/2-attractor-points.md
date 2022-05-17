# Attractor Points

Attractor points are great for experimenting with geometric patterns. They can be used to create gradual changes to objects based on their distance.

This workflow will teach you how to:

* Create, manage and edit lists.
* Move points in the 3D preview using direct manipulation.
* 改变显示模式

![](../images/10-1/2/attractor1.gif)

## Defining our Objectives

在本练习中，我们要创建一个圆（_“目标”_），其中半径输入由距邻近点的距离定义（_“关系”_）。

![手绘圆](../images/10-1/2/00-Hand-Sketch-of-Circle.png)

> 定义基于距离关系的点通常称为“吸引器”。此处，将使用距吸引器点的距离来指定圆应该有多大。

## 后续步骤

> Download the example file by clicking on the link below.
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/10-1/2/DynamoSampleWorkflow-Attractors.dyn" %}

现在，我们已经绘制了“目标”和“关系”，可以开始创建图形。 我们需要节点来表示 Dynamo 将执行的操作序列。****************

![]

> 1. “输入”>“基本”>**“Number”**
> 2. “输入”>“基本”>**“Number Slider”**
> 3. ****
> 4. “几何图形”>“几何图形”>**“DistanceTo”**
> 5. ****

### 使用线连接节点

现在，我们有几个节点，我们需要将节点端口用线连接起来。这些连接将定义数据流。

![]

> 1. **“Number”**连接到**“Point.ByCoordinates”**
> 2. **“Number Sliders”**连接到**“Point.ByCoordinates”**
> 3. **“Point.ByCoordinates”**(2) 连接到**“DistanceTo”**
> 4. **“Point.ByCoordinates”**和**“DistanceTo”**连接到**“Circle.ByCenterPointRadius”**

### 执行程序

在完成定义程序流后，我们只需告知 Dynamo 执行它即可。在执行程序（“自动”或在“手动”模式下单击“运行”）后，数据将流经这些线，并且我们应该可以在三维预览中看到结果。

![]

> 1. （单击“运行”）- 如果“执行栏”处于“手动”模式，则需要单击“运行”才能执行图形
> 2. 节点预览 - 将鼠标光标悬停在节点右下角的框上会弹出结果
> 3. 三维预览 - 如果有节点创建几何图形，我们将在三维预览中看到它。
> 4. 创建节点上的输出几何图形。

### ****

如果程序正常运行，我们应该会在三维预览中看到一个穿过吸引器点的圆。这很棒，但我们可能想要添加更多详细信息或更多控件。我们将调整对圆节点的输入，以便可以校准对半径的影响。将另一个**“Number Slider”**添加到工作空间，然后双击工作空间的空白区域以添加**“Code Block”**节点。编辑代码块中的字段，以便指定 `X/Y`X/Y。

![]

> 1. **代码块**
> 2. **“DistanceTo”**和**“Number Slider”**连接到**“Code Block”**
> 3. **“Code Block”**连接到**“Circle.ByCenterPointRadius”**

### Using Sequences

简单开始后增加复杂性是循序渐进开发程序的有效方法。在一个圆正常工作后，让我们将程序的强大功能应用于多个圆。如果我们使用点栅格（而不是一个中心点）并适应结果数据结构中的变化，那么程序现在将创建多个圆 - 每个圆的唯一半径值由距吸引器点的校准距离进行定义。

![]

> 1. 添加**“Number Sequence”**节点并替换**“Point.ByCoordinates”**的输入 - 在“Point.ByCoordinates”上单击鼠标右键，然后依次选择“连缀”>“叉积”
> 2. 在 Point.ByCoordinates 后添加**“Flatten”**节点。要完全展平列表，请将 `amt`amt`-1` 输入保留为默认值 -1
> 3. 三维预览将使用圆栅格进行更新

### 使用“直接操纵”调整

有时，数字操作并不是正确的方法。现在，可以在后台三维预览中导航时手动推拉点几何图形。我们还可以控制由点构建的其他几何图形。例如，**“Sphere.ByCenterPointRadius”**也可以直接操纵。我们可以使用**“Point.ByCoordinates”**，通过一系列 X、Y 和 Z 控制点的位置。但是，使用“直接操纵”方法，可以通过在**“三维预览导航”**模式下手动移动点来更新滑块的值。这提供了一种更直观的方法，来控制一组用于标识点位置的离散值。

![]

> 1. 要使用**“直接操纵”**，请选择要移动的点面板 - 箭头将显示在选定点上。
> 2. 切换到**“三维预览导航”**模式。

![](../images/10-1/2/attractor\(8\).png)

> 1. 将光标悬停在点上，X、Y 和 Z 轴将显示。
> 2. 单击并拖动彩色箭头以移动相应的轴，**“Number Slider”**值将使用手动移动的点实时更新。

![]

> 1. 请注意，在**“直接操纵”**之前，仅有一个滑块连接到**“Point.ByCoordinates”**组件。当我们在 X 方向上手动移动点时，Dynamo 将自动为 X 输入生成新的**“Number Slider”**。

