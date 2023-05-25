# 更新 Dynamo 2.x 的软件包和 Dynamo 库

### 简介：<a href="#introduction" id="introduction"></a>

Dynamo 2.0 是一个主要版本，一些 API 已更改或已删除。将影响节点和软件包作者的最大更改之一是我们转为使用 JSON 文件格式。

通常，Zero Touch 节点作者几乎无需进行任何操作，即可在 2.0 中运行其软件包。

UI 节点和直接从 NodeModel 派生的节点需要进行更多操作才能在 2.x 中运行。

扩展作者可能还要进行一些潜在更改，具体取决于他们在其扩展中使用的 Dynamo 核心 API 数量。

***

### 常规打包规则：<a href="#general-packaging-rules" id="general-packaging-rules"></a>

* 请勿将 Dynamo 或 Dynamo Revit .dll 与软件包捆绑在一起。这些 dll 会由 Dynamo 进行载入。如果捆绑的版本不同于用户已载入的版本 _（即，您分发 Dynamo Core 1.3，但用户运行的是基于 Dynamo 2.0 的软件包）_，则会发生意外运行时错误。这包括诸如 `DynamoCore.dll`、`DynamoServices.dll`、`DSCodeNodes.dll`、`ProtoGeometry.dll` 之类的 dll
* 如果可以避免，请勿将 `newtonsoft.json.net` 与软件包捆绑在一起并分发。此 dll 也将由 Dynamo 2.x 载入。可能会出现与上述相同的问题。
* 如果可以避免，请勿将 `CEFSharp` 与软件包捆绑在一起并分发。此 dll 也将由 Dynamo 2.x 载入。可能会出现与上述相同的问题。
* 通常，如果需要控制依存关系的版本，请避免将该依存关系与 Dynamo 或 Revit 共享。



### 常见问题：<a href="#common-issues" id="common-issues"></a>

1) 打开图形时，一些节点有多个同名端口，但图形在保存时看起来正常。此问题可能有几个原因。

常见的根本原因是，节点是使用重新创建端口的构造函数创建的。反之，应使用已载入端口的构造函数。这些构造函数通常标记为 `[JsonConstructor]` _参见下文以了解示例_

![损坏的 JSON](images/broken-json.jpg)

这可能是因为：

* 根本没有匹配的 `[JsonConstructor]`，或者未通过 JSON .dyn 给它传递 `Inports` 和 `Outports`。
* 有两个版本的 JSON.net 同时载入到同一进程中导致 .net 运行时失败，因此无法正确使用 `[JsonConstructor]` 属性来标记构造函数。
* 版本不同于当前 Dynamo 版本的 DynamoServices.dll 已与软件包捆绑在一起，并会导致 .net 运行时无法识别 `[MultiReturn]` 属性，因此标记有各种属性的 Zero Touch 节点将无法应用它们。您可能会发现，一个节点返回一个字典输出，而不是多个端口。

2) 在将存在一些错误的图形载入控制台后，节点会完全丢失。

* 如果反序列化因某种原因而失败，则可能会发生这种情况。最好仅序列化所需的特性。我们可以对不需要载入或保存的复杂特性使用 `[JsonIgnore]` 来忽略它们。诸如 `function pointer, delegate, action,` 或 `event` 之类的特性。由于这些特性通常无法反序列化并导致出现运行时错误，因此不应该对它们序列化。


### 全面升级：<a href="#upgrading-in-depth" id="upgrading-in-depth"></a>

### 自定义节点 1.3 - > 2.0 <a href="#custom-nodes-13----20" id="custom-nodes-13----20"></a>

[在 librarie.js 中组织自定义节点](https://github.com/DynamoDS/Dynamo/wiki/Library-2.0-Add-Ons-Organization#customnodes)

已知问题：

* 在 librarie.js 中，同一级别上的自定义节点名称和类别名称重名会导致出现意外行为。[QNTM-3653](https://jira.autodesk.com/browse/QNTM-3653) \- 避免为类别和节点使用相同的名称。
* 注释将转换为块注释，而不是行注释。
* 短类型名称将替换为完整名称。例如，如果再次载入自定义节点时未指定类型，则您会看到 `var[]..[]` - 因为这是默认类型。


### Zero Touch 节点 1.3 -> 2.0 <a href="#zero-touch-nodes-13---20" id="zero-touch-nodes-13---20"></a>

* 在 Dynamo 2.0 中，“列表”和“字典”类型已拆分，并且用于创建列表和字典的语法已更改。列表将使用 `[]` 初始化，而字典使用 `{}`。\
如果以前使用 `DefaultArgument` 属性标记 Zero Touch 节点上的参数，并将列表语法默认用于特定列表（如 `someFunc([DefaultArgument("{0,1,2}")])`），则此语法将不再有效，需要修改 DesignScript 代码段以对列表使用新的初始化语法。
* 如上所述，请勿将 Dynamo DLL 与软件包一起分发。（`DynamoCore`、`DynamoServices` 等）。


### 节点模型节点 1.3 -> 2.0 <a href="#node-model-nodes-13---20" id="node-model-nodes-13---20"></a>

节点模型节点需要做大量操作才能更新到 Dynamo 2.x。在较高级别上，除了用于实例化节点类型的新实例的常规 nodeModel 构造函数外，还需要实现将仅用于从 json 载入节点的构造函数。为了区分这些构造函数，请使用 `[JsonConstructor]`（来自 newtonsoft.Json.net 的属性）来标记载入时间构造函数。

构造函数中参数的名称通常应与 JSON 特性的名称相匹配 - 尽管覆盖使用 [JsonProperty] 属性序列化的名称时，此映射会变得更复杂。\
[有关详细信息，请参见 Json.net 文档。](https://www.newtonsoft.com/json/help/html/Introduction.htm)


#### JSON 构造函数 <a href="#json-constructors" id="json-constructors"></a>

更新派生自 `NodeModel` 基类（或其他 Dynamo 核心基类，即 `DSDropDownBase`）的节点所需的最常见更改是需要将 JSON 构造函数添加到您的类中。

原始无参数构造函数仍将对在 Dynamo 中创建的新节点处理初始化（例如，通过库）。要初始化从保存的 .dyn 或 .dyf 文件反序列化 _（载入）_ 的节点，需要使用 JSON 构造函数。

JSON 构造函数与基础构造函数的不同之处在于，它将 `PortModel` 参数用于 `inPorts` 和 `outPorts`，这些参数由 JSON 载入逻辑提供。由于数据存在于 .dyn 文件中，此处不需要调用来注册节点的端口。JSON 构造函数的示例如下所示：

`using Newtonsoft.Json; //New dependency for Json`

………

`[JsonConstructor] //Attribute required to identity the Json constructor`

`//Minimum constructor implementation. Note that the base method invocation must also be present.`

`FooNode(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts) { }`

此语法 `:base(Inports,outPorts){}` 会调用基础 `nodeModel` 构造函数，并将反序列化端口传递给它。

类构造函数中存在的任何涉及初始化已序列化到 .dyn 文件中的特定数据的特殊逻辑 _（例如，设置端口注册、连缀策略等）_ 都不需要在此构造函数中重复，因为这些值可以从 JSON 中读取。

这是 nodeModel 的 JSON 构造函数和非 JSON 构造函数之间的主要区别。JSON 构造函数在从文件载入时被调用，并被传递载入的数据。但是，必须在 JSON 构造函数中复制其他用户逻辑 _（例如，为节点初始化事件处理程序或附加）_。

可以在此处的 DynamoSamples 存储库 -> [ButtonCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/ButtonCustomNodeModel.cs#L156)、[DropDown](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs#L23) 或 [SliderCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/SliderCustomNodeModel.cs#L123) 中找到示例


#### 公有特性和序列化 <a href="#public-properties-and-serialization" id="public-properties-and-serialization"></a>

以前，开发人员可以通过 `SerializeCore` 和 `DeserializeCore` 方法将特定模型数据序列化和反序列化到 xml 文档。这些方法仍存在于 API 中，但将在将来版本的 Dynamo 中弃用（可以在[此处](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/DoubleSlider.cs#L140)找到示例）。现在，通过 JSON.NET 实现，NodeModel 派生类上的 `public` 特性可以直接序列化到 .dyn 文件。JSON.Net 提供了多个属性来控制如何序列化该特性。

指定 `PropertyName` 的此示例可以在[此处](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/ColorPalette.cs#L38)的 Dynamo 存储库中找到。

`[JsonProperty(PropertyName = "InputValue")]`

`public DSColor DsColor {...`


#### 转换器：<a href="#converters" id="converters"></a>

**注意**\
如果您创建自己的 JSON.net 转换器类，则 Dynamo 目前没有一种机制可以让您将其注入到载入和保存方法中；因此，即使您使用 `[JsonConverter]` 属性标记您的类，它也不可能被使用 - 可以改为直接在 setter 或 getter 中调用您的转换器。_//TODO 需要确认这一限制。欢迎提供任何证据。_

指定序列化方法以将该特性转换为字符串的示例可以在[此处](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L66)的 Dynamo 存储库中找到。

`[JsonProperty("MeasurementType"), JsonConverter(typeof(StringEnumConverter))]`

`public ConversionMetricUnit SelectedMetricConversion{...`


#### 忽略特性 <a href="#ignoring-properties" id="ignoring-properties"></a>

不用于序列化的 `public` 特性需要添加 `[JsonIgnore]` 属性。当将节点保存到 .dyn 文件时，这将确保序列化机制会忽略此数据，并且再次打开图形时不会导致出现意外结果。此情况的示例可以在[此处](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L45)的 Dynamo 存储库中找到。

***

#### 撤消/重做 <a href="#undoredo" id="undoredo"></a>

如上所述，过去使用 `SerializeCore` 和 `DeserializeCore` 方法是为了将节点保存和载入到 xml .dyn 文件中。此外，它们还用于保存和载入节点状态以进行撤消/重做， **现在仍然如此！** 如果要为 nodeModel UI 节点实现复杂的撤消/重做功能，则需要实现这些方法并将其序列化到作为这些方法的参数提供的 XML 文档对象中。除了复杂的 UI 节点之外，这应该是一个罕见用例。


#### 输入和输出端口 API <a href="#input-and-output-port-apis" id="input-and-output-port-apis"></a>

受 2.0 API 更改影响的 nodeModel 节点中常见的一种情况是节点构造函数中的端口注册。查看 Dynamo 或 DynamoSamples 存储库中的示例，您会发现以前使用 `InPortData.Add()` 或 `OutPortData.Add()` 方法。以前，在 Dynamo API 中，`InPortData` 和 `OutPortData` 公有特性标记为已弃用。在 2.0 中，这些特性已删除。现在，开发人员应使用 `InPorts.Add()` 和 `OutPorts.Add()` 方法。此外，这两种 `Add()` 方法的签名略有不同：

`InPortData.Add(new PortData("Port Name", "Port Description")); //Old version valid in 1.3 but now deprecated`

与

`InPorts.Add(new PortModel(PortType.Input, this, new PortData("Port Name", "Port Description"))); //Recommended 2.0`

可以在此处的 Dynamo 存储库 -> [DynamoConvert.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L142) 或 [FileSystem.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/Input/FileSystem.cs#L281) 中找到已转换代码的示例

受 2.0 API 更改影响的另一个常见用例与 `BuildAst()` 方法中的常用方法有关，可根据是否存在端口连接器来确定节点行为。以前，使用 `HasConnectedInput(index)` 来验证已连接端口的状态。现在，开发人员应使用 `InPorts[0].IsConnected` 特性来检查端口连接状态。可以在 Dynamo 存储库中的 [ColorRange.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/ColorRange.cs#L83) 中找到此情况的示例。


### 示例：<a href="#examples" id="examples"></a>

让我们来逐步介绍如何将 1.3 UI 节点升级到 Dynamo 2.x。

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

为了在 2.0 中正确载入和保存该 `nodeModel` 类，我们所需要做的就是添加一个 jsonConstructor 来处理端口的载入。我们只需在基础构造函数中传递端口，而这个实现是空的。

```
[JsonConstructor]
protected GridNodeModel(IEnumerable<PortModel> Inports, IEnumerable<PortModel> Outports ) :
base(Inports,Outports)
{

}
```

注意：请勿在 JsonConstructor 中调用 `RegisterPorts()` 或其某些变体 - 这将使用节点类中的输入和输出参数属性来构造新端口！由于我们希望使用传递给您的构造函数的已载入端口，因此**我们不希望这样**。

```
[InPortNames("xCount", "yCount")]
[InPortTypes("double", "double")]
```

本例尽可能添加最小的载入 JSON 构造函数。但是，如果我们需要执行一些更复杂的构造逻辑（例如，在构造函数中设置一些用于事件处理的侦听器），该怎么办。从\
 [DynamoSamples 存储库](https://github.com/DynamoDS/DynamoSamples)获取的下一个样例的链接位于本文档的 `JsonConstructors Section` 中的上方。

以下是 UI 节点的更复杂构造函数：

```
 public ButtonCustomNodeModel()
        {
            // When you create a UI node, you need to do the
            // work of setting up the ports yourself. To do this,
            // you can populate the InPorts and the OutPorts
            // collections with PortData objects describing your ports.
            InPorts.Add(new PortModel(PortType.Input, this, new PortData("inputString", "a string value displayed on our button")));

            // Nodes can have an arbitrary number of inputs and outputs.
            // If you want more ports, just create more PortData objects.
            OutPorts.Add(new PortModel(PortType.Output, this, new PortData("button value", "returns the string value displayed on our button")));
            OutPorts.Add(new PortModel(PortType.Output, this, new PortData("window value", "returns the string value displayed in our window when button is pressed")));

            // This call is required to ensure that your ports are
            // properly created.
            RegisterAllPorts();

            // Listen for input port disconnection to trigger button UI update
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;

            // The arugment lacing is the way in which Dynamo handles
            // inputs of lists. If you don't want your node to
            // support argument lacing, you can set this to LacingStrategy.Disabled.
            ArgumentLacing = LacingStrategy.Disabled;

            // We create a DelegateCommand object which will be 
            // bound to our button in our custom UI. Clicking the button 
            // will call the ShowMessage method.
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);

            // Setting our property here will trigger a 
            // property change notification and the UI 
            // will be updated to reflect the new value.
            ButtonText = defaultButtonText;
            WindowText = defaultWindowText;
        }
```

当我们添加用于从文件载入此节点的 JSON 构造函数时，我们必须重新创建该逻辑的一部分；但请注意，我们不会包括创建端口、设置连缀或设置特性的默认值（可以从文件载入）的代码。

```
        // This constructor is called when opening a Json graph.

        [JsonConstructor]
        ButtonCustomNodeModel(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts)
        {
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);
        }
```

请注意，已序列化到 JSON 中的其他公有特性（如 `ButtonText` 和 `WindowText`）将不会作为显式参数添加到构造函数中 - JSON.net 会使用这些特性的 setter 自动设置它们。
