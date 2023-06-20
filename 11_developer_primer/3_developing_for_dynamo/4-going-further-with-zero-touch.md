# 深入瞭解 Zero-Touch 

瞭解如何建立 Zero-Touch 專案後，我們可以逐步瀏覽 Dynamo Github 上的 ZeroTouchEssentials 範例，更深入地瞭解建立節點的詳細資訊。

![Zero-touch 節點](images/ootbzerotouch.png)

> 許多 Dynamo 的標準節點本質上是 Zero-Touch 節點，如上面大多數的 Math、Color 和 DateTime 節點。

若要開始，請從 [https://github.com/DynamoDS/ZeroTouchEssentials](https://github.com/DynamoDS/ZeroTouchEssentials) 下載 ZeroTouchEssentials 專案

在 Visual Studio 中，開啟 `ZeroTouchEssentials.sln` 方案檔並建置方案。

![Visual Studio 中的 ZeroTouchEssentials](images/vs-build-zte.jpg)

> `ZeroTouchEssentials.cs` 檔案包含我們要匯入至 Dynamo 的所有方法。

開啟 Dynamo 並匯入 `ZeroTouchEssentials.dll`，以取得我們要在以下範例中參考的節點。

程式碼範例是從 [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/master/ZeroTouchEssentials/ZeroTouchEssentials.cs) 提取，通常與其相符。為了簡潔已移除 XML 文件，每個程式碼範例都會建立其上方影像中的節點。

#### 預設輸入值 <a href="#default-input-values" id="default-input-values"></a>

Dynamo 支援定義節點上輸入埠的預設值。如果埠沒有連接，則會對節點提供這些預設值。預設值使用在《[C# 程式設計手冊](https://msdn.microsoft.com/en-us/library/dd264739.aspx)》中指定可選引數的 C# 機制表示。預設值以下列方式指定：

* 將方法參數設定為預設值：`inputNumber = 2.0`

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

![預設值](images/defaultval.jpg)

> 1. 將游標懸停在節點輸入埠上時，會出現預設值

#### 傳回多個值 <a href="#returning-multiple-values" id="returning-multiple-values"></a>

傳回多個值比建立多個輸入要複雜一些，因此需要使用字典傳回。字典的項目會成為節點輸出端的埠。透過以下方式可以建立多個傳回埠：

* 加入 `using System.Collections.Generic;` 以使用 `Dictionary<>`。
* 加入 `using Autodesk.DesignScript.Runtime;` 以使用 `MultiReturn` 屬性。這會參考 DynamoServices NuGet 套件中的「DynamoServices.dll」。
* 在方法中加入 `[MultiReturn(new[] { "string1", "string2", ... more strings here })]` 屬性。字串會參考字典中的鍵，並成為輸出埠名稱。
* 從函數傳回 `Dictionary<>`，其中的鍵與屬性中的參數名稱相符：`return new Dictionary<string, object>`

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

> 請參閱 [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L70) 中的此程式碼範例

傳回多個輸出的節點。

![多個輸出](images/multipleoutputs.png)

> 1. 請注意，現在有兩個輸出埠，根據我們為字典的鍵輸入的字串進行命名。

#### 文件、工具提示和搜尋 <a href="#documentation-tooltips-and-search" id="documentation-tooltips-and-search"></a>

最佳實踐是在 Dynamo 節點中加入文件，描述節點的功能、輸入、輸出、搜尋標籤等。這可以透過 XML 文件標籤完成。透過以下方式可以建立 XML 文件：

* 前面有三條斜線的任何註解文字都會視為文件
  * 例如：`/// Documentation text and XML goes here`
* 三條斜線後，在 Dynamo 匯入 .dll 時將讀取的方法上方建立 XML 標籤
  * 例如：`/// <summary>...</summary>`
* 在 Visual Studio 中選取「`Project > Project Properties > Build`」並勾選「`XML documentation file`」以啟用 XML 文件

![產生 XML 檔案](images/vs-xml.jpg)

> 1. Visual Studio 將在指定位置產生 XML 檔案

標籤類型如下：

* `/// <summary>...</summary>` 是節點的主文件，會以工具提示的形式出現在左側搜尋列中的節點上
* `/// <param name="inputName">...</param>` 將建立特定輸入參數的文件
* `/// <returns>...</returns>` 將建立輸出參數的文件
* `/// <returns name = "outputName">...</returns>` 將建立多個輸出參數的文件
* `/// <search>...</search>` 將根據逗號分隔清單將您的節點與搜尋結果比對。例如，如果我們建立細分網格的節點，我們可能要加入標籤，例如「網格」、「細分」和「catmull-clark」。

以下是包含輸入和輸出描述的範例節點，以及將顯示在資源庫中的摘要。

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

> 請參閱 [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L80) 中的此程式碼範例

請注意，此範例節點的程式碼包含：

> 1. 節點摘要
> 2. 輸入描述
> 3. 輸出描述

#### 物件 <a href="#objects" id="objects"></a>

Dynamo 沒有 `new` 關鍵字，因此需要使用靜態建構方法建構物件。透過以下方式可以建構物件：

* 除非另有要求，否則將建構函式設為內部的 `internal ZeroTouchEssentials()`
* 使用靜態方法建構物件，例如 `public static ZeroTouchEssentials ByTwoDoubles(a, b)`

> 注意事項：Dynamo 使用「By」字首指出靜態方法是建構函式，雖然這是選擇性的，但使用「By」可協助您的資源庫更符合既有的 Dynamo 型式。

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

> 請參閱 [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L26) 中的此程式碼範例

匯入 ZeroTouchEssentials dll 後，資源庫中將會有 ZeroTouchEssentials 節點。使用 `ByTwoDoubles` 節點可以建立此物件。

![ByTwoDoubles 節點](images/dyn-constructor.jpg)

#### 使用 Dynamo 幾何圖形類型 <a href="#using-dynamo-geometry-types" id="using-dynamo-geometry-types"></a>

Dynamo 資源庫可以使用原生 Dynamo 幾何圖形類型做為輸入，並建立新幾何圖形做為輸出。透過以下方式可以建立幾何圖形類型：

* 在專案中的 C# 檔案頂端包括 `using Autodesk.DesignScript.Geometry;` 參考「ProtoGeometry.dll」，並將 ZeroTouchLibrary NuGet 套件加入專案。
* **重要事項：** 管理未從函數傳回的幾何圖形資源，請參閱下方的〈**Dispose/using 陳述式**〉一節。

> 注意事項：Dynamo 幾何圖形物件的使用方式與任何其他傳入函數的物件相同。

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

> 請參閱 [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86) 中的此程式碼範例

取得曲線長度後加倍的節點。

![曲線輸入](images/doublelength.png)

> 1. 此節點接受曲線幾何圖形類型做為輸入。

#### Dispose/using 陳述式 <a href="#disposeusing-statements" id="disposeusing-statements"></a>

除非您使用 Dynamo 2.5 版或更高版本，否則必須手動管理未從函數傳回的幾何圖形資源。在 Dynamo 2.5 和更高版本中，幾何圖形資源由系統內部處理，但是，如果您的使用案例較複雜，或者您必須在決定性時間減少消耗記憶體，您可能仍需要手動處置幾何圖形。Dynamo 引擎將處理從函數傳回的任何幾何圖形資源。透過以下方式可以手動處理未傳回的幾何圖形資源：

*   使用 using 陳述式：

    ```
    using (Point p1 = Point.ByCoordinates(0, 0, 0))
    {
      using (Point p2 = Point.ByCoordinates(10, 10, 0))
      {
          return Line.ByStartPointEndPoint(p1, p2);
      }
    }
    ```

    > [此處](https://msdn.microsoft.com/en-us/library/yh598w02.aspx)說明 using 陳述式
    >
    > 請參閱 [Dynamo 幾何圖形穩定性的改進](https://forum.dynamobim.com/t/dynamo-geometry-stability-improvements-request-for-feedback/39297)，以進一步瞭解 Dynamo 2.5 中引入的新穩定性功能
*   使用手動的 Dispose 呼叫：

    ```
    Point p1 = Point.ByCoordinates(0, 0, 0);
    Point p2 = Point.ByCoordinates(10, 10, 0);
    Line l = Line.ByStartPointEndPoint(p1, p2);
    p1.Dispose();
    p2.Dispose();
    return l;
    ```

#### 移轉 <a href="#migrations" id="migrations"></a>

發佈較新版本的資源庫時，節點名稱可能會變更。在移轉檔案中可以指定名稱變更，以便在更新時，以舊版資源庫建置的圖表可以持續正常運作。透過以下方式可以實作移轉：

* 使用以下格式，在與 `.dll` 相同的資料夾中建立 `.xml` 檔案：「BaseDLName」.Migrations.xml
* 在 `.xml` 中，建立單一 `<migrations>...</migrations>` 元素
* 在 migrations 元素內，為每個名稱變更建立 `<priorNameHint>...</priorNameHint>` 元素
* 對每個名稱變更，提供 `<oldName>...</oldName>` 和 `<newName>...</newName>` 元素

![移轉檔案](images/vs-migrations-file.jpg)

> 1. 按一下右鍵，然後選取「`Add > New Item`」
> 2. 選擇 `XML File`
> 3. 對於此專案，我們將移轉檔案命名為 `ZeroTouchEssentials.Migrations.xml`

此範例程式碼告訴 Dynamo，任何名為 `GetClosestPoint` 的節點現在都命名為 `ClosestPointTo`。

```
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>Autodesk.DesignScript.Geometry.Geometry.GetClosestPoint</oldName>
    <newName>Autodesk.DesignScript.Geometry.Geometry.ClosestPointTo</newName>
  </priorNameHint>
</migrations>
```

> 請參閱 [ProtoGeometry.Migrations.xml](https://github.com/DynamoDS/Dynamo/blob/master/extern/ProtoGeometry/ProtoGeometry.Migrations.xml) 中的此程式碼範例

#### 泛型 <a href="#generics" id="generics"></a>

Zero-Touch 目前不支援使用泛型。可以使用泛型，但不能在直接匯入且未設定類型的程式碼中使用。無法顯示屬於泛型且未設定類型的方法、性質或類別。

在以下範例中，不會匯入類型為 `T` 的 Zero-Touch 節點。如果將剩餘的資源庫匯入至 Dynamo，則會發生缺少類型的例外狀況。

```
public class SomeGenericClass<T>
{
    public SomeGenericClass()
    {
        Console.WriteLine(typeof(T).ToString());
    }  
}
```

在此範例中設定類型的情況下使用泛型類型，將匯入至 Dynamo。

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
