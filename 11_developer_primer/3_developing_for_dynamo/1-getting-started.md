# 入門

在開始開發之前，為新專案建立堅實的基礎非常重要。Dynamo 開發人員社群中有幾個專案樣板非常適合開始，但瞭解如何從頭開始一個專案更有價值。從頭開始建立一個專案可以更深入地瞭解開發過程。

![Visual Studio](images/visual-studio.jpg)

### 建立 Visual Studio 專案 <a href="#creating-a-visual-studio-project" id="creating-a-visual-studio-project"></a>

Visual Studio 是一個功能強大的 IDE，我們可在其中建立專案、新增參考、建置 `.dlls` 和除錯。建立新專案時，Visual Studio 也會建立一個「方案」，這是組織專案的結構。多個專案可以在單一方案中，也可以一起建置。若要建立 ZeroTouch 節點，我們需要啟動新的 Visual Studio 專案，然後在當中撰寫 C# 類別資源庫並建置 `.dll`。

![在 Visual Studio 中建立新專案](images/vs-new-project-1.jpg)

![在 Visual Studio 中設定新專案](images/vs-new-project-2.jpg)

> Visual Studio 中的「新增專案」視窗
>
> 1. 首先開啟 Visual Studio 並建立新專案：「`File > New > Project`」
> 2. 選擇「`Class Library`」專案樣板
> 3. 為專案命名 (我們已將專案命名為 MyCustomNode)
> 4. 設定專案的檔案路徑。在此範例中，我們讓它保留在預設位置
> 5. 選取「`Ok`」

Visual Studio 會自動建立 C# 檔案並開啟。我們應為其指定適當的名稱、設定工作區，然後以此乘法方法更換預設程式碼。

```
 namespace MyCustomNode
 {
     public class SampleFunctions
     {
         public static double MultiplyByTwo(double inputNumber)
         {
             return inputNumber * 2.0;
         }
     }
 }
```

![使用「方案總管」](images/vs-edit-class.jpg)

> 1. 從「`View`」開啟「方案總管」和「輸出」視窗。
> 2. 在右側的「方案總管」中，將 `Class1.cs` 檔案更名為 `SampleFunctions.cs`。
> 3. 為乘法函數加入上述程式碼。我們稍後會介紹 Dynamo 如何讀取您的 C# 類別。
> 4. 「方案總管」：您可以從這裡存取專案中的所有內容。
> 5. 「輸出」視窗：我們稍後需要此視窗，以查看建置是否成功。

下一步是建置專案，但在那之前，需要檢查一些設定。首先，請確定在「平台目標」選取了「`Any CPU`」或「`x64`」，且「專案屬性」中未勾選「`Prefer 32-bit`」。

![Visual Studio 建置設定](images/vs-build-settings.jpg)

> 1. 選取「`Project > "ProjectName" Properties`」開啟專案屬性
> 2. 選取「`Build`」頁面
> 3. 從下拉式功能表中選取「`Any CPU`」或「`x64`」
> 4. 確保未勾選「`Prefer 32-bit`」

現在我們可以建置專案來建立 `.dll`。若要執行，請從「`Build`」功能表中選取「`Build Solution`」，或使用捷徑 `CTRL+SHIFT+B`。

![建置方案](images/vs-build.jpg)

> 1. 選取「`Build > Build Solution`」
> 2. 您可以查看「輸出」視窗，判斷專案是否成功建置

如果專案已成功建置，則專案的 `bin` 資料夾中會有一個名為 `MyCustomNode` 的 `.dll`。在此範例中，我們將專案的檔案路徑保留為 Visual Studio 的預設路徑：`c:\users\username\documents\visual studio 2015\Projects`。我們來看看專案的檔案結構。

![專案的檔案結構](images/folder-structure.jpg)

> 1. `bin` 資料夾包含從 Visual Studio 建置的 `.dll`。
> 2. Visual Studio 專案檔。
> 3. 類別檔案。
> 4. 由於我們的方案組態已設定為「`Debug`」，因此將在 `bin\Debug` 中建立 `.dll`。

現在我們可以開啟 Dynamo 並匯入 `.dll`。使用「加入」功能，瀏覽至專案的 `bin` 位置，然後選取要開啟的 `.dll`。

![開啟專案的 dll 檔案](images/dyn-import-dll.jpg)

> 1. 選取「加入」按鈕以匯入 `.dll`
> 2. 瀏覽至專案位置。我們的專案位於 Visual Studio 的預設檔案路徑：`C:\Users\username\Documents\Visual Studio 2015\Projects\MyCustomNode`
> 3. 選取要匯入的 `MyCustomNode.dll`
> 4. 按一下「`Open`」以載入 `.dll`

如果在名為 `MyCustomNode` 的資源庫中建立了品類，則表示已成功匯入 .dll！但是，Dynamo 建立了兩個節點，而我們只想要一個節點。在下一節中，我們將說明發生這個狀況的原因，以及 Dynamo 如何讀取 .dll。

![自訂節點](images/dyn-customnode.jpg)

> 1. Dynamo 資源庫中的 MyCustomNode。「資源庫」品類由 `.dll` 名稱決定。
> 2. 圖元區上的 SampleFunctions.MultiplyByTwo。

### Dynamo 如何讀取類別和方法 <a href="#how-dynamo-reads-classes-and-methods" id="how-dynamo-reads-classes-and-methods"></a>

Dynamo 載入 .dll 時，會將所有公用靜態方法顯示為節點。建構函式、方法和性質將分別轉換為「建立」、「動作」和「查詢」節點。在乘法範例中，`MultiplyByTwo()` 方法會變成 Dynamo 中的「動作」節點。這是因為節點已根據其方法和類別命名。

![圖表中的 SampleFunction.MultiplyByTwo 節點](images/multiplybytwo.png)

> 1. 輸入根據方法的參數名稱命名為 `inputNumber`。
> 2. 輸出預設命名為 `double`，因為這是要傳回的資料類型。
> 3. 節點命名為 `SampleFunctions.MultiplyByTwo`，因為這些是類別名稱和方法名稱。

在以上範例中，多建立一個 `SampleFunctions` 建立節點是因為我們未明確提供建構函式，因此自動建立了一個建構函式。我們可以在 `SampleFunctions` 類別中建立一個空的私用建構函式來避免發生這個情況。

```
namespace MyCustomNode
{
    public class SampleFunctions
    {
        //The empty private constructor.
        //This will be not imported into Dynamo.
        private SampleFunctions() { }

        //The public multiplication method. 
        //This will be imported into Dynamo.
        public static double MultiplyByTwo(double inputNumber)
        {
            return inputNumber * 2.0;
        }
    }
}
```

![方法已匯入為建立節點](images/private-constructor.jpg)

> 1. Dynamo 已將我們的方法匯入為「建立」節點

### 加入 Dynamo NuGet 套件參考 <a href="#adding-dynamo-nuget-package-references" id="adding-dynamo-nuget-package-references"></a>

乘法節點非常簡單，不需要參考 Dynamo。例如，如果我們要存取任何 Dynamo 功能來建立幾何圖形，則需要參考 Dynamo NuGet 套件。

* [ZeroTouchLibrary](https://www.nuget.org/packages/DynamoVisualProgramming.ZeroTouchLibrary/2.0.0-beta3026) \- 用於為 Dynamo 建置 zero touch 節點資源庫的套件，包含以下資源庫：DynamoUnits.dll、ProtoGeometry.dll
* [WpfUILibrary](https://www.nuget.org/packages/DynamoVisualProgramming.WpfUILibrary/2.0.0-beta3026) \- 用於為 Dynamo 建置具有 WPF 自訂使用者介面之節點資源庫的套件，包含以下資源庫：DynamoCoreWpf.dll、CoreNodeModels.dll、CoreNodeModelWpf.dll
* [DynamoServices](https://www.nuget.org/packages/DynamoVisualProgramming.WpfUILibrary/2.0.0-beta3026) \- 適用於 Dynamo 的 DynamoServices 資源庫
* [Core](https://www.nuget.org/packages/DynamoVisualProgramming.Core/2.0.0-beta3026) \- Dynamo 的單位與系統測試基礎架構，包含以下資源庫：DSIronPython.dll、DynamoApplications.dll、DynamoCore.dll、DynamoInstallDetective.dll、DynamoUtilities.dll、ProtoCore.dll、VMDataBridge.dll
* [Tests](https://www.nuget.org/packages/DynamoVisualProgramming.Tests/2.0.0-beta3026) \- Dynamo 的單位與系統測試基礎架構，包含以下資源庫：DynamoCoreTests.dll、SystemTestServices.dll、TestServices.dll
* [DynamoCoreNodes](https://www.nuget.org/packages/DynamoVisualProgramming.DynamoCoreNodes/2.0.0-beta3026) \- 用於為 Dynamo 建置核心節點的套件，包含以下資源庫：Analysis.dll、GeometryColor.dll、DSCoreNodes.dll

若要在 Visual Studio 專案中參考這些套件，請從上面連結的 NuGet 下載套件並手動參考 .dll，或使用 Visual Studio 中的 NuGet 套件管理員。首先，我們可以逐步瞭解如何在 Visual Studio 中使用 NuGet 安裝這些套件。

![開啟 NuGet 套件管理員](images/vs-nuget-package-manager2.jpg)

> 1. 選取「`Tools > NuGet Package Manager > Manage NuGet Packages for Solution...`」開啟 NuGet 套件管理員

這是 NuGet 套件管理員。此視窗顯示已為專案安裝的套件，也可讓使用者瀏覽其他套件。如果發行了新版本的 DynamoServices 套件，則可從此處更新套件或回復至舊版。

![NuGet 套件管理員](images/vs-nuget-package-manager.jpg)

> 1. 選取「瀏覽」並搜尋 DynamoVisualProgramming 以顯示 Dynamo 套件。
> 2. Dynamo 套件。選取一個套件會顯示套件的目前版本和內容描述。
> 3. 選取您需要的套件版本，然後按一下「安裝」。這會為您正在處理的特定專案安裝套件。由於我們使用的是 Dynamo 的最新穩定版本 1.3 版，因此請選擇對應的套件版本。

若要手動加入從瀏覽器下載的套件，請從「方案總管」開啟「參考管理員」，然後瀏覽套件。

![參考管理員](images/vs-manual-dynamo-package.jpg)

> 1. 以右鍵按一下「`References`」，然後選取「`Add Reference`」。
> 2. 選取「`Browse`」以瀏覽至套件位置。

現在 Visual Studio 已正確設定，我們也已成功將 `.dll` 加入 Dynamo，因此，我們已經有堅實的基礎可以讓概念繼續發揮。這只是開始，請繼續跟進，進一步瞭解如何建立自訂節點。
