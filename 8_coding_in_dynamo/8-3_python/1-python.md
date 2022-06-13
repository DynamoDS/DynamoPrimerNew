# Python 节点

为什么要在 Dynamo 的可视化编程环境中使用文本编程？[可视化编程](../../a\_appendix/visual-programming-and-dynamo.md)有许多优势。它使您无需在直观的可视化界面中学习特殊语法即可创建程序。但是，可视化程序可能会变得混乱，有时可能会在功能上有所降低。例如，Python 提供了更多可实现的方法来编写条件语句 (if/then) 和循环。Python 是一款功能强大的工具，可扩展 Dynamo 的功能，并允许您将许多节点替换为几行简明的代码。

**可视化程序：**

![](<../images/8-3/1/python node - visual vs textual programming.jpg>)

**文本程序**：

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

solid = IN[0]
seed = IN[1]
xCount = IN[2]
yCount = IN[3]

solids = []

yDist = solid.BoundingBox.MaxPoint.Y-solid.BoundingBox.MinPoint.Y
xDist = solid.BoundingBox.MaxPoint.X-solid.BoundingBox.MinPoint.X

for i in xRange:
	for j in yRange:
		fromCoord = solid.ContextCoordinateSystem
		toCoord = fromCoord.Rotate(solid.ContextCoordinateSystem.Origin,Vector.ByCoordinates(0,0,1),(90*(i+j%val)))
		vec = Vector.ByCoordinates((xDist*i),(yDist*j),0)
		toCoord = toCoord.Translate(vec)
		solids.append(solid.Transform(fromCoord,toCoord))

OUT = solids
```

### Python 节点

与代码块一样，Python 节点也是可视化编程环境中的脚本编写界面。Python 节点位于库中的“脚本”>“编辑器”>“Python 脚本”下。

![](<../images/8-3/1/python node - the python node 01.jpg>)

双击节点会打开 Python 脚本编辑器（也可以在节点上单击鼠标右键，然后选择_“编辑...”_）。 您会注意到顶部的一些样本文字，旨在帮助您引用所需的库。输入存储在 IN 数组中。通过将值指定给 OUT 变量，即可将这些值返回给 Dynamo

![](<../images/8-3/1/python node - the python node 02.jpg>)

Autodesk.DesignScript.Geometry 库允许您使用与代码块类似的点符号。有关 Dynamo 语法的详细信息，请参见 [7-2\_design-script-syntax.md](../../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md "提及")以及 [DesignScript 手册](https://dynamobim.org/wp-content/links/DesignScriptGuide.pdf)（要下载此 PDF 文档，请在链接上单击鼠标右键并选择“将链接另存为...”）。键入几何图形类型（例如“Point.”）将显示用于创建和查询点的一列方法。

![](<../images/8-3/1/python node - the python node 03.jpg>)

> 方法包括构造函数（如 _ByCoordinates_）、操作（如 _Add_）和查询（如 _X_、_Y_ 和 _Z_ 坐标）。

## 练习：使用 Python 脚本从实体模块创建图案的自定义节点

### 第 I 部分：创建 Python 脚本

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/8-2/1/Python_Custom-Node.dyn" %}

在本示例中，我们将编写一个 Python 脚本，该脚本用于从实体模块创建图案，并将其转换为自定义节点。首先，我们使用 Dynamo 节点创建实体模块。

![](<../images/8-3/1/python node - exercise pt I-01.jpg>)

> 1. **Rectangle.ByWidthLength**：创建一个矩形，它将作为实体的基础。
> 2. **Surface.ByPatch**：将矩形连接到“_closedCurve_”输入以创建底部曲面。

![](<../images/8-3/1/python node - exercise pt I-02.jpg>)

> 1. **Geometry.Translate**：将矩形连接到“_geometry_”输入以向上移动它，从而使用代码块指定实体的基础厚度。
> 2. **Polygon.Points**：查询平移的矩形以提取角点。
> 3. **Geometry.Translate**：使用代码块创建与四个点对应的一列四个值，从而向上平移实体的一个角。
> 4. **Polygon.ByPoints**：使用平移的点来重建顶部多边形。
> 5. **Surface.ByPatch**：连接多边形以创建顶部曲面。

现在我们有了顶面和底面，接下来让我们在两个轮廓之间放样来创建实体的侧面。

![](<../images/8-3/1/python node - exercise pt I-03.jpg>)

> 1. **List.Create**：将底部矩形和顶部多边形连接到索引输入。
> 2. **Surface.ByLoft**：放样两个轮廓以创建实体的侧面。
> 3. **List.Create**：将顶面、侧面和底面连接到索引输入以创建曲面列表。
> 4. **Solid.ByJoinedSurfaces**：连接曲面以创建实体模块。

现在我们有了实体，接下来将 Python 脚本节点拖动到工作空间。

![](<../images/8-3/1/python node - exercise pt I-04.jpg>)

> 1. 要向节点添加其他输入，请单击节点上的“+”图标。输入内容命名为 IN\[0]、IN\[1] 等。以指示它们表示列表中的各项目。

首先定义输入和输出。双击该节点以打开 Python 编辑器。 按照下面的代码，在编辑器中修改代码。

![](<../images/8-3/1/python node - exercise pt I-05.jpg>)

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []

# Place your code below this line


# Assign your output to the OUT variable.
OUT = solids
```

随着我们在练习中的进展，此代码将更有意义。接下来，我们需要考虑排列实体模块所需的信息。首先，我们需要知道实体的尺寸以确定平动距离。由于存在边界框 Bug，因此我们需要使用边曲线几何图形来创建边界框。

![](../images/8-3/1/python07.png)

> 在 Dynamo 中查看 Python 节点。请注意，我们使用的语法与在 Dynamo 中节点标题中看到的语法相同。查看下面注释的代码。

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []
#Create an empty list for the edge curves
crvs = []

# Place your code below this line
#Loop through edges an append corresponding curve geometry to the list
for edge in solid.Edges:
    crvs.append(edge.CurveGeometry)

#Get the bounding box of the curves
bbox = BoundingBox.ByGeometry(crvs)

#Get the x and y translation distance based on the bounding box
yDist = bbox.MaxPoint.Y-bbox.MinPoint.Y
xDist = bbox.MaxPoint.X-bbox.MinPoint.X

# Assign your output to the OUT variable.
OUT = solids
```

由于我们将平移并旋转实体模块，因此我们使用 Geometry.Transform 操作。通过查看 Geometry.Transform 节点，我们知道需要源坐标系和目标坐标系来变换实体。源是实体的上下文坐标系，而目标是每个阵列模块的不同坐标系。这意味着我们需要遍历 X 和 Y 值，以每次变换不同的坐标系。

![](<../images/8-3/1/python node - exercise pt I-06.jpg>)

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []
#Create an empty list for the edge curves
crvs = []

# Place your code below this line
#Loop through edges an append corresponding curve geometry to the list
for edge in solid.Edges:
    crvs.append(edge.CurveGeometry)

#Get the bounding box of the curves
bbox = BoundingBox.ByGeometry(crvs)

#Get the x and y translation distance based on the bounding box
yDist = bbox.MaxPoint.Y-bbox.MinPoint.Y
xDist = bbox.MaxPoint.X-bbox.MinPoint.X

#Get the source coordinate system
fromCoord = solid.ContextCoordinateSystem

#Loop through x and y
for i in range(xCount):
    for j in range(yCount):
        #Rotate and translate the coordinate system
        toCoord = fromCoord.Rotate(solid.ContextCoordinateSystem.Origin, Vector.ByCoordinates(0,0,1), (90*(i+j%seed)))
        vec = Vector.ByCoordinates((xDist*i),(yDist*j),0)
        toCoord = toCoord.Translate(vec)
        #Transform the solid from the source coord syste, to the target coord system and append to the list
        solids.append(solid.Transform(fromCoord,toCoord))

# Assign your output to the OUT variable.
OUT = solids
```

单击“运行”，然后保存代码。将 Python 节点与现有脚本连接，如下所示。

![](<../images/8-3/1/python node - exercise pt I-07.jpg>)

> 1. 将**“Solid.ByJoinedSurfaces”**的输出连接为 Python 节点的第一个输入，然后使用“代码块”定义其他输入。
> 2. 创建**“Topology.Edges”**节点，并使用 Python 节点的输出作为其输入。
> 3. 最后，创建**“Edge.CurveGeometry”**节点，并使用“Topology.Edges”的输出作为其输入。

尝试更改种子值以创建不同的图案。还可以更改实体模块本身的参数以实现不同的效果。

![](../images/8-3/1/python10.png)

### 第 II 部分：将 Python 脚本节点转换为自定义节点

现在，我们已创建了一个有用的 Python 脚本，接下来我们将它另存为一个自定义节点。选择 Python 脚本节点、在工作空间上单击鼠标右键，然后选择“创建自定义节点”。

![](<../images/8-3/1/python node - exercise pt II-01.jpg>)

指定名称、描述和类别。

![](<../images/8-3/1/python node - exercise pt II-02.jpg>)

这将打开一个新的工作空间，可以在其中编辑自定义节点。

![](<../images/8-3/1/python node - exercise pt II-03.jpg>)

> 1. **输入**：将输入名称更改为更具描述性的名称，并添加数据类型和默认值。
> 2. **输出：**更改输出名称

将节点另存为 .dyf 文件，您应该会看到自定义节点反映了我们刚才所做的更改。

![](<../images/8-3/1/python node - exercise pt II-04.jpg>)
