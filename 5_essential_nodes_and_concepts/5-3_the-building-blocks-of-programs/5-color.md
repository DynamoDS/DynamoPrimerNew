# 颜色

颜色是用于在可视化程序中创建引入注目的视觉效果以及渲染输出差异的绝佳数据类型。处理抽象数据和改变数字时，有时难以了解变化内容以及变化程度。这是颜色的绝佳应用。

### 创建颜色

Dynamo 中的颜色是使用 ARGB 输入创建的。这对应于 Alpha、红、绿和蓝通道。Alpha 表示颜色的 _透明度_，而其他三种颜色用作原色来协调生成整个色谱。

| 图标                                     | 名称（语法）                 | 输入(Inputs)  | 输出 |
| ---------------------------------------- | ----------------------------- | ------- | ------- |
| ![](<../images/5-1/ColorbyARGB (2).jpg>) | ARGB 颜色 (**Color.ByARGB**) | A,R,G,B | 颜色   |

### 查询颜色值

下表中的颜色查询用于定义相应颜色的特性：Alpha、红、绿和蓝。请注意，Color.Components 节点给出全部四个不同输出，这使得该节点更适合查询颜色的特性。

| 图标                                              | 名称（语法）                     | 输入(Inputs) | 输出    |
| ------------------------------------------------- | --------------------------------- | ------ | ---------- |
| ![](<../images/5-1/ColorAlpha(1)(1) (2) (2).jpg>) | Alpha (**Color.Alpha**)           | 颜色  | A          |
| ![](../images/5-1/ColorRed.jpg)                   | 红色 (**Color.Red**)               | 颜色  | R          |
| ![](<../images/5-1/ColorGreen(1)(1) (2) (1).jpg>) | 绿色 (**Color.Green**)           | 颜色  | G          |
| ![](../images/5-1/ColorBlue.jpg)                  | 蓝色 (**Color.Blue**)             | 颜色  | B          |
| ![](<../images/5-1/ColorComponent (2).jpg>)       | 组件 (**Color.Components**) | 颜色  | A,R,G,B |

下表中的颜色对应于 **HSB 颜色空间**。对于我们如何解释颜色，将颜色分为色调、饱和度和亮度无疑更加直观：应该是什么颜色？应该如何呈现多彩？颜色应该如何变亮或变暗？这分别是色调、饱和度和亮度的细分。

| 图标                                         | 名称（语法）                     | 输入(Inputs) | 输出 (Outputs)    |
| -------------------------------------------- | --------------------------------- | ------ | ---------- |
| ![](../images/5-1/ColorHue.jpg)              | 色调 (**Color.Hue**)               | 颜色  | 色调        |
| ![](<../images/5-1/ColorSaturation (2).jpg>) | 饱和度 (**Color.Saturation**) | 颜色  | 饱和度 |
| ![](<../images/5-1/ColorBrightness (2).jpg>) | 亮度 (**Color.Brightness**) | 颜色  | 亮度 |

### 颜色范围 (Color Range)

颜色范围类似于 [\#part-ii-from-logic-to-geometry](3-logic.md#part-ii-from-logic-to-geometry "mention")练习中的 **“Remap Range”** 节点：它将数字列表重新映射到其他域。但它不会映射到 _“数字”_ 域，而是基于 0 到 1 范围的输入数字映射到 _“颜色渐变”_。

当前节点运作良好，但第一次就使一切正常工作可能会有点勉强。熟悉颜色渐变的最佳方法是以交互方式对其进行测试。让我们快速练习，了解如何使用与数字对应的输出颜色设置渐变。

![](../images/5-3/5/color-colorrange.jpg)

> 1. 定义三种颜色：使用 **“Code Block”** 节点，通过插入相应的 _“0”_ 和 _“255”_ 组合来定义 _红色、绿色_ 和 _蓝色_。
> 2. **创建列表**：将三种颜色合并到一个列表中。
> 3. 定义索引：创建列表以定义每种颜色的夹点位置（范围从 0 到 1）。请注意，值 0.75 表示绿色。这会将绿颜色 3/4 置于颜色范围滑块中水平渐变的位置。
> 4. **代码块**：输入值（介于 0 和 1 之间）以转换为颜色。

### 颜色预览

**“Display.ByGeometry”** 节点使我们能够在 Dynamo 视口中为几何图形着色。这有助于分离不同类型的几何图形、演示参数化概念或定义用于模拟的分析图例。输入很简单：几何图形和颜色。要创建与上图类似的渐变，请将颜色输入连接到 **“Color** **Range”** 节点。

![](../images/5-3/5/color-colorpreview.jpg)

### 曲面上的颜色

**“Display.BySurfaceColors”** 节点使我们能够使用颜色在整个曲面上映射数据！此功能为可视化通过离散分析（例如日光、能量和接近度）获得的数据引入了一些令人兴奋的可能性。在 Dynamo 中将颜色应用于曲面类似于在其他 CAD 环境中将纹理应用于材质。在下面的简短练习中，我们来演示如何使用此工具。

![](../images/5-3/5/12\(1\).jpg)

## 练习

### 基本螺旋线（带颜色）

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/5-3/5/Building Blocks of Programs - Color.dyn" %}

本练习重点介绍如何以参数方式控制与几何图形平行的颜色。该几何图形是基本螺旋线，我们在下面使用 **“代码块”** 对其进行定义。这是一种用于创建参数化函数的快速而简单的方法；由于我们的重点是颜色（而不是几何图形），因此我们使用代码块来有效地创建螺旋线，而不会使画布变得混乱。随着底漆迁移到更高级的材质，我们会更频繁地使用代码块。

![](../images/5-3/5/color-basichelixwithcolors01.jpg)

> 1. **代码块**：使用上述公式定义两个代码块。这是用于创建螺旋的快速参数化方法。
> 2. **Point.ByCoordinates**：将代码块的三个输出连接到该节点的坐标。

现在，我们会看到创建螺旋线的一组点。下一步是通过这些点创建曲线，以便我们可以可视化螺旋线。

![](../images/5-3/5/color-basichelixwithcolors02.jpg)

> 1. **PolyCurve.ByPoints**：将 **“Point.ByCoordinates”** 输出连接到节点的 _“points”_ 输入。我们会得到螺旋曲线。
> 2. **Curve.PointAtParameter**：将 **“PolyCurve.ByPoints”** 输出连接到 _“curve”_ 输入。此步骤的目的是创建一个沿曲线滑动的参数化吸引器点。由于曲线将计算参数处的点，因此我们需要输入一个介于 0 和 1 之间的 _参数_ 值。
> 3. **数字滑块**：添加到画布后，将_最小_值更改为 _0.0_、将 _最大_ 值更改为 _1.0_、将 _步长_ 值更改为 _.01_。将滑块输出插入 **“Curve.PointAtParameter”** 的 _“param”_ 输入。现在，我们会沿螺旋线长度看到一个点，由滑块的百分比表示（起点处为 0，终点处为 1）。

创建参照点后，我们现在将比较参照点与定义螺旋线的原始点之间的距离。此距离值将驱动几何图形以及颜色。

![](../images/5-3/5/color-basichelixwithcolors03.jpg)

> 1. **Geometry.DistanceTo**：将 **“Curve.PointAtParameter”** 输出连接到 _“input”_。将 **“Point.ByCoordinates”** 连接到几何图形输入。
> 2. **Watch**：结果输出会显示沿曲线从每个螺旋点到参照点的距离列表。

下一步是通过从螺旋点到参照点的距离列表来驱动参数。我们使用这些距离值来沿曲线定义一系列球体的半径。为了使球体保持合适大小，我们需要 _重映射_ 距离值。

![](../images/5-3/5/color-basichelixwithcolors04.jpg)

> 1. **Math.RemapRange**：将 **“Geometry.DistanceTo”** 输出连接到数字输入。
> 2. **代码块**：将值为 _0.01_ 的代码块连接到 _“newMin”_ 输入，将值为 _1_ 的代码块连接到 _“newMax”_ 输入。
> 3. **Watch**：将 **“Math.RemapRange”** 输出连接到一个节点，并将 **“Geometry.DistanceTo”** 输出连接到另一个节点。比较结果。

此步骤已将距离列表重新映射为较小的范围。我们可以编辑 _“newMin”_ 和 _“newMax”_ 值，但我们认为合适。这些值将重新映射并在整个域中具有相同的 _分布率_。

![](../images/5-3/5/color-basichelixwithcolors05.jpg)

> 1. **Sphere.ByCenterPointRadius**：将 **“Math.RemapRange”** 输出连接到 _“radius”_ 输入，将原始 **“Point.ByCoordinates”** 输出连接到 _“centerPoint”_ 输入。

更改数字滑块的值，并观察球体更新的大小。现在，我们有了一个参数化夹具

![](../images/5-3/5/color-basichelixwithcolors06.gif)

球体的大小演示了由沿曲线的参照点定义的参数化阵列。让我们对球体半径使用相同的概念来驱动其颜色。

![](../images/5-3/5/color-basichelixwithcolors07.jpg)

> 1. **Color Range**：在画布顶部添加。将光标悬停在 _“value”_ 输入上时，我们注意到请求的数字介于 0 和 1 之间。我们需要重新映射 **“Geometry.DistanceTo”** 输出中的数字，以便它们与此域兼容。
> 2. **Sphere.ByCenterPointRadius**：目前，我们禁用此节点上的预览（_单击鼠标右键 >“预览”_）

![](../images/5-3/5/color-basichelixwithcolors08.jpg)

> 1. **Math.RemapRange**：此过程看起来很熟悉。将 **“Geometry.DistanceTo”** 输出连接到数字输入。
> 2. **Code Block**：与之前的步骤类似，为 _“newMin”_ 输入创建值 _0_，为 _“newMax”_ 输入创建值 _1_。注意，在这种情况下，我们能够在一个代码块中定义两个输出。
> 3. **Color Range**：将 **“Math.RemapRange”** 输出连接到 _“value”_ 输入。

![](../images/5-3/5/color-basichelixwithcolors09.jpg)

> 1. **Color.ByARGB**：这是我们要为创建两种颜色所执行的操作。尽管此过程看起来可能有些古怪，但它与其他软件中的 RGB 颜色相同，我们只需使用可视化编程即可实现。
> 2. **Code Block**：创建由 _0_ 和 _255_ 组成的两个值。将两个输出连接到与上图一致的两个 **“Color.ByARGB”** 输入（或创建您最喜欢的两种颜色）。
> 3. **Color Range**： _“colors”_ 输入要求一列颜色。我们需要基于上一步中创建的两种颜色创建此列表。
> 4. **List.Create**：将两种颜色合并到一个列表中。将输出连接到 **“Color Range”** 的 _“colors”_ 输入。

![](../images/5-3/5/color-basichelixwithcolors10.jpg)

> 1. **Display.ByGeometryColor**：将 **“Sphere.ByCenterPointRadius”** 连接到 _“geometry”_ 输入，将 _“Color Range”_ 连接到 _“color”_ 输入。现在，我们在曲线域上具有平滑渐变。

如果我们之前在定义中更改了 **“Number Slider”** 的值，则颜色和尺寸会更新。在这种情况下，颜色和半径大小直接相关：我们现在在两个参数之间有视觉链接！

![](../images/5-3/5/color-basichelixwithcolors11.gif)

### “曲面上的颜色”练习

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/5-3/5/BuildingBlocks of Programs - ColorOnSurface.zip" %}

首先，我们需要创建（或引用）一个曲面以用作 **“Display.BySurfaceColors”** 节点的输入。在此示例中，我们将在正弦和余弦曲线之间放样。

![](../images/5-3/5/color-coloronsurface01.jpg)

> 1. 该组节点将沿 Z 轴创建点，然后基于正弦和余弦函数置换它们。然后，使用这两个点列表生成 NURBS 曲线。
> 2. **Surface.ByLoft**：在 NURBS 曲线列表之间生成插值曲面。

![](../images/5-3/5/color-coloronsurface02.jpg)

> 1. **File Path**：选择要取样的图像文件以获取下游像素数据
> 2. 使用 **“File.FromPath”** 将文件路径转化为某个文件，然后传递给 **“Image.ReadFromFile”** 以输出图像进行采样
> 3. **Image.Pixels**：输入某个图像并提供要沿图像的 X 和 Y 标注使用的样例值。
> 4. **Slider**：为 **“Image.Pixels”** 提供样例值
> 5. **Display.BySurfaceColors**：在整个曲面上分别沿 X 和 Y 映射颜色值数组

输出曲面的特写预览，分辨率为 400x300 样例

![](../images/5-3/5/color-coloronsurface03.jpg)
