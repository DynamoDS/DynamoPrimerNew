# Autodesk Assistant 還無法做什麼

### 更新套件

Autodesk Assistant 還無法下載套件並安裝到 Dynamo 中

![](../../.gitbook/assets/Aa_Update.jpg)

如上圖所示，Autodesk Assistant 無法修改、安裝或解除安裝與 Dynamo 關聯的外部套件，但能夠提供說明，讓您自己安裝。


### 直接讀取和寫入檔案

Autodesk Assistant 處理 Dynamo 圖表檔案：它可以開啟圖表、將圖表插入圖元區、執行圖表，以及儲存您的工作。無法自行讀取或寫入您硬碟上的其他檔案。

在下面的範例中，圖元區上建立了一些幾何圖形。Autodesk Assistant 被要求擷取組成幾何圖形的點，並匯出為 JSON。雖然 Autodesk Assistant 無法直接產生此 JSON 檔案，但它會建議您建立一個能夠執行此作業的 Dynamo 腳本。

![](../../.gitbook/assets/Aa_WriteFileGeom.jpg)

![](../../.gitbook/assets/Aa_WriteFile.jpg)

請記住這個建議的模式。圖表外的任何內容都要透過節點傳達，而不是由 Assistant 自己傳達。

### 變更沒有圖表的 Revit 模型

![](../../.gitbook/assets/Aa_Data_sampleProject.jpg)

Dynamo 的 Autodesk Assistant 沒有直接連接到 Revit 模型。如果按照您在 Revit 中要求 Assistant 的方式編輯模型，它會告訴您沒辦法。

![](../../.gitbook/assets/Aa_Revit_Limits.jpg)

它還是可以變更您的 Revit 模型，但會採用您平常使用的方式來執行：放置 Revit 節點，將節點連接在一起，然後當圖表執行時，模型就會變更。

![](../../.gitbook/assets/Aa_viewScript.jpg)

以這種方式工作會發生兩件事：

- **在圖表執行之前，不會發生任何變更。**您可以先閱讀圖表並調整值。只有在圖表執行後，不論是您執行或 Assistant 按照您的要求執行，模型才會變更。
- **變更可重複。**圖表會留在圖元區上。儲存圖表後，您就可以在其他模型上再次執行相同的工作，或與您的團隊分享。

如果您希望在不使用圖表的情況下直接編輯一次模型，請使用 Revit 中的 Autodesk Assistant。
