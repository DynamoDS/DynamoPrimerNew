# Dynamo 2.x용 패키지 및 Dynamo 라이브러리 업데이트하기 

### 소개: <a href="#introduction" id="introduction"></a>

Dynamo 2.0은 주 릴리즈이며 일부 API가 변경되거나 제거되었습니다. 노드 및 패키지 작성자에게 영향을 미칠 수 있는 가장 큰 변경 사항 중 하나는 JSON 파일 형식으로의 전환입니다.

일반적으로 Zero Touch 노드 작성자는 2.0에서 패키지를 실행하기 위해 할 작업이 거의 없습니다.

NodeModel에서 직접 파생되는 노드와 UI 노드는 2.x에서 실행하는 데 더 많은 작업이 필요합니다.

확장 작성자는 확장에서 사용하는 Dynamo Core API의 양에 따라 몇 가지 변경 사항을 적용할 수도 있습니다.

***

### 일반 패키징 규칙: <a href="#general-packaging-rules" id="general-packaging-rules"></a>

* Dynamo 또는 Dynamo Revit .dll을 패키지와 번들로 묶지 마십시오. 이러한 dll은 Dynamo에 의해 이미 로드됩니다. 사용자가 로드한 버전과 다른 버전을 번들로 묶으면 _(예: Dynamo Core 1.3을 배포하는데 사용자가 Dynamo 2.0에서 패키지를 실행 중인 경우)_ 이상한 런타임 버그가 발생합니다. 여기에는 `DynamoCore.dll`, `DynamoServices.dll`, `DSCodeNodes.dll`, `ProtoGeometry.dll` 등의 dll이 포함됩니다.
* 가능하다면 `newtonsoft.json.net`을 패키지와 번들로 묶어서 배포하지 마십시오. 이 dll은 Dynamo 2.x에 의해서도 이미 로드됩니다. 위와 동일한 이슈가 발생할 수 있습니다.
* 가능하다면 `CEFSharp`을 패키지와 번들로 묶어서 배포하지 마십시오. 이 dll은 Dynamo 2.x에 의해서도 이미 로드됩니다. 위와 동일한 이슈가 발생할 수 있습니다.
* 일반적으로, 해당 종속성 버전을 제어해야 하는 경우 Dynamo 또는 Revit과 종속성을 공유하지 않도록 합니다.



### 일반적인 이슈: <a href="#common-issues" id="common-issues"></a>

1) 그래프를 열었을 때 일부 노드에 이름이 같은 포트가 여러 개 있었지만, 저장할 때는 그래프가 올바르게 표시되었습니다. 이 이슈는 몇 가지 원인으로 인해 발생할 수 있습니다.

일반적인 근본 원인은 노드를 만들 때 포트를 다시 만든 생성자를 사용했기 때문입니다. 그 대신 포트를 로드한 생성자를 사용했어야 합니다. 이러한 생성자는 일반적으로 `[JsonConstructor]`로 표시됩니다. _예제는 아래를 참조하십시오._

![손상된 JSON](images/broken-json.jpg)

이 문제는 다음과 같은 이유로 발생할 수 있습니다.

* 일치하는 `[JsonConstructor]`가 없거나 JSON .dyn에서 `Inports` 및 `Outports`를 전달하지 않았습니다.
* 동일한 프로세스에 두 가지 버전의 JSON.net이 동시에 로드되어 .net 런타임 오류가 발생해 생성자를 표시하는 데 `[JsonConstructor]` 속성을 올바르게 사용할 수 없었습니다.
* 현재 Dynamo 버전과 다른 버전의 DynamoServices.dll이 패키지와 함께 번들로 묶여서 .net 런타임이 `[MultiReturn]` 속성을 식별하지 못해 다양한 속성으로 표시된 Zero-Touch 노드에 해당 속성이 적용되지 않습니다. 노드가 여러 포트가 아닌 하나의 사전 출력을 반환하는 것을 확인할 수 있습니다.

2) 그래프를 로드할 때 노드가 완전히 누락되어 콘솔에 몇 가지 오류가 표시됩니다.

* 이는 몇 가지 이유로 역직렬화에 실패한 경우 발생할 수 있습니다. 필요한 특성만 직렬화하는 것이 좋습니다. 로드하거나 저장할 필요가 없는 복잡한 특성에는 `[JsonIgnore]`를 사용하여 무시할 수 있습니다. 이러한 특성으로는 `function pointer, delegate, action,` 또는 `event` 등이 있습니다. 이러한 특성은 일반적으로 역직렬화에 실패하여 런타임 오류를 발생시키므로 직렬화해서는 안 됩니다.


### 심층적으로 업그레이드하기: <a href="#upgrading-in-depth" id="upgrading-in-depth"></a>

### 사용자 지정 노드 1.3 - > 2.0 <a href="#custom-nodes-13----20" id="custom-nodes-13----20"></a>

[libraries.js에서 사용자 지정 노드 구성하기](https://github.com/DynamoDS/Dynamo/wiki/Library-2.0-Add-Ons-Organization#customnodes)

알려진 이슈:

* 사용자 지정 노드 이름과 카테고리 이름이 libraries.js에서 동일한 수준으로 일치하면 예기치 않은 동작이 발생합니다. [QNTM-3653](https://jira.autodesk.com/browse/QNTM-3653) \- 카테고리 및 노드에 동일한 이름을 사용하지 마십시오.
* 주석은 선 주석 대신 블록 주석으로 변환됩니다.
* 짧은 유형의 이름이 전체 이름으로 바뀝니다. 예를 들어, 사용자 지정 노드를 다시 로드할 때 유형을 지정하지 않은 경우 기본 유형인 `var[]..[]`이 표시됩니다.


### Zero Touch 노드 1.3 -> 2.0 <a href="#zero-touch-nodes-13---20" id="zero-touch-nodes-13---20"></a>

* Dynamo 2.0에서는 리스트와 사전 유형이 분리되었으며 리스트 및 사전을 생성하기 위한 구문이 변경되었습니다. 리스트는 `[]`를 사용하여 초기화되고 사전은 `{}`를 사용합니다.\
 이전에 `DefaultArgument` 속성을 사용하여 Zero-Touch 노드에 매개변수를 표시하고 리스트 구문을 사용하여 `someFunc([DefaultArgument("{0,1,2}")])`와 같은 특정 리스트를 기본값으로 설정했다면, 이는 더 이상 유효하지 않으며 리스트에 대한 새 초기화 구문을 사용하도록 DesignScript 조각을 수정해야 합니다.
* 위에 설명한 것처럼 Dynamo dll을 패키지와 함께 배포하지 마십시오(`DynamoCore`, `DynamoServices` 등).


### Node Model 노드 1.3 -> 2.0 <a href="#node-model-nodes-13---20" id="node-model-nodes-13---20"></a>

Dynamo 2.x로 업데이트할 때 가장 많은 작업을 수행해야 하는 노드는 Node Model 노드입니다. 상위 수준에서는 노드 유형의 새 인스턴스를 인스턴스화하는 데 사용되는 일반 nodeModel 생성자 외에 json에서 노드를 로드하는 데만 사용되는 생성자를 구현해야 합니다. 이러한 값을 구분하려면 로드 시간 생성자에 newtonsoft.Json.net의 속성인 `[JsonConstructor]`를 표시합니다.

생성자의 매개변수 이름은 일반적으로 JSON 특성의 이름과 일치해야 하지만, [JsonProperty] 속성을 사용하여 직렬화된 이름을 재정의하는 경우 매핑이 더 복잡해집니다.\
 [자세한 내용은 Json.net 문서를 참조하십시오.](https://www.newtonsoft.com/json/help/html/Introduction.htm)


#### JSON 생성자 <a href="#json-constructors" id="json-constructors"></a>

`NodeModel` 기준 클래스(또는 기타 Dynamo 코어 기준 클래스, 예: `DSDropDownBase`)에서 파생된 노드를 업데이트하는 데 필요한 가장 일반적인 변경 사항은 클래스에 JSON 생성자를 추가해야 한다는 것입니다.

매개변수가 없는 원래 생성자는 여전히 Dynamo 내에서 생성된 새 노드의 초기화를 처리합니다(예: 라이브러리를 통해). JSON 생성자는 저장된 .dyn 또는 .dyf 파일에서 역직렬화된(_로드된)_ 노드를 초기화하는 데 필요합니다.

JSON 생성자는 JSON 로드 논리에서 제공하는 `inPorts` 및 `outPorts`에 대한 `PortModel` 매개변수가 있다는 점에서 기본 생성자와 다릅니다. 노드에 대한 포트 등록을 위한 호출은 데이터가 .dyn 파일에 존재하므로 여기서는 필요하지 않습니다. JSON 생성자의 예는 다음과 같습니다.

`using Newtonsoft.Json; //New dependency for Json`

………

`[JsonConstructor] //Attribute required to identity the Json constructor`

`//Minimum constructor implementation. Note that the base method invocation must also be present.`

`FooNode(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts) { }`

이 `:base(Inports,outPorts){}` 구문은 기본 `nodeModel` 생성자를 호출하고 역직렬화된 포트를 이 생성자에 전달합니다.

클래스 생성자에 존재하며 .dyn 파일에 직렬화되는 특정 데이터의 초기화와 관련된 특수 논리 _(예: 포트 등록, 레이싱 전략 설정 등)_ 는 해당 값을 JSON에서 읽을 수 있으므로 이 생성자에서 반복할 필요가 없습니다.

이것이 nodeModels에 대한 JSON 생성자와 비JSON 생성자 간의 주요 차이점입니다. JSON 생성자는 파일에서 로드될 때 호출되며 로드된 데이터를 전달받습니다. 그러나 다른 사용자 로직은 JSON 생성자에서 복제되어야 합니다 _(예: 노드에 대한 이벤트 핸들러 초기화 또는 연결)_.

예제는 DynamoSamples 리포지토리 -> [ButtonCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/ButtonCustomNodeModel.cs#L156), [DropDown](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs#L23) 또는 [SliderCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/SliderCustomNodeModel.cs#L123) 에서 확인할 수 있습니다


#### 공용 특성 및 직렬화 <a href="#public-properties-and-serialization" id="public-properties-and-serialization"></a>

이전에는 개발자가 `SerializeCore` 및 `DeserializeCore` 메서드를 통해 특정 모델 데이터를 xml 문서로 직렬화하고 역직렬화할 수 있었습니다. 이러한 메서드는 API에 여전히 존재하지만 Dynamo의 향후 릴리즈에서는 더 이상 사용되지 않습니다(예는 [여기](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/DoubleSlider.cs#L140) 에서 확인할 수 있음). 이제 JSON.NET 구현을 통해 NodeModel 파생 클래스의 `public` 특성을 .dyn 파일에 직접 직렬화할 수 있습니다. JSON.Net은 특성이 직렬화되는 방식을 제어하는 여러 속성을 제공합니다.

`PropertyName`을 지정하는 이 예는 Dynamo 리포지토리의 [여기](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/ColorPalette.cs#L38)에서 확인할 수 있습니다.

`[JsonProperty(PropertyName = "InputValue")]`

`public DSColor DsColor {...`


#### 변환기: <a href="#converters" id="converters"></a>

**참고**\
 자체 JSON.net 변환기 클래스를 만드는 경우 Dynamo에는 현재 로드 및 저장 메서드에 이 클래스를 주입할 수 있는 메커니즘이 없습니다. 따라서 클래스에 `[JsonConverter]` 속성을 표시하더라도 사용할 수 없습니다. 대신 setter 또는 getter에서 직접 변환기를 호출할 수 있습니다. _//TODO 이 제한에 대한 확인이 필요합니다. 어떤 증거라도 환영합니다._

특성을 문자열로 변환하기 위한 직렬화 메서드를 지정하는 예사는 Dynamo 리포지토리의 [여기](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L66)에서 확인할 수 있습니다.

`[JsonProperty("MeasurementType"), JsonConverter(typeof(StringEnumConverter))]`

`public ConversionMetricUnit SelectedMetricConversion{...`


#### 특성 무시하기 <a href="#ignoring-properties" id="ignoring-properties"></a>

직렬화용이 아닌 `public` 특성에는 `[JsonIgnore]` 속성을 추가해야 합니다. 노드가 .dyn 파일에 저장되면 이 데이터가 직렬화 메커니즘에서 무시되어 그래프를 다시 열 때 예상치 못한 결과가 발생하지 않습니다. 이에 대한 예는 Dynamo 리포지토리의 [여기](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L45)에서 확인할 수 있습니다.

***

#### 실행 취소/다시 실행 <a href="#undoredo" id="undoredo"></a>

위에서 설명한 대로, 과거에는 노드를 xml .dyn 파일에 저장하고 로드하는 데 `SerializeCore` 및 `DeserializeCore` 메서드를 사용했습니다. 또한 실행 취소/다시 실행을 위해 노드 상태를 저장 및 로드하는 데 사용되었으며 **지금도 여전히 사용되고 있습니다!** nodeModel UI 노드에 대해 복잡한 실행 취소/다시 실행 기능을 구현하려면 이러한 메서드를 구현하고 이러한 메서드에 매개변수로 제공된 XML 문서 객체로 직렬화해야 합니다. UI 노드가 복잡할 때를 제외하고는 드물게 사용될 것입니다.


#### 입력 및 출력 포트 API <a href="#input-and-output-port-apis" id="input-and-output-port-apis"></a>

2.0 API 변경 사항의 영향을 받는 nodeModel 노드에서 흔히 발생하는 한 가지 문제는 노드 생성자에서 포트를 등록하는 것과 관련이 있습니다. Dynamo 또는 DynamoSamples 리포지토리의 예제를 보면 이전에 `InPortData.Add()` 또는 `OutPortData.Add()` 메서드를 사용했다는 것을 확인할 수 있습니다. 이전에는 Dynamo API에서 `InPortData` 및 `OutPortData` 공용 특성이 사용되지 않는 것으로 표시되었습니다. 2.0에서는 이러한 특성이 제거되었습니다. 개발자는 이제 `InPorts.Add()` 및 `OutPorts.Add()` 메서드를 사용해야 합니다. 또한 이 두 가지 `Add()` 메서드의 경우 서명이 약간 다릅니다.

`InPortData.Add(new PortData("Port Name", "Port Description")); //Old version valid in 1.3 but now deprecated`

vs

`InPorts.Add(new PortModel(PortType.Input, this, new PortData("Port Name", "Port Description"))); //Recommended 2.0`

변환된 코드의 예제는 Dynamo 리포지토리 -> [DynamoConvert.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L142) 또는 [FileSystem.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/Input/FileSystem.cs#L281)에서 확인할 수 있습니다.

2.0 API 변경 사항의 영향을 받는 다른 일반적인 사용 사례는 포트 커넥터의 유무에 따라 노드 동작을 결정하는 `BuildAst()` 메서드에서 일반적으로 사용하는 메서드와 관련이 있습니다. 이전에는 연결된 포트 상태의 유효성을 검사하는 데 `HasConnectedInput(index)`가 사용되었습니다. 이제 개발자는 `InPorts[0].IsConnected` 특성을 사용하여 포트 연결 상태를 확인해야 합니다. 이 아이콘의 예는 Dynamo 리포지토리의 [ColorRange.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/ColorRange.cs#L83)에서 확인할 수 있습니다.


### 예제: <a href="#examples" id="examples"></a>

1.3 UI 노드를 Dynamo 2.x로 업그레이드하는 과정을 살펴보겠습니다.

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

이 `nodeModel` 클래스가 2.0에서 올바르게 로드 및 저장되도록 하려면 포트 로드를 처리하는 jsonConstructor를 추가하기만 하면 됩니다. 기본 생성자에 포트를 전달하기만 하면 이 구현은 비어 있습니다.

```
[JsonConstructor]
protected GridNodeModel(IEnumerable<PortModel> Inports, IEnumerable<PortModel> Outports ) :
base(Inports,Outports)
{

}
```

참고: `RegisterPorts()`는 노드 클래스의 입력 및 출력 매개변수 속성을 사용하여 새 포트를 생성하므로 JsonConstructor에서 이를 호출하거나 이에 대한 변형을 호출하지 마십시오 우리는 생성자에게 전달되는 로드된 포트를 사용하려고 하므로 **호출해서는 안 됩니다**.

```
[InPortNames("xCount", "yCount")]
[InPortTypes("double", "double")]
```

이 예에서는 가능한 최소한의 로드 JSON 생성자를 추가합니다. 하지만 생성자 내부에 이벤트 처리를 위한 수신기를 설정하는 등 좀 더 복잡한 구성 논리를 수행해야 한다면 어떻게 해야 할까요? \
 [DynamoSamples 리포지토리](https://github.com/DynamoDS/DynamoSamples)에서 가져온 다음 샘플은 이 문서의 `JsonConstructors Section` 위쪽에 링크되어 있습니다.

다음은 UI 노드에 대한 보다 복잡한 생성자입니다.

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

파일에서 이 노드를 로드하기 위한 JSON 생성자를 추가할 때 이 논리 중 일부를 다시 생성해야 합니다. 이때, 포트를 생성하거나 레이싱을 설정하거나 파일에서 로드할 수 있는 특성의 기본값을 설정하는 코드는 포함하지 않습니다.

```
        // This constructor is called when opening a Json graph.

        [JsonConstructor]
        ButtonCustomNodeModel(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts)
        {
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);
        }
```

`ButtonText` 및 `WindowText`와 같은 JSON에 직렬화된 다른 공용 특성은 생성자에 명시적 매개변수로 추가되지 않으며, 이러한 특성의 setter를 사용하여 JSON.net에서 자동으로 설정됩니다.
