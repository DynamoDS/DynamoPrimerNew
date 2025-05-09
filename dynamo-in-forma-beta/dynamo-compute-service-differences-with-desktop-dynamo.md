# Dynamo 計算服務與桌面版 Dynamo 的差異

本頁重點介紹編寫要在 Dynamo 計算服務雲端環境中執行的 Dynamo 程式時應注意的差異。

## 什麼是 DaaS？

DaaS、Dynamo 即服務、Dynamo 計算服務等詞彙，都是指同一件事：Dynamo 在雲端環境中執行的核心執行階段。這表示您的圖表並未在您的電腦上執行。DaaS 目前只能透過 Forma 的 Dynamo Player 延伸存取，使用者可以在當中上傳和管理在桌面環境中建立的 `.dyn` 檔案、透過延伸執行同事共用的 `.dyn` 檔案，或使用 Autodesk 提供的預先載入 `.dyn` 常式作為範例。

由於您的圖表是在此雲端環境中而不是在您的電腦上執行，因此 DaaS 目前無法直接使用傳統的 Dynamo 主體環境 (Revit、Civil 3D 等)。如果您要在圖表中使用來自這些程式的類型，必須使用 `Data.Remember` 節點或其他圖表內序列化技術，將這些類型序列化 (儲存) 到圖表中。這些工作流程類似於在 Revit 中為「衍生式設計」編寫圖表時需要使用的工作流程。

## 哪個版本的 Dynamo 在執行我的程式碼？

此版本以 3.x 為基礎，會根據 Dynamo 的開放原始碼主要分支經常更新。

## 此版本的 Dynamo 中有哪些套件/節點可用？

* 大多數核心節點，請參閱下一節瞭解一些特定限制。
* `DynamoFormaBeta` 套件，用來與 Forma API 互動。
* `VASA`，用於立體像素化/高效率分析。
* `MeshToolKit`，用於網格操控。從 Dynamo 3.4 開始，Mesh Toolkit 也可以開箱即用。
* `RefineryToolkit`，用於可進行衝突測試、檢視距離、最短路徑、等視域等等的實用演算法。

## 為 DaaS 編寫圖表時，應該注意什麼？

* Python 節點將無法運作。這些節點_目前_ 無法執行。
* 無法使用自訂套件。
* UI 節點的 UI/視圖層將不會執行。我們認為這不是核心功能的問題，但如果您發現與具有自訂 UI 的節點相關的錯誤，請記住這一點。
* 僅限 Windows 的功能無法運作。例如，如果您嘗試使用 Windows 登錄或 WPF，作業將會失敗。
* 不會載入視圖延伸。
* 檔案系統節點不太有用。您在本端電腦上參考的任何檔案，在以 DaaS 執行時都將不存在。
* Excel/DSOffice 互通性節點將無法運作。Open XML 節點應該可以運作。
* 網路請求通常無法運作，但您可以呼叫 Forma API。

## 我要記住這全部內容？如果改變了怎麼辦？

* 未來，我們打算在桌面版 Dynamo 內提供工具，更輕鬆地確保圖表在兩種環境中都同樣能執行。

## 價格為何？

* 在此 Beta 版期間，目前不收取計算時間費用。

## 如何開始使用？

請查看[部落格文章](https://dynamobim.org/dynamo-as-a-service-powers-up-dynamo-player-in-forma/)、[YouTube 系列](https://www.youtube.com/playlist?list=PLY-ggSrSwbZqlbQG1i45bpT8clCJp08wD)或 Forma 延伸中的範例以開始使用。這些內容將指導您完成：

* 存取 Autodesk Forma。
* 在桌面版安裝適用於 Dynamo 的 DynamoFormaBeta，以及在 Forma 中安裝 Dynamo 延伸。
* 編寫您的第一個圖表。

## 安全性

* 請注意，您共用的圖表會儲存在 Forma 中。
* 最長的圖表執行時間目前少於 30 分鐘。此值可能會改變。
* 執行請求受到速率限制，因此如果在太短的時間內發出許多計算請求，您可能會看到錯誤。
