# NodeModel 案例研究 - 自訂使用者介面 

以 NodeModel 為基礎的節點提供的靈活性和功能比 Zero-Touch 節點要大得多。在此範例中，我們將加入一個可隨機顯示矩形大小的整合滑棒，將 Zero-Touch 網格節點提升到下一個層次。

![矩形網格圖表](images/cover-image-2.jpg)

> 滑棒會相對於儲存格的大小調整比例，因此使用者不必為滑棒提供正確範圍。

#### 模型-視圖-視圖模型模式 <a href="#the-model-view-viewmodel-pattern" id="the-model-view-viewmodel-pattern"></a>

Dynamo 以[模型-視圖-視圖模型](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel) (MVVM) 軟體架構模式為基礎，讓使用者介面與後端保持獨立。建立 ZeroTouch 節點時，Dynamo 會在節點的資料與其使用者介面之間繫結資料。若要建立自訂使用者介面，我們必須加入資料繫結邏輯。

在 Dynamo 較高的層次，有兩個部分可以建立模型-視圖關係：

* `NodeModel` 類別建立節點的核心邏輯 (「模型」)
* `INodeViewCustomization` 類別自訂檢視 `NodeModel` 的方式 (「視圖」)

> NodeModel 物件已經有一個關聯的視圖-模型 (NodeViewModel)，因此我們只需將焦點放在模型上，以及自訂使用者介面的視圖。

#### 如何實作 NodeModel <a href="#how-to-implement-nodemodel" id="how-to-implement-nodemodel"></a>

NodeModel 節點與 Zero-Touch 節點有幾項顯著差異，我們將在此範例中進行介紹。在跳至使用者介面自訂之前，我們先建置 NodeModel 邏輯。

**1\.建立專案結構：**

NodeModel 節點只能呼叫函數，因此我們需要將 NodeModel 和函數分到不同的資源庫。對 Dynamo 套件執行此作業的標準方式是為每個套件建立單獨的專案。首先，建立新的方案以包含專案。

> 1. 選取「`File > New > Project`」
> 2. 選取「`Other Project Types`」以顯示方案選項
> 3. 選取「`Blank Solution`」
> 4. 將方案命名為 `CustomNodeModel`
> 5. 選取「`Ok`」

在方案中建立兩個 C# 類別資源庫專案：一個用於函數，一個用於實作 NodeModel 介面。

![新增類別資源庫](images/vs-new-class-projects.jpg)

> 1. 在「方案」上按一下右鍵，然後選取「`Add > New Project`」
> 2. 選擇「類別庫」
> 3. 將其命名為 `CustomNodeModel`
> 4. 按一下「`Ok`」
> 5. 重複此程序，加入另一個名為 `CustomNodeModelFunctions` 的專案

接下來，我們需要將自動建立的類別資源庫更名，並在 `CustomNodeModel` 專案中加入一個資源庫。`GridNodeModel` 類別實作抽象 NodeModel 類別，`GridNodeView` 用於自訂視圖，`GridFunction` 包含我們需要呼叫的任何函數。

![方案總管](images/vs-new-class.jpg)

> 1. 在 `CustomNodeModel` 專案上按一下右鍵，選取「`Add > New Item...`」並選擇「`Class`」，以加入其他類別。
> 2. 在 `CustomNodeModel` 專案中，我們需要 `GridNodeModel.cs` 和 `GridNodeView.cs` 類別
> 3. 在 `CustomNodeModelFunction` 專案中，我們需要 `GridFunctions.cs` 類別

在類別中加入任何程式碼之前，請先為此專案加入必要的套件。`CustomNodeModel` 需要 ZeroTouchLibrary 和 WpfUILibrary，`CustomNodeModelFunction` 只需要 ZeroTouchLibrary。WpfUILibrary 將用於我們稍後執行的使用者介面自訂，ZeroTouchLibrary 將用於建立幾何圖形。可以個別為專案加入套件。由於這些套件具有相依性，因此將自動安裝 Core 和 DynamoServices。

![安裝套件](images/vs-add-packages.jpg)

> 1. 在專案上按一下右鍵，然後選取「`Manage NuGet Packages`」
> 2. 只會為該專案安裝必要的套件

Visual Studio 會複製我們參考建置目錄的 NuGet 套件。這可以設定為 False，讓套件中沒有任何不必要的檔案。

![停用本端套件複本](images/vs-disable-package-copying.jpg)

> 1. 選取 Dynamo NuGet 套件
> 2. 將 `Copy Local` 設定為 False

**2\.繼承 NodeModel 類別**

如前所述，NodeModel 節點與 ZeroTouch 節點不同的主要層面是對 `NodeModel` 類別的實作。NodeModel 節點需要此類別中的幾個函數，我們可以透過在類別名稱後加入 `:NodeModel` 來取得這些函數。

將以下程式碼複製到 `GridNodeModel.cs`。

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

這與 Zero-Touch 節點不同。我們來瞭解每個部分在做什麼。

* 指定節點屬性，如名稱、品類、InPort/OutPort 名稱、InPort/OutPort 類型、描述。
* `public class GridNodeModel : NodeModel` 是從 `Dynamo.Graph.Nodes` 繼承 `NodeModel` 類別的類別。
* `public GridNodeModel() { RegisterAllPorts(); }` 是註冊節點輸入和輸出的建構函式。
* `BuildOutputAst()` 傳回 AST (抽象語法樹)，這是從 NodeModel 節點傳回資料必要的結構。
* `AstFactory.BuildFunctionCall()` 從 `GridFunctions.cs` 呼叫 RectangularGrid 函數。
* `new Func<int, int, double, List<Rectangle>>(GridFunction.RectangularGrid)` 指定函數及其參數。
* `new List<AssociativeNode> { inputAstNodes[0], inputAstNodes[1], sliderValue });` 將節點輸入對映至函數參數
* 如果輸入埠未連接，`AstFactory.BuildNullNode()` 會建置空節點。這可避免在節點上顯示警告。
* `RaisePropertyChanged("SliderValue")` 在滑棒值變更時通知使用者介面
* `var sliderValue = AstFactory.BuildDoubleNode(SliderValue)` 在 AST 中建置表示滑棒值的節點
* 在 functionCall 變數 `new List<AssociativeNode> { inputAstNodes[0], sliderValue });` 中將輸入變更為 `sliderValue` 變數

**3\.呼叫函數**

`CustomNodeModelFunction` 專案將建置到與 `CustomNodeModel` 分開的組合中，以便能被呼叫。

將以下程式碼複製到 `GridFunction.cs`。

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

此函數類別與 Zero-Touch 網格案例研究非常類似，但有一項差異：

* `[IsVisibleInDynamoLibrary(false)]` 讓 Dynamo 看不到下列方法和類別，因為已經從 `CustomNodeModel` 呼叫函數。

正如我們加入 NuGet 套件的參考一樣，`CustomNodeModel` 需要參考 `CustomNodeModelFunction` 才能呼叫函數。

![加入參考](images/vs-add-project-reference.jpg)

> CustomNodeModel 的 using 陳述式將處於非作用中狀態，直到我們參考該函數
>
> 1. 在 `CustomNodeModel` 上按一下右鍵，然後選取「`Add > Reference`」
> 2. 選擇「`Projects > Solution`」
> 3. 勾選 `CustomNodeModelFunction`
> 4. 按一下「`Ok`」

**4\.自訂視圖**

若要建立滑棒，我們需要透過實作 `INodeViewCustomization` 介面來自訂使用者介面。

將以下程式碼複製到 `GridNodeView.cs`

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

* `public class CustomNodeModelView : INodeViewCustomization<GridNodeModel>` 定義自訂使用者介面必要的函數。

設定專案結構後，請使用 Visual Studio 的設計環境建置使用者控制項，並在 `.xaml` 檔案中定義其參數。從工具方塊中，將滑棒加入 `<Grid>...</Grid>`。

![新增滑棒](images/vs-usercontrol.jpg)

> 1. 在 `CustomNodeModel` 上按一下右鍵，然後選取「`Add > New Item`」
> 2. 選取「`WPF`」
> 3. 將使用者控制項命名為 `Slider`
> 4. 按一下「`Add`」

將以下程式碼複製到 `Slider.xaml`

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

* 滑棒控制項的參數定義在 `.xaml` 檔案中。_Minimum 和 Maximum_ 屬性定義此滑棒的數值範圍。
* 在 `<Grid>...</Grid>` 內，我們可以從 Visual Studio 工具箱放置不同的使用者控制項

建立 `Slider.xaml` 檔案時，Visual Studio 會自動建立一個名為 `Slider.xaml.cs` 的 C# 檔案以初始化滑棒。變更此檔案中的名稱空間。

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

* 名稱空間應為 `CustomNodeModel.CustomNodeModel`

`GridNodeModel.cs` 定義滑棒計算邏輯。

**5\.設定為套件**

在建置專案之前，最後一步是加入 `pkg.json` 檔案，讓 Dynamo 可以讀取套件。

![加入 JSON 檔案](images/vs-pkg-json.jpg)

> 1. 在 `CustomNodeModel` 上按一下右鍵，然後選取「`Add > New Item`」
> 2. 選取「`Web`」
> 3. 選取 `JSON File`
> 4. 將檔案命名為 `pkg.json`
> 5. 按一下「`Add`」

* 將以下程式碼複製到 `pkg.json`

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

* `"name":` 決定 Dynamo 資源庫中套件及其群組的名稱
* `"keywords":` 提供搜尋 Dynamo 資源庫的搜尋術語
*   `"node_libraries": []` 是與套件關聯的資源庫

    最後一步是建置方案，並發佈為 Dynamo 套件。請參閱〈套件部署〉一章，以瞭解如何在線上發佈之前建立本端套件，以及如何直接從 Visual Studio 建置套件。
