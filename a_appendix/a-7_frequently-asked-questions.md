# 常見問題 (FAQ) 

## 如何利用 Dynamo 建置版本

### 每日建置版本與穩定建置版本
Autodesk 的 Dynamo 團隊傳統上會在每次提交時發佈每日建置版本，並在系統測試與發佈週期之後發佈穩定的建置版本，來維持快速的更迭步調。我們的團隊很樂意繼續提供每日與穩定的建置版本，讓使用者能控制 DynamoCore 在本機磁碟上的解壓縮位置，而使用者可以放心使用，不會影響其他 ADSK 產品的 Dynamo。基於這個原因有一些自然的候選者，包括 .nupkg、.zip檔或專用安裝程式，使用者可以在其中選擇安裝路徑或其他選項。 

我們的目標是盡可能以最簡單的方式讓使用者取得最新程式碼，因此我們決定提供一個 .zip 檔案，其中包含 DynamoCore 二進位檔案和可在沒有Revit 的情況下使用的 Dynamo Sandbox (但有一些限制)。

### Dynamo Zip 建置版本
#### 定義和來源
DynamoCoreRuntime Zip 建置版本是在自動建置版本期間建立的 DynamoCore 二進位檔案的快照。 

您應該可以在解壓縮的資料夾中啟動 DynamoSandbox.exe，以最少的設定使用 Dynamo。


#### 必要元件

| Dynamo 版本  |Microsoft Visual C++  | DirectX  |   |   |   |   |
|---|---|---|---|---|---|---|
|  2.0 - 2.6 |  2015 可轉散發套件  | 10  |   |   |   |   |
| 2.7  | 2019 可轉散發套件  | 11/12 (Windows 10 隨附)  |   |   |   |   |
| >=2.8  | 2019 可轉散發套件  | 11/12 (Windows 10 隨附)  |   |   |   |   |
##### Microsoft DirectX，在我們[此處](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX)的 Dynamo Github 儲存庫中也能公開取得

##### 7zip ([此處](https://www.7-zip.org/download.html))，用於解壓縮套件


##### Microsoft Visual C++ 2015-2024 可轉散發套件 (x64) [連結](https://aka.ms/vs/17/release/vc_redist.x64.exe)

##### 選擇性元件
幾何圖形資源庫 (只有特定的 Autodesk 塑型工具，如 Revit、Civil 3D、Advanced Steel 等才提供)

### 疑難排解
如果您解壓縮了建置版本，卻完全無法啟動 DynamoSandbox.exe，請務必使用 [7zip](https://www.7-zip.org/download.html) 解壓縮建置版本。如果您有電腦的權限，也可以在解壓縮*之前* 手動解除封鎖 .zip 歸檔檔案。

![](images/a-7/dynamo-builds-1.png)


如果您缺少任何必要的元件，使用 Dynamo 時可能會遇到問題，UI 的某些部分可能無法載入。

以下列螢幕擷取畫面為例，在沒有 GPU 的全新安裝 Windows 10 VM 上解壓縮我們的建置版本，電腦缺少兩個必要元件。這在 Dynamo 主控台中有指示。

![](images/a-7/dynamo-builds-2.png)

##### 安裝 DirectX
請按照此處的 Microsoft 指示，檢查是否已安裝 DirectX。如果沒有，您可以在我們[此處](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX)的 Dynamo Github 儲存庫中開啟 DXSETUP.exe。看到下面的對話方塊後，請按「下一步」，將 DirectX 安裝到預設位置。

![](images/a-7/dynamo-builds-3.png)

##### 安裝 Microsoft Visual C++ 2015-2024 可轉散發套件 (x64)
請在[此處](https://aka.ms/vs/17/release/vc_redist.x64.exe)下載最新版本。然後，您應該可以在瀏覽器下載位置中執行名為 vc_redist.x64.exe 的安裝程式。看到下面的對話方塊後，請按一下「安裝」，將此元件放在預設位置。

![](images/a-7/dynamo-builds-4.png)


透過上面的連結安裝兩個必要元件後，重新啟動 DynamoSandbox.exe，您應該會看到以下結果：

![](images/a-7/dynamo-builds-5.png)

##### 缺少 3D 圖形。 

您第一次執行 Sandbox 時可能還會遇到圖形問題，可以按照以下的標準圖形問題 FAQ 處理：

[https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ)

一般而言，使用 DynamoSandbox.exe 時，您可能需要為圖形卡強制使用高效能 GPU 模式

_NVIDIA 控制台範例：_

![](images/a-7/dynamo-builds-6.png)

##### 安裝 WebView2 Runtime
接下來這些 Dynamo 模組將會使用 WebView2 元件：文件瀏覽器、導覽和資源庫，因此為了確保 Dynamo 的這些部分能正確顯示網路內容，我們需要安裝 WebView2 Evergreen Runtime 安裝程式 (您需要驗證電腦中是否已安裝或需要安裝)。

這是安裝 WebView2 Runtime 時的連結：[https://developer.microsoft.com/en-us/microsoft-edge/webview2/#download-section](https://developer.microsoft.com/en-us/microsoft-edge/webview2/#download-section)

![](images/a-7/dynamo-builds-7.png)

應該安裝的 (只是其中之一) 是 Evergreen Bootstrapper 或 Evergreen Standalone Installer，第一個會下載一個 1.50 MB 安裝程式，第二個會下載一個 130 MB 安裝程式。

安裝 Runtime 後，接下來的 Dynamo 元件應該都可以正常運作：

![](images/a-7/dynamo-builds-8.png)


##### Dynamo Excel 節點問題
您可以參考此篇[文章](https://www.autodesk.com.cn/support/technical/article/caas/sfdcarticles/sfdcarticles/CHS/Warning-Data-ImportExcel-operation-failed-Could-not-load-file-or-assembly-Microsoft-Office-Interop-Excel-when-running-the-Dynamo-script-in-Revit.html)進行診斷。

### Dynamo 建置版本位置
穩定發行版本

[https://dynamobim.org/download/](https://dynamobim.org/download/)

[https://github.com/DynamoDS/Dynamo/releases](https://github.com/DynamoDS/Dynamo/releases)

每日建置版本和穩定發行版本

[https://dynamobuilds.com/](https://dynamobuilds.com/)

