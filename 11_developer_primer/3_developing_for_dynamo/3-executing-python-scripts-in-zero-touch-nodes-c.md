# 在 Zero-Touch 節點中執行 Python 指令碼 (C#) 

### 在 Zero-Touch 節點中執行 Python 指令碼 (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

如果您能輕鬆地以 Python 撰寫指令碼，也想要從標準 Dynamo Python 節點獲得更多功能，我們可以使用 Zero-Touch 建立自己的指令碼。我們先從一個簡單的範例開始，我們將一個 Python 指令碼做為字串傳入執行指令碼並傳回結果的 Zero-Touch 節點。此案例研究以「入門」區段中的逐步解說和範例為基礎，如果您是初次建立 Zero-Touch 節點，請參閱這些內容。

![將執行 Python 指令碼字串的 Zero-Touch 節點](images/python-case-study.png)

> 將執行 Python 指令碼字串的 Zero-Touch 節點

#### Python 引擎 <a href="#python-engine" id="python-engine"></a>

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

Python 指令碼傳回變數 `output`，這表示我們在 Python 指令碼中需要一個 `output` 變數。請使用此範例指令碼測試 Dynamo 中的節點。如果您曾在 Dynamo 中使用過 Python 節點，以下內容應該看起來很熟悉。如需更多資訊，請參閱 [Primer 的 Python 部分](http://dynamoprimer.com/en/09\_Custom-Nodes/9-4\_Python.html)。

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

![此節點可讓我們傳回立方體的體積及其形心。](images/python-multi-case-study.png)

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
