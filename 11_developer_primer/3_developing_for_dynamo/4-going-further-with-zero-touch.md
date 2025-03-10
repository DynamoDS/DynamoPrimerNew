# 进一步了解 Zero-Touch

在了解如何创建 Zero-Touch 项目后，我们可以通过浏览 Dynamo Github 中的 ZeroTouchEssentials 示例，来更深入地了解创建节点的具体细节。

![Zero-Touch 节点](images/ootbzerotouch.png)

> Dynamo 的许多标准节点本质上都是 Zero-Touch 节点，就像上面的大多数 Math、Color 和 DateTime 节点一样。

首先，从以下位置下载 ZeroTouchEssentials 项目：[https://github.com/DynamoDS/ZeroTouchEssentials](https://github.com/DynamoDS/ZeroTouchEssentials)

在 Visual Studio 中，打开 `ZeroTouchEssentials.sln` 解决方案文件并构建解决方案。

![Visual Studio 中的 ZeroTouchEssentials](images/vs-build-zte.jpg)

> `ZeroTouchEssentials.cs` 文件包含我们将要输入到 Dynamo 中的所有方法。

打开 Dynamo 并输入 `ZeroTouchEssentials.dll`，以获取我们将要在以下示例中参照的节点。

代码示例拉取自 [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/master/ZeroTouchEssentials/ZeroTouchEssentials.cs)，通常与之匹配。为了保持简洁，已删除 XML 文档；每个代码示例都会在其上方的图像中创建节点。

### 默认输入值 <a href="#default-input-values" id="default-input-values"></a>

Dynamo 支持为节点上的输入端口定义默认值。如果端口没有连接，则这些默认值会提供给节点。默认值是使用[《C# 编程手册》](https://msdn.microsoft.com/en-us/library/dd264739.aspx)中指定可选参数的 C# 机制来表示的。通过以下方式指定默认值：

* 将方法参数设置为默认值：`inputNumber = 2.0`

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // Set the method parameter to a default value
        public static double MultiplyByTwo(double inputNumber = 2.0) 
        {
            return inputNumber * 2.0;
        }
    }
}
```

![默认值](images/defaultval.jpg)

> 1. 将光标悬停在节点输入端口上方时，即会显示默认值

### 返回多个值 <a href="#returning-multiple-values" id="returning-multiple-values"></a>

返回多个值比创建多个输入要复杂得多，并且需要使用字典返回。字典的条目会成为节点输出端的端口。通过以下方式创建多个返回端口：

* 添加 `using System.Collections.Generic;` 以使用 `Dictionary<>`。
* 添加 `using Autodesk.DesignScript.Runtime;` 以使用 `MultiReturn` 属性。这会参照 DynamoServices NuGet 软件包中的“DynamoServices.dll”。
* 将 `[MultiReturn(new[] { "string1", "string2", ... more strings here })]` 属性添加到方法。这些字符串会引用字典中的键，并会成为输出端口名称。
* 从函数返回 `Dictionary<>`，其中包含与属性中的参数名称匹配的键：`return new Dictionary<string, object>`

```
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        [MultiReturn(new[] { "add", "mult" })]
        public static Dictionary<string, object> ReturnMultiExample(double a, double b)
        {
            return new Dictionary<string, object>

                { "add", (a + b) },
                { "mult", (a * b) }
            };
        }
    }
}
```

> 请参见 [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L70) 中的此代码示例

返回多个输出的节点。

![多个输出](images/multipleoutputs.png)

> 1. 请注意，现在有两个输出端口，它们是根据我们为字典的键输入的字符串命名的。

### 文档、工具提示和搜索 <a href="#documentation-tooltips-and-search" id="documentation-tooltips-and-search"></a>

最佳做法是向 Dynamo 节点添加描述节点的功能、输入、输出、搜索标记等的文档。这是通过 XML 文档标记实现的。通过以下方式创建 XML 文档：

* 任何前面带有三个正斜杠的注释文字都会被视为文档
  * 例如：`/// Documentation text and XML goes here`
* 在三个斜杠之后，在 Dynamo 输入 .dll 时将读取的方法上方创建 XML 标记
  * 例如：`/// <summary>...</summary>`
* 在 Visual Studio 中，通过选择 `Project > [Project] Properties > Build > Output` 并选中 `Documentation file` 来启用 XML 文档

![生成 XML 文件](images/vs-xml.jpg)

> 1. Visual Studio 将在指定位置处生成 XML 文件

标记的类型如下所示：

* `/// <summary>...</summary>` 是节点的主文档，将在左侧搜索栏中作为工具提示显示在节点上方
* `/// <param name="inputName">...</param>` 将为特定输入参数创建文档
* `/// <returns>...</returns>` 将为输出参数创建文档
* `/// <returns name = "outputName">...</returns>` 将为多个输出参数创建文档
* `/// <search>...</search>` 将根据逗号分隔的列表将您的节点与搜索结果匹配。例如，如果我们创建一个细分网格的节点，则我们可能需要添加诸如“mesh”、“subdivision”和“catmull-clark”之类的标记。

以下是一个示例节点，其中包含输入和输出描述，以及将在库中显示的摘要。

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        /// <summary>
        /// This method demonstrates how to use a native geometry object from Dynamo
        /// in a custom method
        /// </summary>
        /// <param name="curve">Input Curve. This can be of any type deriving from Curve, such as NurbsCurve, Arc, Circle, etc</param>
        /// <returns>The twice the length of the Curve </returns>
        /// <search>example,curve</search>
        public static double DoubleLength(Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> 请参见 [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L80) 中的此代码示例

请注意，此示例节点的代码包含：

> 1. 节点摘要
> 2. 输入描述
> 3. 输出描述

#### Dynamo 节点描述最佳实践 

节点描述简要介绍了节点的功能和输出。在 Dynamo 中，它们显示在两个位置：

- 在节点工具提示中
- 在文档浏览器中

![节点描述](images/node-description.png)

请遵循以下准则以确保一致性，并帮助在编写或更新节点描述时节省时间。

##### 概述

描述应该是一到两句话。如果需要更多信息，请将其包含在文档浏览器的“深度”下。

句首字母大写（将句子的第一个单词和任何专有名词大写）。结束时没有句号。

语言应尽可能清晰和简单。首次提及时应定义首字母缩略词，除非非专业用户也知道这些缩略词。

始终优先考虑明确性，即使这意味着偏离这些准则。

##### Guidelines

| 该做的事      | 不该做的事 |
| ----------- | ----------- |
| 以第三人称动词开始描述。<ul><li>示例：*确定*一个几何图形对象是否与另一个几何图形对象相交</li></ul>      | 不要以第二人称动词或任何名词开头。<ul><li>示例：*确定*一个几何图形对象是否与另一个几何图形对象相交</li></ul>       |
| 使用“返回”、“创建”或其他描述性动词代替“获取”。<ul><li>示例：*返回*曲面的 Nurbs 表示</li></ul>   | 请勿使用“获取”。它不太具体，有几种可能的翻译。<ul><li>示例：*获取*曲面的 Nurbs 表示</li></ul>        |
| 在引用输入时，请使用“给定”或“输入”，而不是“指定”或任何其他术语。尽可能省略“给定”或“输入”，以简化说明并减少字数。<ul><li>示例：删除*给定*文件</li><li>示例：沿*给定*投影方向将曲线投影到*给定*基础几何图形上</li></ul>当不直接引用输入时，可以使用“指定”。<ul><li>示例：按给定路径将文本内容写入*指定*的文件</li></ul>       | 在引用输入时，为了确保一致性，请勿使用“指定”或除“给定”或“输入”以外的任何其他术语。不要在同一描述中混合使用“给定”和“输入”，除非需要澄清。<ul><li>示例：删除*指定*的文件</li><li>示例：沿*给定*投影方向将*输入*曲线投影到*指定*的基础几何图形上</li></ul>      |
| 首次引用输入时，请使用“一个”或“某个”。为清楚起见，根据需要使用“该给定”或“该输入”而不是“一个”或“某个”。<ul><li>示例：沿路径曲线扫掠*一条*曲线</li></ul>      | 在第一次引用输入时，不要使用“此”。<ul><li>示例：沿路径曲线扫掠*此*曲线      |
| 当第一次引用作为节点操作目标的输出或其他名词时，请使用“一个”或“某个”。仅当将其与“输入”或“给定”配对时，才使用“该”。<ul><li>示例：复制*一个*文件</li><li>示例：复制*该给定*文件</li></ul>      | 首次引用作为节点操作目标的输出或其他名词时，请勿单独使用“该”。<ul><li>示例：复制*该*文件</li></ul>      |
| 将句子的第一个单词和任何专有名词（例如姓名和传统大写名词）大写。<ul><li>示例： 返回两个 *BoundingBoxes* 的交点</li></ul>      | 不要将常见的几何图形对象和概念大写，除非需要澄清。<ul><li>示例：围绕给定*平面*非均匀缩放      |
| 大写布尔值。当引用布尔值的输出时，请大写 True 和 False。<ul><li>示例：如果两个值不同，则返回 *True*</li><li>示例：根据 *Boolean* 参数将字符串全部转换为大写或小写字符      | 不要小写布尔值。在引用布尔值的输出时，不要将 True 和 False 小写。<ul><li>示例：如果两个值不同，则返回 *true*</li><li>示例：根据 *boolean* 参数将字符串全部转换为大写字符或小写字符</li></ul>

#### Dynamo 节点警告和错误

节点警告和错误警报用户有关图形的问题。它们通过在节点上方显示图标和展开的文本气泡来通知用户干扰正常图表操作的问题。节点错误和警告的严重性可能会有所不同：某些图形可以充分运行并显示警告，而另一些图形则会阻止预期结果。在所有情况下，节点错误和警告都是使用户及时了解其图形问题的重要工具。

有关在编写或更新节点警告和错误消息时确保一致性并帮助节省时间的指南，请参见[内容模式：节点警告和错误](https://github.com/DynamoDS/Dynamo/wiki/Content-Pattern:-Node-Warnings-and-Errors) Wiki 页面。

### 对象 <a href="#objects" id="objects"></a>

Dynamo 没有 `new` 关键字，因此需要使用静态构造方法来构造对象。通过以下方式构造对象：

* 除非另有要求，否则使构造函数成为内部构造函数 `internal ZeroTouchEssentials()`
* 使用静态方法构造对象，例如 `public static ZeroTouchEssentials ByTwoDoubles(a, b)`

> 注意：Dynamo 使用“By”前缀来指示静态方法是构造函数；尽管这是可选方法，但使用“By”将帮助您的库更好地适应现有 Dynamo 样式。

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        private double _a;
        private double _b;

        // Make the constructor internal
        internal ZeroTouchEssentials(double a, double b)
        {
            _a = a;
            _b = b;
        }

        // The static method that Dynamo will convert into a Create node
        public static ZeroTouchEssentials ByTwoDoubles(double a, double b)
        {
            return new ZeroTouchEssentials(a, b);
        }
    }
}
```

> 请参见 [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L26) 中的此代码示例

在已输入 ZeroTouchEssentials dll 后，库中会有一个 ZeroTouchEssentials 节点。可以使用 `ByTwoDoubles` 节点创建此对象。

![ByTwoDoubles 节点](images/dyn-constructor.jpg)

### 使用 Dynamo 几何图形类型 <a href="#using-dynamo-geometry-types" id="using-dynamo-geometry-types"></a>

Dynamo 库可以使用原生 Dynamo 几何图形类型作为输入，并创建新的几何图形作为输出。通过以下方式创建几何图形类型：

* 通过在 C# 文件顶部包含 `using Autodesk.DesignScript.Geometry;` 并将 ZeroTouchLibrary NuGet 软件包添加到项目中，可参照项目中的“ProtoGeometry.dll”。
* **重要信息：** 管理未从函数中返回的几何图形资源，请参见下面的 **Dispose/using 语句**部分。

> 注意：Dynamo 几何图形对象与传递给函数的任何其他对象一样使用。

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // "Autodesk.DesignScript.Geometry.Curve" is specifying the type of geometry input, 
        // just as you would specify a double, string, or integer 
        public static double DoubleLength(Autodesk.DesignScript.Geometry.Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> 请参见 [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86) 中的此代码示例

获取曲线长度并使其加倍的节点。

![曲线输入](images/doublelength.png)

> 1. 此节点接受曲线几何图形类型作为输入。

### Dispose/using 语句 <a href="#disposeusing-statements" id="disposeusing-statements"></a>

除非您使用的是 Dynamo 2.5 版或更高版本，否则不会从函数中返回的几何图形资源需要进行手动管理。在 Dynamo 2.5 及更高版本中，几何图形资源由系统进行内部处理；但是，如果您有复杂的用例，或者您必须在确定时间减少内存，则可能仍需要手动处理几何图形。Dynamo 引擎将处理从函数中返回的任何几何图形资源。可以通过以下方式手动处理未返回的几何图形资源：

*   使用 using 语句：

    ```
    using (Point p1 = Point.ByCoordinates(0, 0, 0))
    {
      using (Point p2 = Point.ByCoordinates(10, 10, 0))
      {
          return Line.ByStartPointEndPoint(p1, p2);
      }
    }
    ```

    > 在[此处](https://msdn.microsoft.com/en-us/library/yh598w02.aspx)介绍了 using 语句
    >
    > 请参见 [Dynamo 几何图形稳定性改进](https://forum.dynamobim.com/t/dynamo-geometry-stability-improvements-request-for-feedback/39297)，以详细了解 Dynamo 2.5 中引入的新稳定性功能
*   使用手动 Dispose 调用：

    ```
    Point p1 = Point.ByCoordinates(0, 0, 0);
    Point p2 = Point.ByCoordinates(10, 10, 0);
    Line l = Line.ByStartPointEndPoint(p1, p2);
    p1.Dispose();
    p2.Dispose();
    return l;
    ```

### 移植 <a href="#migrations" id="migrations"></a>

发布库的较新版本时，节点名称可能会更改。可以在移植文件中指定名称更改，以便在完成更新后，基于库的以前版本构建的图形可以继续正常使用。通过以下方式实现移植：

* 在与 `.dll` 所在的同一个文件夹中创建 `.xml` 文件，其格式如下所示："BaseDLLName".Migrations.xml
* 在 `.xml` 中，创建单个 `<migrations>...</migrations>` 元素
* 在移植元素内，为每个名称更改创建 `<priorNameHint>...</priorNameHint>` 元素
* 对于每个名称更改，请提供 `<oldName>...</oldName>` 和 `<newName>...</newName>` 元素

![移植文件](images/vs-migrations-file.jpg)

> 1. 单击鼠标右键，然后选择 `Add > New Item`
> 2. 选择 `XML File`
> 3. 对于此项目，我们会将移植文件命名为 `ZeroTouchEssentials.Migrations.xml`

此示例代码会告知 Dynamo，任何名为 `GetClosestPoint` 的节点现在都已命名为 `ClosestPointTo`。

```
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>Autodesk.DesignScript.Geometry.Geometry.GetClosestPoint</oldName>
    <newName>Autodesk.DesignScript.Geometry.Geometry.ClosestPointTo</newName>
  </priorNameHint>
</migrations>
```

> 请参见 [ProtoGeometry.Migrations.xml](https://github.com/DynamoDS/Dynamo/blob/master/extern/ProtoGeometry/ProtoGeometry.Migrations.xml) 中的此代码示例

### 泛型 <a href="#generics" id="generics"></a>

Zero-Touch 当前不支持使用泛型。可以使用这些泛型，但无法在直接输入且未设置类型的代码中使用它们。不能公开属于泛型且未设置类型的方法、特性或类。

在下面的示例中，将不会输入类型为 `T` 的 Zero-Touch 节点。如果库的其余部分输入到 Dynamo 中，则会出现丢失的类型异常。

```
public class SomeGenericClass<T>
{
    public SomeGenericClass()
    {
        Console.WriteLine(typeof(T).ToString());
    }  
}
```

使用在本例中所设置类型的泛型类型将输入到 Dynamo 中。

```
public class SomeWrapper
{
    public object wrapped;
    public SomeWrapper(SomeGenericClass<double> someConstrainedType)
    {
        Console.WriteLine(this.wrapped.GetType().ToString());
    }
}
```
