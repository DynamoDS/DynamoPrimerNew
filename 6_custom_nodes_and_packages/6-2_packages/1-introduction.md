# 套件簡介

簡言之，套件是自訂節點的集合。Dynamo Package Manager 是供社群對已經線上發佈的套件進行下載的入口網站。這些工具集由協力廠商為了延伸 Dynamo 的核心功能而開發，任何人都能存取，按一下按鈕即可隨時下載。

![Package Manager 網站](../images/6-2/1/dpm.jpg)

開放原始碼專案 (例如 Dynamo) 在此類型的社群參與下蓬勃發展。使用專屬的協力廠商開發人員，Dynamo 可以將其適用範圍延伸到一系列產業的工作流程中。因此，Dynamo 團隊齊心協力簡化套件的開發與發佈 (在後續各節中將更詳細地討論這一點)。

### 安裝套件

最簡易的套件安裝方式是使用 Dynamo 介面中的套件工具列。現在我們使用該工具列並安裝一個套件。在此簡單範例中，我們將安裝某常見套件，以便在格線上建立四邊形面板。

在 Dynamo 中，移至 _「套件」>「搜尋套件...」_

![](<../images/6-2/1/package introduction - installing a package 01.jpg>)

在搜尋列中，我們搜尋「quads from rectangular grid」。片刻之後，您應該會看到符合此搜尋查詢的所有套件。我們要選取具有相符名稱的第一個套件。

按一下「安裝」，將此套件加入您的資源庫。完成！

![](<../images/6-2/1/package introduction - installing a package 02.jpg>)

請注意，Dynamo 資源庫中現在存在另一個群組，稱為「buildz」。此名稱是指套件的開發人員，此群組中已放置自訂節點。我們可以立即開始使用此群組。

![](<../images/6-2/1/package introduction - installing a package 03.jpg>)

使用 **Code Block** 可快速定義矩形格線，將結果輸出至 **Polygon.ByPoints** 節點，然後輸出至 **Surface.ByPatch** 節點，以檢視您剛剛建立的矩形板清單。

![](<../images/6-2/1/package introduction - installing a package 04.jpg>)

### 安裝套件資料夾 - DynamoUnfold

上述範例著重針對具有一個自訂節點的套件，不過您可以使用相同程序下載具有多個自訂節點的套件並支援資料檔案。現在使用更全面的套件 (Dynamo Unfold) 演示這一點。

在上述範例中，首先選取 _「套件」>「搜尋套件...」_。

這一次我們將搜尋 _「DynamoUnfold」_，這是一個單字，請注意大寫。當我們看到套件時，按一下「安裝」來下載，將 Dynamo Unfold 加入您的 Dynamo 資源庫。

![](<../images/6-2/1/package introduction - installing package folder 01.jpg>)

在 Dynamo 資源庫中，我們的 _DynamoUnfold_ 群組具有多個品類和自訂節點。

![](<../images/6-2/1/package introduction - installing package folder 02.jpg>)

現在，我們看一下套件的檔案結構。首先，選取「Dynamo」>「偏好」

![](<../images/6-2/1/package introduction - installing package folder 03.jpg>)

從「偏好」快顯中，開啟「Package Manager」>「DynamoUnfold」旁邊，選取垂直圓點功能表 ![](<../images/6-2/1/package introduction - vertical dots menu.jpg>) >「展示根目錄」以開啟此套件的根資料夾。

![](<../images/6-2/1/package introduction - installing package folder 04.jpg>)

這會將我們移至套件的根目錄。請注意，我們有 3 個資料夾和 1 個檔案。

![](<../images/6-2/1/package introduction - installing package folder 05.jpg>)

> 1. _bin_ 資料夾包含 .dll 檔案。此 Dynamo 套件使用 Zero-Touch 進行開發，因此自訂節點保留在此資料夾中。
> 2. _dyf_ 資料夾包含自訂節點。此套件未使用 Dynamo 自訂節點進行開發，所以此資料夾不包含此套件的內容。
> 3. extra 資料夾包含所有其他檔案 (包括範例檔案)。
> 4. pkg 檔案是定義套件設定的基本文字檔案。現在我們可以忽略該檔案。

開啟「extra」資料夾，我們可以看到隨安裝而下載的一系列範例檔案。並非所有套件都有範例檔案，但此若套件有範例檔案，您可以在此處找到這些檔案。

接下來開啟「SphereUnfold」。

![](../images/6-2/1/rd2.jpg)

開啟檔案並按一下解析器上的「執行」後，即可展開圓球！ 諸如此類的範例檔案有助於學習如何使用新 Dynamo 套件。

![](<../images/6-2/1/package introduction - installing package folder 07.jpg>)

### Dynamo Package Manager

探索 Dynamo 套件的另一種方式是線上探究 [Dynamo Package Manager](http://dynamopackages.com)。這是瀏覽套件的良好方式，因為儲存庫會根據下載計數與受歡迎程度的順序對套件排序。此外，使用該方式可以輕鬆收集套件最近更新的相關資訊，因為某些 Dynamo 套件受 Dynamo 版次的版本與相依性限制。

在 Dynamo Package Manager 中按一下 _「Quads from Rectangular Grid」_，您可以查看其描述、版本、開發人員及可能的相依性。

![](../images/6-2/1/dpm2.jpg)

您也可以從 Dynamo Package Manager 下載套件檔案，但是從 Dynamo 直接執行此作業會更順暢。

### 套件儲存在本端的什麼位置？

如果您從 Dynamo Package Manager 下載檔案，或想要查看所有套件檔案的儲存位置，請按一下「Dynamo」>「Package Manager」>「節點和套件路徑」，就可以從此處找到您目前的根資料夾目錄。

![](<../images/6-2/1/package introduction - installing package folder 08.jpg>)

套件預設會安裝在與以下資料夾路徑類似的位置：_C:/Users/\[使用者名稱]/AppData/Roaming/Dynamo/\[Dynamo 版本]_。

### 進一步使用套件

Dynamo 社群在不斷成長與發展。透過不時地探索 Dynamo Package Manager，您會發現一些激動人心的新開發功能。在以下各節，我們將從終端使用者的視角到建立您自己的 Dynamo 套件，更深入地查看套件。
