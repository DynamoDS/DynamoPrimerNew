# Zero-Touch 案例研究 - 網格節點

在 Visual Studio 專案啟動並執行時，我們將逐步瞭解如何建置一個自訂節點來建立一個矩形的儲存格網格。雖然我們可以使用幾個標準節點建立此節點，但它是一個實用的工具，可以輕鬆包含在 Zero-Touch 節點中。與網格線相反，儲存格可以繞其中心點調整比例、查詢其角落頂點，或建置到面中。

本範例將介紹建立 Zero-Touch 節點時要注意的一些功能和概念。建置自訂節點並加入 Dynamo 後，請務必檢閱「深入瞭解 Zero-Touch」頁面，以更深入地查看預設輸入值、傳回多個值、文件、物件、使用 Dynamo 幾何圖形類型，以及移轉。

![矩形網格圖表](images/cover-image.jpg)

#### 自訂矩形網格節點 <a href="#custom-rectangular-grid-node" id="custom-rectangular-grid-node"></a>

若要開始建置網格節點，請建立新的 Visual Studio 類別資源庫專案。請參閱「入門」頁面，以取得如何設定專案的深入逐步解說。

![在 Visual Studio 中建立新專案](images/vs-new-project-1.jpg)

![在 Visual Studio 中設定新專案](images/vs-new-project-2.jpg)

> 1. 選擇「`Class Library`」做為專案類型
> 2. 將專案命名為 `CustomNodes`

由於我們要建立幾何圖形，因此需要參考適當的 NuGet 套件。從 Nuget 套件管理員安裝 ZeroTouchLibrary 套件。`using Autodesk.DesignScript.Geometry;` 陳述式需要此套件。

![ZeroTouchLibrary 套件](images/vs-nugetpackage.jpg)

> 1. 瀏覽 ZeroTouchLibrary 套件
> 2. 我們將在 Dynamo Studio 的目前建置 (1.3) 中使用此節點。選取與此相符的套件版本。
> 3. 請注意，我們也已將類別檔案更名為 `Grids.cs`

接下來，我們需要建立 RectangularGrid 方法將位於的命名空間和類別。節點會根據方法名稱和類別名稱在 Dynamo 中命名。我們還不需要將此內容複製到 Visual Studio。

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

> `Autodesk.DesignScript.Geometry;` 參考 ZeroTouchLibrary 套件中的 ProtoGeometry.dll。建立清單需要 `System.Collections.Generic`

現在，我們可以加入繪製矩形的方法。類別檔案應如下所示，可複製到 Visual Studio。

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

如果專案看起來與此類似，請繼續並嘗試建置 `.dll`。

![建置 DLL](images/vs-grids.jpg)

> 1. 選擇「建置」>「建置方案」

檢查專案的 `bin` 資料夾是否有 `.dll`。如果建置成功，我們可以將 `.dll` 加入 Dynamo。

![Dynamo 中的自訂節點](images/RectangularGrid-Dynamo.jpg)

> 1. Dynamo 資源庫中的自訂 RectangularGrids 節點
> 2. 圖元區上的自訂節點
> 3. 將 `.dll` 加入 Dynamo 的「加入」按鈕

#### 修改自訂節點 <a href="#custom-node-modifications" id="custom-node-modifications"></a>

在以上範例中，我們建立了一個相當簡單的節點，此節點除了 `RectangularGrids` 方法外沒有定義太多其他內容。但是，我們也許需要建立輸入埠的工具提示，或為節點提供類似標準 Dynamo 節點的摘要。將這些功能加入自訂節點，可以讓使用者更輕鬆地使用這些節點，尤其是當使用者想要在資源庫中搜尋這些節點時。

![輸入工具提示](images/nodemodification.png)

> 1. 預設輸入值
> 2. xCount 輸入的工具提示

RectangularGrid 節點需要某些基本功能。在以下程式碼中，我們加入了輸入埠和輸出埠的描述、摘要和預設輸入值。

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

* 對方法參數指定值來提供輸入預設值：`RectangularGrid(int xCount = 10, int yCount = 10)`
* 建立輸入和輸出的工具提示、搜尋關鍵字，以及 XML 文件前面有 `///` 的摘要。

若要加入工具提示，專案目錄中需要一個 xml 檔案。透過啟用選項，Visual Studio 可以自動產生 `.xml`。

![啟用 XML 文件](images/vs-xml.jpg)

> 1. 在此啟用 XML 文件檔案並指定檔案路徑。這會產生 XML 檔案。

這樣就可以了！我們建立了一個有幾項標準功能的新節點。下一章〈Zero-Touch 基礎知識〉會更詳細介紹 Zero-Touch 節點開發及需要注意的問題。
