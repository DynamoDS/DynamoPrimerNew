# 發佈套件

在先前各節中，我們詳細瞭解了如何使用自訂節點與範例檔案設置 _MapToSurface_ 套件。但是，如何發佈已在本端開發的套件呢？此案例研究將示範如何從本端資料夾的一組檔案發佈套件。

![](<../images/6-2/3/develop package - custom nodes 01 (1) (1).jpg>)

有許多方式可以發佈套件。以下是建議的程序：**本端發佈、本端開發，然後線上發佈**。我們從包含套件中所有檔案的資料夾開始。

### 解除安裝套件

在對發佈 MapToSurface 套件進行瞭解之前，若您已在上一課程中安裝該套件，請將其解除安裝，以便不會使用相同的套件。

首先，請前往「套件」>「套件管理員」>「已安裝的套件」頁籤，然後按一下「MapToSurface」旁的垂直圓點功能表 >「刪除」。

<figure><img src="../../.gitbook/assets/delete-map-to-surface.png" alt=""><figcaption></figcaption></figure>

然後重新啟動 Dynamo。重新開啟後，若查看 _「管理套件」_ 視窗，會發現其中應該不再包含 _MapToSurface_。現在我們準備好重新開始！

### 本端發佈套件

{% hint style="warning" %} 在 Dynamo Sandbox 2.17 版及更新版本中，只要自訂節點和套件沒有主 API 相依性，即可加以發佈。在較舊版中，只有 Dynamo for Revit 和 Dynamo for Civil 3D 中可發佈自訂節點和套件。{% endhint %}

> 在下方的連結按一下，下載範例檔案。
>
> 附錄中提供完整的範例檔案清單。

{% file src="../datasets/6-2/4/MapToSurface.zip" %}

這是為套件首次提交的檔案，我們已將所有範例檔案與自訂節點置於一個資料夾中。準備好此資料夾後，我們就準備好上傳到 Dynamo Package Manager。

![](../images/6-2/4/publishapackage-publishlocally01.jpg)

> 1. 此資料夾包含五個自訂節點 (.dyf)。
> 2. 此資料夾還包含五個範例檔案 (.dyn) 與一個匯入的向量檔案 (.svg)。這些檔案將作為介紹練習，用以向使用者示範如何使用自訂節點。

在 Dynamo 中，首先按一下 _「套件」>「Package Manager」>「發佈新套件」_ 頁籤。

在 _「發佈套件」_ 頁籤中，填寫視窗左側的相關欄位。

<figure><img src="../../.gitbook/assets/package-details.png" alt=""><figcaption></figcaption></figure>

接下來，我們將加入套件檔案。您可以選取「加入目錄」(1)，逐個加入檔案，或加入整個資料夾。若要加入非 .dyf 的檔案，請務必在瀏覽器視窗中將檔案類型變更為 **「所有檔案 (**_._**)」**。請注意，我們會加入所有檔案，不會區分該檔案是自訂節點檔案 (.dyf) 或範例檔案 (.dyn)。發佈套件時，Dynamo 會將這些項目分類。

<figure><img src="../../.gitbook/assets/map-to-surface-contents.png" alt=""><figcaption></figcaption></figure>

選取 MapToSurface 資料夾後，Package Manager 會顯示資料夾內容。如果您上傳您自己的套件，且套件的資料夾結構複雜，而您不希望 Dynamo 變更資料夾結構，則可以啟用「保留資料夾結構」切換開關。此選項適用於進階使用者，如果您的套件並非以特定方式刻意設定，最好不要開啟此切換開關，讓 Dynamo 視需要組織檔案。按一下「下一步」以繼續。

<figure><img src="../../.gitbook/assets/map-to-surface-contents-preview.png" alt=""><figcaption></figcaption></figure>

發佈前，您可以在這裡預覽 Dynamo 組織套件檔案的方式。按一下「完成」以繼續。

<figure><img src="../../.gitbook/assets/publish-locally.png" alt=""><figcaption></figcaption></figure>

按一下「本端發佈」(1)來進行發佈。若您跟著這裡的說明操作，請確定您按的是 _「本端發佈」_，**而不是** _「線上發佈」_，避免 Package Manager 中有多個重複的套件。

發佈後，在「DynamoPrimer」群組或 Dynamo 資源庫下應該會顯示自訂節點。

![](<../images/6-2/3/develop package - install package 02 (1) (1).jpg>)

現在，我們看一下根目錄，以瞭解 Dynamo 如何格式化我們剛剛建立的套件。前往「已安裝的套件」頁籤 > 按一下「MapToSurface」旁的垂直圓點功能表 > 選取「展示根目錄」，即可查看根目錄。

<figure><img src="../../.gitbook/assets/show-root-directory.png" alt=""><figcaption></figcaption></figure>

請注意，根目錄位於套件的本端位置 (請記住，我們在「本端」發佈套件)。Dynamo 目前參考此資料夾以讀取自訂節點。因此，請務必使用本端發佈功能，將資料夾發佈到永久的資料夾位置 (即不是您的桌面)。以下將分解講述 Dynamo 套件資料夾。

![](../images/6-2/4/publishapackage-publishlocally06.jpg)

> 1. _bin_ 資料夾包含使用 C# 或 Zero-Touch 資源庫建立的 .dll 檔案。我們沒有為此套件建立任何內容，所以此範例的此資料夾為空白。
> 2. _dyf_ 資料夾包含自訂節點。開啟此資料夾將顯示此套件的所有自訂節點 (.dyf 檔案)。
> 3. extra 資料夾包含所有其他檔案。這些檔案可能是 Dynamo 檔案 (.dyn)，也可能是所需的任何其他檔案 (.svg、.xls、.jpeg、.sat 等)。
> 4. pkg 檔案是定義套件設定的基本文字檔案。它是 Dynamo 中自動建立的檔案，但是如果您希望取得詳細資料，可以編輯該檔案。

### 線上發佈套件

{% hint style="warning" %} 注意：除非您要真的發佈自己的套件，否則請勿繼續執行此步驟！{% endhint %}

<figure><img src="../../.gitbook/assets/publish-version.png" alt=""><figcaption></figcaption></figure>

1. 準備好發佈後，在「套件」>「Package Manager」>「已安裝的套件」視窗中，選取要發佈之套件右側的按鈕，然後選擇「發佈」。
2. 如果您要更新已發佈的套件，請選擇「發佈版本」，Dynamo 將根據該套件根目錄中的新檔案，線上更新您的套件。非常簡單！

### 發佈版本...

若要更新已發佈套件根資料夾中的檔案，您也可以在 _「我的套件」_ 頁籤中選取 _「發佈版本...」_，以發佈新版本的套件。這是一個很順暢的方式，可以對內容進行必要更新以及與社群分享。只有當您是套件的維護者時，才能使用 _「發佈版本」_。

### 轉移套件的所有權

您目前無法透過 Package Manager 轉移套件所有權。您可以要求 Dynamo 團隊增加其他擁有者。請注意，我們無法移除現有的擁有者，只能增加更多套件維護者。如果您想要增加某個帳戶成為現有套件的擁有者，請寄送電子郵件到 [dynamoteam@dynamobim.org](mailto:dynamoteam@dynamobim.org)。請務必提供要增加的套件名稱和帳戶名稱。
