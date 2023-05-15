# NodeModel 案例研究 - 自定义 UI

相较于 Zero-Touch 节点，基于 NodeModel 的节点提供了更大的灵活性和更强大的功能。在本例中，我们将通过添加一个随机化矩形大小的集成滑块，来将 Zero-Touch 网格节点提升到下一个级别。

![矩形网格图形](images/cover-image-2.jpg)

> 滑块会相对于单元大小缩放单元，因此用户不必为滑块提供正确的范围。

#### Model-View-Viewmodel 模式 <a href="#the-model-view-viewmodel-pattern" id="the-model-view-viewmodel-pattern"></a>

Dynamo 基于 [model-view-viewmodel](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel) (MVVM) 软件体系结构模式，以使 UI 与后端保持分离。当创建 ZeroTouch 节点时，Dynamo 会在节点的数据与其 UI 之间绑定数据。要创建自定义 UI，我们必须添加数据绑定逻辑。

在较高级别上，在 Dynamo 中建立模型-视图关系有两个部分：

* `NodeModel` 类用于建立节点的核心逻辑（“模型”）
* `INodeViewCustomization` 类用于自定义如何查看 `NodeModel`（“视图”）

> NodeModel 对象已有关联的视图-模型 (NodeViewModel)，因此我们可以仅关注自定义 UI 的模型和视图。

#### 如何实现 NodeModel <a href="#how-to-implement-nodemodel" id="how-to-implement-nodemodel"></a>

NodeModel 节点与 Zero-Touch 节点有几个明显差异，我们将在本例中介绍这些差异。在我们进入 UI 自定义之前，让我们先构建 NodeModel 逻辑。

**1\.创建项目结构：**

NodeModel 节点只能调用函数，因此我们需要将 NodeModel 和函数分离到不同的库中。对 Dynamo 软件包执行此操作的标准方法是为每个软件包创建单独的项目。先创建一个新的解决方案来包含项目。

> 1. 选择 `File > New > Project`
> 2. 选择 `Other Project Types` 以显示“解决方案”选项
> 3. 选择 `Blank Solution`
> 4. 将解决方案命名为 `CustomNodeModel`
> 5. 选择 `Ok`

在解决方案中创建两个 C# 类库项目：一个用于函数，一个用于实现 NodeModel 接口。

![添加新类库](images/vs-new-class-projects.jpg)

> 1. 在解决方案上单击鼠标右键，然后选择 `Add > New Project`
> 2. 选择类库
> 3. 将它命名为 `CustomNodeModel`
> 4. 单击 `Ok`
> 5. 重复该过程以添加另一个名为 `CustomNodeModelFunctions` 的项目

接下来，我们需要重命名自动创建的类库，然后将其添加到 `CustomNodeModel` 项目中。类 `GridNodeModel` 实现抽象的 NodeModel 类、`GridNodeView` 用于自定义视图，`GridFunction` 包含我们需要调用的任何函数。

![解决方案资源管理器](images/vs-new-class.jpg)

> 1. 添加另一个类，方法是在 `CustomNodeModel` 项目上单击鼠标右键、选择 `Add > New Item...`，然后选择 `Class`。
> 2. 在 `CustomNodeModel` 项目中，我们需要 `GridNodeModel.cs` 和 `GridNodeView.cs` 类
> 3. 在 `CustomNodeModelFunction` 项目中，我们需要 `GridFunctions.cs` 类

在我们将任何代码添加到类之前，请为此项目添加必需的软件包。`CustomNodeModel` 将需要 ZeroTouchLibrary 和 WpfUILibrary，`CustomNodeModelFunction` 将仅需要 ZeroTouchLibrary。WpfUILibrary 将用于我们稍后进行操作的 UI 自定义，ZeroTouchLibrary 将用于创建几何图形。可以为项目单独添加软件包。由于这些软件包具有依存关系，因此将自动安装 Core 和 DynamoServices。

![安装软件包](images/vs-add-packages.jpg)

> 1. 在项目上单击鼠标右键，然后选择 `Manage NuGet Packages`
> 2. 仅安装该项目所需的软件包

Visual Studio 会将我们参照的 NuGet 软件包复制到构建目录中。这可以设置为 false，这样一来软件包中就不会存在任何不必要的文件。

![禁用本地软件包复制](images/vs-disable-package-copying.jpg)

> 1. 选择 Dynamo NuGet 软件包
> 2. 将 `Copy Local` 设置为 false

**2\.继承 NodeModel 类**

如前所述，使 NodeModel 节点与 ZeroTouch 节点不同的主要方面是其对 `NodeModel` 类的实现。NodeModel 节点需要此类中的多个函数，我们可以通过在类名后添加 `:NodeModel` 来获取这些函数。

将以下代码复制到 `GridNodeModel.cs` 中。

```
using System;
using System.Collections.Generic;
using Dynamo.Graph.Nodes;
using CustomNodeModel.CustomNodeModelFunction;
using ProtoCore.AST.AssociativeAST;
using Autodesk.DesignScript.Geometry;

namespace CustomNodeModel.CustomNodeModel
{
    [NodeName("RectangularGrid")]
    [NodeDescription("An example NodeModel node that creates a rectangular grid. The slider randomly scales the cells.")]
    [NodeCategory("CustomNodeModel")]
    [InPortNames("xCount", "yCount")]
    [InPortTypes("double", "double")]
    [InPortDescriptions("Number of cells in the X direction", "Number of cells in the Y direction")]
    [OutPortNames("Rectangles")]
    [OutPortTypes("Autodesk.DesignScript.Geometry.Rectangle[]")]
    [OutPortDescriptions("A list of rectangles")]
    [IsDesignScriptCompatible]
    public class GridNodeModel : NodeModel
    {
        private double _sliderValue;
        public double SliderValue
        {
            get { return _sliderValue; }
            set
            {
                _sliderValue = value;
                RaisePropertyChanged("SliderValue");
                OnNodeModified(false);
            }
        }
        public GridNodeModel()
        {
            RegisterAllPorts();
        }
        public override IEnumerable<AssociativeNode> BuildOutputAst(List<AssociativeNode> inputAstNodes)
        {
            if (!HasConnectedInput(0) || !HasConnectedInput(1))
            {
                return new[] { AstFactory.BuildAssignment(GetAstIdentifierForOutputIndex(0), AstFactory.BuildNullNode()) };
            }
            var sliderValue = AstFactory.BuildDoubleNode(SliderValue);
            var functionCall =
              AstFactory.BuildFunctionCall(
                new Func<int, int, double, List<Rectangle>>(GridFunction.RectangularGrid),
                new List<AssociativeNode> { inputAstNodes[0], inputAstNodes[1], sliderValue });

            return new[] { AstFactory.BuildAssignment(GetAstIdentifierForOutputIndex(0), functionCall) };
        }
    }
}
```

这不同于 Zero-Touch 节点。让我们了解一下每个部分的作用。

* 指定节点属性，如名称、类别、InPort/OutPort 名称、InPort/OutPort 类型、描述。
* `public class GridNodeModel : NodeModel` 是一个从 `Dynamo.Graph.Nodes` 中继承 `NodeModel` 类的类。
* `public GridNodeModel() { RegisterAllPorts(); }` 是一个注册节点输入和输出的构造函数。
* `BuildOutputAst()` 返回 AST（抽象语法树），这是从 NodeModel 节点返回数据所需的结构。
* `AstFactory.BuildFunctionCall()` 调用 `GridFunctions.cs` 中的 RectangularGrid 函数。
* `new Func<int, int, double, List<Rectangle>>(GridFunction.RectangularGrid)` 指定函数及其参数。
* `new List<AssociativeNode> { inputAstNodes[0], inputAstNodes[1], sliderValue });` 将节点输入映射到函数参数。
* 如果输入端口未连接，则 `AstFactory.BuildNullNode()` 会构建空节点。这是为了避免在节点上显示警告。
* `RaisePropertyChanged("SliderValue")` 会在滑块值发生更改时通知 UI
* `var sliderValue = AstFactory.BuildDoubleNode(SliderValue)` 在 AST 中构建表示滑块值的节点
* 将输入更改为 functionCall 变量 `new List<AssociativeNode> { inputAstNodes[0], sliderValue });` 中的 `sliderValue` 变量

**3\.调用函数**

`CustomNodeModelFunction` 项目将构建到 `CustomNodeModel` 中的单独程序集，以便可以调用它。

将以下代码复制到 `GridFunction.cs` 中。

```
using Autodesk.DesignScript.Geometry;
using Autodesk.DesignScript.Runtime;
using System;
using System.Collections.Generic;

namespace CustomNodeModel.CustomNodeModelFunction
{
    [IsVisibleInDynamoLibrary(false)]
    public class GridFunction
    {
        [IsVisibleInDynamoLibrary(false)]
        public static List<Rectangle> RectangularGrid(int xCount = 10, int yCount = 10, double rand = 1)
        {
            double x = 0;
            double y = 0;

            Point pt = null;
            Vector vec = null;
            Plane bP = null;

            Random rnd = new Random(2);

            var pList = new List<Rectangle>();
            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    double rNum = rnd.NextDouble();
                    double scale = rNum * (1 - rand) + rand;
                    x++;
                    pt = Point.ByCoordinates(x, y);
                    vec = Vector.ZAxis();
                    bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, scale, scale);
                    pList.Add(rect);
                }
            }
            pt.Dispose();
            vec.Dispose();
            bP.Dispose();
            return pList;
        }
    }
}
```

此函数类与 Zero-Touch 网格案例研究非常相似，但有一点不同：

* 由于已从 `CustomNodeModel` 调用函数，因此 `[IsVisibleInDynamoLibrary(false)]` 会阻止 Dynamo“看到”以下方法和类。

正如我们为 NuGet 软件包添加参照一样，`CustomNodeModel` 需要参照 `CustomNodeModelFunction` 才能调用函数。

![添加参照](images/vs-add-project-reference.jpg)

> 在我们参照函数之前，CustomNodeModel 的 using 语句将处于不活动状态
>
> 1. 在 `CustomNodeModel` 上单击鼠标右键，然后选择 `Add > Reference`
> 2. 选择 `Projects > Solution`
> 3. 选中 `CustomNodeModelFunction`
> 4. 单击 `Ok`

**4\.自定义视图**

要创建滑块，我们需要通过实现 `INodeViewCustomization` 接口来自定义 UI。

将以下代码复制到 `GridNodeView.cs` 中

```
using Dynamo.Controls;
using Dynamo.Wpf;

namespace CustomNodeModel.CustomNodeModel
{
    public class CustomNodeModelView : INodeViewCustomization<GridNodeModel>
    {
        public void CustomizeView(GridNodeModel model, NodeView nodeView)
        {
            var slider = new Slider();
            nodeView.inputGrid.Children.Add(slider);
            slider.DataContext = model;
        }

        public void Dispose()
        {
        }
    }
}
```

* `public class CustomNodeModelView : INodeViewCustomization<GridNodeModel>` 定义自定义 UI 所需的功能。

在完成设置项目结构后，使用 Visual Studio 的设计环境构建用户控件并在 `.xaml` 文件中定义其参数。通过工具箱，将滑块添加到 `<Grid>...</Grid>`。

![添加新滑块](images/vs-usercontrol.jpg)

> 1. 在 `CustomNodeModel` 上单击鼠标右键，然后选择 `Add > New Item`
> 2. 选择 `WPF`
> 3. 将用户控件命名为 `Slider`
> 4. 单击 `Add`

将以下代码复制到 `Slider.xaml` 中

```
<UserControl x:Class="CustomNodeModel.CustomNodeModel.Slider"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:CustomNodeModel.CustomNodeModel"
             mc:Ignorable="d" 
             d:DesignHeight="75" d:DesignWidth="100">
    <Grid Margin="10">
        <Slider Grid.Row="0" Width="80" Minimum="0" Maximum="1" IsSnapToTickEnabled="True" TickFrequency="0.01" Value="{Binding SliderValue}"/>
    </Grid>
</UserControl>
```

* 在 `.xaml` 文件中定义滑块控件的参数。_Minimum 和 Maximum_ 属性定义此滑块的数值范围。
* 在 `<Grid>...</Grid>` 中，我们可以通过 Visual Studio 工具箱放置不同的用户控件

当创建 `Slider.xaml` 文件时，Visual Studio 会自动创建一个名为 `Slider.xaml.cs` 的 C# 文件，用于初始化滑块。更改此文件中的名称空间。

```
using System.Windows.Controls;

namespace CustomNodeModel.CustomNodeModel
{
    /// <summary>
    /// Interaction logic for Slider.xaml
    /// </summary>
    public partial class Slider : UserControl
    {
        public Slider()
        {
            InitializeComponent();
        }
    }
}
```

* 该名称空间应为 `CustomNodeModel.CustomNodeModel`

`GridNodeModel.cs` 定义滑块计算逻辑。

** 5.配置为软件包**

在我们构建项目之前，最后一步是添加一个 `pkg.json` 文件，以便 Dynamo 可以读取软件包。

![添加 JSON 文件](images/vs-pkg-json.jpg)

> 1. 在 `CustomNodeModel` 上单击鼠标右键，然后选择 `Add > New Item`
> 2. 选择 `Web`
> 3. 选择 `JSON File`
> 4. 将该文件命名为 `pkg.json`
> 5. 单击 `Add`

* 将以下代码复制到 `pkg.json` 中

```
{
  "license": "MIT",
  "file_hash": null,
  "name": "CustomNodeModel",
  "version": "1.0.0",
  "description": "Sample node",
  "group": "CustomNodes",
  "keywords": [ "grid", "random" ],
  "dependencies": [],
  "contents": "Sample node",
  "engine_version": "1.3.0",
  "engine": "dynamo",
  "engine_metadata": "",
  "site_url": "",
  "repository_url": "",
  "contains_binaries": true,
  "node_libraries": [
    "CustomNodeModel, Version=1.0.0, Culture=neutral, PublicKeyToken=null",
    "CustomNodeModelFunction, Version=1.0.0, Culture=neutral, PublicKeyToken=null"
  ]
}
```

* `"name":` 确定软件包的名称及其在 Dynamo 库中的分组
* `"keywords":` 提供用于搜索 Dynamo 库的搜索词
*   `"node_libraries": []` 指示与软件包关联的库

    最后一步是构建解决方案并发布为 Dynamo 软件包。请参见“软件包展开”一章，以了解如何在联机发布之前创建本地软件包以及如何直接从 Visual Studio 构建软件包。
