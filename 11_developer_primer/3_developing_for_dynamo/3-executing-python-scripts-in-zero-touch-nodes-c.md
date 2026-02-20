# 在 Zero-Touch 節點中執行 Python 指令碼 (C#)

### 在 Zero-Touch 節點中執行 Python 指令碼 (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

如果您能輕鬆地以 Python 撰寫指令碼，也想要從標準 Dynamo Python 節點獲得更多功能，我們可以使用 Zero-Touch 建立自己的指令碼。我們先從一個簡單的範例開始，我們將一個 Python 指令碼做為字串傳入執行指令碼並傳回結果的 Zero-Touch 節點。此案例研究以「入門」區段中的逐步解說和範例為基礎，如果您是初次建立 Zero-Touch 節點，請參閱這些內容。

![將執行 Python 指令碼字串的 Zero-Touch 節點](../../.gitbook/assets/python-case-study.png)

> 將執行 Python 指令碼字串的 Zero-Touch 節點

#### Python 引擎 <a href="#python-engine" id="python-engine"></a>

如果您從 CPython 移轉，PythonNet3 現在是預設引擎，可提供更順暢的體驗。所有在 Dynamo 4.0+ 中建立的新 Python 節點都從 PythonNet3 開始。

此節點依賴一個 IronPython 指令碼引擎例證。若要執行，我們需要參考一些其他組合。請遵循以下步驟，在 Visual Studio 中設定基本樣板：

* 建立新的 Visual Studio 類別專案
* 在位於 `C:\Program Files (x86)\IronPython 2.7\IronPython.dll` 的 `IronPython.dll` 中加入參考
* 在位於 `C:\Program Files (x86)\IronPython 2.7\Platforms\Net40\Microsoft.Scripting.dll` 的 `Microsoft.Scripting.dll` 中加入參考
* 在您的類別中包括 `IronPython.Hosting` 和`Microsoft.Scripting.Hosting` 兩個 `using` 陳述式
* 加入一個空的私用建構函式，以防止額外的節點隨著我們的套件被加到 Dynamo 資源庫
* 建立一個採用單一字串做為輸入參數的新方法
* 在此方法中，我們將實體化新的 Python 引擎，並建立空的指令碼範圍。您可以將此範圍想像為 Python 解譯器例證內的全域變數
* 接下來，在引擎呼叫 `Execute`，並傳入輸入字串和範圍做為參數
* 最後，在範圍呼叫 `GetVariable`，並從包含您嘗試傳回之值的 Python 指令碼傳入變數名稱，來擷取並傳回指令碼的結果。(請參閱以下範例，以取得進一步的詳細資料)

以下程式碼提供上述步驟的範例。建置方案會在專案的 bin 資料夾中建立一個新的 `.dll`。現在，此 `.dll` 可匯入至 Dynamo 做為套件的一部分，或瀏覽至「`File < Import Library...`」

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;

namespace PythonLibrary
{
    public class PythonZT
    {
        // Unless a constructor is provided, Dynamo will automatically create one and add it to the library
        // To avoid this, create a private constructor
        private PythonZT() { }

        // The method that executes the Python string
        public static string executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of the 'output' variable from the Python script below
            var output = scope.GetVariable("output");
            return (output);
        }
    }
}
```

Python 指令碼傳回變數 `output`，這表示我們在 Python 指令碼中需要一個 `output` 變數。請使用此範例指令碼測試 Dynamo 中的節點。如果您曾在 Dynamo 中使用過 Python 節點，以下內容應該看起來很熟悉。如需更多資訊，請參閱 [Primer 的 Python 一節](https://primer2.dynamobim.org/8_coding_in_dynamo/8-3_python/1-python)。

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
volume = cube.Volume
output = str(volume)
```

#### 多個輸出 <a href="#multiple-outputs" id="multiple-outputs"></a>

標準 Python 節點的一個限制是它們只有單一輸出埠，因此，如果我們希望傳回多個物件，則必須建構清單並擷取其中的每個物件。如果我們修改上述範例以傳回字典，就可以加入任意數目的輸出埠。請參閱〈深入瞭解 Zero-Touch〉中的〈傳回多個值〉一節，以取得有關字典的更多詳細資料。

![此節點可讓我們傳回立方體的體積及其形心。](../../.gitbook/assets/python-multi-case-study.png)

> 此節點可讓我們傳回立方體的體積及其形心。

接下來使用以下步驟修改先前的範例：

* 從 NuGet 套件管理員加入 `DynamoServices.dll` 的參考
* 除了先前的組合，還包括 `System.Collections.Generic` 和 `Autodesk.DesignScript.Runtime`
* 修改方法的傳回類型，以傳回包含輸出的字典
* 每個輸出都必須從範圍中單獨擷取 (請考慮為較大的輸出集設定簡單迴路)

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace PythonLibrary
{
    public class PythonZT
    {
        private PythonZT() { }

        [MultiReturn(new[] { "output1", "output2" })]
        public static Dictionary<string, object> executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of 'output1' from script
            var output1 = scope.GetVariable("output1");
            // Return the value of 'output2' from script
            var output2 = scope.GetVariable("output2");
            // Define the names of outputs and the objects to return
            return new Dictionary<string, object> {
                { "output1", (output1) },
                { "output2", (output2) }
            };
        }
    }
}
```

我們也在範例 Python 指令碼中加入其他輸出變數 (`output2`)。請記住，這些變數可以使用任何合法的 Python 命名慣例，在此範例中是為了清楚起見，所以只使用 output。

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
centroid = Cuboid.Centroid(cube);
volume = cube.Volume
output1 = str(volume)
output2 = str(centroid)
```

#### PythonNet3 已知的限制和解決方法<a href="#pythonnet3-known-Issues-Workarounds" id="pythonnet3-known-Issues-Workarounds"></a>

以下是使用 PythonNet3 時的一些已知限制和解決方法

* .NET 集合不會自動轉換為 Python 清單
    * 在使用 ```len()```、索引或迭代之前，必須使用 ```list(...)``` 明確轉換 ```.NET``` 陣列或集合。
* 一般 .NET 方法可能需要明確的類型參數
    * 除非您手動指定一般類型而不是依賴自動推斷，否則某些方法 (例如 ```GroupBy```) 會失敗。
* 透過 ```dir()``` 或自動完成無法探索延伸方法
    * 延伸方法在明確呼叫時可能仍然有作用，但不會在自省或程式碼完成時出現。
* 不支援 DataTable 延伸方法
    * 匯入 ```System.Data.DataTableExtensions``` 失敗；這些輔助工具方法不能直接使用。
* 某些 Dynamo 核心方法在 PythonNet3 中的行為不同
    * 由於集合處理比較嚴格，因此某些功能 (例如清單展開) 可能無法如預期般運作。
* 如果 Python 類別是從 .NET 類型繼承，則無法在節點之間傳遞類別
    * 從 .NET 類型或介面衍生的類別無法在 Python 節點之間安全地轉移。
* Python ```set()``` 不接受某些 .NET 物件
    * 諸如 ```InvalidElementId``` 之類的物件必須篩選掉或改用 .NET 集合處理。
* 頻繁呼叫 ```print()``` 可能會導致記憶體增長
    * 請避免在迴圈或長時間執行的腳本中大量使用 ```print()```。
* Dynamo 與 Python 之間的字典互通性有限
    * Dynamo 字典和 Python 字典無法完全互換，可能需要手動轉換。
* 無法再使用 ```Marshal.GetActiveObject()``` 方法取得指定物件正在執行的 COM 例證
    * 如果您知道使用中檔案的路徑，請使用 ```BindToMoniker```。
    * 使用 C# 的 ```Marshal.GetActiveObject()``` 類別結構撰寫資源庫程式碼

#### 從 CPython3 移轉至 PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Dynamo 會自動將 CPython 節點移轉至 PythonNet 3。以下是發生的情況：

> 1. 自動建立原始檔的備份複本。
> 2. 所有 CPython 節點 (包括使用 CPython 的自訂節點) 都轉換為 PythonNet3。 
> 3. 出現一則快顯通知讓您知道移轉的節點數。
> 4. 儲存時，您會看到一個提醒，指出您的 Python 節點現在將使用 PythonNet3。您同樣不須擔心向下相容性：對於在多版本產品 (例如，Revit 或 Civil 3D 2025/2026) 中工作的使用者，請在 Dynamo 3.3-3.6 中安裝 PythonNet3 引擎套件以維持相容性。 

#### 從 IronPython2 移轉至 PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

如果您的圖表使用 IronPython 引擎，則不會自動移轉。 

如果安裝相符的 IronPython 套件，圖表會正常執行。如果缺少套件，您會在「工作區參考」延伸中看到要求您下載套件的相依性警告。您可以重新安裝套件以繼續使用 IronPython。但是，由於 IronPython 已經好幾年沒有更新，而且 Dynamo 也已經有一段時間沒有積極支援這些引擎，我們強烈建議您移轉到 PythonNet3，確保您的圖表可以繼續可靠地運作。雖然 DynamoIronPython2.7 和 DynamoIronPython3 會繼續以套件的形式在 Dynamo Package Manager 中提供，但 Dynamo 團隊不會再加以維護。 

在此情況下，您可以利用 Python 編輯器中的「移轉助理」，將節點逐一進行移轉。  

如需有關移轉的更多資訊，請參閱此[部落格](https://dynamobim.org/dynamo-pythonnet3-upgrade-a-practical-guide-to-migrating-your-dynamo-graphs/)