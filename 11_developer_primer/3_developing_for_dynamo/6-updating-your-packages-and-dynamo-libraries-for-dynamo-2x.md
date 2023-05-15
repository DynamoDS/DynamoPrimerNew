# Dynamo 2.x 用のパッケージと Dynamo ライブラリを更新する

### はじめに: <a href="#introduction" id="introduction"></a>

Dynamo 2.0 は主要なリリースであり、いくつかの API が変更または削除されました。中でも、ノードおよびパッケージの作成者に多大な影響をおよぼす変更の 1 つは、ファイル形式が JSON に移行されたことです。

Zero-Touch ノードの作成者が、パッケージを 2.0 で実行するために必要となる作業は、ほとんどないのが一般的です。

UI ノードと、NodeModel から直接派生したノードを 2.x で実行するためには、より多くの作業を必要とします。

また、拡張機能作成者は、拡張機能で使用する Dynamo Core API の数によっては、変更が必要になる可能性があります。

***

### 一般的なパッケージ化のルール: <a href="#general-packaging-rules" id="general-packaging-rules"></a>

* パッケージに Dynamo または Dynamo Revit の .dll をバンドルしないでください。これらの dll は、Dynamoによってもすでにロードされています。ユーザがロードしているバージョンとは異なるバージョンをバンドルした場合_(つまり、Dynamo Core 1.3 を配布したが、ユーザが Dynamo 2.0 でパッケージを実行している場合)_、予期しないランタイム エラーが発生します。これには、`DynamoCore.dll`、`DynamoServices.dll`、`DSCodeNodes.dll`、`ProtoGeometry.dll` などの dll が該当します 。
* 可能な限り、パッケージに `newtonsoft.json.net` をバンドルして配布しないでください。この dll は、Dynamo 2.x によってもすでにロードされています。先ほどと同じ問題が発生する可能性があります。
* 可能な限り、パッケージに `CEFSharp` をバンドルして配布しないでください。この dll は、Dynamo 2.x によってもすでにロードされています。先ほどと同じ問題が発生する可能性があります。
* 一般的に、依存関係のバージョンを制御する必要がある場合は、Dynamo や Revit との依存関係を共有することは避けてください。



### 一般的な問題: <a href="#common-issues" id="common-issues"></a>

1) グラフを開くと、一部のノードに同じ名前の複数のポートが存在するが、保存するとグラフは適切に表示される。この問題には、いくつかの原因が考えられます。

一般的な原因は、ポートを再作成するコンストラクタを使用してノードを作成したことによるものです。その場合、ポートをロードしたコンストラクタを使用する必要があります。これらのコンストラクタには通常、`[JsonConstructor]` マークが付いています。_次のサンプルを参照してください_。

![JSON が壊れている](images/broken-json.jpg)

これは次のような場合に発生します。

* 単に `[JsonConstructor]` と一致するものがなかったか、JSON .dyn から `Inports` と `Outports` が渡されなかった。
* JSON.net の 2 つのバージョンが同時に同じプロセスにロードされ、.net ランタイム エラーが発生したため、コンストラクタをマークするための `[JsonConstructor]` 属性を正しく使用できなかった。
* 現在の Dynamo バージョンとは異なるバージョンの DynamoServices.dll がパッケージにバンドルされており、.net ランタイムで `[MultiReturn]` 属性を識別できなくなっているために、さまざまな属性が設定されている Zero-Touch ノードを適用することができない。ノードは複数のポートではなく、単一のディクショナリ出力を返すことがあります。

2) コンソールでいくつかのエラーが発生した状態でグラフをロードすると、ノードが完全に失われる。

* これは、何らかの理由でシリアル化解除に失敗した場合に発生する可能性があります。必要なプロパティのみをシリアル化することをお勧めします。ロードや保存を必要としない複雑なプロパティに対して `[JsonIgnore]` を使用して、それらを無視することができます。`function pointer, delegate, action,` や `event` などのプロパティです。これらはシリアル化できないため、シリアル化解除に失敗し、ランタイム エラーが発生します。


### 詳細なアップグレード: <a href="#upgrading-in-depth" id="upgrading-in-depth"></a>

### カスタム ノード 1.3 > 2.0<a href="#custom-nodes-13----20" id="custom-nodes-13----20"></a>

[librarie.js のカスタム ノードを整理する](https://github.com/DynamoDS/Dynamo/wiki/Library-2.0-Add-Ons-Organization#customnodes)

既知の問題:

* librarie.js 内の同じレベルで、一致したカスタムノード名とカテゴリ名を使用すると、予期しない動作が発生します。[QNTM-3653](https://jira.autodesk.com/browse/QNTM-3653) \- カテゴリとノードに同じ名前を使用しないでください。
* コメントは、行コメントではなくブロック コメントに変換されます。
* ショート タイプ名はフル ネームに置き換えられます。たとえば、カスタム ノードを再ロードするときにタイプを指定しなかった場合、`var[]..[]` - が既定のタイプとして表示されます。


### Zero-Touch ノード 1.3 -> 2.0 <a href="#zero-touch-nodes-13---20" id="zero-touch-nodes-13---20"></a>

* Dynamo 2.0 では、リストとディクショナリのタイプが分割され、リストとディクショナリを作成するための構文が変更されました。リストは `[]` を使用して初期化され、ディクショナリは `{}` を使用して初期化されます。\
以前に `DefaultArgument` 属性を使用して Zero-Touch ノードのパラメータをマークし、`someFunc([DefaultArgument("{0,1,2}")])` リスト構文を使用して特定のリストを既定として使用していた場合は、これは無効になり、リストに新しい初期化構文を使用するように DesignScript スニペットを変更する必要があります。
* 前述ように、パッケージに Dynamo の dll を含めて配布しないでください。(`DynamoCore`、`DynamoServices` など)


### ノード モデル ノード 1.3 -> 2.0 <a href="#node-model-nodes-13---20" id="node-model-nodes-13---20"></a>

ノード モデル ノードは、Dynamo 2.x に更新する際に多くの作業量を必要とするノードです。大まかに言うと、ノード タイプの新しいインスタンスのインスタンス化に使用される通常の nodeMode lコンストラクタに加えて、JSON からノードをロードするためにのみ使用されるコンストラクタを実装する必要があります。これらを区別するには、newtonsoft.Json.net の属性である `[JsonConstructor]` で、ロード時間コンストラクタをマークします。

コンストラクタのパラメータの名前は、通常 JSON プロパティの名前と一致させる必要があります。ただし、[JsonProperty]属性を使用してシリアル化された名前を上書きすると、このマッピングはより複雑になります。\
[詳細については、Json.net ドキュメントを参照してください。](https://www.newtonsoft.com/json/help/html/Introduction.htm)


#### JSON コンストラクタ <a href="#json-constructors" id="json-constructors"></a>

`NodeModel` ベース クラス(または、その他の Dynamo のコア ベース クラス `DSDropDownBase` など)から派生したノードを更新するために必要となる最も一般的な変更は、クラスに JSON コンストラクタを追加する必要があることです。

パラメータを持たない元のコンストラクタは、Dynamoで作成された新しいノードの初期化を引き続き処理します(たとえばライブラリなどを使用)。JSON コンストラクタは、保存した .dyn または .dyf ファイルからシリアル化解除した_(ロードした)_ノードを初期化するために必要です。

JSON コンストラクタは、JSON ロード ロジックによって提供される `inPorts` および `outPorts` 用の `PortModel` パラメータがあるという点で、基本コンストラクタとは異なります。ノードのポートを登録する呼び出しは、データが .dyn ファイルに存在するためここでは必要ありません。JSON コンストラクタのサンプルは次のようになります。

`using Newtonsoft.Json; //New dependency for Json`

………

`[JsonConstructor] //Attribute required to identity the Json constructor`

`//Minimum constructor implementation. Note that the base method invocation must also be present.`

`FooNode(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts) { }`

この構文 `:base(Inports,outPorts){}` は、ベース `nodeModel` コンストラクタを呼び出し、シリアル化解除されたポートを渡します。

クラス コンストラクタに存在し、.dyn ファイルにシリアル化した特定のデータの初期化を含む特殊なロジック_(たとえば、ポート登録、レーシング戦略などの設定)_は、このコンストラクタで繰り返す必要はありません。これらの値は JSON から読み取ることができます。

これが nodeModel の JSON コンストラクタと非 JSON コンストラクタの主な違いです。JSON コンストラクタは、ファイルからロードするときに呼び出し、ロードしたデータを渡します。ただし、その他のユーザ ロジックは JSON コンストラクタで複製する必要があります_(たとえば、ノードのイベント ハンドラの初期化やアタッチなど)_。

サンプルについては、DynamoSamples リポジトリの [ButtonCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/ButtonCustomNodeModel.cs#L156)、[DropDown](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs#L23)、または [SliderCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/SliderCustomNodeModel.cs#L123) を参照してください。


#### パブリック プロパティとシリアル化 <a href="#public-properties-and-serialization" id="public-properties-and-serialization"></a>

これまでは、開発者は `SerializeCore` および `DeserializeCore` メソッドを使用して、特定のモデル データを xml ドキュメントにシリアル化およびシリアル化解除できました。これらのメソッドは、API に引き続き存在しますが、Dynamo の将来のリリースでは廃止される予定です(サンプルについては、[こちら](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/DoubleSlider.cs#L140)を参照してください)。現在、JSON.NET が実装されたことで、NodeModel 派生クラスの `public` プロパティを、.dyn ファイルに直接シリアル化できるようになりました。JSON.Net には、プロパティをシリアル化する方法をコントロールするための複数の属性があります。

`PropertyName` を指定するサンプルは、Dynamo リポジトリ内の[こちら](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/ColorPalette.cs#L38)を参照してください。

`[JsonProperty(PropertyName = "InputValue")]`

`public DSColor DsColor {...`


#### コンバータ: <a href="#converters" id="converters"></a>

**注:**\
独自の JSON.net コンバータ クラスを作成した場合、現在の Dynamo にはロード メソッドと保存メソッドに挿入するためのメカニズムがないため、クラスを `[JsonConverter]` 属性でマークしても使用できない可能性があります。その場合は、コンバータをセッターまたはゲッターで直接呼び出すことができます。_//TODO この制限を確認する必要があります。これに関するどのようなコメントも歓迎します。_

プロパティを文字列に変換するシリアル化メソッドを指定するサンプルは、Dynamo リポジトリの[こちら](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L66)を参照してください。

`[JsonProperty("MeasurementType"), JsonConverter(typeof(StringEnumConverter))]`

`public ConversionMetricUnit SelectedMetricConversion{...`


#### プロパティを無視する <a href="#ignoring-properties" id="ignoring-properties"></a>

シリアル化を意図していない `public` プロパティには、`[JsonIgnore]` 属性を追加する必要があります。ノードを .dyn ファイルに保存すると、そのデータはシリアル化メカニズムによって無視されることが確実となり、グラフを再度開いたときに予期しない結果となることがなくなります。このサンプルについては Dynamo のリポジトリの[こちら](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L45)を参照してください。

***

#### 元に戻す/やり直し<a href="#undoredo" id="undoredo"></a>

すでに述べたように、`SerializeCore` および `DeserializeCore` メソッドは、以前 xml .dyn ファイルにノードを保存およびロードするために使用されていました。また、元に戻す/やり直し用のノード状態を保存およびロードする場合にも使用されていて、**現在も使用可能**です。nodeModel UI ノードに対して複雑な元に戻す/やり直し機能を実装する場合は、これらのメソッドを実装し、これらのメソッドのパラメータとして提供されるXML ドキュメント オブジェクトにシリアル化する必要があります。これは、複雑な UI ノードを除いて、まれな使用事例です。


#### 入力および出力ポート API <a href="#input-and-output-port-apis" id="input-and-output-port-apis"></a>

2.0 API の変更によって影響を受ける nodeModelノードでよく見られるのが、ノード コンストラクタでのポート登録です。Dynamo または DynamoSamples リポジトリ内のサンプルを確認すると、以前は、`InPortData.Add()` または `OutPortData.Add()` メソッドを使用していることがわかります。以前は、Dynamo API で `InPortData` および `OutPortData` パブリック プロパティが廃止とマークされていました。2.0 では、これらのプロパティは削除されました。開発者は、`InPorts.Add()` および `OutPorts.Add()` メソッドを使用する必要があります。また、これら 2 つの `Add()` メソッドには少し異なる要素があります。

`InPortData.Add(new PortData("Port Name", "Port Description")); //Old version valid in 1.3 but now deprecated`

vs

`InPorts.Add(new PortModel(PortType.Input, this, new PortData("Port Name", "Port Description"))); //Recommended 2.0`

変換されたコードのサンプルについては、Dynamo リポジトリの [DynamoConvert.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L142) または [FileSystem.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/Input/FileSystem.cs#L281) を参照してください。

2.0 API の変更によって影響を受ける、その他の共通使用例は、ポート コネクタの有無に基づいてノードの動作を決定する手法で一般的に使用されている `BuildAst()` メソッドに関連するものです。以前は、接続ポートの状態を確認するために `HasConnectedInput(index)` が使用されていました。今後開発者は、`InPorts[0].IsConnected` プロパティを使用してポートの接続状態を確認する必要があります。これに関するサンプルは、Dynamo リポジトリの [ColorRange.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/ColorRange.cs#L83) を参照してください。


### サンプル <a href="#examples" id="examples"></a>

1.3 UI ノードを Dynamo 2.x にアップグレードする方法について説明します。

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

この `nodeModel` クラスをロードして 2.0 で正しく保存するためにやるべきことは、ポートのロードを処理する jsonConstructor の追加です。単純にベース コンストラクタのポートを渡すだけであり、この実装は空です。

```
[JsonConstructor]
protected GridNodeModel(IEnumerable<PortModel> Inports, IEnumerable<PortModel> Outports ) :
base(Inports,Outports)
{

}
```

注: JsonConstructor で `RegisterPorts()` や類似のものを呼び出さないでください。ノード クラスの入力および出力パラメータ属性を使用し、新しいポートを構築します。コンストラクタに渡されたロード済みポートを使用したいため、**これは望ましい結果ではありません。**

```
[InPortNames("xCount", "yCount")]
[InPortTypes("double", "double")]
```

このサンプルでは、可能な限り最小限のロード JSON コンストラクタを追加しています。しかし、コンストラクタ内でのイベント処理のリスナーを設定するなど、より複雑なコンストラクション ロジックを実行する必要がある場合はどうでしょうか。\
[DynamoSamples リポジトリ](https://github.com/DynamoDS/DynamoSamples)から取得した次のサンプルは、このドキュメントの `JsonConstructors Section` にリンクされています。

以下に、UI ノードの複雑なコンストラクタを示します。

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

ファイルからこのノードをロードするための JSON コンストラクタを追加する場合は、このロジックの一部を再作成する必要があります。ただし、ポートの作成、レーシングの設定、またはファイルからロードできるプロパティの既定値を設定するコードは含んでいないことに注意してください。

```
        // This constructor is called when opening a Json graph.

        [JsonConstructor]
        ButtonCustomNodeModel(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts)
        {
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);
        }
```

JSON にシリアル化したその他のパブリック プロパティ(`ButtonText`、`WindowText` など)は、コンストラクタに明示的なパラメータとして追加されていないことに注意してください。これらは、それぞれのプロパティのセッターを使用して JSON.net によって自動的に設定されます。
