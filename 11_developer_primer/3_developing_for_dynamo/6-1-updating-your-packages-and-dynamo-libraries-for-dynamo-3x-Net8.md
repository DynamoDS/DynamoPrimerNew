# 針對 Dynamo 3.x 和 NET8 更新您的套件和 Dynamo 資源庫

### 簡介 <a href="#introduction" id="introduction"></a>

本部分包含將圖表、套件和資源庫移轉至 Dynamo 3.x 時，可能會遇到之問題相關資訊。

Dynamo 3.0 為主要版本，某些 API 已變更或移除。其中可能會影響 Dynamo 3.x 開發人員或使用者的最大變更是改為使用 .NET8。

Dotnet/.NET 是執行階段，支援編寫 Dynamo 時使用的 C# 語言。此項目及 Autodesk 生態系統中的其他項目已更新為使用此執行階段的新版本。

您可以在[我們的部落格文章](https://dynamobim.org/dynamo-on-net-8/)中閱讀更多相關資訊。
***

### 套件相容性 <a href="#package-compatibility" id="package-compatibility"></a>

#### 在 Dynamo 3.x 中使用 Dynamo 2.x 套件 
由於 Dynamo 3.x 現在於 .NET8 執行階段上執行，因此針對 Dynamo 2.x 建置的套件 (*使用 .NET48*) 不保證可在 Dynamo 3.x 中運作。嘗試在 Dynamo 3.x 中下載從低於 3.0 的 Dynamo 版本發佈的套件時，您會收到警告，指出套件來自舊版 Dynamo。 

**這不表示套件無法運作**。這只是警告您可能有相容性問題。一般而言，建議您檢查是否有專為 Dynamo 3.x 建置的較新版本。

載入套件時，您也可能會在 Dynamo 記錄檔中看到此類警告。如果所有一切都正常運作，即可忽略警告。

#### 在 Dynamo 2.x 中使用 Dynamo 3.x 套件 

針對 Dynamo 3.x 建置的套件 (*使用.Net8*) 在 Dynamo 2.x上很可能無法運作。若使用舊版，下載為更新版 Dynamo 建置的套件時，您也會看到警告。


