# NodeModel ケース スタディ - カスタム UI 

NodeModel ベースのノードは、Zero-Touch ノードよりも大幅に柔軟性に優れ強力です。この例では、矩形のサイズをランダム化する統合されたスライダを追加して、Zero-Touch グリッド ノードのレベルを高めます。

![矩形グリッドのグラフ](images/cover-image-2.jpg)

> スライダでセルのスケールをそのサイズに対して相対的に変更するため、スライダで正確な範囲を設定する必要はありません。

#### モデル - ビュー - ビューモデル パターン <a href="#the-model-view-viewmodel-pattern" id="the-model-view-viewmodel-pattern"></a>

Dynamo は、UI をバックエンドから分離しておくための[モデル - ビュー - ビューモデル](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel)(MVVM)ソフトウェア アーキテクチャ パターンに基づいています。ZeroTouch ノードを作成する場合は、Dynamo はノードのデータとその UI の間でデータ バインドを実行します。カスタム UI を作成するには、データバインド ロジックを追加する必要があります。

大まかには、Dynamo では次の 2 つのパーツでモデルとビューの関係を確立します。

* ノードのコア ロジック(「モデル」)を確立する `NodeModel` クラス
* `NodeModel` の表示方法(「ビュー」)をカスタマイズする `INodeViewCustomization` クラス

> NodeModel オブジェクトには既にビューとモデルの関連付け(NodeViewModel)があるため、検討するのはカスタム UI のモデルとビューのみです。

#### NodeModel を実装する方法 <a href="#how-to-implement-nodemodel" id="how-to-implement-nodemodel"></a>

NodeModel ノードには Zero-Touch ノードと大きく異なる点がいくつかあり、この例でそれを説明します。UI のカスタマイズに進む前に、まず NodeModel ロジックをビルドします。

**1\.プロジェクト構造を作成する。**

NodeModel ノードは関数のみを呼び出すことができるため、NodeModel と関数を別のライブラリに分割する必要があります。Dynamo パッケージでこの操作を行う標準的な方法として、それぞれ別のプロジェクトを作成します。まず、プロジェクトを包含する新しいソリューションを作成します。

> 1. `File > New > Project` を選択します。
> 2. `Other Project Types` を選択して Solution オプションを表示します。
> 3. `Blank Solution` を選択します。
> 4. ソリューションに `CustomNodeModel` という名前を付けます。
> 5. `Ok` を選択します。

ソリューションで 2 つの C# クラス ライブラリ プロジェクトを作成します。関数用のプロジェクトと、NodeModel インタフェースを実装するためのプロジェクトです。

![新しいクラス ライブラリを追加する](images/vs-new-class-projects.jpg)

> 1. ソリューションを右クリックして、`Add > New Project` を選択します。
> 2. クラス ライブラリを選びます。
> 3. `CustomNodeModel` という名前を付けます。
> 4. [`Ok`]をクリックします。
> 5. このプロセスを繰り返して、`CustomNodeModelFunctions` という名前の別のプロジェクトを追加します。

次に、自動的に作成されたクラス ライブラリの名前を変更し、`CustomNodeModel` プロジェクトに追加する必要があります。クラス `GridNodeModel` は抽象クラス NodeModel を実装し、ビューのカスタマイズには `GridNodeView` が使用され、`GridFunction` には呼び出す必要のある関数が含まれています。

![ソリューション エクスプローラ](images/vs-new-class.jpg)

> 1. `CustomNodeModel` プロジェクトを右クリックして `Add > New Item...` を選択し、`Class` を選んで別のクラスを追加します。
> 2. `CustomNodeModel` プロジェクトには `GridNodeModel.cs` クラスと `GridNodeView.cs` クラスが必要です
> 3. `CustomNodeModelFunction` プロジェクトには `GridFunctions.cs` クラスが必要です

クラスにコードを追加する前に、このプロジェクトに必要なパッケージを追加します。`CustomNodeModel` には ZeroTouchLibrary と WpfUILibrary が必要で、`CustomNodeModelFunction` には ZeroTouchLibrary のみが必要です。WpfUILibrary は、後で説明する UI のカスタマイズに使用し、ZeroTouchLibrary はジオメトリの作成に使用します。パッケージは、プロジェクトに個別に追加できます。これらのパッケージには依存関係があるため、Core および DynamoServices が自動的にインストールされます。

![パッケージをインストールする](images/vs-add-packages.jpg)

> 1. プロジェクトを右クリックして、`Manage NuGet Packages` を選択します。
> 2. そのプロジェクトに必要なパッケージのみをインストールします。

Visual Studio は、参照した NuGet パッケージをビルド フォルダにコピーします。これを false に設定できるため、パッケージ内に不要なファイルはありません。

![ローカルへのパッケージのコピーを無効にする](images/vs-disable-package-copying.jpg)

> 1. Dynamo NuGet パッケージを選択します。
> 2. `Copy Local` を false に設定します。

**2\.NodeModel クラスを継承する**

前述のとおり、NodeModel ノードが ZeroTouch ノードと異なるのは、主に `NodeModel` クラスを実装するという点です。NodeModel ノードにはこのクラスのいくつかの関数が必要です。クラス名の後に `:NodeModel` を追加すると、これらの関数を取得できます。

次のコードを `GridNodeModel.cs` にコピーします。

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

これが、Zero-Touch ノードとは異なるところです。それぞれの機能について説明します。

* Name、Category、InPort/OutPort の名前、InPort/OutPort のタイプ、説明などの、Node 属性を指定します。
* `public class GridNodeModel : NodeModel` は、`NodeModel` クラスを `Dynamo.Graph.Nodes` から継承するクラスです。
* `public GridNodeModel() { RegisterAllPorts(); }` は、ノードの入力と出力を登録するコンストラクタです。
* `BuildOutputAst()` は、AST (抽象構文ツリー)を返します。これは、NodeModel ノードからデータを返すために必要な構造です。
* `AstFactory.BuildFunctionCall()` は、`GridFunctions.cs` から RectangularGrid 関数を呼び出します。
* `new Func<int, int, double, List<Rectangle>>(GridFunction.RectangularGrid)` は、関数とそのパラメータを指定します。
* `new List<AssociativeNode> { inputAstNodes[0], inputAstNodes[1], sliderValue });` は、ノードの入力を関数パラメータにマッピングします。
* `AstFactory.BuildNullNode()` は、入力ポートが接続されていない場合に、null ノードを作成します。これは、ノードに警告が表示されないようにするためです。
* `RaisePropertyChanged("SliderValue")` は、スライダの値が変化したときに UI に通知します。
* `var sliderValue = AstFactory.BuildDoubleNode(SliderValue)` は、スライダ値を表すノードを AST で作成します。
* functionCall 変数 `new List<AssociativeNode> { inputAstNodes[0], sliderValue });` にある変数 `sliderValue` への入力を変更します。

**3\.関数を呼び出す**

`CustomNodeModelFunction` プロジェクトは、呼び出すことができるように、`CustomNodeModel` とは別のアセンブリにビルドされます。

次のコードを `GridFunction.cs` にコピーします。

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

この関数クラスは、Zero-Touch グリッドのケース スタディに非常に似ていますが、次の点のみ異なります。

* `[IsVisibleInDynamoLibrary(false)]` によって Dynamo には後に続くメソッドとクラスが「見えなく」なります。これは、関数が既に `CustomNodeModel` から呼び出されているためです。

NuGet パッケージの参照の追加と同様に、`CustomNodeModel` は関数を呼び出すために `CustomNodeModelFunction` を参照する必要があります。

![参照を追加する](images/vs-add-project-reference.jpg)

> CustomNodeModel の using ステートメントは、関数を参照するまで非アクティブになります。
>
> 1. `CustomNodeModel` を右クリックして、`Add > Reference` を選択します。
> 2. `Projects > Solution` を選びます。
> 3. `CustomNodeModelFunction` をオンにします。
> 4. [`Ok`]をクリックします。

**4\.ビューをカスタマイズする**

スライダを作成するには、`INodeViewCustomization` インタフェースを実装して UI をカスタマイズする必要があります。

次のコードを `GridNodeView.cs` にコピーします。

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

* `public class CustomNodeModelView : INodeViewCustomization<GridNodeModel>` で、UI をカスタマイズするために必要な関数を定義します。

プロジェクトの構造を設定したら、Visual Studio の設計環境を使用してユーザ コントロールを作成し、`.xaml` ファイルでそのパラメータを定義します。ツール ボックスから、`<Grid>...</Grid>` にスライダを追加します。

![新規スライダを追加する](images/vs-usercontrol.jpg)

> 1. `CustomNodeModel` を右クリックして、`Add > New Item` を選択します。
> 2. `WPF` を選択します。
> 3. ユーザ コントロールに `Slider` という名前を付けます。
> 4. [`Add`]をクリックします。

次のコードを `Slider.xaml` にコピーします。

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

* スライダ コントロールのパラメータは、`.xaml` ファイルの In で定義されています。Minimum および Maximum__ の属性は、このスライダの数値範囲を定義します。
* `<Grid>...</Grid>` 内に、Visual Studio ツールボックスからさまざまなユーザ コントロールを配置できます。

`Slider.xaml` ファイルを作成すると、Visual Studio によって自動的に `Slider.xaml.cs` という C# ファイルが作成され、スライダが初期化されます。このファイルの名前空間を変更します。

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

* 名前空間は `CustomNodeModel.CustomNodeModel` にする必要があります

`GridNodeModel.cs` で、スライダの計算ロジックを定義します。

**5\.パッケージとして構成する**

プロジェクトをビルドする前に、最後の手順として、`pkg.json` ファイルを追加して Dynamo でパッケージの読み込みができるようにします。

![JSON ファイルを追加する](images/vs-pkg-json.jpg)

> 1. `CustomNodeModel` を右クリックして、`Add > New Item` を選択します。
> 2. `Web` を選択します。
> 3. `JSON File` を選択します。
> 4. ファイルに `pkg.json` という名前を付けます。
> 5. [`Add`]をクリックします。

* 次のコードを `pkg.json` にコピーします。

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

* `"name":` で、Dynamo ライブラリでのパッケージとそのグループの名前を決定します。
* `"keywords":` で、Dynamo ライブラリを検索するための検索語を指定します。
*   `"node_libraries": []` は、パッケージに関連付けられているライブラリです。

    最後に、ソリューションをビルドし、Dynamo パッケージとしてパブリッシュします。オンラインでパブリッシュする前にローカル パッケージを作成する方法と、Visual Studio から直接パッケージをビルドする方法については、「パッケージの配置」の章を参照してください。
