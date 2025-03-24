# 發佈至資源庫

我們剛剛建立了自訂節點並將其套用至 Dynamo 圖形中的特定程序。我們非常喜歡此節點，因此，我們要將其保留在我們的 Dynamo 資源庫以在其他圖形中進行參考。若要執行此作業，我們將在本端發佈此節點。此程序與發佈套件的程序類似，我們將在下一個章節中進行詳細討論。

透過在本端發佈節點，當您開啟一個新的階段作業時該節點將可在 Dynamo 資源庫中存取。如果不發佈節點，參照自訂節點的 Dynamo 圖表也必須在其資料夾中具有該自訂節點 (或必須使用 _「檔案」>「匯入資源庫」_ 將自訂節點匯入 Dynamo 中)。

{% hint style="warning" %} 在 Dynamo Sandbox 2.17 版及更新版本中，只要自訂節點和套件沒有主 API 相依性，即可加以發佈。在較舊版中，只有 Dynamo for Revit 和 Dynamo for Civil 3D 中可發佈自訂節點和套件。 {% endhint %}

## 練習：在本端發佈自訂節點

> 按一下下方的連結下載範例檔案。
>
> 附錄中提供範例檔案的完整清單。

{% file src="../datasets/6-1/3/PointsToSurface.dyf" %}

讓我們繼續瞭解在前一個部份中建立的自訂節點。開啟 PointsToSurface 自訂節點後，我們會在 Dynamo 自訂節點編輯器中看到圖表。您也可以在「Dynamo 圖表編輯器」中按兩下自訂節點來開啟。

![](../images/6-1/3/publishcustomnodelocally01.jpg)

若要在本端發佈自訂節點，只需在圖元區上按一下右鍵，然後選取 _「發佈此自訂節點...」_

![](../images/6-1/3/publishcustomnodeexercise-02.jpg)

參照上圖填寫相關資訊，並選取 _「本端發佈」_。請注意，「群組」欄位定義可從 Dynamo 功能表存取的主要元素。

<figure><img src="../../.gitbook/assets/publish_a_package.png" alt=""><figcaption></figcaption></figure>

選擇資料夾，以容納所有您打算在本端發佈的自訂節點。Dynamo 每次載入時都會檢查此資料夾，因此請確認該資料夾位於固定位置。導覽至此資料夾，然後選擇 _「選擇資料夾」_。Dynamo 節點現已在本端發佈，每次您載入程式時，該節點都會在您的 Dynamo 資源庫中！

![](../images/6-1/3/publishcustomnodeexercise-04.jpg)

若要查看自訂節點的資料夾位置，請前往 _「Dynamo」>「偏好」>「套件設定」>「節點和套件路徑」。_

<figure><img src="../../.gitbook/assets/settings.png" alt="" width="520"><figcaption></figcaption></figure>

在此視窗中，我們看到路徑清單。

<figure><img src="../../.gitbook/assets/package-locations.png" alt=""><figcaption></figcaption></figure>

> 1. _Documents\\DynamoCustomNodes..._ 是本端發佈之自訂節點的位置。
> 2. _AppData\\Roaming\\Dynamo..._ 是線上安裝的 Dynamo 套件的預設位置。
> 3. 您可能想要在清單中將本端資料夾路徑的順序下移，只要按一下路徑名稱左側的向下箭頭即可。頂層資料夾是套件安裝的預設路徑。因此，透過將預設 Dynamo 套件安裝路徑保留為預設資料夾，線上套件將與本端發佈的節點分離。

我們已切換路徑名稱的順序，以將 Dynamo 的預設路徑作為套件的安裝位置。

<figure><img src="../../.gitbook/assets/updated-package-locations.png" alt=""><figcaption></figcaption></figure>

導覽至此本端資料夾，我們可以在 _「.dyf」_ 資料夾中找到原始自訂節點，.dyf 是 Dynamo 自訂節點檔案的副檔名。我們可以編輯此資料夾中的檔案，節點將在使用者介面中更新。我們還可以新增更多節點至主要的 _DynamoCustomNode_ 資料夾，Dynamo 會在重新啟動時將其新增到您的資源庫！

![](../images/6-1/3/publishcustomnodeexercise-08.jpg)

Dynamo 現在每次都會將「PointsToSurface」載入到 Dynamo 資源庫的「DynamoPrimer」群組中。

![](../images/6-1/3/publishcustomnodeexercise-09.jpg)
