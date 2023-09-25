# 針對 Dynamo 2.x 更新您的套件和 Dynamo 資源庫

### 簡介：<a href="#introduction" id="introduction"></a>

Dynamo 2.0 一個是重大版本，某些 API 已變更或移除。影響節點和套件作者其中一項最大的改變是我們改用 JSON 檔案格式。

一般而言，Zero Touch 節點的作者幾乎不需要完成什麼工作，就能在 2.0 中執行他們的套件。

直接從 NodeModel 衍生的使用者介面節點和節點則要付出較多的工作，才能在 2.x 中執行。

延伸的作者也可能需要進行一些潛在的變更 - 取決於他們在延伸中使用多少 Dynamo 核心 API。



### 一般封裝規則：<a href="#general-packaging-rules" id="general-packaging-rules"></a>

* 請勿將 Dynamo 或 Dynamo Revit .dll 與套件一起封裝。這些 dll 將由 Dynamo 載入。如果您組合的版本與使用者載入的版本不同 _(例如您散發 dynamo core 1.3，但使用者在 dynamo 2.0 上執行套件)_，將會發生神秘的執行時期錯誤。這包括 dll，例如 `DynamoCore.dll`、`DynamoServices.dll`、`DSCodeNodes.dll`、`ProtoGeometry.dll`
* 如果可以避免，請勿將 `newtonsoft.json.net` 與套件一起封裝並散發。此 dll 也將由 Dynamo 2.x 載入。可能會發生與上述相同的問題。
* 如果可以避免，請勿將 `CEFSharp` 與套件一起封裝並散發。此 dll 也將由 Dynamo 2.x 載入。可能會發生與上述相同的問題。
* 如果您需要控制相依性的版本，一般而言請避免與 dynamo 或 revit 共用相依性。

### 常見問題：<a href="#common-issues" id="common-issues"></a>

1) 開啟圖表時，某些節點有多個埠具有相同名稱，但圖表在儲存時看起來正常。此問題可能有幾種原因。

常見的根本原因是，節點是使用重新建立埠的建構函式建立的。應該使用載入埠的建構函式。這些建構函式通常會標記 `[JsonConstructor]` _請參閱下方範例_

![JSON 損毀](images/broken-json.jpg)

這可能是因為：

* 沒有相符的 `[JsonConstructor]`，或未從 JSON .dyn 傳入 `Inports` 和 `Outports`。
* 同時將兩個版本的 JSON.net 載入到同一個程序中，會導致 .net 執行時期失敗，因此無法正確使用 `[JsonConstructor]` 屬性來標記建構函式。
* 與目前 Dynamo 版本不同的 DynamoServices.dll 與套件一起封裝，導致 .net 執行時期無法識別 `[MultiReturn]` 屬性，因此以各種屬性標記的 zero touch 節點無法套用這些屬性。您可能會發現節點傳回單一字典輸出，而不是多個埠。

2) 在主控台中載入帶有某些錯誤的圖表時，節點會完全遺失。

* 如果還原序列化因某些原因失敗，則可能會發生此情況。最好只序列化您需要的性質。我們可以對您不需要載入或儲存的複雜性質使用 `[JsonIgnore]` 加以忽略。例如 `function pointer, delegate, action,` 或 `event` 等性質。這些性質不應該序列化，因為它們通常無法還原序列化而導致執行時期錯誤。

### 深度升級：<a href="#upgrading-in-depth" id="upgrading-in-depth"></a>

### 自訂節點 1.3 -> 2.0 <a href="#custom-nodes-13----20" id="custom-nodes-13----20"></a>

[在 librarie.js 中組織自訂節點](https://github.com/DynamoDS/Dynamo/wiki/Library-2.0-Add-Ons-Organization#customnodes)

已知問題：

* 自訂節點名稱和品類名稱在 library.js 中的同一層級如果相同，會導致非預期的行為。[QNTM-3653](https://jira.autodesk.com/browse/QNTM-3653) \- 請避免對品類和節點使用相同的名稱。
* 註解將轉換為區塊註解，而不是行註解。
* 簡短類型名稱將取代為完整名稱。例如，如果您在再次載入自訂節點時未指定類型，您會看到 `var[]..[]` - 因為這是預設類型。

### Zero Touch 節點 1.3 -> 2.0 <a href="#zero-touch-nodes-13---20" id="zero-touch-nodes-13---20"></a>

* 在 Dynamo 2.0 中，清單和字典類型已經分開，用於建立清單和字典的語法也已經變更。清單使用 `[]` 初始化，字典則是使用 `{}`。\
如果您先前使用 `DefaultArgument` 屬性標記 Zero Touch 節點上的參數，並使用清單語法預設為特定清單 (例如 `someFunc([DefaultArgument("{0,1,2}")])`)，此作業將不再有效，您需要修改 DesignScript 片段對清單使用新的初始化語法。
* 如上所述，請勿將 Dynamo dll 與您的套件 (`DynamoCore`、`DynamoServices` 等) 一起散發。

### NodeModel 節點 1.3 -> 2.0 <a href="#node-model-nodes-13---20" id="node-model-nodes-13---20"></a>

NodeModel 節點需要最多工作才能更新到 Dynamo 2.x。除了用於實體化節點類型新例證的正規 nodeModel 建構函式外，進階一點，您還需要實作只用於從 json 載入節點的建構函式。若要區分這些建構函式，請使用來自 newtonsoft.Json.net 的屬性 `[JsonConstructor]` 標記載入時間建構函式。

建構函式中的參數名稱通常應與 JSON 性質的名稱相符，但是如果您使用 [JsonProperty] 屬性取代序列化的名稱，則此對映會更複雜。\
[請參閱 Json.net 文件，以取得更多資訊。](https://www.newtonsoft.com/json/help/html/Introduction.htm)

#### JSON 建構函式 <a href="#json-constructors" id="json-constructors"></a>

更新衍生自 `NodeModel` 基準類別 (或其他 Dynamo 核心基準類別，例如 `DSDropDownBase`) 的節點時，最常見的變更是需要將 JSON 建構函式加入您的類別。

您原始的無參數建構函式仍會處理初始化在 Dynamo 中建立的新節點 (例如，透過資源庫)。必須有 JSON 建構函式，才能初始化從儲存的 .dyn 或 .dyf 檔案還原序列化 _(已載入)_ 的節點。

JSON 建構函式與基本建構函式不同，因為它有 `inPorts` 和 `outPorts` 的 `PortModel` 參數，由 JSON 載入邏輯提供。此處不需要呼叫註冊節點的埠，因為資料存在於 .dyn 檔案中。JSON 建構函式的範例如下所示：

`using Newtonsoft.Json; //New dependency for Json`

………

`[JsonConstructor] //Attribute required to identity the Json constructor`

`//Minimum constructor implementation. Note that the base method invocation must also be present.`

`FooNode(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts) { }`

此語法 `:base(Inports,outPorts){}` 呼叫基本 `nodeModel` 建構函式，並將還原序列化的埠傳入。

類別建構函式中牽涉到已序列化至 .dyn 檔案之特定資料初始化而存在的任何特殊邏輯 _(例如設定埠註冊、交織策略等)_，在此建構函式中都不需要重複，因為這些值可以從 JSON 中讀取。

這是 nodeModels 的 JSON 建構函式與非 JSON 建構函式之間的主要差異。JSON 建構函式是從檔案載入時呼叫，並傳入載入的資料。但是，在 JSON 建構函式中必須複製其他使用者邏輯 _(例如，初始化節點的事件處理常式或附加)_。

在 DynamoSamples 儲存庫 -> [ButtonCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/ButtonCustomNodeModel.cs#L156)、[DropDown](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs#L23) 或 [SliderCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/SliderCustomNodeModel.cs#L123) 中可以找到範例

#### 公用性質和序列化 <a href="#public-properties-and-serialization" id="public-properties-and-serialization"></a>

先前，開發人員可以透過 `SerializeCore` 和 `DeserializeCore` 方法將特定模型資料序列化和還原序列化為 xml 文件。這些方法仍存在於 API 中，但在未來版本的 Dynamo 中將棄用 (在[此處](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/DoubleSlider.cs#L140)可找到範例)。現在實作 JSON.NET，NodeModel 衍生類別的 `public` 性質可以直接序列化為 .dyn 檔案。JSON.Net 提供多個屬性，可控制如何序列化性質。

在 Dynamo 儲存庫中的[此處](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/ColorPalette.cs#L38)可以找到指定 `PropertyName` 的此範例。

`[JsonProperty(PropertyName = "InputValue")]`

`public DSColor DsColor {...`

#### 轉換器：<a href="#converters" id="converters"></a>

**注意事項**\
 如果您建立自己的 JSON.net 轉換器類別，Dynamo 目前沒有機制可讓您將其插入載入方法和儲存方法，因此即使您使用 `[JsonConverter]` 屬性標記類別，也可能不會使用它 - 您可以改為直接在 setter 或 getter 中呼叫轉換器。_//TODO 需要確認此限制。歡迎提出任何證據。_

在 Dynamo 儲存庫中的[此處](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L66)可以找到一個範例，指定將性質轉換為字串的序列化方法。

`[JsonProperty("MeasurementType"), JsonConverter(typeof(StringEnumConverter))]`

`public ConversionMetricUnit SelectedMetricConversion{...`

#### 忽略性質 <a href="#ignoring-properties" id="ignoring-properties"></a>

不用於序列化的 `public` 性質需要加入 `[JsonIgnore]` 屬性。節點儲存為 .dyn 檔案後，可確保序列化機制忽略此資料，再次開啟圖表時，不會導致非預期的結果。在 Dynamo 儲存庫中的[此處](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L45)可以找到此內容的範例。



#### 退回/重做 <a href="#undoredo" id="undoredo"></a>

如上所述，過去曾使用 `SerializeCore` 和 `DeserializeCore` 方法將節點儲存並載入至 xml .dyn 檔案。此外，這些方法也用來儲存和載入節點狀態以用於退回/重做，**現在也還是！**如果您要為 nodeModel 使用者介面節點實作複雜的退回/重做功能，則需要實作這些方法，並序列化為以這些方法的參數提供的 XML 文件物件。除了複雜的使用者介面節點，這應該是很少見的使用案例。

#### 輸入和輸出埠 API <a href="#input-and-output-port-apis" id="input-and-output-port-apis"></a>

受 2.0 API 變更影響的 nodeModel 節點中，一個常見情況是節點建構函式中的埠註冊。查看 Dynamo 或 DynamoSamples 儲存庫中的範例，您先前會發現使用 `InPortData.Add()` 或 `OutPortData.Add()` 方法。先前在 Dynamo API 中，`InPortData` 和 `OutPortData` 公用性質已標記為棄用。在 2.0 中，這些性質已移除。開發人員現在應使用 `InPorts.Add()` 和 `OutPorts.Add()` 方法。此外，這兩種 `Add()` 方法的簽章略有不同：

`InPortData.Add(new PortData("Port Name", "Port Description")); //Old version valid in 1.3 but now deprecated`

與

`InPorts.Add(new PortModel(PortType.Input, this, new PortData("Port Name", "Port Description"))); //Recommended 2.0`

在 Dynamo 儲存庫 -> [DynamoConvert.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L142) 或 [FileSystem.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/Input/FileSystem.cs#L281) 中可以找到轉換的程式碼範例

受 2.0 API 變更影響的其他常見使用案例與 `BuildAst()` 方法中常用的方法有關，該方法可根據埠連接器是否存在決定節點行為。先前 `HasConnectedInput(index)` 是用於驗證連接的埠狀態。開發人員現在應使用 `InPorts[0].IsConnected` 性質檢查埠連接狀態。在 Dynamo 儲存庫中的 [ColorRange.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/ColorRange.cs#L83) 可以找到此內容的範例。

### 範例：<a href="#examples" id="examples"></a>

接下來逐步解說將 1.3 使用者介面節點升級到 Dynamo 2.x。

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

我們只需對此 `nodeModel` 類別加入一個 jsonConstructor 處理埠的載入，就能在 2.0 中正確載入和儲存。我們只是在基本建構函式上傳入埠，此實作為空的。

```
[JsonConstructor]
protected GridNodeModel(IEnumerable<PortModel> Inports, IEnumerable<PortModel> Outports ) :
base(Inports,Outports)
{

}
```

注意事項：請勿在 JsonConstructor 中呼叫 `RegisterPorts()` 或其某些變體 - 這會在您的節點類別上使用輸入和輸出參數屬性來建構新的埠！**我們不想要這個結果**，因為我們要使用傳入建構函式的已載入埠。

```
[InPortNames("xCount", "yCount")]
[InPortTypes("double", "double")]
```

此範例加入負載很低的 JSON 建構函式。但如果我們需要執行一些更複雜的建構邏輯，例如設定一些接聽程式以在建構函式內處理事件，該怎麼辦。下一個範例是從\
 [DynamoSamples 儲存庫](https://github.com/DynamoDS/DynamoSamples)取得，在本文件上方的 。`JsonConstructors Section` 有連結。

以下是使用者介面節點更複雜的建構函式：

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

當我們加入 JSON 建構函式以從檔案載入此節點時，必須重新建立其中的某些邏輯，但請注意，我們不包括建立埠、設定交織或設定性質預設值的程式碼，這些程式碼可從檔案載入。

```
        // This constructor is called when opening a Json graph.

        [JsonConstructor]
        ButtonCustomNodeModel(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts)
        {
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);
        }
```

請注意，已序列化為 JSON 的其他公用性質 (例如 `ButtonText` 和 `WindowText`) 將不會以明確參數加到建構函式中 - JSON.net 會使用這些性質的 setter 自動設定這些性質。
