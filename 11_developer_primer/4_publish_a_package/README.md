# 發佈套件 

### 發佈套件 <a href="#publish-a-package" id="publish-a-package"></a>

套件是儲存節點並與 Dynamo 社群分享節點一個很便利的方式。套件可以包含從在 Dynamo 工作區中建立的自訂節點到 NodeModel 衍生的節點等所有內容。套件的發佈和安裝是透過 Package Manager 進行。除了此頁面，[Primer](https://primer2.dynamobim.org/6_custom_nodes_and_packages/6-2_packages/1-introduction) 也提供套件的一般指南。

#### 什麼是 Package Manager？<a href="#what-is-a-package-manager" id="what-is-a-package-manager"></a>

Dynamo Package Manager 是一種可從 Dynamo 或網頁瀏覽器存取的軟體登錄 (類似於 npm)。Package Manager 包括安裝、發佈、更新和檢視套件。與 npm 一樣，它維護不同版本的套件。它也能協助管理專案的相依性。

在瀏覽器中，搜尋套件並檢視統計資訊：[https://dynamopackages.com/](https://dynamopackages.com)

* 在 Dynamo 中，Package Manager 包括安裝、發佈和更新套件。

![搜尋套件](images/dynamopackagemanager.jpg)

> 1. 線上搜尋套件：`Packages > Search for a Package...`
> 2. 檢視/編輯安裝的套件：`Packages > Manage Packages...`
> 3. 發佈新套件：`Packages > Publish New Package...`

#### 發佈套件 <a href="#publishing-a-package" id="publishing-a-package"></a>

套件的發佈是透過 Dynamo 內的 Package Manager 進行。建議的程序是在本端發佈、測試套件，然後線上發佈與社群分享。使用 NodeModel 案例研究，我們將逐步完成必要步驟，將 RectangularGrid 節點以套件方式本端發佈，然後線上發佈。

啟動 Dynamo，然後選取「`Packages > Publish New Package...`」以開啟「`Publish a Package`」視窗。

![發佈套件](images/dyn-publish-package-add-files.jpg)

> 1. 選取「`Add file...`」瀏覽要加入套件的檔案
> 2. 從 NodeModel 案例研究中選取兩個 `.dll` 檔案
> 3. 選取「`Ok`」

將檔案加入套件內容後，為套件指定名稱、描述和版本。使用 Dynamo 發佈套件會自動建立一個 `pkg.json` 檔案。

![套件設定](images/dyn-publish-package.jpg)

> 一個準備好要發佈的套件。
>
> 1. 提供名稱、描述和版本等必要資訊。
> 2. 按一下「本端發佈」，然後選取 Dynamo 的套件資料夾：`AppData\Roaming\Dynamo\Dynamo Core\1.3\packages` 讓節點能在 Core 當中使用。在套件準備好分享之前，請永遠在本端發佈。

發佈套件後，在 Dynamo 資源庫的 `CustomNodeModel` 品類下就能使用節點。

![Dynamo 資源庫中的套件](images/dyn-publish-package-library.jpg)

> 1. 我們剛剛在 Dynamo 資源庫中建立的套件

套件一旦準備好線上發佈，請開啟 Package Manager，選擇「`Publish`」，然後選擇「`Publish Online`」。

![在 Package Manager 中發佈套件](images/dyn-publish-package-directory.jpg)

> 1. 若要查看 Dynamo 如何格式化套件，請按一下「CustomNodeModel」右側的三個垂直點，然後選擇「展示根目錄」
> 2. 在「發佈 Dynamo 套件」視窗中，選取「`Publish`」，然後選取「`Publish Online`」。
> 3. 若要刪除套件，請選取「`Delete`」。

#### 如何更新套件？<a href="#how-do-i-update-a-package" id="how-do-i-update-a-package"></a>

更新套件的程序與發佈類似。開啟 Package Manager，然後對需要更新的套件選取「`Publish Version...`」，並輸入一個更高的版本。

![發佈套件版本](images/dyn-publish-package-version.jpg)

> 1. 選取「`Publish Version`」以使用根目錄中的新檔案更新既有套件，然後選擇在本端還是線上發佈。

#### Package Manager 網路用戶端 <a href="#package-manager-web-client" id="package-manager-web-client"></a>

Package Manager 網路用戶端專門用於搜尋和檢視套件資料，例如版本管理和下載統計資訊。

透過此連結 [https://dynamopackages.com/](https://dynamopackages.com) 可以存取 Package Manager 網路用戶端

![Package Manager 網路用戶端](images/packagemanager-browser.jpg)
