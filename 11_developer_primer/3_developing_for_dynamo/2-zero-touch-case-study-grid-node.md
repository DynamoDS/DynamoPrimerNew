# Zero-Touch 案例研究 - 网格节点

在 Visual Studio 项目启动并运行后，我们将介绍如何构建一个自定义节点来创建一个单元的矩形网格。尽管我们可以使用多个标准节点创建该矩形网格，但它是一个非常有用的工具，可轻松包含在 Zero-Touch 节点中。与网格线不同，单元可以围绕其中心点进行单元、查询其角顶点，或将其构建到面中。

本例将介绍创建 Zero-Touch 节点时需要注意的一些功能和概念。在我们构建自定义节点并将其添加到 Dynamo 中后，请确保查看“进一步了解 Zero-Touch”页面，以更深入地了解默认输入值、返回多个值、文档、对象、使用 Dynamo 几何类型和移植。

![矩形网格图形](images/cover-image.jpg)

### 自定义矩形网格节点 <a href="#custom-rectangular-grid-node" id="custom-rectangular-grid-node"></a>

要开始构建网格节点，请创建一个新的 Visual Studio 类库项目。有关如何设置项目的深入介绍，请参见“快速入门”页面。

![在 Visual Studio 中创建新项目](images/vs-new-project-1.jpg)

![在 Visual Studio 中配置新项目](images/vs-new-project-2.jpg)

> 1. 选择 `Class Library` 作为项目类型
> 2. 将项目命名为 `CustomNodes`

由于我们将要创建几何图形，因此我们需要参照相应的 NuGet 软件包。通过 Nuget 软件包管理器安装 ZeroTouchLibrary 软件包。对于 `using Autodesk.DesignScript.Geometry;` 语句而言，此软件包是必需的。

![ZeroTouchLibrary 软件包](images/vs-nugetpackage.jpg)

> 1. 浏览 ZeroTouchLibrary 软件包
> 2. 我们将要在 Dynamo Studio 的当前版本（即 1.3）中使用此节点。选择与此版本匹配的软件包版本。
> 3. 请注意，我们还将类文件重命名为 `Grids.cs`

接下来，我们需要建立 RectangularGrid 方法将在其中运行的名称空间和类。在 Dynamo 中，该节点将根据方法和类名进行命名。我们尚不需要将该节点复制到 Visual Studio 中。

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        public static List<Rectangle> RectangularGrid(int xCount, int yCount)
        {
        //The method for creating a rectangular grid will live in here
        }
    }
}
```

> `Autodesk.DesignScript.Geometry;` 将参照创建列表时所需的 ZeroTouchLibrary 软件包 `System.Collections.Generic` 中的 ProtoGeometry.dll

现在，我们可以添加用于绘制矩形的方法。类文件应如下所示，并可以复制到 Visual Studio 中。

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        public static List<Rectangle> RectangularGrid(int xCount, int yCount)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                }
            }
            return pList;
        }
    }
}
```

如果项目看起来与此类似，请继续并尝试构建 `.dll`。

![构建 DLL](images/vs-grids.jpg)

> 1. 选择“构建”>“构建解决方案”

检查项目的 `bin` 文件夹以查找 `.dll`。在构建成功后，我们可以将 `.dll` 添加到 Dynamo 中。

![Dynamo 中的自定义节点](images/RectangularGrid-Dynamo.jpg)

> 1. Dynamo 库中的自定义 RectangularGrids 节点
> 2. 画布上的自定义节点
> 3. 用于将 `.dll` 添加到 Dynamo 中的“添加”按钮

### 自定义节点修改 <a href="#custom-node-modifications" id="custom-node-modifications"></a>

在上例中，我们创建了一个非常简单的节点，该节点未定义 `RectangularGrids` 方法之外的太多其他内容。但是，我们可能要为输入端口创建工具提示，或者像标准 Dynamo 节点一样为该节点提供摘要。将这些功能添加到自定义节点会使其更易于使用，尤其是当用户要在库中搜索这些节点时。

![输入工具提示](images/nodemodification.png)

> 1. 默认输入值
> 2. xCount 输入的工具提示

RectangularGrid 节点需要其中的一些基本功能。在下面的代码中，我们添加了输入和输出端口描述、摘要和默认输入值。

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        /// <summary>
        /// This method creates a rectangular grid from an X and Y count.
        /// </summary>
        /// <param name="xCount">Number of grid cells in the X direction</param>
        /// <param name="yCount">Number of grid cells in the Y direction</param>
        /// <returns>A list of rectangles</returns>
        /// <search>grid, rectangle</search>
        public static List<Rectangle> RectangularGrid(int xCount = 10, int yCount = 10)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                    Point cPt = rect.Center();
                }
            }
            return pList;
        }
    }
}
```

* 通过为方法参数赋值，来为输入提供默认值：`RectangularGrid(int xCount = 10, int yCount = 10)`
* 创建输入和输出工具提示、搜索关键字以及前面带有 `///` 的 XML 文档摘要。

要添加工具提示，我们需要在项目目录中有一个 xml 文件。通过启用该选项，`.xml` 可以由 Visual Studio 自动生成。

![启用 XML 文档](images/vs-xml.jpg)

> 1. 在此处启用 XML 文档文件，然后指定文件路径。这将生成一个 XML 文件。

大功告成！我们已创建了一个包含多个标准功能的新节点。以下“Zero-Touch 基础知识”章节将更详细地介绍 Zero-Touch 节点开发以及需要注意的问题。
