# 針對 Dynamo 4.x 更新套件與 Dynamo 資料庫

### 簡介 <a href="#introduction" id="introduction"></a>

本部分包含將圖表、套件和資源庫移轉至 Dynamo 4.x 時，可能會遇到之問題相關資訊。Dynamo 4.0 引入：
* 顯著的效能改進
* 穩定性和錯誤修正更新
* 將程式碼庫現代化
* 移除先前在 1.x 中標記為舊型的 API
* 主要執行階段從 .NET 8 更新到 .NET 10
* PythonNet 3 現在是所有新 Python 節點的預設 Python 引擎

.NET 10 移轉工作確保 Dynamo 與 Microsoft 的技術路線圖保持一致，遠遠早於 2026 年 11 月 .NET 8 的終止支援。

當您啟動 Dynamo 4.0 時，系統會要求您更新至 .NET 10 (如果尚未更新)。套件作者必須將其專案更新為目標 .NET 10，以確保完全相容。

所有在 Dynamo 4.0+ 中建立的新 Python 節點都從 PythonNet3 開始。不須擔心向下相容性：對於在多版本產品 (例如，Revit 或 Civil 3D 2025/2026) 中工作的使用者，請在 Dynamo 3.3-3.6 中安裝 PythonNet3 引擎套件以維持相容性。您可以在[這裡](https://dynamobim.org/dynamo-core-4-0-release/)找到更多資訊。

在 1.x 中標記為舊型的 API 與節點在 Dynamo 4.0 中已移除。您可以參考[這裡完整的變更清單](https://github.com/DynamoDS/Dynamo/wiki/API-Changes-in-Dynamo-4.0.0)。

### 套件相容性 <a href="#package-compatibility" id="package-compatibility"></a>

#### 在 Dynamo 4.x 中使用 Dynamo 2.x 和 3.x 套件 
由於 Dynamo 4.x 現在於 .NET 10 執行階段執行，因此針對 Dynamo 2.x 建置的套件 (*使用 .NET48*) 和 Dynamo 3.x (*使用 .NET 8*) 不保證可在 Dynamo 4.x 中運作。嘗試在 Dynamo 4.x 中下載從低於 4.0 的 Dynamo 版本發佈的套件時，您會收到警告，指出套件來自舊版 Dynamo。

**這不表示套件無法運作**。這只是警告您可能有相容性問題。一般而言，建議您檢查是否有專為 Dynamo 4.x 建置的較新版本。

載入套件時，您也可能會在 Dynamo 記錄檔中看到此類警告。如果所有一切都正常運作，即可忽略警告。

#### 在 Dynamo 2.x 中使用 Dynamo 4.x 套件 

針對 Dynamo 4.x 建置的套件 (*使用.Net 10*) 在 Dynamo 2.x 上很可能無法運作。當您嘗試在 Dynamo 2.x 中安裝針對 Dynamo 4.x 建置的套件時，您也會看到以下警告。

![套件相容性警告](images/6-2-packages-new-version-compatibility-warning.png)


#### 在 Dynamo 3.x 中使用 Dynamo 4.x 套件 

針對 Dynamo 4.x 建置的套件 (*使用 .NET 10*) 可能可以在 Dynamo 3.x 上運作 (但套件中使用的所有 API 都必須在 .NET 8 中)。但並不保證套件能夠運作。當您嘗試在 Dynamo 3.x 中安裝針對 Dynamo 4.x 建置的套件時，您也會看到以下警告。

![套件相容性警告](images/6-2-packages-compatibility-warning.png)

#### 套件作者的最佳實踐 
最佳實踐是修改 .csproj，將專案同時針對 .NET 8 和 .NET 10 設計。

```
<TargetFrameworks>net8.0;net10.0</TargetFrameworks>
```
這可以確保：
* 支援仍使用 .NET 8 並以 Revit 為主體的 Dynamo 版本
* 與使用 .NET 10 的單機版 Dynamo 4.x 相容