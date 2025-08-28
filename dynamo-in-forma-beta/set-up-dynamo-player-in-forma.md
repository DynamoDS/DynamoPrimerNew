# 設定 Forma 中的 Dynamo Player

若要將 Dynamo 與 Forma 搭配使用，您有兩個選項：雲端型的 Dynamo 即服務 (DaaS)，或桌面版 Dynamo。兩種各有優點，取決於您要執行的作業，因此在開始之前，請先考慮哪個選項最能滿足您的需求。不過請記住，您可以隨時在這些選項之間切換。

**Dynamo 即服務與 Dynamo 桌面版的比較**

<table><thead><tr><th>Dynamo 即服務</th><th>桌面版</th><th data-hidden></th></tr></thead><tbody><tr><td>容易設定</td><td>多步驟安裝程序</td><td></td></tr><tr><td>無需安裝或開啟 Dynamo</td><td>必須開啟 Dynamo</td><td></td></tr><tr><td>不會辨識已在 Dynamo 中開啟的圖表</td><td>按一下按鈕，即可在 Player 中開啟已在 Dynamo 中開啟的圖表</td><td></td></tr><tr><td>無法使用 Python</td><td>可以使用 Python</td><td></td></tr><tr><td>只能使用經過認可的套件</td><td>可以使用任何套件</td><td></td></tr></tbody></table>

我們將在此頁面說明如何設定這兩種選項。

### 設定 Dynamo 即服務

Dynamo in Forma 版目前以搶先體驗開放測試版提供，這表示功能和使用者介面可能會經常更改。

首先，我們在 Forma 中安裝 Dynamo Player。

1. 在您的 Forma 網站中，移到左側側邊欄的**「Extensions」**，然後按一下**「Add extension」**。此時會開啟 Autodesk App Store。
2. 搜尋 Dynamo，並加入 Dynamo Player Beta。閱讀免責聲明，然後按一下**「Agree」**。

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. 您的延伸功能現在提供 Dynamo Player。按一下以將其開啟。
4. 您現在可以使用 Dynamo Player 了！

### 設定 Dynamo 桌面版

若要使用 Dynamo 桌面版，您需要 Dynamo，可以是一個獨立的 Sandbox，也可以連接至 Revit 或 Civil 3D。您也需要 DynamoForma 套件。

#### Revit

請按照以下指示，設定 Revit 中的 Dynamo 和 DynamoForma 套件。

1. 請確保您已安裝 Revit 2024.1 或更高版本。
2. 在 Revit 中移至「管理」>「Dynamo」開啟 Dynamo。
3. 在 Dynamo 中，安裝 DynamoForma 套件。移至「套件」>「Package Manager」，然後搜尋 DynamoForma。
   1. 如果您有 Revit 2024，請安裝適用於 2.x 的 DynamoForma 套件。
   2. 如果您有 Revit 2025，請安裝 DynamoForma 套件。

#### Civil 3D

請按照以下指示，設定 Civil 3D 中的 Dynamo 和 DynamoForma 套件。

1. 請確保您已安裝 Civil 3D 2024.1 或更高版本。
2. 在 Civil 3D 中移至「管理」>「Dynamo」開啟 Dynamo。
3. 在 Dynamo 中，安裝 DynamoForma 套件。移至「套件」>「Package Manager」，然後搜尋 DynamoForma。
   1. 如果您有 Civil 3D 2024，請安裝適用於 2.x 的 DynamoForma 套件。
   2. 如果您有 Civil 3D 2025，請安裝 DynamoForma 套件。

#### Dynamo Sandbox

請按照以下指示安裝 Dynamo Sandbox 和 DynamoForma 套件。

1. 從 [Dynamo 建置版本](https://dynamobuilds.com/)下載 Dynamo 2.18.0 或更高版本。為了獲得最佳體驗，請選擇頂端列出的最新、最穩定的版本。
   1. Daily (每日) 版本是開發版本，可能包含不完整或正在進行的功能。
2. 使用 [7zip](https://www.developershome.com/7-zip/) 將 Dynamo 解壓縮到您選擇的資料夾中。
3. 從 Dynamo 安裝資料夾執行 DynamoSandbox.exe。
4. 在 Dynamo 中，安裝 DynamoForma 套件。移至「套件」>「Package Manager」，然後搜尋 DynamoForma。
   1. 如果您有 Dynamo 2.x，請安裝適用於 2.x 的 DynamoForma 套件。
   2. 如果您有 Dynamo 3.x，請安裝 DynamoForma 套件。

安裝 Dynamo 後，即可開始與 Forma 搭配使用。執行 Dynamo in Forma 的桌面版選項時，您必須開啟 Dynamo 才能使用 Dynamo Player 延伸。

#### 在 Forma 中存取 Dynamo 桌面版

首先，我們在 Forma 中安裝 Dynamo Player。

1. 在您的 Forma 網站中，移到左側側邊欄的**「Extensions」**，然後按一下**「Add extension」**。此時會開啟 Autodesk App Store。
2. 搜尋 Dynamo，並加入 Dynamo Player Beta。閱讀免責聲明，然後按一下**「Agree」**。

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. 您的延伸功能現在提供 Dynamo Player。按一下以將其開啟。
4. 在頂端附近，按一下「Desktop」存取 Dynamo 桌面版。

<figure><img src="../.gitbook/assets/dynamo-desktop.png" alt=""><figcaption></figcaption></figure>

5. 您現在可以使用 Dynamo Player 了！如果您已經在 Dynamo 中開啟了圖表，只需按一下**「Connected graph」**下的「Open」，即可在 Player 中檢視。
